# The canvass, the audit, and belay

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

## 8. The canvass, the readout, and the audit

### 8.1 The canvass

To canvass is to ask **every** witness in scope, one by one, for a verdict. Three properties come free:

- It is complete by definition. A poll samples; a canvass does not.
- It asks. It does not execute. So the word is true for a test and for a prose witness alike.
- No answer is a normal outcome of a canvass. That is the silent verdict, with no special case.

**The canvass asks the witnesses that can answer differently at different commits. The rest are defended by form
and by audit.** A type witness says only _affirms, if the project compiles_. A lint config line says only _the rule
is still installed_. Their real defence is [§6.6](./format.md#66-form-errors) and §8.5.

### 8.2 The join: from a marker to a verdict

Every verdict source produces the same thing: a list of `(handle, status)`. Every marker resolves to a handle. An
**adapter** is whatever converts one tool's report into that list.

| Kind         | The handle          | Where it comes from                                            |
| ------------ | ------------------- | -------------------------------------------------------------- |
| type         | the whole project   | nothing to join. A failed type check is a stop, not a readout. |
| test         | file and line range | the extent of the marker                                       |
| lint rule    | the rule id         | a literal in the declaration the marker owns                   |
| witness file | the file path       | the agent names the witness in its answer                      |

**The test join.** For each result, take the marker in the same file whose extent contains the result's line. Then
reduce per marker: any failure denies; all passed affirms; any skip and no failure is silent. This needs line
numbers and a file path only — no AST, no language knowledge — which is why an adapter stays about fifty lines and
why the same shape works for a runner in any language.

The adapter degrades in three steps, by what the runner reports:

1. **A location.** Join by the extent. Exact.
2. **Names only.** The core already read the name of the declaration the marker owns. Join by file and name prefix.
3. **Neither.** Attribute at file scope.

**The lint join** is a dictionary lookup on the rule id. Note the one asymmetry: a test that does not run is
**silent**, and a lint rule that reports nothing is **affirming**, because the linter ran over everything. Absence
means different things for the two kinds, and it is the only kind-specific logic in the whole join.

**Where adapters live.** Belay ships them. The repository states which apply and how to produce the report. The
core never sees this.

```toml
# .belay/witnesses.toml
[[source]]
kind    = "test"
command = "vitest run --reporter=json --outputFile=.belay/out/test.json"
adapter = "vitest"

[[source]]
kind    = "lint"
command = "oxlint --format=json > .belay/out/lint.json"
adapter = "oxlint"
```

**Two rules keep the join honest:**

- **Silence is the default, not a case to handle.** The join produces verdicts only for handles it finds. Every
  marker with no matching entry is silent by construction.
- **Over-attribution is permitted. Under-attribution is not.** When an adapter must guess, it attributes a denial
  to more claims rather than fewer. A false red costs a builder minutes. A false green is the failure this system
  exists to prevent.

### 8.3 A lint witness, in full

A lint witness need not be a custom rule. The handle is the rule id, and the marker's home is wherever that rule id
is declared — your own rule, or the config line that turns a built-in rule on.

```ts
/** @attests no-subprocess    crux-ignore */
export const noSubprocess = createRule({ name: "no-subprocess", … })
```

Three failure modes, three different questions, and only one of them is a verdict:

| What happened                       | Question  | Mechanism                                            | Result                        |
| ----------------------------------- | --------- | ---------------------------------------------------- | ----------------------------- |
| the rule was deleted from config    | existence | the marker was deleted with the line                 | unattested — red              |
| the rule was set to `warn` or `off` | standing  | the config line changed, so it is in the audit scope | a reader marks it **unsound** |
| the code violates the rule          | verdict   | the adapter joins on rule id                         | **denies**                    |

**Do not add a test that reads the config and asserts the rule is enabled.** It solves a problem the marker's
position already solves, and it reports a standing failure as a verdict.

### 8.4 The readout

The canvass produces the readout. **One block for each claim in scope**, headed by the claim and its coverage,
then one line for each witness:

```
core/token/cannot-be-guessed                                        covered (proposed)
  token.test.ts:14                    web crypto bytes     affirms            sound
  vite.config.ts:31                   no Math.random       affirms            sound
  token.test.ts:52                    length and alphabet  affirms            sound

renderers/colour-is-passed                                                     covered
  witnesses/renderers-take-style.md   every renderer       affirms (carried)   sound
  witnesses/style-is-not-dropped.md   the passed value     silent          unaudited

report/reads-at-a-glance                                                     unaudited
  witnesses/report-legibility.md      the summary view     silent          unaudited
```

Three questions, and each has its own place. _What does this witness say?_ — the verdict column. _Can I believe
this witness?_ — the standing column. _Is this claim held up?_ — the coverage on the claim line, where the eye
lands first.

**The block is the shape of the audit, not only of the report.** An auditor works claim by claim: collect every
witness of one claim, set a standing for each one that is in the audit scope, then read the whole set together and
set the coverage. Marker order is the wrong order, because coverage is not visible from it ([§5.8](./claims.md#58-coverage)).

The second block shows why coverage is not a summary of the standings. One of its witnesses is unaudited, and the
claim is still covered: the auditor read the set and found that the other witness reaches the whole claim on its
own. The two axes answer different questions, so neither column can be computed from the other.

**Keep the meaning narrow.** The readout is the verdicts, the standings, and the coverage. It is not the whole
package. The operator receives the amendment beside it, because the ruling compares what was wanted to what the
witnesses say:

```
amendment    what was wanted
readout      what the witnesses say
```

**Order the readout by claim.** Put the claim text first and the outcome second. Do not sort by witness kind or by
colour. That order answers a question the canvass already answered.

### 8.5 The audit, and why it needs no cache

A standing is a judgment about the **instrument**: the body of the witness, and the text of the claim it attests.
The standing is void when either side changes. Both sides are in the diff.

> **Audit scope** = markers whose instrument changed in this diff **∪** markers that attest a claim whose text
> changed in this diff.

The second term alone is not sufficient. The first term catches the case that matters most: a builder that adds a
witness to a claim that is **not** in the amendment. An extra test on an existing claim is exactly where a builder
defines its own success.

**Coverage widens the reading, and it does not widen the scope.** A coverage is void when any of the claim's
instruments changes, or when the claim text does ([§5.4](./claims.md#54-the-four-questions)). So the claims to re-cover are exactly the claims named by
the marker scope above, and the diff still bounds the work. What changes is the reading: answering coverage for
one claim needs **every** witness of that claim, and most of them did not change. An auditor reads unchanged
markers to judge a claim it was already going to judge. It sets no standing on them.

**The invariant holds by induction across merges.** Every merge audits every marker in that scope, sets the
coverage of every claim they touch, and changes nothing else. So each untouched marker still has the instrument
and the claim text that it was audited against, and each untouched claim still has the witness set it was covered
against. Therefore **a witness on the main branch is presumed sound and a claim on it is presumed covered**, and
there is nothing to look up.

**One audit pass is not a fixed point.** On the first build, an audit marked six claims red, the builder repaired
them, and a second audit found one further gap in the same package. That is normal and it is not a defect: the
repair changes the instrument, and a changed instrument is back in scope by the rule above. §9.3's cycle budget is
what bounds it.

**The record of an audit is the merge.** It sits on the PR with the readout. You never query it.

**Do not add a `last audited` directive.** A date does not tell you whether the instrument changed, and file times
do not survive a checkout. A content hash is the stronger version and is still not worth it: it protects against
one skipped audit, which is a process failure, and it costs a diff on every edit.

**The base case.** In an existing repository every witness starts unaudited and every claim starts uncovered, and
each becomes audited the first time its instrument or its claim is touched. The first audit produces the base
case. The readout shows the truth, in yellow.

**The blind spot, stated and accepted.** A witness can become vacuous without either side of its instrument
changing. A helper it calls becomes a no-op, and the witness still affirms. Neither the induction nor a hash
catches this, because both watch the instrument and not what the instrument depends on.

This is not a defect of this design. It is a condition of all software, with or without agents. The goal of this
system is to make automated development easier and less risky. It is not to solve a problem that every codebase
already has. So name the blind spot, and leave it.

### 8.6 Why the canvass and the audit stay separate

| Act         | Question                                                       | Subject                      | Voided by                               |
| ----------- | -------------------------------------------------------------- | ---------------------------- | --------------------------------------- |
| **canvass** | Does the code satisfy the claims?                              | the subject                  | any change inside `@scope`              |
| **audit**   | Do these instruments support this claim, and do they reach it? | the instrument and the claim | a change to the instrument or the claim |

The canvass is worthless without the audit, because a builder that writes its own witness decides its own success.

**This was measured, not assumed.** The first package built under this vocabulary passed every test and passed the
repository's own gate. An independent audit then marked six of its claims red. The tests asserted the ordinary
path; the claims promised the edges. Neither the runner nor the gate can find that, because both ask the subject
and the failure was in the instrument.

**Both belong to the reviewer. Neither belongs to the builder.**

> The gate is applied by somebody who did not build.

They are two acts and one artifact. Do not split the page the operator reads.

## 9. Belay

### 9.1 The sequence

1. A human operator gives belay an **amendment**.
2. Belay instructs a **builder** to implement the amendment.
3. **At any point during step 2 the builder may escalate.** Writing a witness can show that a claim does not say
   what it means ([§7](./lifecycle.md#7-the-amendment)). The builder states the proposed change to the amendment and stops. Belay puts it to the
   operator, who accepts or refuses it. The amendment is then what the operator says it is, and step 2 continues.
4. Belay instructs a **reviewer** to canvass the witnesses and audit the claims in the audit scope. This produces
   the **readout**.
5. Belay forwards the amendment and the readout to the human operator for the **ruling**.

**Step 3 is a first-class step, not an escape hatch.** The first build used it three times, and each use improved
the specification. A builder that cannot escalate has two choices, and both are worse: implement a claim it knows
is wrong, or change the specification on its own authority. §9.5 gives that authority to the operator, and step 3
is how the operator exercises it without waiting for the ruling.

It is bounded by the same budget as the rest of the loop (§9.3). A builder that escalates on every claim has been
given fog, not an amendment ([§10](./lifecycle.md#10-fog)).

### 9.2 The builder's exit condition

> **The builder hands off when the canvass is green.**

**That is not the same condition as the ruling**, and an earlier revision said it was. A green canvass says the
subject satisfies the witnesses. It says nothing about whether the witnesses support the claims, and [§4.2](./claims.md#42-how-the-condition-fails) makes an
unsound witness and an under-covered claim both red. The first build made this concrete: every test passed, the
repository gate passed, and an audit then marked six claims red.

So green is the moment the builder gives the work to a reviewer. Doneness is decided by somebody who did not
build, which is what §8.6 requires. A `change` witness is changed until it affirms. A `delete` witness is deleted
with its claim.

**There is no failure classifier.** An earlier design classified a denial as _expected_ or as _regression_ by
looking up its claim in the amendment. It is deleted. It could not resolve a marker that attests one claim inside
the amendment and one outside it, and it was never needed: the exit condition above does the same work with no
lookup and no conflict.

### 9.3 The supervisor

The cycle needs two different things, and only one of them needs a process.

- **Facts** — the phase, the gate commit, the last readout. All are derivable. The phase is _the canvass at HEAD is
  red_. The readout is on the PR.
- **Liveness** — something must notice that the builder finished, start a reviewer, and notice a stall or a crash.
  This cannot be derived.

> The supervisor holds no facts. It holds a loop. Every fact it needs, it re-derives from the branch at HEAD.

Kill it and restart it, and nothing is lost. Two rules it must carry:

- **A fresh reviewer each cycle.** A reviewer that carried context from the previous cycle has been argued at by
  the builder.
- **A cycle budget.** Without a bound the loop never ends. Exceeding it is an abort, and an abort is the third
  acceptable interruption.

**Budget for more than one audit round.** A repair changes the instrument, and a changed instrument is back in the
audit scope (§8.5), so a second audit is the normal case and not the exception. The first build needed two rounds
on one package. A budget of one round is a budget that aborts good work.

### 9.4 Where the readout is published

The readout must be where the ruling happens, tied to a commit, and not in the tree. It is void at the next commit,
so a file in the tree would go stale and would churn.

- **The amendment goes in the PR description.** It is stable for the life of the branch, and it stays visible after
  the amendment file is deleted.
- **The readout goes in a comment, one per canvass, each naming its commit.** Comments are append-only, so the
  cycle stays visible and the newest one wins. A readout whose commit is not HEAD is void, and a human can see that
  at a glance.
- **The machine form is a build artifact.** Never committed.

A check run is the better surface on one property only — the platform marks it stale for you. It is worth doing
later. It is not worth the API work first, and it renders prose badly.

### 9.5 What the human decides

The reviewer has already collected the verdicts, set the standings, and set the coverage. So the human is not
making a quality decision. **The human makes a specification decision: were these the right claims?** That is the
same authority as step 1, exercised again with the output visible.

The ruling has exactly three kinds of item to clear:

- **yellow verdicts** — witnesses whose subject changed and whom nobody asked
- **unconfirmed standings** — soundness that an agent proposed and no human confirmed
- **unconfirmed coverage** — a claim an agent proposed as covered, that no human confirmed

All three are the point where the machine reached the edge of what it can say.

**Coverage is where this authority does the most work**, because it is the question that asks how much proof a
promise needs ([§5.8](./claims.md#58-coverage)). An agent can say that a claim is reached by nothing. Only the operator can say that a claim
is reached far enough.

**The same authority is exercised at §9.1's step 3**, in the middle of a build, when a builder finds that a claim
does not say what it means. It is the same question — _is this the right claim?_ — asked before the work is
finished rather than after. Nothing about it is a lesser act.
