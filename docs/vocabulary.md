# The vocabulary and the glossary

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

## 2. The vocabulary

| Word           | Meaning                                                                                               |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| **glossary**   | What the words in a claim mean. A file carrying `@glossary`. Prose, never parsed.                     |
| **claim**      | A short, falsifiable statement of a requirement the codebase must satisfy. It carries a stable slug.  |
| **catalog**    | The set of every claim in the repository.                                                             |
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
| **project**            | A named subdivision of a repository, owning one glossary, one catalog, and one slug prefix. Tried in three forms and dropped in all three. §3.4 holds the trail; do not reintroduce it without reading that section first.                                                                  |
| **workspace**          | A name for the root project, from the era when projects existed. Kept because it records that the naming question was live and was answered twice before the thing being named was deleted.                                                                                                 |
| **false**              | The former name of the **unsound** standing. It does not pair with _sound_, so a reader had to learn the two ends separately. This document also uses _false_ in its ordinary English sense in several places, and one word must not do both jobs. See [§5.6](./claims.md#56-the-standing). |
| **complete**           | A name for the **covered** coverage. It promises that nothing is missing, which no audit can establish. _Covered_ says how far the witnesses reach, which is what the auditor actually judges. See [§5.8](./claims.md#58-coverage).                                                         |

Five directives were proposed and rejected. Record these too.

| Directive             | Rejected because                                                                                                                                                                                                                                                        |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@decision`           | An anchor from code to a rationale. A plain Markdown link does the same for a reader, and the core resolved nothing.                                                                                                                                                    |
| `@witness`            | An opener for a witness block. `@attests` already opens it, so the directive carried no information.                                                                                                                                                                    |
| `@record`             | A noun for a rationale. The only query it served is `ls`.                                                                                                                                                                                                               |
| a required terminator | It makes the extent **stated** rather than **correct**, and a hand-placed one under-extends. See [§6.2](./format.md#62-the-extent-of-a-marker).                                                                                                                         |
| a bare `@end`         | `@end` is a keyword in Objective-C and a command in Texinfo. A stray one truncates a real marker's extent, which is the one failure that lets an unsound witness survive. The terminators name their opener instead. See [§6.2](./format.md#62-the-extent-of-a-marker). |
| `@project <prefix>`   | It declared a project and bound the prefix that project's slugs carried. Deleted with projects themselves. `@glossary` took its place in the file where it used to sit, and carries no token. See §3.4.                                                                 |

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
most expensive. So a repository settles its words first, and the settlement is a **glossary**: any file carrying
the `@glossary` directive.

**A repository holds as many glossaries as it finds useful.** One file for a large vocabulary is unmanageable, and
crux has no reason to insist on one — it never reads a definition, so the number of files costs it nothing. Split
them by area, by package, by whatever grouping a reader would look under. Which glossary governs a given claim is
a question for the auditor, which §3.2 says is always an intelligence, and an intelligence reads a directory.

### 3.1 What it holds

A glossary, and nothing else. Not a specification, not a design note, not a scratch pad. Every entry is a word and
what it means.

```md
> @glossary crux-ignore

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
- **Only terms this repository gives a special meaning.** A general programming word belongs here when the
  repository narrows it, and never because the code uses it a lot.
- **Group terms under subheadings when clusters appear.** A flat list is right until it is not.

**The glossary is not claimable.** It states what words mean, and no code can falsify a definition, so there is
nothing for a witness to be asked. A repository may still claim conformance _to_ it — _no identifier uses a
rejected term_ is a claim with an obvious lint witness — but that claim is about the code. The definition is not.

### 3.2 Crux does not read it

> **Crux knows which files are glossaries, and nothing else about them. It never learns what a term means.**

This is the same refusal as [§6.1](./format.md#61-directives), one level up. There, the core never learns the comment syntax of any language.
Here, it never learns what a term is.

**The `@glossary` line is not an exception to this.** It is the ordinary line scan of [§6.1](./format.md#61-directives), and none of the three
arguments below touches it: it needs no grammar, it matches nothing fuzzily, and it protects a case a person
cannot close by hand. What §3.2 refuses is the **definitions**, and a flag on a file is not one.

**It is declared rather than derived, and that is what buys the split above.** An earlier design found glossaries
by the fixed filename `GLOSSARY.md`, which forced one file per area and made the name load-bearing. A directive
moves the fact from the filename into the file, so a glossary may be called anything, live anywhere, and be split
as finely as its readers want. This is the same trade [§6.4](./format.md#64-what-is-not-a-directive) asks of every
directive: the core must resolve _is this a glossary_ without intelligence, and a filename convention is the
weaker way to tell it.

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

**A glossary is privileged without being parsed.** It has one directive, a reader on every audit, and a row on
the readout when it moves. None of that requires a grammar.

### 3.3 A changed glossary is a yellow row

The one mechanical hook, and it is deliberately weak:

> **When a glossary changes in a diff, the readout names it.**

It is not an audit trigger and it voids nothing. It is a row the human clears at the ruling, in the same family as
[§4.2](./claims.md#42-how-the-condition-fails)'s two yellow rows: the machine has reached the edge of what it can say, and the question — _did the meaning
move?_ — goes to the person who moved it.

**It names the file and stops there.** An earlier version listed the claims of the changed glossary's project.
Without projects there is no subset to list, and listing every claim in the repository would be noise rather than
signal. The loss is smaller than it looks: §3.2's third argument already says the case is closed by the person
making it — if you redefine a term and it changes what a claim promises, you reword the claim, and reworded claim
text is already the second term of the audit scope ([§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache)).

**Splitting glossaries makes this row better, not worse.** One enormous glossary moving tells an operator almost
nothing. A small glossary named for one area moving tells them where to look. The finer the split, the more the
row is worth — which is the opposite of what a mechanical hook usually does when you subdivide its subject.

The cost is a file-level diff and no parsing.

### 3.4 There are no projects, and the trail that got here

> **One repository, one catalog. A slug is whatever its author writes, and nothing derives, checks, or decorates
> it.**

A claim slug is a free-form token: `close/deletes-first`, `checkin/it-does-a-thing`, `foo/bar/baz/it-does-fizz`.
The slashes are a convention for reading, not a structure crux parses. Prefixes are how a team groups its claims,
`crux ls --prefix belay/` is pure string work, and nothing needs declaring for that to work.

**This is a deletion, and three earlier designs are underneath it.** Each was load-bearing, each had an argument,
and each failed on a different repository shape. Record all three, because the pull toward reinventing them is
strong and each one looks right until the case that kills it.

| Design                                              | It bought                                       | It died on                                          |
| --------------------------------------------------- | ----------------------------------------------- | --------------------------------------------------- |
| **1.** the slug carries a declared project prefix   | a tracker places a claim with pure string work  | the rename cost, paid by projects that succeeded    |
| **2.** position: the nearest glossary owns a marker | free splits, clean slugs, a structural arrow    | a project spread across `packages/` **and** `apps/` |
| **3.** no projects at all                           | all of the above, and the core resolves no path | nothing checks that a team's prefixes stay coherent |

**Design 1 — the prefix.** Every slug began with a declared project prefix, and crux reported a prefix naming no
project as **misfiled**. The argument was §12's interface: a tracker stores slugs and never claim text, so a slug
must place a claim without a checkout, and a prefix does that with pure string work.

What killed it was measured rather than argued. Collapsing a four-project repository into one moved about
twenty-six slugs across twelve files, and none of it was mechanical, because a slug that names something **live**
must move and a slug that names something **past** must not
([§6.6](./format.md#a-rename-does-not-touch-every-mention-of-the-slug)). The cost fell entirely on projects that
had grown enough to reorganise, which is the worst population to charge.

**Design 2 — position.** Drop the prefix, and let a marker belong to the nearest glossary above it. Slugs get
short, a split rewrites nothing, and the arrow between a parent and a child becomes structural: a child cannot
attest a parent's claim, because the slug resolves to the child's catalog and dangles. It also strengthened
misfiling, catching a catalog that declared a slug belonging to a different project.

What killed it is that **position implies a project is one contiguous subtree.** A tool implemented as
`packages/belay-core`, `apps/belay-server`, and `apps/belay-supervisor` has three different nearest ancestors and
no way to share one catalog without moving all three under one directory. Real monorepos put modules of one
product in several top-level directories, and a format that forbids it is a format that does not get adopted. The
escape hatch — let a marker declare its project — **is the prefix**, which is how you know the circle closed.

**Design 3 — the deletion.** Both of the above existed to answer _which project owns this claim_, and nothing else
needed the answer. The catalog is a set, the tracker wants uniqueness rather than structure, and grouping is a
reading convenience that a naming convention already provides. So the question goes away with the thing that
asked it.

### What the deletion costs, stated plainly

**Nothing enforces the convention.** With `belay/`, `cairn/`, and `core/` as prefixes by agreement, nothing stops
a claim from being filed under a prefix that means nothing, and nothing notices when a repository's prefixes drift
into two competing schemes. That is a house rule now, and it is the kind that rots quietly at scale.

Two things soften it and neither closes it. A typo'd prefix in an `@attests` still shows as **orphaned**, and a
typo'd prefix in a `@claim` still shows as **unattested**, so nothing that was caught before becomes silent — what
is lost is a check on prefixes nobody typo'd but nobody agreed on either. And a catalog that is tiring to read is
already evidence about the design ([§13.3](./roadmap.md#133-the-dogfooding-rule)), which is the same signal
arriving through a person instead of a check.

**The structural arrow is not lost, it dissolves.** _No module attests another module's claims_ was invented to
police design 2's nesting, and with no nesting there is nothing to police. If a repository wants it back, it is a
**claim in that repository's own catalog** witnessed by a lint rule — not a format feature, and not a reason to
reintroduce projects.

### 3.5 What survived, and why it is stronger

Three things the deletion improves rather than merely leaves alone.

**Crux resolves no paths at all.** [§6.6](./format.md#66-form-errors) states this absolutely, and design 2 was
about to buy one carefully-negotiated exception to it. The core is a line scanner again, and where every file sits
is a house rule that a repository may decline.

**Slugs are globally unique by construction.** §12's hard interface — _a tracker stores slugs and never claim
text_ — is what design 1 introduced a prefix to protect. One catalog per repository gives it for free, with no
prefix, no shadowing rule, and no disambiguation. Cairn and beacon place a claim with pure string work, which is
more than the prefix ever guaranteed: under design 2 a shadowed slug named two claims and neither tool could tell
them apart.

**A split never rewrites a slug.** Design 1 claimed this and did not deliver it, because splitting a project
changed every prefix it held. Here it is true, because no slug ever named a project.
