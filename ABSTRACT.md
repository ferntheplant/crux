# FRAMEWORK — claims, witnesses, and the ruling

It is written in ASD-STE100 Simplified Technical English.

**Status.** The vocabulary and the mechanism are settled. No code implements them. §13 holds what gets built
first. §15 holds what is still open.

This revision is the first one shaped by a project outside crux. §4.1, §5.8, §10, §11.2, and §12.1 all exist
because something was tried and did not survive contact with the format. §13.2 records that the dogfooding period
of §13.3 has met its exit condition.

Three durable artifacts, each answering a different question. The **catalog** says what the codebase promises
(§4). A **glossary** says what the words in a promise mean (§3). A **rationale** says why a promise reads as it
does, and what was rejected on the way (§11).

---

## 1. What the system is for

> Organise the requirements of a project so that it is cheap to judge whether the codebase satisfies them.

Two failures motivate this. An agent builds the wrong thing. An agent builds the right thing badly. Both are the
same failure: work was accepted, and there was nothing to judge it against. The repair is to make the thing that
work is judged against into a first-class artifact.

## 2. The vocabulary

| Word           | Meaning                                                                                              |
| -------------- | ---------------------------------------------------------------------------------------------------- |
| **project**    | What a `GLOSSARY.md` declares. It owns one catalog, one rationale directory, and one slug prefix.    |
| **glossary**   | What a project's words mean. One `GLOSSARY.md` per project. Prose, never parsed.                     |
| **claim**      | A short, falsifiable statement of a requirement the codebase must satisfy. It carries a stable slug. |
| **catalog**    | The organised set of all claims in a project.                                                        |
| **rationale**  | A document that says why a claim reads as it does, and what was rejected on the way.                 |
| **fog**        | Material you want, but cannot yet state as a claim.                                                  |
| **witness**    | A mechanism that judges whether some part of the codebase satisfies a claim.                         |
| **marker**     | The comment block that designates a witness. A witness **is** its marker.                            |
| **directive**  | One `@name` instruction inside a block. §6.1 lists the six.                                          |
| **attest**     | The relation a marker records. Witness X attests claim Y.                                            |
| **subject**    | The code a witness judges. `@scope` names it.                                                        |
| **instrument** | The witness itself: the marker and the lines it owns, plus the claim text it attests.                |
| **existence**  | Whether a witness is still installed. A form check over the markers.                                 |
| **verdict**    | What a witness says about the **subject**: **affirms**, **denies**, or **silent**.                   |
| **standing**   | Whether the **instrument** logically attests its claim: **sound**, **false**, or **unaudited**.      |
| **amendment**  | The set of claim changes that one unit of work proposes.                                             |
| **canvass**    | The act of asking every witness in scope for a verdict.                                              |
| **adapter**    | A converter from one tool's report into verdicts, keyed on markers.                                  |
| **auditor**    | An intelligence that converts a marker into a standing.                                              |
| **readout**    | What a canvass produces. One entry per claim in scope, with its verdicts and standings.              |
| **audit**      | The act of reading an instrument to decide its standing.                                             |
| **ruling**     | The decision a human makes at the merge.                                                             |

### 2.1 The naming rule

> Use a formal word for a **thing**. Use a plain word for an **event** or a **mechanic**.

A thing is a concept that a person must learn once, so it earns a precise word. An event does not, because the
objects in the sentence already say what happens. This is why the operations inside an amendment stay **add**,
**change**, and **delete**.

The vocabulary is not a metaphor to maintain. Most of the words above are everyday English that carry a precise
meaning here. Only **amendment** and **ruling** are legal in shape, and they are legal because the two moments
really are: a change to a standing body of statements, and a decision that only an authorised person can make.

### 2.2 Words that were rejected, and why

Record these so that they are not proposed again.

| Word                   | Rejected because                                                                                                                                                                                                                                                       |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **delta**              | Too generic for a core object.                                                                                                                                                                                                                                         |
| **review**             | Too generic, and it fused the canvass with the audit.                                                                                                                                                                                                                  |
| **run**                | You cannot run a prose witness. The word is false for the witnesses that need a human most.                                                                                                                                                                            |
| **hearing**            | A hearing is a formal event where the judgment happens. A canvass decides nothing.                                                                                                                                                                                     |
| **deposition**         | Correct in meaning, strange to say.                                                                                                                                                                                                                                    |
| **discovery**          | It means _find out what you do not know yet_. That is fog. The two would collide.                                                                                                                                                                                      |
| **sweep**              | It can also mean _clear away_.                                                                                                                                                                                                                                         |
| **poll**               | A poll samples. A canvass is complete. Held as the fallback if the spelling of _canvass_ is too much friction.                                                                                                                                                         |
| **passes** / **fails** | Ambiguous. "The witness fails" can mean the code is bad or the witness is bad.                                                                                                                                                                                         |
| **decision**           | The decision is the claim. A word for it here would name the same thing twice.                                                                                                                                                                                         |
| **ADR**                | It holds the decision **and** the reasoning. Crux splits the two, so the name is false. See §11.                                                                                                                                                                       |
| **`CONTEXT.md`**       | _Context_ is already ambiguous, and it names a boundary rather than the word list the file holds.                                                                                                                                                                      |
| **workspace**          | A name for the root project. _The root project is the project at the root_, so the overload describes rather than puns. The first repository to adopt this vocabulary rejected the word on sight, and rejected the argument for it a second time when it was restated. |

Four directives were proposed and rejected. Record these too.

| Directive             | Rejected because                                                                                                                                                                                                            |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@decision`           | An anchor from code to a rationale. A plain Markdown link does the same for a reader, and the core resolved nothing.                                                                                                        |
| `@witness`            | An opener for a witness block. `@attests` already opens it, so the directive carried no information.                                                                                                                        |
| `@record`             | A noun for a rationale. The only query it served is `ls`.                                                                                                                                                                   |
| a required terminator | It makes the extent **stated** rather than **correct**, and a hand-placed one under-extends. See §6.2.                                                                                                                      |
| a bare `@end`         | `@end` is a keyword in Objective-C and a command in Texinfo. A stray one truncates a real marker's extent, which is the one failure that lets a false witness survive. The terminators name their opener instead. See §6.2. |

### 2.3 The tools are named outside the vocabulary

The tools are **crux**, **belay**, **cairn**, and **beacon**. The names come from mountains, and none of them means
anything in this system. §12 says what each one does.

This is deliberate. The vocabulary above is what a person must learn, and it is already long. A tool name that also
carried a meaning here would be a second thing to learn, and it would compete with the word it resembles. So a tool
name is an address and nothing more.

Two results:

- **A tool can be renamed, and no migration follows.** No word in §2 depends on a tool name.
- **Do not name a tool after a word in the vocabulary, and do not pull a tool name into it.** A tool named _canvass_
  or _marker_ would give one word two meanings, which §2.1 exists to prevent.

## 3. The glossary

**A claim is prose, and prose is falsifiable only if its words are.** Two people who read _cancellation_
differently cannot agree on whether a witness attests it, and the disagreement surfaces at the audit, where it is
most expensive. So a project settles its words first, and the settlement is a `GLOSSARY.md`.

### 3.1 What it holds

A glossary, and nothing else. Not a specification, not a design note, not a scratch pad. Every entry is a word and
what it means.

```md
> @project ordering crux-ignore

# Ordering

Receives and tracks customer orders.

## Language

**Order**:
A request from a customer for goods at agreed prices.
_Avoid_: purchase, transaction

