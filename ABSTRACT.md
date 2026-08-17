# FRAMEWORK — claims, witnesses, and the ruling

It is written in ASD-STE100 Simplified Technical English.

**Status.** The vocabulary and the mechanism are settled. No code implements them. §13 holds what gets built
first. §15 holds what is still open.

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
| **glossary**   | What a project's words mean. One `GLOSSARY.md` per project. Prose, never parsed.                     |
| **claim**      | A short, falsifiable statement of a requirement the codebase must satisfy. It carries a stable slug. |
| **catalog**    | The organised set of all claims in a project.                                                        |
| **rationale**  | A document that says why a claim reads as it does, and what was rejected on the way.                 |
| **fog**        | Material you want, but cannot yet state as a claim.                                                  |
| **witness**    | A mechanism that judges whether some part of the codebase satisfies a claim.                         |
| **marker**     | The comment block that designates a witness. A witness **is** its marker.                            |
| **directive**  | One `@name` instruction inside a block. §6.1 lists the four.                                         |
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

| Word                   | Rejected because                                                                                               |
| ---------------------- | -------------------------------------------------------------------------------------------------------------- |
| **delta**              | Too generic for a core object.                                                                                 |
| **review**             | Too generic, and it fused the canvass with the audit.                                                          |
| **run**                | You cannot run a prose witness. The word is false for the witnesses that need a human most.                    |
| **hearing**            | A hearing is a formal event where the judgment happens. A canvass decides nothing.                             |
| **deposition**         | Correct in meaning, strange to say.                                                                            |
| **discovery**          | It means _find out what you do not know yet_. That is fog. The two would collide.                              |
| **sweep**              | It can also mean _clear away_.                                                                                 |
| **poll**               | A poll samples. A canvass is complete. Held as the fallback if the spelling of _canvass_ is too much friction. |
| **passes** / **fails** | Ambiguous. "The witness fails" can mean the code is bad or the witness is bad.                                 |
| **decision**           | The decision is the claim. A word for it here would name the same thing twice.                                 |
| **ADR**                | It holds the decision **and** the reasoning. Crux splits the two, so the name is false. See §11.               |
| **`CONTEXT.md`**       | _Context_ is already ambiguous, and it names a boundary rather than the word list the file holds.              |

Four directives were proposed and rejected. Record these too.

| Directive         | Rejected because                                                                                                     |
| ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| `@decision`       | An anchor from code to a rationale. A plain Markdown link does the same for a reader, and the core resolved nothing. |
| `@witness`        | An opener for a witness block. `@attests` already opens it, so the directive carried no information.                 |
| `@record`         | A noun for a rationale. The only query it served is `ls`.                                                            |
| a required `@end` | It makes the extent **stated** rather than **correct**, and a hand-placed `@end` under-extends. See §6.2.            |

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

> **Crux knows the glossary by name and by position. It never knows the contents.**

This is the same refusal as §6.1, one level up. There, the core never learns the comment syntax of any language.
Here, it never learns what a term is.

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

**The glossary is privileged without being parsed.** It has a fixed name, a fixed position, a reader on every audit,
and a row on the readout when it moves. None of that requires a grammar.

### 3.3 A changed glossary is a yellow row

The one mechanical hook, and it is deliberately weak:

> **When a glossary changes in a diff, the readout says so and lists the claims of that project.**

It is not an audit trigger and it voids nothing. It is a row the human clears at the ruling, in the same family as
§4.1's two yellow rows: the machine has reached the edge of what it can say, and the question — _did the meaning
move?_ — goes to the person who moved it.

The cost is a file-level diff and no parsing.

### 3.4 One glossary per project

A standalone project holds one `GLOSSARY.md`, one catalog, and one rationale directory. A monorepo holds several
sets, one per project, and the repository root is a project like any other when it holds claims of its own. **The
root project is named `workspace`**, and not `root`, because this document already uses _root_ to mean a position
rather than a thing.

```
/
├── GLOSSARY.md              the workspace's own words
├── docs/
│   ├── catalog/
│   └── rationale/
└── packages/
    ├── crux/
    │   ├── GLOSSARY.md
    │   └── docs/{catalog,rationale}/
    └── belay/
        ├── GLOSSARY.md
        └── docs/{catalog,rationale}/
```

