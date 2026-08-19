# Claims and witnesses

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

## 4. Claims and the catalog

**A claim is prose with a slug.** The prose is falsifiable, and it is written in the words its project settled
([§3](./vocabulary.md#3-the-glossary)). The slug is stable and machine-friendly, it names that project ([§3.5](./vocabulary.md#35-the-slug-carries-the-project)), and it survives a rewording of the
prose.

**A claim is declared by `@claim`, and its extent is its text** ([§6.1](./format.md#61-directives)). So a catalog file is an ordinary document
— a heading, prose that orients the reader, and then the claims — and the narrative above the first declaration
is owned by nothing, because it orients rather than promises.

**Not everything decided becomes a claim.** A claim exists to be checked, and there is no point checking something
that nothing can violate. [§11.3](./lifecycle.md#113-settled-by-construction-and-the-index-that-does-not-exist) draws that line and says where the rest goes. §4.1 draws a second line, and it cuts
somewhere else entirely.

**The catalog is present tense.** It states what the system promises **now**. One condition enforces this:

> **A claim is in the catalog only if its witnesses affirm it, each of them is sound, and together they cover
> it.**

The condition has three parts because a claim can fail in three places. A witness can deny. A witness can be
sound-looking and support nothing (§5.6). Every witness can be sound and the set can still fall short of the
claim (§5.8). An earlier revision wrote this as _a sound witness affirms it_, and the singular was wrong: it
described a claim with one witness, and it pushed every claim down to the size of one check. §5.9 records what
that cost.

Three results follow:

- A claim enters the catalog in the same merge that makes it true. It never enters earlier.
- There is no `pending` status and no promotion lifecycle. Intent lives outside the catalog. See [§10](./lifecycle.md#10-fog).
- The condition is established at entry and maintained by each merge. This is the same shape as the audit
  invariant in [§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache). So a claim that is already in the catalog may have a silent witness at a later commit, and a
  claim that an amendment **adds** may not. A new claim has no earlier affirmation to carry forward.

Do not call such a claim _unsound_. **Unsound** is the standing of a witness, and one word must not have two
meanings.

### 4.1 The subject must be rederivable from the repository

> **Crux judges the repository. A claim whose state is not rederivable from the repository alone is not a claim.**

Everything else in this document rests on _the repository is the only thing that changes between merges_. §5.5
computes silence from that, and [§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache)'s induction is that sentence written as an invariant. Neither said it out
loud, and both were assuming it.

**Reality drifting from the repository is a different problem with a different shape.** It is monitoring: scheduled
rather than diff-triggered, with no pull request to attach a verdict to and no merge at which to rule. Crux is not
shaped to catch it, and stretching it to try produces the worst artifact available — a claim whose witness goes
green after its first audit and stays green no matter what is true.

That is the failure worth spelling out, because the drafts that reach for it all look reasonable. Give such a
witness a `@scope` and nothing inside the scope ever changes, so §5.5 carries the affirmation forever. Take the
`@scope` away and it is silent on every canvass instead, which is honest and puts a permanent yellow row in front
of the operator at every ruling — which is how an operator learns to wave yellow through.

**The rule has teeth, and it is not a restatement of _witness what you can_.** Applied to a real project it deleted
four claims about DNS records and dashboard settings, and cut a fifth in half mid-sentence: of _the sending address
is on the onboarded subdomain, and the recipient is a verified destination address_, the first clause survived
because the configuration file is in the repository, and the second did not because the verification happened in a
dashboard.

**The corollary is not a loophole.** A command that talks to a service is a legitimate witness when it runs fully
locally — a dry run, an emulator, a `--local` flag. Those read repository configuration and ask the service
nothing, so their answer is rederivable from a checkout.

**A value the repository does not hold raises the claim. It does not delete it.** This is the near neighbour of the
rule above, it arrives from the other direction, and the two have opposite remedies:

|                       | Lives outside the repository | Remedy                                 |
| --------------------- | ---------------------------- | -------------------------------------- |
| the claim's **state** | is it still true?            | **delete the claim.** This is not one. |
| the claim's **data**  | what is the value?           | **raise the claim** one level          |

A public repository keeps its domain in configuration, so _the sender address is on the mail subdomain_ is not
rederivable — no checkout holds the value to compare against. What survives is a claim one level more abstract:

> No address is written as a literal. Every address the worker sends from is constructed from the configured mail
> domain.

The value is unavailable, so the claim becomes about the **mechanism** that consumes the value. And the abstract
version is the better claim: it is violated by a hardcoded address anywhere in the worker, which is the failure
that would actually happen, where the concrete version only ever checked one string.

**This line does more work than any other in the document.** Inside the checkout or outside it decides whether a
claim can exist, whether a claim must be raised a level, and — per [§10](./lifecycle.md#10-fog) — how expensive a piece of fog is to clear.
Three unrelated problems, one boundary.

### 4.2 How the condition fails

A claim with no marker at all is one case of a claim that no sound witness affirms. It is vacuously the same
failure. It gets its own name only because a machine finds it without asking anything, which makes it the cheapest
of the three to detect.

| Failure                                                                        | Question  | Found by                                            | Result              |
| ------------------------------------------------------------------------------ | --------- | --------------------------------------------------- | ------------------- |
| the claim has no marker — **unattested**                                       | existence | a form check. Nothing is asked and nothing is read. | red — stop          |
| a witness **denies**                                                           | verdict   | the canvass                                         | red — stop          |
| a witness is **unsound**                                                       | standing  | the audit                                           | red — stop          |
| the claim is **under-covered**                                                 | coverage  | the audit                                           | red — stop          |
| every witness is **silent**                                                    | verdict   | the canvass                                         | yellow — the ruling |
| a standing is **unaudited**, or an agent proposed it and no human confirmed it | standing  | the audit                                           | yellow — the ruling |
| coverage is **unaudited**, or an agent proposed it and no human confirmed it   | coverage  | the audit                                           | yellow — the ruling |

**Unattested and under-covered are the same failure at two sizes.** A claim with no marker is not reached by
anything. A claim that is under-covered is reached in part. The first is free to find and the second needs an
intelligence, which is why they are separate rows and one condition.

**A red item never reaches a human.** It is a stop before the ruling, and not an entry in a readout that anybody
looks at. Work returns to the builder.

So the readout that an operator receives always has, for every claim in scope, a set of witnesses where none is
unsound, none denies, and the set covers the claim. The only open items are the three yellow rows, and the ruling
is what closes them. After the ruling, the condition above holds on the human's authority.

## 5. Witnesses

### 5.1 A witness is a marker

There is no witness registry, no witness id, and no witness record. **A witness exists only as a marker**, and the
marker index is derived on every run and stored nowhere. Three results:

- **Existence is self-enforcing.** Delete the test, the type declaration, or the lint config line, and the marker
  goes with it. The claim becomes unattested and stops at the form check, with no tool-specific knowledge in the
  core.
- **A marker has exactly two consumers.** An **adapter** converts it into a verdict. An **auditor** converts it
  into a standing. Neither writes anything back.
- **A witness needs no stable identity.** Every question about it is answered by a diff, and a diff already tracks
  a moved file. There is nothing to key and nothing to migrate.

### 5.2 The four kinds

Push each claim as far up this list as it honestly goes.

|     | Kind           | Verdict from         | Can it deny before the build?                     |
| --- | -------------- | -------------------- | ------------------------------------------------- |
| 1   | type or schema | the compiler         | no — it lands with the work                       |
| 2   | test           | the runner           | yes                                               |
| 3   | lint rule      | the linter           | not cleanly — a new rule denies on untouched code |
| 4   | witness file   | a person or an agent | no such state — it is prose                       |

Kinds 1 to 3 are **computational** and need an adapter. Kind 4 is **inferential** and needs none, because the judge
names the witness in its answer.

**The ladder has a supply side, and the ceiling is not set by the claim alone.** _Push it as far up as it honestly
goes_ reads as though the claim decides how far that is. What your ecosystem hands you decides it too. Adopting a
library that ships type-aware lint rules raises the ceiling for every claim in the project at once: each rule you
enable becomes a development claim with a working witness at the cost of one comment on the config line that turns
it on ([§8.3](./review.md#83-a-lint-witness-in-full)), with no custom rule and no new adapter.

That is a reason to choose a library that has nothing to do with the library's runtime behaviour, and it is worth
weighing at the moment of choosing, because the ceiling it sets is permanent.

A witness file names a target and states a judgment. The target is a command to run, or the code to read.

```md
> @attests report/reads-at-a-glance crux-ignore
> @scope src/report/** crux-ignore

# The summary report reads at a glance

Run `demo report --fixture test/fixtures/mixed-status`.

Valid when: a failing row is distinguishable from a skipped row without reading
the labels; nothing wraps at 80 columns.
```

**A claim whose content is a file is witnessed by a test that reads the file.** That is a test, not a separate
kind. The kinds that cannot carry a comment — JSON, and similar — are covered in [§6.3](./format.md#63-markdown-and-files-that-take-no-comment).

### 5.3 Subject and instrument

Every witness has two sides, and each question belongs to exactly one of them.

> The **verdict** is about the subject. The **standing** is about the instrument. Each is voided only by changes to
> its own side.

- The **subject** is the code the witness judges. `@scope` names it.
- The **instrument** is the marker, the lines it owns, and the text of the claims it attests.

The audit follows from this in one sentence: **the audit confirms that the instrument supports the marked claim.**
Nothing about the subject enters it.

**A standing belongs to a marker and a claim together, not to a marker.** A marker that attests three claims has
three standings, and they can differ. The same test can support one claim and say nothing at all about the next
one on the same line. So the audit reads a pair, and the repair for one bad pair is to remove that one `@attests`
directive, not always to delete the marker.

### 5.4 The four questions

| Question      | Answered by                         | Cost                 | Voided by                                                            | Default                           |
| ------------- | ----------------------------------- | -------------------- | -------------------------------------------------------------------- | --------------------------------- |
| **existence** | a form check over the marker index  | free                 | nothing — recomputed every run                                       | always recomputed                 |
| **verdict**   | an adapter, or a judge              | free, or expensive   | a change inside `@scope`                                             | **asked**; carried by exception   |
| **standing**  | an auditor. Always an intelligence. | **always expensive** | a change to the instrument, or to the claim text                     | **carried**; audited by exception |
| **coverage**  | an auditor. Always an intelligence. | **always expensive** | a change to **any** of the claim's instruments, or to the claim text | **carried**; audited by exception |

Existence and verdict attach to one witness. Standing attaches to one witness and one claim. **Coverage attaches
to the claim alone**, and it is the only question that cannot be answered by looking at one marker.

The symmetry inverts, and that inversion is the design:

- A **verdict** is asked by default, and carried only when asking is expensive — that is, only for a witness file.
- A **standing** and a **coverage** are carried by default, and audited only by exception — always, for every
  kind, because an intelligence is the only thing that can set either.

### 5.5 The verdict

| Verdict               | Colour | Meaning                                                                  |
| --------------------- | ------ | ------------------------------------------------------------------------ |
| **affirms**           | green  | The witness was asked, and the subject satisfies the claim.              |
| **affirms (carried)** | green  | Nothing in the subject changed, so the previous affirmation still holds. |
| **silent**            | yellow | The subject changed, and nobody asked this witness.                      |
| **denies**            | red    | The witness was asked, and the subject does not satisfy the claim.       |

**Silence is computed, not decided.** By the same induction as [§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache), the last verdict of every witness on the main
branch is _affirms_, because §4's condition held at the merge. So on a branch:

- Nothing inside `@scope` changed since the merge base → **affirms (carried)**.
- Something changed and nobody re-asked → **silent**.

There is no prior verdict to store, because the prior verdict is always _affirms_. This drops yellow from _every
inferential witness on every PR_ to _only the ones this diff could have touched_.

**Carry-forward is where §4.1 is load-bearing.** _Nothing changed, so the affirmation still holds_ is only sound
because the repository is the only thing that could have changed. A claim about state outside it would be carried
forever on that reasoning while the world moved underneath — which is why §4.1 refuses the claim rather than
patching the carry rule.

A computational witness is never carried. It is cheap to ask, so it is asked every canvass. Only a skipped test is
silent.

> A red readout does not go to a ruling. A yellow readout does, and each yellow entry is a decision the human must
> make.

### 5.6 The standing

A standing is set for one marker against one claim (§5.3).

| Standing      | Meaning                                                                                      |
| ------------- | -------------------------------------------------------------------------------------------- |
| **sound**     | An auditor read the instrument. It supports this claim.                                      |
| **unsound**   | It does not support this claim. Remove this `@attests`, repair the instrument, or delete it. |
| **unaudited** | Nobody has read it against this claim.                                                       |

**Sound does not mean sufficient.** A sound witness supports its claim. It does not have to reach the whole of
it. A test that observes the token's byte source is sound for _nobody can guess the token_, and it settles only
one of the ways that claim can fail. Whether the set of witnesses reaches the whole claim is a different question,
and §5.8 asks it.

This is the narrow reading, and it is deliberate. The wide reading — _sound means this marker attests the full
claim_ — is what an earlier revision implied, and §5.9 records the damage it did.

**A standing carries its source.**

- An agent can set **unsound** by itself. To find a bad witness needs no authority.
- An agent **proposes** sound. The human confirms it at the ruling.

### 5.7 Witnesses that attest several claims

A marker may carry several `@attests` directives. Two rules keep this honest.

**Every claim needs at least one witness that attests it alone**, whatever its kind. Otherwise its verdict is
coupled to another claim's forever. An integration test that attests four claims is legitimate as a supplement. It
is a problem only when it is the sole proof, and then the coupling is the signal: either these are one claim, or
they lack real witnesses.

**The cost of a shared marker is its audit.** A marker that attests three claims carries three standings (§5.3),
and a change to the instrument re-opens all three. Nothing forbids stacking claims on one marker, and nobody does
it twice. The claims that belong on one marker are the ones that rise and fall together.

For an inferential witness, sharing is nearly free: a witness file is a container, and three markers in one file
are three witnesses with three scopes and three judgments.

### 5.8 Coverage

§5.7 looks at one marker with many claims. This is the other direction: one claim with many markers. It is the
ordinary case, and until this revision the document had no word for the question it raises.

> **Coverage is whether a claim's witnesses, taken together, uphold it.**

| Coverage          | Meaning                                                                           |
| ----------------- | --------------------------------------------------------------------------------- |
| **covered**       | An auditor read every witness of this claim together. They reach the whole claim. |
| **under-covered** | Part of the claim is reached by nothing. Add a witness, or lower the claim.       |
| **unaudited**     | Nobody has read the set as a set.                                                 |

**Every witness sound does not mean the claim is covered.** That sentence is the reason the axis exists. Standing
is a judgment about one instrument, and no number of them adds up to a judgment about the claim. A claim can hold
three sound witnesses and still promise something that nothing checks.

**Coverage carries its source, on the same asymmetry as a standing** (§5.6). An agent can declare
**under-covered** by itself, because finding a gap needs no authority. An agent **proposes** covered, and the
human confirms it at the ruling. That is the same authority as [§9.5](./review.md#95-what-the-human-decides)'s, and it is the same reason: an agent can see
that a claim is not reached, and only the operator can say that a claim is reached far enough.

#### The gap that appears most often

> **A witness that closes one way to fail does not affirm the way to succeed.**

The set of wrong implementations is unbounded. A prohibition removes one member of it. So a lint rule that forbids
`Math.random` is sound for _nobody can guess the token_ — it removes a real way to fail — and a hand-written weak
generator passes it without complaint. Nothing in that marker observes what the code does instead.

The repair is a second witness with the opposite polarity: a test that watches the production path take its bytes
from the approved source. Note what the repair is **not**. It is not a second claim. The claim was never compound.
One promise, two failures, two witnesses, one coverage.

This was the first thing the audit of the first build found, and it is the check to run first on any claim whose
witnesses are all prohibitions.

#### The two repairs, and why the second one matters

Under-coverage has two exits, and a builder may take either.

- **Add a witness**, until the set reaches the claim.
- **Lower the claim**, until the claim reaches the set.

The second exit is not a defeat. It is the force that keeps §5.9 honest. §5.9 pushes a claim up, toward the
failure a reader can see. Coverage pushes it down, toward what the witnesses actually reach. Neither force is
correct on its own:

> The altitude of a claim is where those two forces balance. A claim above it promises what nothing checks. A
> claim below it describes its own witness, and no operator can rule on it.

#### The cost, stated and accepted

Coverage has no mechanical form and never will. It joins [§13.3](./roadmap.md#133-the-dogfooding-rule)'s _never automate_ column, beside _claim, or
settled by construction?_ It is also the one judgment in this document that cannot be made by reading the diff
alone: answering it needs every witness of the claim read together, and most of them did not change. [§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache) pays
that cost and says why the bound still holds.

The alternative was to require that each marker attest its full claim. That rule needs no new axis and it is
cheaper to audit. It was tried, on a real package, and §5.9 is what it produced.

### 5.9 Group claims by the failure a reader can see

> **Group claims by the failure a reader would notice. Do not group them by the check that finds it.**

Two properties are two claims when they can fail separately **and** their separate failures mean different things
to the person the promise is made to. If the reader sees one failure, it is one claim, whatever number of checks
it takes to hold it up.

Apply it to a token. A weak generator, a `Math.random` call, and a sixteen-byte value are three defects with three
different checks. They produce one visible failure: **somebody can guess the token.** So the claim is _nobody can
guess the token_, and it holds three witnesses of three kinds. Apply it to configuration. An absent time zone and
the string `"Mars/Olympus"` both produce one visible failure: the system uses the wrong local date, or it does not
start.

The rule cuts the other way just as often. _At most one entry exists for a local date_ and _a second answer
replaces the first_ can each fail while the other holds, and a reader sees two different things — duplicated rows,
or stale data. Two claims.

#### What this replaces, and why the old rule looked true

An earlier revision of this section said the opposite:

> ~~A claim that appears to need two witnesses of different kinds should be split — or its second half is out of
> scope.~~

**That is retracted.** A mismatch of witness kinds is not evidence that a claim is compound. It is the ordinary
shape of a claim that a reader would recognise, because one visible failure usually has several causes, and
different causes are found by different instruments.

The rule looked true because all three of its examples were doing other rules' work:

| The example                          | The rule that actually applied                                               |
| ------------------------------------ | ---------------------------------------------------------------------------- |
| _the token is random_                | §5.8 — a prohibition does not affirm a provenance. One claim, two witnesses. |
| _the backup lands in object storage_ | §4.1 — the second half was not rederivable from the repository               |
| _configuration is required_          | §5.8 — compile-time and runtime absence are two witnesses on one claim       |

None of the three needed a split on the ground the rule gave. Two of them were split anyway, and the splitting is
what caused the damage below.

#### What the wrong altitude costs

The first package built under this vocabulary entered its build with eleven claims and left with fourteen. The
splits were forced: the auditor required each marker to attest its full claim, so a claim that no single marker
could reach had to be cut until one could. Three things went wrong, and all three were reported by the operator
who had to read the result.

- **The catalog stopped reading as a set of promises.** Three claims described the byte source, the forbidden
  call, and the output shape of one token. The system promises none of those three things to anybody. It promises
  that the token cannot be guessed.
- **A witness took a claim's name.** `core/config/is-context-service` states that the configuration is a service
  in the framework's context. That is a description of the instrument, not of the subject (§5.3). It was the type
  witness for the configuration claim, promoted to a claim because nothing else could hold it.
- **The review cost rose with no gain in safety.** Every split multiplied the audit: a separate question, a
  separate standing, a separate row, for parts of one promise that rise and fall together.

> A claim that describes its own witness is a witness that took a claim's name. A claim is about the subject.

**The catalog is read by a human at the ruling**, and [§9.5](./review.md#95-what-the-human-decides) says what that human decides: _were these the right
claims?_ An operator has an opinion about _nobody can guess the token_. An operator has no opinion about which
context service holds the time zone. A catalog written at witness altitude cannot be ruled on, and a catalog that
cannot be ruled on has lost the job §4 gave it.