**Invoice**:
A request for payment, sent after delivery.
_Avoid_: bill, payment request
```

Four rules:

- **Be opinionated.** When several words name one concept, choose one and list the rest under `_Avoid_`. A
  glossary that admits synonyms has not done its job.
- **Define what a term is, not what it does.** One or two sentences.
- **Only terms this project gives a special meaning.** A general programming word belongs here when this project
  narrows it, and never because the project uses it a lot.
- **Group terms under subheadings when clusters appear.** A flat list is right until it is not.

**The glossary is not claimable.** It states what words mean, and no code can falsify a definition, so there is
nothing for a witness to be asked. A project may still claim conformance _to_ it — _no identifier uses a rejected
term_ is a claim with an obvious lint witness — but that claim is about the code. The definition is not.

### 3.2 Crux does not read it

> **Crux knows the glossary by name, and it reads one directive in it. It never learns what a term means.**

This is the same refusal as §6.1, one level up. There, the core never learns the comment syntax of any language.
Here, it never learns what a term is.

**The `@project` line is not an exception to this.** It is the ordinary line scan of §6.1, applied in a file crux
already had to find, and none of the three arguments below touches it: it needs no grammar, it matches nothing
fuzzily, and it protects a case a person cannot close by hand. What §3.2 refuses is the **definitions**, and a
prefix is not one.

An earlier draft made the glossary load-bearing: crux would index the terms, and a changed definition would put
every claim using that term into the audit scope. Three things killed it.

- **The file would need a grammar.** `**Order**:` followed by a definition is a Markdown micro-format, and §6.3
  rejects frontmatter for exactly this reason — it makes the core learn a second format and a rule for when to
  apply which.
- **The match would be fuzzy in the unsafe direction.** Even with a perfect term index, _does this claim use this
  term_ is a natural-language question. A word-boundary match misses a claim that is **about** orders without
  containing the word. Under-attribution is the direction §14 property 11 forbids, and a matcher that under-matches is
  worse than none, because §8.5's induction would then rest on it silently.
- **The case it protects is already closed by the person making it.** If you redefine a term and it changes what a
  claim promises, you reword the claim — and rewording claim text is already the second term of the audit scope.
  What remains is the case where the editor read every claim using that word and judged them all still correct.
  That is not a gap a machine needs to close.

So the glossary is an input to the **auditor**, which §5.4 says is always an intelligence. An auditor reads the
instrument and the code in `@scope`; it reads the glossary beside them. That needs no format at all.

**The glossary is privileged without being parsed.** It has a fixed name, one directive, a reader on every audit,
and a row on the readout when it moves. None of that requires a grammar.

### 3.3 A changed glossary is a yellow row

The one mechanical hook, and it is deliberately weak:

> **When a glossary changes in a diff, the readout says so and lists the claims of that project.**

It is not an audit trigger and it voids nothing. It is a row the human clears at the ruling, in the same family as
§4.2's two yellow rows: the machine has reached the edge of what it can say, and the question — _did the meaning
move?_ — goes to the person who moved it.

The cost is a file-level diff and no parsing.

### 3.4 A glossary declares a project

> **A project is a `GLOSSARY.md`. Its `@project <prefix>` declares the prefix that its claims carry.**

That is the whole definition, and it is the only place the set of projects comes from. A project holds one
glossary, one catalog, and one rationale directory. A monorepo holds several sets, and the repository root is a
project like any other. **The root project is named `root`.** The overload with _root_ as a position is a
description rather than a pun — the root project **is** the project at the root — so the thing needs no
disambiguating from the position. `workspace` was proposed and rejected; see §2.2.

```
/
├── GLOSSARY.md              declares root
├── docs/
│   ├── catalog/
│   └── rationale/
└── apps/
    ├── cairn/
    │   ├── GLOSSARY.md      declares cairn
    │   └── docs/{catalog,rationale}/
    └── belay/
        ├── GLOSSARY.md      declares belay
        └── docs/{catalog,rationale}/
```

**The prefix is declared, not derived, and `apps/cairn` is why.** A prefix taken from the directory would have to
choose between `apps` and `cairn`, would collide the moment `apps/core` sits beside `packages/core`, and would
break whenever a directory is named differently from the thing inside it. Worse, deriving it at all means reading
where a file sits, which §6.6 forbids outright. One declared token removes the layout from the question, and the
**misfiled** check becomes what every other check already is: one directive compared against another.

**The name is a format rule. The position is a house rule.** `GLOSSARY.md` is how crux finds a project, so that
name is load-bearing and a repository does not get to rename it. Where the file sits is free, because the prefix
no longer depends on it. The split is sharper than the one it replaces, not looser.

**A project needs no claims to exist.** It exists because its glossary declares it. An application repository can
reach a working catalog with the root project holding nothing at all, where a tool monorepo uses its root
immediately — and crux does not have to tell the two apart.

**Shared vocabulary is handled in prose.** Belay and cairn both use crux's words. Rather than a root glossary that
every project inherits, or a dependency graph that crux would have to learn, `apps/belay/GLOSSARY.md` opens with a
sentence: _this project uses the crux vocabulary; the words below are belay's own._ An intelligence reading it does
the right thing. That is the dividend of not parsing it.

### 3.5 The slug carries the project

> **A claim slug begins with its project's prefix. Always, and in a single-project repository too.**

`belay/readout/names-its-commit`. `crux/marker/extent-ends-at-end`. `root/squash-merges-only`, at the root. And
`demo/close/deletes-first` in a repository that holds only `demo`.

The reason is §12: **a tracker stores slugs and never claim text**, and beacon routes on a slug with no checkout.
Both must place a claim without an index, and a prefix is the only thing that does that with pure string work.

**The single-project exemption is deleted, and `@project` is what paid for it.** An earlier draft dropped the
prefix when a repository held one project, on the grounds that `close/deletes-first` reads better and that the
rewrite on a later split is mechanical. Two things changed. The prefix is now one declared token rather than a
convention, so writing it costs one line in one file. And an exemption would make `@project`'s token optional —
the only directive in the format with a variable arity, and a rule to memorise for a saving of one path segment.

What it buys is that **a split never rewrites a slug**. The cost fell only on projects that succeeded, which is
the worst population to charge, and a slug that appears in a tracker beside another project's is better off
prefixed from the first day it is written.

**The kind is not in the slug.** `@kind` carries it (§6.1), so a claim is never `belay/capability/...`. A slug
names a project and then an area, and the audience it is written for is an attribute that can be corrected
without a rename.

The kind is also **not derivable from the prefix**, which is what earns it a directive rather than a convention. A
root project's claims are mostly development-kind — conventional commits, the lint rules, the gate — and that
correlation invites the shortcut. It is only a correlation: a package holds development claims too, because a
package has its own lint rules.

**Crux checks the prefix.** A claim whose prefix names no declared project is misfiled. §6.6.

## 4. Claims and the catalog

**A claim is prose with a slug.** The prose is falsifiable, and it is written in the words its project settled
(§3). The slug is stable and machine-friendly, it names that project (§3.5), and it survives a rewording of the
prose.

**A claim is declared by `@claim`, and its extent is its text** (§6.1). So a catalog file is an ordinary document
— a heading, prose that orients the reader, and then the claims — and the narrative above the first declaration
is owned by nothing, because it orients rather than promises.

**Not everything decided becomes a claim.** A claim exists to be checked, and there is no point checking something
that nothing can violate. §11.3 draws that line and says where the rest goes. §4.1 draws a second line, and it cuts
somewhere else entirely.

**The catalog is present tense.** It states what the system promises **now**. One condition enforces this:

> **A claim is in the catalog only if a sound witness affirms it.**

Three results follow:

- A claim enters the catalog in the same merge that makes it true. It never enters earlier.
- There is no `pending` status and no promotion lifecycle. Intent lives outside the catalog. See §10.
- The condition is established at entry and maintained by each merge. This is the same shape as the audit
  invariant in §8.5. So a claim that is already in the catalog may have a silent witness at a later commit, and a
  claim that an amendment **adds** may not. A new claim has no earlier affirmation to carry forward.

Do not call such a claim _false_. **False** is the standing of a witness, and one word must not have two meanings.

### 4.1 The subject must be rederivable from the repository

> **Crux judges the repository. A claim whose state is not rederivable from the repository alone is not a claim.**

Everything else in this document rests on _the repository is the only thing that changes between merges_. §5.5
computes silence from that, and §8.5's induction is that sentence written as an invariant. Neither said it out
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
claim can exist, whether a claim must be raised a level, and — per §10 — how expensive a piece of fog is to clear.
Three unrelated problems, one boundary.

### 4.2 How the condition fails

A claim with no marker at all is one case of a claim that no sound witness affirms. It is vacuously the same
failure. It gets its own name only because a machine finds it without asking anything, which makes it the cheapest
of the three to detect.

| Failure                                                                        | Question  | Found by                                            | Result              |
| ------------------------------------------------------------------------------ | --------- | --------------------------------------------------- | ------------------- |
| the claim has no marker — **unattested**                                       | existence | a form check. Nothing is asked and nothing is read. | red — stop          |
| a witness **denies**                                                           | verdict   | the canvass                                         | red — stop          |
| a witness is **false**                                                         | standing  | the audit                                           | red — stop          |
| every witness is **silent**                                                    | verdict   | the canvass                                         | yellow — the ruling |
| a standing is **unaudited**, or an agent proposed it and no human confirmed it | standing  | the audit                                           | yellow — the ruling |

**A red item never reaches a human.** It is a stop before the ruling, and not an entry in a readout that anybody
looks at. Work returns to the builder.

So the readout that an operator receives always has, for every claim in scope, at least one witness that is not
false and does not deny. The only open items are the two yellow rows, and the ruling is what closes them. After the
ruling, the condition above holds on the human's authority.

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
it on (§8.3), with no custom rule and no new adapter.

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
kind. The kinds that cannot carry a comment — JSON, and similar — are covered in §6.3.

### 5.3 Subject and instrument

Every witness has two sides, and each question belongs to exactly one of them.

> The **verdict** is about the subject. The **standing** is about the instrument. Each is voided only by changes to
> its own side.

- The **subject** is the code the witness judges. `@scope` names it.
- The **instrument** is the marker, the lines it owns, and the text of the claims it attests.

The audit follows from this in one sentence: **the audit confirms that the instrument logically attests the marked
claim.** Nothing about the subject enters it.

### 5.4 The three questions

| Question      | Answered by                         | Cost                 | Voided by                                        | Default                           |
| ------------- | ----------------------------------- | -------------------- | ------------------------------------------------ | --------------------------------- |
| **existence** | a form check over the marker index  | free                 | nothing — recomputed every run                   | always recomputed                 |
| **verdict**   | an adapter, or a judge              | free, or expensive   | a change inside `@scope`                         | **asked**; carried by exception   |
| **standing**  | an auditor. Always an intelligence. | **always expensive** | a change to the instrument, or to the claim text | **carried**; audited by exception |

The symmetry inverts, and that inversion is the design:

- A **verdict** is asked by default, and carried only when asking is expensive — that is, only for a witness file.
- A **standing** is carried by default, and audited only by exception — always, for every kind, because an
  intelligence is the only thing that can set it.

### 5.5 The verdict

| Verdict               | Colour | Meaning                                                                  |
| --------------------- | ------ | ------------------------------------------------------------------------ |
| **affirms**           | green  | The witness was asked, and the subject satisfies the claim.              |
| **affirms (carried)** | green  | Nothing in the subject changed, so the previous affirmation still holds. |
| **silent**            | yellow | The subject changed, and nobody asked this witness.                      |
| **denies**            | red    | The witness was asked, and the subject does not satisfy the claim.       |

**Silence is computed, not decided.** By the same induction as §8.5, the last verdict of every witness on the main
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

| Standing      | Meaning                                                         |
| ------------- | --------------------------------------------------------------- |
| **sound**     | An auditor read the instrument. It logically attests its claim. |
| **false**     | It does not attest its claim. Repair it or delete it.           |
| **unaudited** | Nobody has read it against its claim.                           |

**A standing carries its source.**

- An agent can set **false** by itself. To find a bad witness needs no authority.
- An agent **proposes** sound. The human confirms it at the ruling.

### 5.7 Witnesses that attest several claims

A marker may carry several `@attests` directives. Two rules keep this honest.

**Every claim needs at least one witness that attests it alone**, whatever its kind. Otherwise its verdict is
coupled to another claim's forever. An integration test that attests four claims is legitimate as a supplement. It
is a problem only when it is the sole proof, and then the coupling is the signal: either these are one claim, or
they lack real witnesses.

**The cost of a shared marker is its audit.** The audit cost of a marker is proportional to the number of claims it
attests, because a change to the instrument re-opens the question for all of them. Nothing forbids stacking claims
on one marker, and nobody does it twice. The claims that belong on one marker are the ones that rise and fall
together.

For an inferential witness, sharing is nearly free: a witness file is a container, and three markers in one file
are three witnesses with three scopes and three judgments.

### 5.8 A claim that needs two kinds of witness is two claims

The mirror of §5.7, and the same observation read from the opposite end. §5.7 warns about one marker carrying many
claims, and the diagnosis there is that the claims are coupled. This is one claim needing many markers:

> **A claim that appears to need two witnesses of different kinds should be split — or its second half is out of
> scope.**

Not two witnesses of the **same** kind. Two unit tests on one claim is ordinary supplementary evidence. The signal
is the mismatch of kinds, because a lint rule and a test answer questions of different shapes, and needing both
means two questions were written on one line.

It has held in three different directions, which is why it is a rule rather than an anecdote:

| The claim as written                 | Given                    | Was actually                                               |
| ------------------------------------ | ------------------------ | ---------------------------------------------------------- |
| _the token is random_                | a lint rule and a test   | **no weak randomness** and **the right shape**             |
| _the backup lands in object storage_ | a local test and a probe | one claim; the other half was out of scope entirely (§4.1) |
| _configuration is required_          | a type and a test        | **availability** and **validation**                        |

A compound predicate, an out-of-scope subject, and two distinct properties sharing a slug. Splitting is the answer
every time, and each half comes out with one honest witness.

## 6. The marker format

### 6.1 Directives

> A directive is `@name`, then whitespace, then **exactly one whitespace-free token**. Some directives take no
> token. Everything else on the line is ignored.

**A name may contain a colon**, and the terminators of §6.2 are the only names that use one. This is the single
place the format has no give: matching the name as `@(\w+)` would read `@attests:end` as `@attests` carrying the
token `:end`, which is a marker attesting a claim called `:end`. The pattern admits the colon deliberately, or it
admits it by accident and silently.

A slug has no spaces. A glob has no spaces. A comma-separated list of either has no spaces. So the rule never needs
to know where a comment ends, and **the core never learns the comment syntax of any language**:

```ts
/**
 * @attests closing/ordered-not-atomic    crux-ignore
 * @scope packages/core/src/close.ts      crux-ignore
 */
