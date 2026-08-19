# The vocabulary and the glossary

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

## 2. The vocabulary

| Word           | Meaning                                                                                               |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| **project**    | What a `GLOSSARY.md` declares. It owns one catalog, one rationale directory, and one slug prefix.     |
| **glossary**   | What a project's words mean. One `GLOSSARY.md` per project. Prose, never parsed.                      |
| **claim**      | A short, falsifiable statement of a requirement the codebase must satisfy. It carries a stable slug.  |
| **catalog**    | The organised set of all claims in a project.                                                         |
| **rationale**  | A document that says why a claim reads as it does, and what was rejected on the way.                  |
| **fog**        | Material you want, but cannot yet state as a claim.                                                   |
| **witness**    | A mechanism that judges whether some part of the codebase satisfies a claim.                          |
| **marker**     | The comment block that designates a witness. A witness **is** its marker.                             |
| **directive**  | One `@name` instruction inside a block. [§6.1](./format.md#61-directives) lists the six.              |
| **attest**     | The relation a marker records. Witness X attests claim Y.                                             |
| **subject**    | The code a witness judges. `@scope` names it.                                                         |
| **instrument** | The witness itself: the marker and the lines it owns, plus the claim text it attests.                 |
| **existence**  | Whether a witness is still installed. A form check over the markers.                                  |
| **verdict**    | What a witness says about the **subject**: **affirms**, **denies**, or **silent**.                    |
| **standing**   | Whether one **instrument** supports one **claim**: **sound**, **unsound**, or **unaudited**.          |
| **coverage**   | Whether a claim's witnesses **together** uphold it: **covered**, **under-covered**, or **unaudited**. |
| **amendment**  | The set of claim changes that one unit of work proposes.                                              |
| **canvass**    | The act of asking every witness in scope for a verdict.                                               |
| **adapter**    | A converter from one tool's report into verdicts, keyed on markers.                                   |
| **auditor**    | An intelligence that sets the standings of a claim's instruments, and then its coverage.              |
| **readout**    | What a canvass produces. One block per claim in scope. See [§8.4](./review.md#84-the-readout).        |
| **audit**      | The act of reading a claim's instruments to set their standings and its coverage.                     |
| **ruling**     | The decision a human makes at the merge.                                                              |

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

| Word                   | Rejected because                                                                                                                                                                                                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **delta**              | Too generic for a core object.                                                                                                                                                                                                                                                              |
| **review**             | Too generic, and it fused the canvass with the audit.                                                                                                                                                                                                                                       |
| **run**                | You cannot run a prose witness. The word is false for the witnesses that need a human most.                                                                                                                                                                                                 |
| **hearing**            | A hearing is a formal event where the judgment happens. A canvass decides nothing.                                                                                                                                                                                                          |
| **deposition**         | Correct in meaning, strange to say.                                                                                                                                                                                                                                                         |
| **discovery**          | It means _find out what you do not know yet_. That is fog. The two would collide.                                                                                                                                                                                                           |
| **sweep**              | It can also mean _clear away_.                                                                                                                                                                                                                                                              |
| **poll**               | A poll samples. A canvass is complete. Held as the fallback if the spelling of _canvass_ is too much friction.                                                                                                                                                                              |
| **passes** / **fails** | Ambiguous. "The witness fails" can mean the code is bad or the witness is bad.                                                                                                                                                                                                              |
| **decision**           | The decision is the claim. A word for it here would name the same thing twice.                                                                                                                                                                                                              |
| **ADR**                | It holds the decision **and** the reasoning. Crux splits the two, so the name is false. See [§11](./lifecycle.md#11-rationale).                                                                                                                                                             |
| **`CONTEXT.md`**       | _Context_ is already ambiguous, and it names a boundary rather than the word list the file holds.                                                                                                                                                                                           |
| **workspace**          | A name for the root project. _The root project is the project at the root_, so the overload describes rather than puns. The first repository to adopt this vocabulary rejected the word on sight, and rejected the argument for it a second time when it was restated.                      |
| **false**              | The former name of the **unsound** standing. It does not pair with _sound_, so a reader had to learn the two ends separately. This document also uses _false_ in its ordinary English sense in several places, and one word must not do both jobs. See [§5.6](./claims.md#56-the-standing). |
| **complete**           | A name for the **covered** coverage. It promises that nothing is missing, which no audit can establish. _Covered_ says how far the witnesses reach, which is what the auditor actually judges. See [§5.8](./claims.md#58-coverage).                                                         |

Four directives were proposed and rejected. Record these too.

| Directive             | Rejected because                                                                                                                                                                                                                                                        |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@decision`           | An anchor from code to a rationale. A plain Markdown link does the same for a reader, and the core resolved nothing.                                                                                                                                                    |
| `@witness`            | An opener for a witness block. `@attests` already opens it, so the directive carried no information.                                                                                                                                                                    |
| `@record`             | A noun for a rationale. The only query it served is `ls`.                                                                                                                                                                                                               |
| a required terminator | It makes the extent **stated** rather than **correct**, and a hand-placed one under-extends. See [§6.2](./format.md#62-the-extent-of-a-marker).                                                                                                                         |
| a bare `@end`         | `@end` is a keyword in Objective-C and a command in Texinfo. A stray one truncates a real marker's extent, which is the one failure that lets an unsound witness survive. The terminators name their opener instead. See [§6.2](./format.md#62-the-extent-of-a-marker). |

### 2.3 The tools are named outside the vocabulary

The tools are **crux**, **belay**, **cairn**, and **beacon**. The names come from mountains, and none of them means
anything in this system. [§12](./roadmap.md#12-the-tools) says what each one does.

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

This is the same refusal as [§6.1](./format.md#61-directives), one level up. There, the core never learns the comment syntax of any language.
Here, it never learns what a term is.

**The `@project` line is not an exception to this.** It is the ordinary line scan of [§6.1](./format.md#61-directives), applied in a file crux
already had to find, and none of the three arguments below touches it: it needs no grammar, it matches nothing
fuzzily, and it protects a case a person cannot close by hand. What §3.2 refuses is the **definitions**, and a
prefix is not one.

An earlier draft made the glossary load-bearing: crux would index the terms, and a changed definition would put
every claim using that term into the audit scope. Three things killed it.

- **The file would need a grammar.** `**Order**:` followed by a definition is a Markdown micro-format, and [§6.3](./format.md#63-markdown-and-files-that-take-no-comment)
  rejects frontmatter for exactly this reason — it makes the core learn a second format and a rule for when to
  apply which.
- **The match would be fuzzy in the unsafe direction.** Even with a perfect term index, _does this claim use this
  term_ is a natural-language question. A word-boundary match misses a claim that is **about** orders without
  containing the word. Under-attribution is the direction [§14](./roadmap.md#14-the-properties) property 11 forbids, and a matcher that under-matches is
  worse than none, because [§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache)'s induction would then rest on it silently.
- **The case it protects is already closed by the person making it.** If you redefine a term and it changes what a
  claim promises, you reword the claim — and rewording claim text is already the second term of the audit scope.
  What remains is the case where the editor read every claim using that word and judged them all still correct.
  That is not a gap a machine needs to close.

So the glossary is an input to the **auditor**, which [§5.4](./claims.md#54-the-four-questions) says is always an intelligence. An auditor reads the
instrument and the code in `@scope`; it reads the glossary beside them. That needs no format at all.

**The glossary is privileged without being parsed.** It has a fixed name, one directive, a reader on every audit,
and a row on the readout when it moves. None of that requires a grammar.

### 3.3 A changed glossary is a yellow row

The one mechanical hook, and it is deliberately weak:

> **When a glossary changes in a diff, the readout says so and lists the claims of that project.**

It is not an audit trigger and it voids nothing. It is a row the human clears at the ruling, in the same family as
[§4.2](./claims.md#42-how-the-condition-fails)'s two yellow rows: the machine has reached the edge of what it can say, and the question — _did the meaning
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
where a file sits, which [§6.6](./format.md#66-form-errors) forbids outright. One declared token removes the layout from the question, and the
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

The reason is [§12](./roadmap.md#12-the-tools): **a tracker stores slugs and never claim text**, and beacon routes on a slug with no checkout.
Both must place a claim without an index, and a prefix is the only thing that does that with pure string work.

**The single-project exemption is deleted, and `@project` is what paid for it.** An earlier draft dropped the
prefix when a repository held one project, on the grounds that `close/deletes-first` reads better and that the
rewrite on a later split is mechanical. Two things changed. The prefix is now one declared token rather than a
convention, so writing it costs one line in one file. And an exemption would make `@project`'s token optional —
the only directive in the format with a variable arity, and a rule to memorise for a saving of one path segment.

What it buys is that **a split never rewrites a slug**. The cost fell only on projects that succeeded, which is
the worst population to charge, and a slug that appears in a tracker beside another project's is better off
prefixed from the first day it is written.

**The kind is not in the slug.** `@kind` carries it ([§6.1](./format.md#61-directives)), so a claim is never `belay/capability/...`. A slug
names a project and then an area, and the audience it is written for is an attribute that can be corrected
without a rename.

The kind is also **not derivable from the prefix**, which is what earns it a directive rather than a convention. A
root project's claims are mostly development-kind — conventional commits, the lint rules, the gate — and that
correlation invites the shortcut. It is only a correlation: a package holds development claims too, because a
package has its own lint rules.

**Crux checks the prefix.** A claim whose prefix names no declared project is misfiled. [§6.6](./format.md#66-form-errors).