**Shared vocabulary is handled in prose.** Belay and cairn both use crux's words. Rather than a root glossary that
every project inherits, or a dependency graph that crux would have to learn, `packages/belay/GLOSSARY.md` opens
with a sentence: _this project uses the crux vocabulary; the words below are belay's own._ An intelligence reading
it does the right thing. That is the dividend of not parsing it.

**These positions are house rules, not format rules.** Crux resolves slugs and never paths (§6.6), so a project
that wants its glossary elsewhere is not broken — it is unconventional.

### 3.5 The slug carries the project

> **In a monorepo, a claim slug begins with the name of its project.** In a single-project repository it does not.

`belay/readout/names-its-commit`. `crux/marker/extent-ends-at-end`. `workspace/squash-merges-only`, at the root.
In a repository holding one project, `readout/names-its-commit` and nothing more.

The reason is §12: **a tracker stores slugs and never claim text**, and beacon routes on a slug with no checkout.
Both must place a claim without an index, and a prefix is the only thing that does that with pure string work.

**The kind is not in the slug.** `@kind` carries it (§6.1), so a claim is never `belay/capability/...`. A slug
names a project and then an area, and the audience it is written for is an attribute that can be corrected
without a rename.

**The cost of dropping the prefix in a single-project repository.** If that project later becomes a monorepo,
every slug changes — in the repository, and in every tracker holding one. The cost is accepted because the rewrite
is mechanical, and because it only ever falls on a project that grew enough to split.

**Crux checks the prefix.** A claim whose prefix names no project is misfiled. §6.6.

## 4. Claims and the catalog

**A claim is prose with a slug.** The prose is falsifiable, and it is written in the words its project settled
(§3). The slug is stable and machine-friendly, it names that project (§3.5), and it survives a rewording of the
prose.

**A claim is declared by `@claim`, and its extent is its text** (§6.1). So a catalog file is an ordinary document
— a heading, prose that orients the reader, and then the claims — and the narrative above the first declaration
is owned by nothing, because it orients rather than promises.

**Not everything decided becomes a claim.** A claim exists to be checked, and there is no point checking something
that nothing can violate. §11.3 draws that line and says where the rest goes.

**The catalog is present tense.** It states what the system promises **now**. One condition enforces this:

> **A claim is in the catalog only if a sound witness affirms it.**

Three results follow:

- A claim enters the catalog in the same merge that makes it true. It never enters earlier.
- There is no `pending` status and no promotion lifecycle. Intent lives outside the catalog. See §10.
- The condition is established at entry and maintained by each merge. This is the same shape as the audit
  invariant in §8.5. So a claim that is already in the catalog may have a silent witness at a later commit, and a
  claim that an amendment **adds** may not. A new claim has no earlier affirmation to carry forward.

Do not call such a claim _false_. **False** is the standing of a witness, and one word must not have two meanings.

### 4.1 How the condition fails

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

A witness file names a target and states a judgment. The target is a command to run, or the code to read.

```md
<!-- @attests report/reads-at-a-glance -->
<!-- @scope src/report/** -->

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

## 6. The marker format

### 6.1 Directives

> A directive is `@name`, then whitespace, then **exactly one whitespace-free token**. Some directives take no
> token. Everything else on the line is ignored.

A slug has no spaces. A glob has no spaces. A comma-separated list of either has no spaces. So the rule never needs
to know where a comment ends, and **the core never learns the comment syntax of any language**:

```ts
/**
 * @attests closing/ordered-not-atomic
 * @scope packages/core/src/close.ts
 */