describe("close", () => { … })
```

```py
# @attests ingest/rejects-malformed    crux-ignore
# @scope src/ingest/**                 crux-ignore
def test_rejects_malformed(): …
```

The trailing `*/` and the leading `*` are noise after the token, and the rule discards them without knowing what
they are.

There are six directives, and they divide into three shapes. A **noun** binds a name. A **verb** references a name
that is already bound. An **attribute** modifies the block it sits in. Each opener also has a terminator, and §6.2
covers those as one family.

| Shape         | Directive           | Token                                      | Opens         | Repeatable |
| ------------- | ------------------- | ------------------------------------------ | ------------- | ---------- |
| **noun**      | `@project <prefix>` | one slug segment, with no `/`              | a project     | no         |
| **noun**      | `@claim <slug>`     | a slug                                     | a declaration | no         |
| **verb**      | `@attests <slug>`   | a slug, or a comma-separated list of slugs | a marker      | yes        |
| **verb**      | `@grounds <slug>`   | a slug, or a comma-separated list of slugs | a grounding   | yes        |
| **attribute** | `@scope <globs>`    | a glob, or a comma-separated list of globs | —             | yes        |
| **attribute** | `@kind <word>`      | `capability` or `development`              | —             | no         |

**`@project` is read only in a `GLOSSARY.md`.** It is the one directive whose file matters, and §3.4 says why: the
glossary is what declares a project, so the directive that names the project has to live where the project is
declared. Everywhere else the name is ordinary text. This is also what makes the name safe — `@project` appears in
legacy file headers across several ecosystems, and none of those files is a glossary.

**Every token is one word.** An attribute never takes free text, and this is not a style preference: the moment a
token may contain a space, the core must know where the comment ends in order to know where the value stops, and
that is the knowledge §6.1 exists to refuse. `/* @kind a long phrase */` would parse as `a long phrase */`.

**A token containing `<`, `>`, or a backtick is not a directive.** This is the rule for prose, and the population
it protects is larger than it looks: it is not only this document, it is **every adopting repository's
`AGENTS.md`**, because explaining the format to your agents means writing `@claim <slug>` somewhere on day one.

Keep the character set narrow on purpose. A broader _any invalid slug character is ignored_ would swallow a
typo'd real slug — a stray capital, say — and that is under-detection, the one direction §8.2 forbids. These three
characters never appear in a typo of a real slug, so the rule has no false-negative surface at all.

**A line containing the exact substring `crux-ignore` holds no directive.** Case-sensitive, anywhere on the line,
before or after the directive it suppresses.

It is line-local by design. There is no fence to track, no block to close, and no file type to know — which is
what keeps the core a line scanner. A line that says it is not a directive is not a directive, in any language, in
any file, with no state carried from the line above.

**It is also the one deliberate hole in §8.2's rule against under-detection**, so keep the string ugly and keep it
exact. A marker that is silently not a marker is the failure this whole system exists to prevent, and the only
defence is that nobody writes `crux-ignore` by accident. That is why it is not `ignore`, not `crux:ignore`, and
not case-insensitive.

The two suppressions divide cleanly, and neither replaces the other:

| Rule                       | Fires      | For                                                                |
| -------------------------- | ---------- | ------------------------------------------------------------------ |
| the `<`/`>`/backtick token | on its own | prose that mentions the format — `@claim <slug>` in a sentence     |
| `crux-ignore`              | on request | a line that would otherwise be a real directive — a worked example |

The first has to be automatic, because nobody decorates a sentence. The second has to be deliberate, because a
line carrying a well-formed slug is indistinguishable from a real marker by any rule that does not ask.

**Suppressing an opener leaves nothing behind.** An attribute whose opener was ignored has no block to modify, and
an attribute outside a block is inert. So ignoring the opener line of an example is enough, and a partly decorated
block degrades to nothing rather than to half a marker.

**A block opens with `@project`, `@claim`, `@attests`, or `@grounds`, and the opener fixes what the block is.**

| Opens with | Construct       | Its extent is  | Takes    |
| ---------- | --------------- | -------------- | -------- |
| `@project` | **project**     | inert          | —        |
| `@claim`   | **declaration** | the claim text | `@kind`  |
| `@attests` | **marker**      | the instrument | `@scope` |
| `@grounds` | **grounding**   | inert          | —        |

**One asymmetry, and it is deliberate.** Two openers are nouns and two are verbs. A noun binds — `@project` binds
a prefix, `@claim` binds a slug to the prose below it. A verb should have been a noun in both cases and is not, for
the same reason: **a witness has no identity to bind** (§5.1), and neither has a rationale. Making the grammar
regular would cost a second line on every witness in every repository forever, and §5.2 wants as many witnesses as
it can get.

The repeatability follows the same logic. Repeat where the reference is many-to-one: one test proves several
claims, one rationale grounds several claims. Never repeat where the extent **is** the content, because one body
of prose cannot be two claims.

**An attribute outside a block is inert**, and that is what makes the collisions survivable. `@kind` is a JSDoc
tag and `@scope` is a CSS at-rule, so both appear in the wild followed by a space and a token. Neither means
anything without an opener above it, so a stylesheet with no `@attests` in it is silent no matter how many
`@scope (.card)` rules it holds. This is the intended reading everywhere it matters, and §6.6 states it as a
condition on the check rather than leaving it to be inferred.

The four ordinary English words all collide and the two unusual ones do not. `@attests` and `@grounds` are clean
because nobody else reaches for them, which extends §2.1's naming rule into a domain it was not written for: **an
unusual word is collision-resistant, and a common one is a shared namespace with every tool that named the same
concept.**

**A block is a contiguous run of directive lines.** The first line with no directive ends it, and the next
directive in that file starts a new block.

```ts
/** @attests close/deletes-first    crux-ignore */
/** @attests close/cleans-blockers  crux-ignore */
describe("close, end to end", () => { … })
```

That is **one** marker with two claims, because the lines are contiguous.

**Mixing openers in one block is a form error.** A block that opens with `@claim` and then carries `@attests` is a
declaration trying to be a witness, and no rule about extents could say which it is.

### 6.2 The extent of a marker

The extent is the instrument. It has two consumers: the test adapter joins results against it, and the audit uses
it to decide whether the instrument changed.

> A block owns from itself to its **terminator**, to the start of the next block, or to the end of the file.

**A marker is a block opened by `@attests`, and only that.** A declaration and a grounding are blocks too, and
neither is a marker: a declaration has no verdict and no standing, and a grounding owns nothing at all. Keeping
one word for one construct is why §5.1, §6.2, and the join in §8.2 need no qualification — they were written
about the witness case and they still mean it.

**A grounding's extent is inert.** Nothing audits a rationale and nothing joins against it, so the lines below a
`@grounds` are prose and no more. Do not look for meaning there.

**A terminator names the opener it closes**: `@claim:end`, `@attests:end`, `@grounds:end`. There is no bare
`@end`, and its absence is the point. `@end` is a keyword in Objective-C and a command in Texinfo, so a stray one
would truncate a real marker's extent — and §6.2 is about to say that under-extension is the one failure that lets
a false witness survive. It is also the least-written directive, so a broken extent is the least likely to be
noticed. Naming the opener kills both collisions outright, and it makes the terminator checkable against the block
that is actually open.

Spell them with the **opener tokens** and not with construct names. `@witness:end` and `@rationale:end` would
reintroduce two words §2.2 deliberately killed.

Two of the three do nothing. A grounding's extent is already inert, and a catalog file is sequential prose where
the default extent is right, so `@attests:end` is the only terminator that ever changes an answer. All three are
legal anyway, because a format that accepts one terminator and rejects two others is a rule to memorise for no
gain.

**A terminator is the escape hatch, and it is opt-in.** The default extent is right for a test and for a witness
file, because those are sequential blocks. It is too wide inside a map of configuration, where one marked line
sits among many unmarked ones:

```jsonc
{
  "rules": {
    /* @attests no-console    crux-ignore */
    "no-console": "error",
    /* @attests:end          crux-ignore */

    "no-debugger": "error",
  },
}
```

**Over-extension is safe.** It causes extra audits. Under-extension lets a false witness survive. So the coarse
default is never wrong, only expensive, and the extent is an optimisation problem rather than a correctness one.

**This is why a terminator is not required.** Requiring one would not make the extent correct, only **stated**, and
a hand-placed terminator fails in the unsafe direction: too early is under-extension, which is the one failure
above that lets a false witness survive. It also goes stale silently, because there is nothing to check a written
extent against, whereas a default that runs to the next block tracks a `describe` as it grows and shrinks.

**There is no file-level marker and no nesting.** An earlier version made a marker _before any non-comment content_
own the whole file. A line scanner cannot compute that, because it would have to know the comment syntax it
deliberately refuses to learn. When a claim covers a whole file and another covers one block inside it, repeat the
slug:

```ts
/** @attests close/deletes-first    crux-ignore */
describe("close, unit", () => { … })

/** @attests close/deletes-first    crux-ignore */
/** @attests close/cleans-blockers  crux-ignore */
describe("close, end to end", () => { … })
```

Two markers, two witnesses, one claim attested by both. A slug repeated in six blocks of one file is visible, and
it is telling you the claim is too broad.

**When to write a parser.** A parser gives the better answer — the instrument is the `describe` block below the
marker, or the value of the key below the marker. It belongs in an **adapter**, where the language knowledge
already lives, and never in the core. The signal to build one is `@attests:end` appearing on nearly every marker
in one file type. Until then the parser is a cost with no evidence behind it.

### 6.3 Markdown, and files that take no comment

**Markdown uses the same rule. Not frontmatter.** Only a directive line is ever read by a machine, and the
universal rule already covers it. Frontmatter would make the core learn a second format, and a rule for when to
apply which, and gain nothing.

**The construct that carries it is free**, because §6.1 already tolerates leading noise — that is how
`* @attests foo/bar` works inside a JSDoc block. Only _trailing_ junk breaks a directive, since the token is
whitespace-delimited.

> **A Markdown marker may use any construct that leaves the directive line's trailing token intact.**

| Form                     | Renders as        | Scans?                                        |
| ------------------------ | ----------------- | --------------------------------------------- |
| `> @claim <slug>`        | blockquote        | yes                                           |
| `<!-- @claim <slug> -->` | **nothing**       | yes                                           |
| `## @claim <slug>`       | heading, anchored | yes, but see below                            |
| `` `@claim <slug>` ``    | code span         | **no** — the closing backtick joins the token |

**Prefer the blockquote.** An HTML comment renders as nothing: in GitHub, in an editor preview, in any docs site,
the directives are gone. For a rationale that is mildly annoying. For the catalog it is worse, because the slug is
the identifier you cite in a pull request, in a ticket, and in conversation — so the artifact humans read most
would be the one that hides its own identifiers. A blockquote carries a multi-directive block, renders as a band
that reads as metadata, and costs the format nothing.

**The inline-code form is the trap.** It looks like the obvious answer and it corrupts the token, because no
whitespace precedes the closing backtick. §6.1's backtick rule turns that from a wrong slug into no directive at
all, which is the safe failure — but it is still not a marker, and writing one that way is silent.

**The heading form is tempting and does not work.** Every claim would earn a table-of-contents entry and an anchor
to link at. But a block is a contiguous run of directive lines and Markdown wants a blank line after a heading,
which ends the block, so `@kind` would have to sit immediately under the heading and render as a stray paragraph.
Duplicating the slug into a heading beside the directive is worse: it is one fact in two places.

**A worked example in a code block is a real directive, and `crux-ignore` is how you say otherwise.** Every
document that **teaches** this format is made of examples — this one, and **every adopting repository's
`AGENTS.md`** on day one. Prose that merely mentions a slug is already handled by §6.1's token rule, but an example
that shows a well-formed marker is indistinguishable from one, because it **is** one. Nothing about being inside a
fence changes that, and asking the core to notice the fence would make it learn where Markdown's code blocks begin
and end — a caveat about fence lengths, another about indented blocks, and a state machine in a line scanner.