describe("close", () => { … })
```

```py
# @attests ingest/rejects-malformed
# @scope src/ingest/**
def test_rejects_malformed(): …
```

The trailing `*/` and the leading `*` are noise after the token, and the rule discards them without knowing what
they are.

There are four directives, and they divide into three shapes. A **noun** binds a name. A **verb** references a
name that is already bound. An **attribute** modifies the block it sits in.

| Shape         | Directive         | Token                                      | Opens         | Repeatable |
| ------------- | ----------------- | ------------------------------------------ | ------------- | ---------- |
| **noun**      | `@claim <slug>`   | a slug                                     | a declaration | no         |
| **verb**      | `@attests <slug>` | a slug, or a comma-separated list of slugs | a marker      | yes        |
| **verb**      | `@grounds <slug>` | a slug, or a comma-separated list of slugs | a grounding   | yes        |
| **attribute** | `@scope <globs>`  | a glob, or a comma-separated list of globs | —             | yes        |
| **attribute** | `@kind <word>`    | `capability` or `development`              | —             | no         |
| —             | `@end`            | none                                       | —             | no         |

**Every token is one word.** An attribute never takes free text, and this is not a style preference: the moment a
token may contain a space, the core must know where the comment ends in order to know where the value stops, and
that is the knowledge §6.1 exists to refuse. `/* @kind a long phrase */` would parse as `a long phrase */`.

**A block opens with `@claim`, `@attests`, or `@grounds`, and the opener fixes what the block is.**

| Opens with | Construct       | Its extent is  | Takes    |
| ---------- | --------------- | -------------- | -------- |
| `@claim`   | **declaration** | the claim text | `@kind`  |
| `@attests` | **marker**      | the instrument | `@scope` |
| `@grounds` | **grounding**   | inert          | —        |

**One asymmetry, and it is deliberate.** Two openers are nouns and one is a verb, because a witness has no
identity to bind — §5.1 says so, and there is nothing for a noun to name. Making the grammar regular would cost a
second line on every witness in every repository forever, and §5.2 wants as many witnesses as it can get.

The repeatability follows the same logic. Repeat where the reference is many-to-one: one test proves several
claims, one rationale grounds several claims. Never repeat where the extent **is** the content, because one body
of prose cannot be two claims.

**A block is a contiguous run of directive lines.** The first line with no directive ends it, and the next
directive in that file starts a new block.

```ts
/** @attests close/deletes-first */
/** @attests close/cleans-blockers */
describe("close, end to end", () => { … })
```

That is **one** marker with two claims, because the lines are contiguous.

**Mixing openers in one block is a form error.** A block that opens with `@claim` and then carries `@attests` is a
declaration trying to be a witness, and no rule about extents could say which it is.

### 6.2 The extent of a marker

The extent is the instrument. It has two consumers: the test adapter joins results against it, and the audit uses
it to decide whether the instrument changed.

> A block owns from itself to `@end`, to the start of the next block, or to the end of the file.

**A marker is a block opened by `@attests`, and only that.** A declaration and a grounding are blocks too, and
neither is a marker: a declaration has no verdict and no standing, and a grounding owns nothing at all. Keeping
one word for one construct is why §5.1, §6.2, and the join in §8.2 need no qualification — they were written
about the witness case and they still mean it.

**A grounding's extent is inert.** Nothing audits a rationale and nothing joins against it, so the lines below a
`@grounds` are prose and no more. Do not look for meaning there.

**`@end` is the escape hatch, and it is opt-in.** The default extent is right for a test and for a witness file,
because those are sequential blocks. It is too wide inside a map of configuration, where one marked line sits
among many unmarked ones:

```jsonc
{
  "rules": {
    /* @attests no-console */
    "no-console": "error",
    /* @end */

    "no-debugger": "error",
  },
}
```

**Over-extension is safe.** It causes extra audits. Under-extension lets a false witness survive. So the coarse
default is never wrong, only expensive, and the extent is an optimisation problem rather than a correctness one.

**This is why `@end` is not required.** A required `@end` would not make the extent correct, only **stated**, and
a hand-placed terminator fails in the unsafe direction: too early is under-extension, which is the one failure
above that lets a false witness survive. It also goes stale silently, because there is nothing to check a written
extent against, whereas a default that runs to the next block tracks a `describe` as it grows and shrinks.

**There is no file-level marker and no nesting.** An earlier version made a marker _before any non-comment content_
own the whole file. A line scanner cannot compute that, because it would have to know the comment syntax it
deliberately refuses to learn. When a claim covers a whole file and another covers one block inside it, repeat the
slug:

```ts
/** @attests close/deletes-first */
describe("close, unit", () => { … })