So the example carries the marker instead:

````md
```ts
/** @attests close/deletes-first    crux-ignore */
describe("close, unit", () => { … })
```
````

The cost is one token per line, paid only by documents that quote the format. The gain is that the rule is the
same rule everywhere, and a reader never has to ask whether this particular context is scanned.

**A file that takes no comment at all**, such as JSON, has nowhere to put a marker. Two resolutions, in order of
preference:

1. Move the file to a format that takes comments.
2. Witness it with a **witness file** whose `@scope` is that file, and whose judgment states what must be true of
   it.

**Check the extension against the parser, not against the name.** This set is smaller than it looks, and the two
files that matter most are in it by mistake. `tsconfig.json` is read as JSONC by TypeScript, and `wrangler.jsonc`
is JSONC by name. Both take comments, so both take markers, and resolution 1 would have been followed for nothing.
A `.json` extension is evidence about a file's parser, not proof — and §8.3 makes this worth checking, because a
config file is where a lint witness lives.

### 6.4 What is not a directive

There is no `@run`. A command has spaces and arguments, and it does not need to be a directive, because **the core
never runs anything**. Only the agent reads the command, and an agent reads prose.

> A directive exists only for what the core must resolve without intelligence. Everything else is prose.

By that test `@attests` qualifies, because the form check and the join need it. `@claim` qualifies, because the
slug must bind to the prose that defines it. `@scope` qualifies, because carry-forward, claim surfacing, and the
auditor's reading list need it. `@grounds` qualifies, because prose that **mentions** a slug and prose that is
**about** it look identical to a machine, and §11.1 needs them apart. `@kind` qualifies, because the readout is
ordered by it. `@project` qualifies, because the **misfiled** check compares a slug's prefix against the set of
projects, and without the directive that set could only come from where files sit — which §6.6 forbids.

The target command, the judgment, the reason, and the rejected alternative do not. They are read only by an
intelligence, and an intelligence reads prose.

### 6.5 Scope, and who needs it

`@scope` has three consumers, and only the first is inferential-only:

1. **Verdict carry-forward.** Nothing in the scope changed, so the expensive judgment is reused. §5.5.
2. **The auditor's reading.** An auditor must read the instrument **and** the code it judges, or it cannot say
   whether the instrument attests the claim. Without a scope it has to search.
3. **Claim surfacing.** Which claims are in play for a diff is _whose scope the diff intersects_.

**But scope does not choose the audit set.** The diff chooses which markers to audit — the instrument changed, or
the claim text changed. A change inside a witness's scope does **not** put it in the audit set. Add a renderer, and
a witness that says _every renderer takes a `Style`_ has a void verdict and an untouched standing.

| Kind            | `@scope`                        | If absent                                                                   |
| --------------- | ------------------------------- | --------------------------------------------------------------------------- |
| witness file    | **required**                    | the verdict can never be carried, so it is silent on every canvass          |
| test            | optional                        | the auditor reads the test's imports; surfacing falls back to the test file |
| lint rule, type | optional, and usually pointless | the subject is universal by construction                                    |

### 6.6 Form errors

A machine computes all of these, and none of them needs a tool, a language, or an intelligence.

- A claim that no marker names is **unattested**.
- A marker that names no existing claim is **orphaned**.
- A `@scope` that matches no file is a **dead scope**. This catches the common decay: a directory is renamed, and
  an inferential witness carries its verdict forward forever because nothing in its scope changes again. That turns
  a green readout into a lie, and it is invisible without the check.
- A block that carries two different openers is **mixed**. §6.1.
- An attribute that its opener does not take is **misplaced**. A `@kind` under `@attests`, or a `@scope` under
  `@claim`. This closes the reachable edge where a JSDoc `@kind class` sits contiguous with a real `@attests`
  block — `@kind` is not an opener, so the block is not _mixed_, and without this check the case is unspecified.
- A `@kind` **inside a declaration** whose token is outside `capability` and `development` is **unknown**. The set
  is closed, because §8.4 orders the readout by it and an open set has no order. The scoping is the whole check: a
  naive version fires on every JSDoc `@kind class` in the repository.
- A claim whose slug prefix names no declared project is **misfiled**. §3.4.
- Two projects declaring one prefix is a **collision**. Whichever claims they hold, one set is misfiled and
  nothing can say which.
- A repository that declares no project at all is **unfounded**. Every claim in it is misfiled, so reporting each
  one separately would bury the single fact that explains them.
- A `@grounds` that names no declared claim is a **forward dangle**. §11.5 says why this is an error where the
  backward case is not.

**Crux resolves slugs. It never resolves paths.** Every check above reads a directive and compares it with
another directive — the **misfiled** check included, since `@project` is what a slug prefix is compared against.
None of them looks at where a file sits, which is why the positions in §3.4 are house rules that a repository may
decline without breaking anything.

**A form error must be fixable by the person who caused it, at the moment they caused it.** That is the line
between this list and the two reports below it, and it is the whole reason the forward dangle is an error while
the backward one is not. A forward dangle is preventable while writing. A backward dangle is created by a later
merge, and it reaches into a document that cannot be edited from there and that was true when it was written.

Two things are reported and are **not** errors:

- **A rationale that grounds a claim which no longer exists.** §11.5.
- **A glossary that changed in this diff.** §3.3.

## 7. The amendment

An amendment is the set of claim changes that one unit of work proposes. It is the specification.

Its operations are **add**, **change**, and **delete**. Each entry names a claim, and for an add it also names the
witness that will attest it.

A delete removes the claim and its witnesses in the same merge.

**Naming the witness is not bookkeeping. It is where the design happens.** An add that names its claim and leaves
the witness for later has recorded a wish, and the wish is usually wrong in a way only the witness reveals. _The
dashboard never writes_ is a sentence about intent until you ask what would judge it, and the answer is a decision
about who constructs the database handle — a type with no write method, which is a kind 1 witness and a change to
the design. _The token is random_ is trivially statable and untestable as stated. In both cases the claim as
written would have shipped something worse.

So the witness field is the step that finishes the claim, and §10's exit gate already says so: fog is clear when
you can write the claim **and** assign its witness. An amendment is authorable at exactly that moment and not
before.

**An amendment is proposed until the merge. The merge enacts it.** Do not merge an amendment to the main branch on
its own. The rule that decides this:

> Keep a pre-work artifact only if it carries information that the post-work artifact cannot reconstruct.

An amendment qualifies. The merged code shows what was built. Only the amendment shows what was **wanted**.

## 8. The canvass, the readout, and the audit

### 8.1 The canvass

To canvass is to ask **every** witness in scope, one by one, for a verdict. Three properties come free:

- It is complete by definition. A poll samples; a canvass does not.
- It asks. It does not execute. So the word is true for a test and for a prose witness alike.
- No answer is a normal outcome of a canvass. That is the silent verdict, with no special case.

**The canvass asks the witnesses that can answer differently at different commits. The rest are defended by form
and by audit.** A type witness says only _affirms, if the project compiles_. A lint config line says only _the rule
is still installed_. Their real defence is §6.6 and §8.5.

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

| What happened                       | Question  | Mechanism                                            | Result                      |
| ----------------------------------- | --------- | ---------------------------------------------------- | --------------------------- |
| the rule was deleted from config    | existence | the marker was deleted with the line                 | unattested — red            |
| the rule was set to `warn` or `off` | standing  | the config line changed, so it is in the audit scope | a reader marks it **false** |
| the code violates the rule          | verdict   | the adapter joins on rule id                         | **denies**                  |

**Do not add a test that reads the config and asserts the rule is enabled.** It solves a problem the marker's
position already solves, and it reports a standing failure as a verdict.

### 8.4 The readout

The canvass produces the readout. One entry for each claim in scope:

| claim                        | witness                             | verdict           | standing  |
| ---------------------------- | ----------------------------------- | ----------------- | --------- |
| `close/deletes-first`        | `close.test.ts:12`                  | affirms           | sound     |
| `renderers/colour-is-passed` | `witnesses/renderers-take-style.md` | affirms (carried) | sound     |
| `report/reads-at-a-glance`   | `witnesses/report-legibility.md`    | silent            | unaudited |

Two columns, two questions. _What does this witness say?_ And _can I believe it?_

**Keep the meaning narrow.** The readout is the verdicts and the standings. It is not the whole package. The
operator receives the amendment beside it, because the ruling compares what was wanted to what the witnesses say:

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

**The invariant holds by induction across merges.** Every merge audits every marker in that scope, and a merge
changes nothing else. So each untouched marker still has the instrument and the claim text that it was audited
against. Therefore **a witness on the main branch is presumed sound**, and there is nothing to look up.

**The record of an audit is the merge.** It sits on the PR with the readout. You never query it.

**Do not add a `last audited` directive.** A date does not tell you whether the instrument changed, and file times
do not survive a checkout. A content hash is the stronger version and is still not worth it: it protects against
one skipped audit, which is a process failure, and it costs a diff on every edit.

**The base case.** In an existing repository every witness starts unaudited, and it becomes audited the first time
its instrument or its claim is touched. The first audit produces the base case. The readout shows the truth, in
yellow.

**The blind spot, stated and accepted.** A witness can become vacuous without either side of its instrument
changing. A helper it calls becomes a no-op, and the witness still affirms. Neither the induction nor a hash
catches this, because both watch the instrument and not what the instrument depends on.

This is not a defect of this design. It is a condition of all software, with or without agents. The goal of this
system is to make automated development easier and less risky. It is not to solve a problem that every codebase
already has. So name the blind spot, and leave it.

### 8.6 Why the canvass and the audit stay separate

| Act         | Question                                  | Subject        | Voided by                               |
| ----------- | ----------------------------------------- | -------------- | --------------------------------------- |
| **canvass** | Does the code satisfy the claims?         | the subject    | any change inside `@scope`              |
| **audit**   | Do these instruments attest their claims? | the instrument | a change to the instrument or the claim |

The canvass is worthless without the audit, because a builder that writes its own witness decides its own success.

**Both belong to the reviewer. Neither belongs to the builder.**

> The gate is applied by somebody who did not build.

They are two acts and one artifact. Do not split the page the operator reads.

## 9. Belay

### 9.1 The sequence

1. A human operator gives belay an **amendment**.
2. Belay instructs a **builder** to implement the amendment.
3. Belay instructs a **reviewer** to canvass the witnesses and audit the markers in the audit scope. This produces
   the **readout**.
4. Belay forwards the amendment and the readout to the human operator for the **ruling**.

### 9.2 The builder's exit condition

> **The builder is done when the canvass is green.**

That is the same condition as the ruling. A `change` witness is changed until it affirms. A `delete` witness is
deleted with its claim.

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

The reviewer has already collected the verdicts and set the standings. So the human is not making a quality
decision. **The human makes a specification decision: were these the right claims?** That is the same authority as
step 1, exercised again with the output visible.

The ruling has exactly two kinds of item to clear:

- **yellow verdicts** — witnesses whose subject changed and whom nobody asked
- **unconfirmed standings** — soundness that an agent proposed and no human confirmed

Both are the point where the machine reached the edge of what it can say.

## 10. Fog

Fog is material you want, but cannot yet state as a claim.

**Fog is not a verdict, and it is not a fourth colour.** A verdict is a report about a claim. Fog has no claim, so
there is nothing to report. The colour scale measures the catalog, and fog is not in the catalog.

But fog is not outside this framework either. The framework owns two things about it:

- **The definition.** Fog is the negative space of the catalog. You cannot define it without the definition of a
  claim.
- **The exit gate.** Fog is clear when you can write the claim **and** assign its witness. That is the same test as
  _is the amendment authorable_. It is the main exit and not the only one; see §11.6.

Cairn owns the rest: where fog is stored, how it is cleared, and the record of the route taken.

**There are exactly two states outside the catalog:**

| State                  | Can you write the claim? | Who holds it | Exit                               |
| ---------------------- | ------------------------ | ------------ | ---------------------------------- |
| **fog**                | no                       | cairn        | ask the checkout, or ask the world |
| **proposed amendment** | yes                      | the branch   | the merge                          |

So a tracker needs no third object. Fog discharges into an amendment, or — when what it resolves into is settled
by construction — into a rationale. The amendment discharges into the catalog. **A rationale is not a third
state**, because it is answered rather than pending, and it leaves the tracker for the repository at the moment
it is written.

**The exit cut is _inside the checkout or outside it_, and not _judgment or evidence_.** An earlier draft used the
second pair and it predicted nothing, because the two are not alternatives: a real item is often both, with an
agent gathering the facts and a human making the call. What varies by orders of magnitude is where the fact lives.

| Fog item                                  | Cleared by                            | Cost         |
| ----------------------------------------- | ------------------------------------- | ------------ |
| _is this dependency already pinned?_      | one read of the lockfile              | seconds      |
| _which of these two tools do we adopt?_   | reading two repositories, then a call | two fetches  |
| _does this handler support that API?_     | deploying a throwaway service         | hours        |
| _can this subdomain be onboarded safely?_ | a rehearsal on a spare domain         | an afternoon |

The top two are answerable from a checkout and the bottom two are not, and that line sorts the queue when
_judgment or evidence_ leaves all four looking alike. This is §4.1's boundary arriving a third time, from an
unrelated direction.

**So a fog item records what would clear it.** Not as documentation — as the field that makes the queue sortable.
Without it, an item answerable in one grep and an item requiring a deployment are indistinguishable, and the human
picks by guessing. Cairn holds the field; this document only says why it must exist.

**Fog is defined by inability, not by absence of effort.** A claim you can write but have not written yet is not
fog. It is an unwritten amendment. If you allow that mistake, fog becomes a backlog, and it stops being a signal
that a human is needed.

**And _not now_ produces nothing at all.** It is not fog, because the test is inability rather than unwillingness.
It is not an amendment and it is not a rationale, because §11.2's three tests reject writing one for a refusal —
it is cheap to reverse, nobody would be surprised, and no alternative was examined. A framework that emitted a
ticket here would be worse. The absence of an artifact is the feature.

## 11. Rationale

The catalog says what the system promises. It does not say why. A **rationale** is the document that does.

**A conventional ADR holds both halves** — the decision (_we will use React_) and the reasoning (_because X,
having rejected Svelte for Y_). Crux takes the first half away and puts it in the catalog as a present-tense
claim with a witness. What is left in the document is only the second half.

So the artifact is not an ADR, and it does not carry that name. See §2.2.

### 11.1 What a rationale grounds

> **`@grounds <slug>` — this rationale grounds that claim.**

Repeatable. One rationale often grounds several claims that were decided together.

The direction is deliberate, and it is the direction everything else already points. A marker names its claims; a
rationale names its claims; the catalog names nothing. **The claim slug is the hub.** Three results:

- **The catalog stays undecorated.** A claim is falsifiable prose and a slug, and it does not carry a list of
  documents that would rot.
- **The link sits on the stable side.** Rationale is written once. Claims churn.
- **The reverse index is derived**, so _show me this claim's reasoning_ costs nothing to maintain.

The query it answers is **why is this claim the way it is, and what was rejected on the way?** It is asked at the
ruling, and it is asked at amendment time, before proposing a change to a claim that somebody already reasoned
about.

**Prose that mentions a slug is not a grounding.** A rationale may discuss a claim in passing, or contrast it with
another. The directive is what separates _this document is about that claim_ from _this document mentions it_, and
resolving that without intelligence is the §6.4 test.

**A `@grounds` list that grows over time is a signal**, and it is the reason §11.2 says to cite only the claims
that exist rather than every claim you expect. A document that accumulates groundings across several merges is one
whose decision turned out to be load-bearing across the system. Writing the full list upfront destroys that
information, and it is not recoverable afterwards.

### 11.2 When to write one, and when to ship it

All three must be true. If any is missing, skip it.

1. **Hard to reverse** — the cost of changing your mind later is real. If it is cheap to reverse, you will just
   reverse it.
2. **Surprising without the reasoning** — a future reader will look at this and ask why. If nobody would wonder,
   nobody will read the document.
3. **The result of a real trade-off** — there were genuine alternatives, and you picked one for stated reasons. If
   there was no alternative, there is nothing to write beyond _we did the obvious thing_.

**And it names what was neglected.** This is a requirement on content, not on form. The rejected option is the
half that the code cannot show you, so a rationale that omits it has recorded only what was already visible.

**A rationale ships with the claims it grounds, and never ahead of them.** A rationale explains why a claim _reads
as it does_. If the claim does not exist, the document is explaining a decision nobody has enacted — which is
intent, and intent is the amendment's job. A rationale committed ahead of its claim is doing the amendment's work
in the wrong artifact, so §6.6 makes a `@grounds` that names no declared claim a form error.

The mechanism needs nothing new. A claim is **declared** by `@claim` on the branch, and §4's condition — a sound
witness affirms it — is enforced at the merge rather than at declaration. So the claim, its witness, and its
rationale are authored on one branch and land in one merge, and `@grounds` resolves the whole time.