/** @attests close/deletes-first */
/** @attests close/cleans-blockers */
describe("close, end to end", () => { … })
```

Two markers, two witnesses, one claim attested by both. A slug repeated in six blocks of one file is visible, and
it is telling you the claim is too broad.

**When to write a parser.** A parser gives the better answer — the instrument is the `describe` block below the
marker, or the value of the key below the marker. It belongs in an **adapter**, where the language knowledge
already lives, and never in the core. The signal to build one is `@end` appearing on nearly every marker in one
file type. Until then the parser is a cost with no evidence behind it.

### 6.3 Markdown, and files that take no comment

**Markdown uses the same rule, in an HTML comment. Not frontmatter.** Only two fields are ever read by a machine,
and the universal rule already covers both. Frontmatter would make the core learn a second format, and a rule for
when to apply which, and gain nothing.

**A file that takes no comment at all**, such as JSON, has nowhere to put a marker. Two resolutions, in order of
preference:

1. Move the file to a format that takes comments.
2. Witness it with a **witness file** whose `@scope` is that file, and whose judgment states what must be true of
   it.

### 6.4 What is not a directive

There is no `@run`. A command has spaces and arguments, and it does not need to be a directive, because **the core
never runs anything**. Only the agent reads the command, and an agent reads prose.

> A directive exists only for what the core must resolve without intelligence. Everything else is prose.

By that test `@attests` qualifies, because the form check and the join need it. `@claim` qualifies, because the
slug must bind to the prose that defines it. `@scope` qualifies, because carry-forward, claim surfacing, and the
auditor's reading list need it. `@grounds` qualifies, because prose that **mentions** a slug and prose that is
**about** it look identical to a machine, and §11.1 needs them apart. `@kind` qualifies, because the readout is
ordered by it.

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
- A `@kind` outside `capability` and `development` is **unknown**. The set is closed, because §8.4 orders the
  readout by it and an open set has no order.
- A claim whose slug prefix names no project in this repository is **misfiled**. §3.4.

**Crux resolves slugs. It never resolves paths.** Every check above reads a directive and compares it with
another directive. None of them looks at where a file sits, which is why the positions in §3.4 are house rules
that a repository may decline without breaking anything.

Two things are reported and are **not** errors:

- **A rationale that grounds a claim which no longer exists.** §11.5.
- **A glossary that changed in this diff.** §3.3.

## 7. The amendment

An amendment is the set of claim changes that one unit of work proposes. It is the specification.

Its operations are **add**, **change**, and **delete**. Each entry names a claim, and for an add it also names the
witness that will attest it.

A delete removes the claim and its witnesses in the same merge.

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
/** @attests no-subprocess */
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

| State                  | Can you write the claim? | Who holds it | Exit                        |
| ---------------------- | ------------------------ | ------------ | --------------------------- |
| **fog**                | no                       | cairn        | human judgment, or evidence |
| **proposed amendment** | yes                      | the branch   | the merge                   |

So a tracker needs no third object. Fog discharges into an amendment, or — when what it resolves into is settled
by construction — into a rationale. The amendment discharges into the catalog. **A rationale is not a third
state**, because it is answered rather than pending, and it leaves the tracker for the repository at the moment
it is written.

**Fog is defined by inability, not by absence of effort.** A claim you can write but have not written yet is not
fog. It is an unwritten amendment. If you allow that mistake, fog becomes a backlog, and it stops being a signal
that a human is needed.

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

### 11.2 When to write one

All three must be true. If any is missing, skip it.

1. **Hard to reverse** — the cost of changing your mind later is real. If it is cheap to reverse, you will just
   reverse it.
2. **Surprising without the reasoning** — a future reader will look at this and ask why. If nobody would wonder,
   nobody will read the document.
3. **The result of a real trade-off** — there were genuine alternatives, and you picked one for stated reasons. If
   there was no alternative, there is nothing to write beyond _we did the obvious thing_.

**And it names what was neglected.** This is a requirement on content, not on form. The rejected option is the
half that the code cannot show you, so a rationale that omits it has recorded only what was already visible.

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
<!-- @grounds belay/close/ordered-not-atomic -->

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

### 11.5 A grounded claim that is deleted is reported, not failed

An amendment deletes a claim. A rationale that grounds it now points at nothing.

**This is reported and it is not a form error.** Making it fail would force editing history, and the document is
still true — it records reasoning that was correct when it was written. What the report says is that a decision
was reversed and nothing yet records the reversal, which is the one decay in this artifact that a machine can see.

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

## 13. What gets built first, and what is being watched

### 13.1 Two tools, in order

**1. Crux.** It reads and lists claims, witnesses, and rationale links. Its whole scope is already stated above:

- list the claims, each with its kind and the witnesses that attest it
- list the markers, each with its claims, its extent, and its scope
- list, for each claim, the rationale that ground it (§11.1)
- report the form errors of §6.6 — unattested, orphaned, dead scope, mixed, unknown kind, misfiled
- report the two non-errors of §6.6 — a rationale grounding a deleted claim, and a changed glossary
- emit the marker index as a machine form, for adapters to join against

It runs nothing. It stores nothing.

**2. A belay MVP.** A local supervisor that goes from an amendment on a branch to a PR with a posted readout:

- run the sources named in `.belay/witnesses.toml`, map each report through its adapter, and join on the marker
  index (§8.2)
- produce the readout (§8.4)
- open or update the PR, and post the readout as a comment naming its commit (§9.4)
- loop the builder and the reviewer, with a fresh reviewer each cycle and a cycle budget (§9.3)

It does **not** hold fog, and it is not a tracker or a router.

### 13.2 The tracker is deliberately not third

Cairn is not on this list, and that is the point. Several threads in §15 are about fog, and none of them has an
answer. A tracker built now would encode guesses about material nobody has handled yet under this vocabulary.

So the two tools above are built **by hand at the fog layer**. Every amendment is authored the slow way, and every
conversion from fog to a claim is done by a person with an agent. That is the experiment.

### 13.3 The dogfooding rule

> **While building tools 1 and 2, the pain of tracking fog and of converting fog into an amendment is the data.
> Record it as it happens.**

This applies to the human and to every agent in the work. An agent that notices one of the following states it in
its report, and does not silently work around it:

| Watch for                                                | What it tells you                                                      |
| -------------------------------------------------------- | ---------------------------------------------------------------------- |
| a claim that could not be written, and what unblocked it | whether judgment or evidence clears fog more often                     |
| a claim that was written and then reworded               | the right granularity for a claim                                      |
| a witness assignment that was harder than the claim text | the ladder is wrong, or the claim is at the wrong altitude             |
| an amendment that had to change during the build         | how much design must precede a build, and where the entry gate belongs |
| what had to be re-derived at the start of a session      | exactly what the tracker must hold                                     |
| a by-hand step that felt **clerical**                    | a candidate for the tool to absorb                                     |
| a by-hand step that felt like **thinking**               | never automate it                                                      |

The last two are the test that matters. Clerical work is a missing feature. Thinking is the work.

**Where the notes go.** One append-only file, `FOG-LOG.md`, until §15 settles where fog lives. It is fog by
definition, so nothing in it is a claim, and none of it is durable.

**When the period ends.** Not when the tools are finished. It ends when the log holds a pain point that has
**repeated**. One occurrence is an anecdote and it redesigns nothing. A repeat is evidence, and it is the input to
the tracker design.

## 14. The properties

1. Nothing is built without a claim, and nothing merges without being judged against one.
2. A claim is finished when something that is not you can falsify it.
3. Fog is named as fog and cleared deliberately, by judgment or by evidence.
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

## 15. Open threads

- **Does belay receive an amendment, or a branch with failing witnesses?** Not decided. The strongest argument for
  the branch is not test-first discipline and is not the deleted classifier: **a witness that denied before the
  build is the only mechanical evidence that it can deny at all.** Every other soundness check is a person reading.
  Against it: three of the four kinds have no failing state, so the gate would read _every witness that can deny,
  does deny_, and the work moves rather than disappears. Revisit.
- **The spelling of _canvass_.** One letter separates it from _canvas_, and the wrong one is also a real word. This
  is friction for a command name. **poll** is the fallback, and it loses only the completeness property.
- **Where the amendment lives.** A file on the branch is the current assumption, rendered into the PR description.
  Not decided.
- **How the audit scope gets its diff base.** The merge base of the branch is the obvious answer, and it needs git.
  Whose job that is has no answer yet.
- **Where fog is stored.** It is the one piece of durable state that is not in the target repository. A second
  repository, keyed to the target, is the current preference.
- **How a lint-rule witness denies before the build.** A new rule denies on untouched code, not in one place. Two
  candidates: an allowlist, or land the rule with the refactor.
- **Beacon.** Unowned. Its payload is fully determined by the amendment and the readout, so it invents no data
  model and can be deferred.
- **Whether a single-project repository should carry a prefix anyway.** §3.4 drops it, so `close/deletes-first`
  reads well and a later split into a monorepo rewrites every slug in the repository and in every tracker that
  holds one. The cost was accepted on the grounds that it is mechanical and it only happens to a project that
  succeeded. Revisit if a split ever actually happens.
- **Whether this document's §2 is the workspace glossary.** For every other repository the two are separate — the
  framework's words and the project's words. Here they collide, because the project is the framework. Keeping
  both would give one word two homes.
- **Where the catalog sits inside a project.** §3.4 draws `docs/catalog/`, and §6.6 says crux never resolves a
  path, so nothing enforces it. Whether that freedom is worth having, or whether it only invites drift between
  repositories, has no answer yet.