**When a rationale grounds claims that land in different merges, write it as soon as the first one needs it.** This
is common rather than exotic: one thought often spans a layer boundary, and the packages on either side are built
separately. The alternative — hold the document until the last claim it grounds — keeps the check strict and hides
the reasoning during exactly the window when a reader goes looking, because some of its claims are already live. A
later amendment adds its own `@grounds` line to the existing document. Nothing dangles, the check stays strict, and
the reasoning is available from the first merge.

**The failure this accepts, stated plainly.** The later amendment's author may not find the earlier rationale, and
then the document under-grounds: it is about that claim, does not cite it, and _why is this claim the way it is?_
returns nothing when an answer exists.

This is uncheckable by construction. §11.1 already says a machine cannot tell _this document is about that claim_
from _this document mentions it_ — that is why `@grounds` exists at all. No form check reaches it and no audit
reaches it, because a rationale is not an instrument. It is survivable because §11.3 already accepts that some
rationales are found by reading the directory rather than by index, so under-grounding degrades to a baseline this
document already tolerates. The case to watch is the layer boundary, since that is where correlated claims reliably
land in different merges.

### 11.3 Settled by construction, and the index that does not exist

**A claim exists to be checked.** That is its whole purpose — §4 admits it to the catalog only when a sound
witness affirms it, and §5.2 pushes it as far up the ladder of witnesses as it honestly goes. So there is no point
claiming something that nothing can violate, because the check can never do any work.

Most decisions leave a claim behind. _We will not do X_ is usually _we do Y instead_, and Y is claimable. But one
class is not, and it is not an edge case:

> **Claim it if something could plausibly violate it. Otherwise write only the rationale.**

**This is the gap a rationale fills.** Something that should be recorded, and that no witness could ever
usefully check.

| Decision               | Could anything violate it?      | Result                  |
| ---------------------- | ------------------------------- | ----------------------- |
| we use React           | someone adds a Svelte component | claim + lint witness    |
| squash merges only     | someone lands a merge commit    | claim + ruleset witness |
| no AWS, for compliance | someone adds the SDK            | claim + lint witness    |
| we use a monorepo      | no                              | **rationale only**      |
| _canvass_, not _poll_  | no                              | **rationale only**      |

This is a stricter bar than §14 property 2, which asks only that a claim be falsifiable. Falsifiable **in principle**
and **plausibly violable** are different, and the catalog pays for the second one: every claim needs a witness,
and every witness carries audit cost forever (§8.5) and a row on every readout. A claim that nothing can break
buys a permanent cost with no protection.

**And the question has no mechanical form. Do not build one.** _Claim, or settled by construction?_ goes the
counter-intuitive way often enough that any rule you write will be wrong. _Use two workers, not one_ reads like a
claim and is not — nothing drifts into one worker by accident. _The dashboard reads data only_ reads like
documentation and is the most important claim in its project. Getting the pair backwards produces a catalog of
unfalsifiable statements that costs audit time forever, while missing the one property worth protecting. It is a
judgment, it stays a judgment, and §13.3 files it permanently under _never automate_.

So a decision that is settled by construction gets a rationale and no claim. §2.2 of this document is one. So is
the deleted file-level marker in §6.2 and the deleted failure classifier in §9.2.

**These rationales have no index, and that is deliberate.** A rationale with no `@grounds` is invisible to crux.
There is nothing to key it on — the whole point is that no claim exists — and a shared bucket to hold them all
would be a pile rather than an index, no cheaper to search than the directory itself.

**They are found by reading.** The retrieval mechanism for a settled-by-construction rationale is an agent
reading `docs/rationale/` at the start of work, over descriptive filenames, in a directory that §11.2 keeps small
by construction. Do not build an index for this. Two facts make reading sufficient:

- The set is small, because most decisions do leave a claim.
- The question it answers — _has anyone already considered X?_ — is a **search**, not a lookup. No index keyed on
  claims could ever have answered it.

### 11.4 The format

A rationale is a Markdown file. There is no numbering: uniqueness comes from the filename and ordering comes from
git, and _scan the directory and add one_ collides the moment two branches both write `0038`.

```md
> @grounds belay/close/ordered-not-atomic crux-ignore

# Closing is ordered, not atomic

Closing deletes before it cleans, and a partial close leaves the blockers behind.

The rejected option was a transaction around both steps. It was dropped because the blocker store is not in the
same database, so the transaction would have been a lie that only showed itself under load.

## Consequences

A failed close is re-runnable, so every caller must tolerate a second call.
```

**The whole document can be one paragraph.** The value is that the reasoning was written and that the neglected
option was named. It is not in filling in sections. Add `## Consequences` only when a downstream effect is not
obvious. Add a status only when a rationale is superseded, and then it names its successor and stays where it is.

The title states the decision in the present tense, which is the tense of a claim.

### 11.5 A dangling grounding, in both directions

A `@grounds` can name a claim that does not exist for two opposite reasons, and they are not the same failure.

| Direction           | Cause                         | Result                     |
| ------------------- | ----------------------------- | -------------------------- |
| **forward dangle**  | the claim is not declared yet | **form error.** §6.6       |
| **backward dangle** | the claim was deleted later   | **reported, not an error** |

**The backward case is reported and it is not a form error.** Making it fail would force editing history, and the
document is still true — it records reasoning that was correct when it was written. What the report says is that a
decision was reversed and nothing yet records the reversal, which is the one decay in this artifact that a machine
can see.

**The forward case is an error**, for the reason §6.6 states generally: a form error must be fixable by the person
who caused it, at the moment they caused it. A forward dangle is preventable while writing, and §11.2 says how —
the claim, its witness, and its rationale land in one merge. A backward dangle is created by a later merge that
cannot reach back into a document that was true when written.

### 11.6 Fog can discharge into a rationale

§10 gives fog one exit: write the claim and assign its witness. There is a second, and it is the minority path.

> **Fog that resolves into a decision nothing can violate discharges into a rationale.**

Without it, fog can only leave by becoming a claim, so anything investigated and settled by construction stays in
the fog forever — and §10 says that is how fog turns into a backlog and stops being a signal that a human is
needed.

Both exits need the same authority. Deciding is a specification decision, and §9.5 says those belong to the human.

## 12. The tools

The format is the product. The tools sit on it and are replaceable.

|      | Name       | Does                                                                |
| ---- | ---------- | ------------------------------------------------------------------- |
| core | **crux**   | defines and parses claims, markers, rationale links, and amendments |
| 1    | **cairn**  | holds fog; turns fog into an amendment                              |
| 2    | **belay**  | takes an amendment; produces a PR that is cheap to rule on          |
| 3    | **beacon** | attention router; moves the operator between fog work and rulings   |

**The format and the core share the name crux.** The format is the product, and the core is the only thing that
reads it, so there is nothing left for a second name to distinguish. Say _a crux catalog_, _a crux marker_.

One line is hard, and the rest are soft: **crux takes no dependency on cairn, on belay, or on beacon.** Delete any
of the three, and crux still works. The reason is the cost of error. The format goes into other repositories that
you do not control, so a change to it is a migration you cannot run. The three tools are your own code, and you
can divide them again at any time.

**Crux reads lines and resolves globs.** It knows no language, no runner, and no linter, and it never resolves a
path. Everything that needs that knowledge is an adapter, and adapters live in belay. A glossary and a rationale
cost it nothing beyond what it already does: it finds one by name and the other by directive, and it reads the
contents of neither.

**A tracker stores slugs. A tracker never stores claim text.** Whoever has the checkout resolves the slug. This is
what makes a tracker outside the repository possible. The interface is small: **a tracker emits an amendment.**

### 12.1 Cairn keeps its state outside the repository it serves

That interface is not a concession to an external tracker. It is the interface designed for one, and two things
settle that it must be used.

**An authorable amendment has nowhere to live before there is a branch.** An amendment is held by the branch and
enacted by the merge (§7), but amendments exist — and are worth ordering against each other — before any work
starts. _A file on the branch_ cannot be the whole answer when there is no branch yet.

**Fog inside the target repository pollutes product history.** Every clarification of a fog item becomes a commit
interleaved with commits that change the product. This was observed rather than predicted, and it is the argument
for a second store keyed to the target.

**The bill comes due immediately, and it is worth stating before the tool is built.** When fog lives in the
repository, a slug rename is atomic with the code that caused it. Moving the tracker out breaks that atomicity, so
a dangling reference now lives in a different system, on a different release cycle, that cannot see the rename
happen.

**So cairn needs a watcher, and it is a repair rather than a feature.** It checks out the target at its main
branch, runs crux, and warns about stale references in its own tickets and fog. Three constraints:

- **It holds no facts.** Re-derive from HEAD every run, like §9.3's supervisor. Kill it, restart it, lose nothing.
- **It consumes what crux already emits.** §13.1's machine-form marker index. The interface is a file, not an API,
  and crux learns nothing about cairn.
- **The warnings point one way only.** Cairn warns about cairn's references, never about the repository. The
  repository is authoritative. A watcher reporting _into_ the repository would quietly create the dependency the
  hard line above forbids.

**The symmetry is worth naming.** §4.1 ruled that reality drifting from the repository is monitoring rather than
review, and pushed it out of crux. The watcher is monitoring, for tracker drift from the repository. Same shape,
different subject, excluded from crux for the identical reason, and landing in cairn because cairn is the thing
that can be wrong.

## 13. What gets built first, and what is being watched

### 13.1 Two tools, in order

**1. Crux.** It reads and lists claims, witnesses, and rationale links. Its whole scope is already stated above:

- find every `GLOSSARY.md` and read its `@project`, which is the set of projects and the only input the checks
  below cannot derive from the markers themselves (§3.4)
- list the claims, each with its kind and the witnesses that attest it
- list the markers, each with its claims, its extent, and its scope
- list, for each claim, the rationale that ground it (§11.1)
- report the form errors of §6.6 — unattested, orphaned, dead scope, mixed, misplaced, unknown kind, misfiled,
  collision, unfounded, forward dangle
- report the two non-errors of §6.6 — a rationale grounding a deleted claim, and a changed glossary
- emit the marker index as a machine form, for adapters and for cairn's watcher to join against

It runs nothing. It stores nothing.

**2. A belay MVP.** A local supervisor that goes from an amendment on a branch to a PR with a posted readout:

- run the sources named in `.belay/witnesses.toml`, map each report through its adapter, and join on the marker
  index (§8.2)
- produce the readout (§8.4)
- open or update the PR, and post the readout as a comment naming its commit (§9.4)
- loop the builder and the reviewer, with a fresh reviewer each cycle and a cycle budget (§9.3)

It does **not** hold fog, and it is not a tracker or a router.

### 13.2 The tracker was deliberately not third, and the period has now closed

Cairn was left off this list on purpose: several threads in §15 were about fog, none had an answer, and a tracker
built then would have encoded guesses about material nobody had handled under this vocabulary. So the two tools
above are built **by hand at the fog layer** — every amendment authored the slow way, every conversion from fog to
a claim done by a person with an agent. That was the experiment.

**§13.3's exit condition is met.** The first project built under this vocabulary that is not crux itself produced
two pain points that repeated, which is the bar. Both are now written into this document rather than left in a log:

| Repeated                               | Where it landed                                        |
| -------------------------------------- | ------------------------------------------------------ |
| every cross-reference is grep and hope | §12.1 — cairn holds each fact once, derives every view |
| the cost of clearing fog varies wildly | §10 — a fog item records what would clear it           |

**So cairn is designable, and it is still not second.** The order stands, because belay is what makes the catalog
worth having and cairn is what makes it comfortable. What changed is that cairn's design no longer waits on
evidence — it waits on a queue.

### 13.3 The dogfooding rule

> **While building tools 1 and 2, the pain of tracking fog and of converting fog into an amendment is the data.
> Record it as it happens.**

This applies to the human and to every agent in the work. An agent that notices one of the following states it in
its report, and does not silently work around it:

| Watch for                                                | What it tells you                                                      |
| -------------------------------------------------------- | ---------------------------------------------------------------------- |
| a claim that could not be written, and what unblocked it | where the fog boundary of §10 actually falls                           |
| a claim that was written and then reworded               | the right granularity for a claim                                      |
| a witness assignment that changed the design             | §7 — the altitude was wrong, and the witness is what found it          |
| an amendment that had to change during the build         | how much design must precede a build, and where the entry gate belongs |
| what had to be re-derived at the start of a session      | exactly what the tracker must hold                                     |
| a by-hand step that felt **clerical**                    | a candidate for the tool to absorb                                     |
| a by-hand step that felt like **thinking**               | never automate it                                                      |

The last two are the test that matters. Clerical work is a missing feature. Thinking is the work.

**One entry in the never-automate column is settled**, and §11.3 holds it: _claim, or settled by construction?_ has
no mechanical form and never will.

**Where the notes go.** One append-only file, `FOG-LOG.md`, in the project doing the building. It is fog by
definition, so nothing in it is a claim, and none of it is durable. What survives review is lifted into this
document, and the log keeps the trail.

**When the period ends.** Not when the tools are finished. It ends when the log holds a pain point that has
**repeated**. One occurrence is an anecdote and it redesigns nothing; a repeat is evidence. §13.2 records that this
has now happened once, and the rule does not retire — it applies again to every project that adopts the vocabulary,
because the second adopter tests what the first one could not.

## 14. The properties

1. Nothing **in the repository** is built without a claim, and nothing merges without being judged against one.
2. A claim is finished when something that is not you can falsify it.
3. Fog is named as fog and cleared deliberately, and what would clear it is recorded before the work starts.
4. Prefer artifacts that are legible in both directions — guidance going in, feedback coming out.
5. Evidence is indexed by claim, and generated rather than collected by hand.
6. Human attention is spent only on authority: set a claim, rule on an outcome, abort. Never on status.
7. Cheap re-entry over deep autonomy.
8. Durable state is plain files. No single tool owns them. Each tool derives its view and stores nothing.
9. The machine checks form. A human or a model checks truth.
10. The gate is applied by somebody who did not build.
11. Over-attribution is permitted. Under-attribution is not.
12. A directive exists only for what the core must resolve without intelligence.
13. Words are settled before the claims that use them.
14. The catalog holds the decision. A rationale holds the reasoning.
15. A claim earns its place only if something could plausibly violate it.
16. Crux judges the repository. What is not rederivable from a checkout is not a claim.

Property 1 is scoped on purpose. Property 16 creates a legitimate class of work that has no claim and never will —
the infrastructure a repository is deployed onto — and that work belongs in a runbook, with the risk named there.
Pretending otherwise is how a permanently green witness gets written.

**Do not renumber this list.** §3.2 cites property 11 and §11.3 cites properties 2 and 15. New properties are
appended.

## 15. Open threads

Each thread says what it blocks, because §10's exit gate does work that a bare question does not — writing down
_what does this block_ is what surfaces a contradiction, and it surfaced one here.

- **Does belay receive an amendment, or a branch with failing witnesses?** _Blocks belay's entry gate, so it blocks
  tool 2._ The strongest argument for the branch is not test-first discipline and is not the deleted classifier:
  **a witness that denied before the build is the only mechanical evidence that it can deny at all.** Every other
  soundness check is a person reading. Against it: three of the four kinds have no failing state, so the gate would
  read _every witness that can deny, does deny_, and the work moves rather than disappears.
- **How the audit scope gets its diff base.** _Blocks tool 2, and nothing before it._ The merge base of the branch
  is the obvious answer, and it needs git. Whose job that is has no answer yet.
- **Where the amendment lives.** _Blocks nothing until cairn emits one._ A file on the branch is the current
  assumption, rendered into the PR description. §12.1 has since settled that cairn holds the amendment before there
  is a branch, so this thread is now only about the branch-side representation.
- **How a lint-rule witness denies before the build.** _Blocks the thread above, and only if it resolves toward the
  branch._ A new rule denies on untouched code, not in one place. Two candidates: an allowlist, or land the rule
  with the refactor.
- **The spelling of _canvass_.** _Blocks belay's command names, so it blocks nothing until tool 2 has a CLI._ One
  letter separates it from _canvas_, and the wrong one is also a real word. **poll** is the fallback, and it loses
  only the completeness property.
- **Beacon.** _Blocks nothing._ Unowned. Its payload is fully determined by the amendment and the readout, so it
  invents no data model and can be deferred.
- **Whether this document's §2 is the root project's glossary.** _Blocks the root project's first claim, because
  that claim needs a `GLOSSARY.md` with `@project root` in it (§3.4)._ For every other repository the two are
  separate — the framework's words and the project's words. Here they collide, because the project is the
  framework. Keeping both would give one word two homes.
- **Where the catalog sits inside a project.** _Blocks nothing; it is a house rule either way._ §3.4 draws
  `docs/catalog/`, and §6.6 says crux never resolves a path, so nothing enforces it. Whether that freedom is worth
  having, or whether it only invites drift between repositories, has no answer yet.

**Closed since the last revision.** _Where fog is stored_ — §12.1, outside the target repository, with a watcher.
_Whether a single-project repository carries a prefix_ — §3.5, yes, always, and `@project` is what made it cheap.
