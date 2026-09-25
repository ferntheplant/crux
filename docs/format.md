# The marker format

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

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

| Shape         | Directive         | Token                                      | Opens         | Repeatable |
| ------------- | ----------------- | ------------------------------------------ | ------------- | ---------- |
| **noun**      | `@glossary`       | none                                       | a glossary    | no         |
| **noun**      | `@claim <slug>`   | a slug                                     | a declaration | no         |
| **verb**      | `@attests <slug>` | a slug, or a comma-separated list of slugs | a marker      | yes        |
| **verb**      | `@grounds <slug>` | a slug, or a comma-separated list of slugs | a grounding   | yes        |
| **attribute** | `@scope <globs>`  | a glob, or a comma-separated list of globs | —             | yes        |
| **attribute** | `@kind <word>`    | `capability` or `development`              | —             | no         |

**`@glossary` takes no token, and it is the only directive that marks a whole file.** Its extent is inert: crux
learns that this file is a glossary and nothing else about it, which is the whole of what
[§3.2](./vocabulary.md#32-crux-does-not-read-it) permits. One consequence of taking no token is that its match
surface is wider than the rest — every other directive is immunised by the one-token rule, and this one is not. A
sentence beginning _@glossary is the word we use_ would mark the file. `crux-ignore` is the remedy, and prose that
discusses the format should expect to reach for it.

**Every token is one word.** An attribute never takes free text, and this is not a style preference: the moment a
token may contain a space, the core must know where the comment ends in order to know where the value stops, and
that is the knowledge §6.1 exists to refuse. `/* @kind a long phrase */` would parse as `a long phrase */`.

**A token containing `<`, `>`, or a backtick is not a directive.** This is the rule for prose, and the population
it protects is larger than it looks: it is not only this document, it is **every adopting repository's
`AGENTS.md`**, because explaining the format to your agents means writing `@claim <slug>` somewhere on day one.

Keep the character set narrow on purpose. A broader _any invalid slug character is ignored_ would swallow a
typo'd real slug — a stray capital, say — and that is under-detection, the one direction [§8.2](./review.md#82-the-join-from-a-marker-to-a-verdict) forbids. These three
characters never appear in a typo of a real slug, so the rule has no false-negative surface at all.

**A line containing the exact substring `crux-ignore` holds no directive.** Case-sensitive, anywhere on the line,
before or after the directive it suppresses.

It is line-local by design. There is no fence to track, no block to close, and no file type to know — which is
what keeps the core a line scanner. A line that says it is not a directive is not a directive, in any language, in
any file, with no state carried from the line above.

**It is also the one deliberate hole in [§8.2](./review.md#82-the-join-from-a-marker-to-a-verdict)'s rule against under-detection**, so keep the string ugly and keep it
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

**A block opens with `@glossary`, `@claim`, `@attests`, or `@grounds`, and the opener fixes what the block is.**

| Opens with  | Construct       | Its extent is  | Takes    |
| ----------- | --------------- | -------------- | -------- |
| `@glossary` | **glossary**    | inert          | —        |
| `@claim`    | **declaration** | the claim text | `@kind`  |
| `@attests`  | **marker**      | the instrument | `@scope` |
| `@grounds`  | **grounding**   | inert          | —        |

**One asymmetry, and it is deliberate.** Two openers are nouns and two are verbs. A noun binds — `@glossary` binds
nothing but the file it sits in, `@claim` binds a slug to the prose below it. A verb should have been a noun in both cases and is not, for
the same reason: **a witness has no identity to bind** ([§5.1](./claims.md#51-a-witness-is-a-marker)), and neither has a rationale. Making the grammar
regular would cost a second line on every witness in every repository forever, and [§5.2](./claims.md#52-the-four-kinds) wants as many witnesses as
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
because nobody else reaches for them, which extends [§2.1](./vocabulary.md#21-the-naming-rule)'s naming rule into a domain it was not written for: **an
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
one word for one construct is why [§5.1](./claims.md#51-a-witness-is-a-marker), §6.2, and the join in [§8.2](./review.md#82-the-join-from-a-marker-to-a-verdict) need no qualification — they were written
about the witness case and they still mean it.

**A grounding's extent is inert.** Nothing audits a rationale and nothing joins against it, so the lines below a
`@grounds` are prose and no more. Do not look for meaning there.

**A terminator names the opener it closes**: `@claim:end`, `@attests:end`, `@grounds:end`. There is no bare
`@end`, and its absence is the point. `@end` is a keyword in Objective-C and a command in Texinfo, so a stray one
would truncate a real marker's extent — and §6.2 is about to say that under-extension is the one failure that lets
an unsound witness survive. It is also the least-written directive, so a broken extent is the least likely to be
noticed. Naming the opener kills both collisions outright, and it makes the terminator checkable against the block
that is actually open.

Spell them with the **opener tokens** and not with construct names. `@witness:end` and `@rationale:end` would
reintroduce two words [§2.2](./vocabulary.md#22-words-that-were-rejected-and-why) deliberately killed.

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

**Over-extension is safe.** It causes extra audits. Under-extension lets an unsound witness survive. So the coarse
default is never wrong, only expensive, and the extent is an optimisation problem rather than a correctness one.

**This is why a terminator is not required.** Requiring one would not make the extent correct, only **stated**, and
a hand-placed terminator fails in the unsafe direction: too early is under-extension, which is the one failure
above that lets an unsound witness survive. It also goes stale silently, because there is nothing to check a written
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
A `.json` extension is evidence about a file's parser, not proof — and [§8.3](./review.md#83-a-lint-witness-in-full) makes this worth checking, because a
config file is where a lint witness lives.

### 6.4 What is not a directive

There is no `@run`. A command has spaces and arguments, and it does not need to be a directive, because **the core
never runs anything**. Only the agent reads the command, and an agent reads prose.

> A directive exists only for what the core must resolve without intelligence. Everything else is prose.

By that test `@attests` qualifies, because the form check and the join need it. `@claim` qualifies, because the
slug must bind to the prose that defines it. `@scope` qualifies, because carry-forward, claim surfacing, and the
auditor's reading list need it. `@grounds` qualifies, because prose that **mentions** a slug and prose that is
**about** it look identical to a machine, and [§11.1](./lifecycle.md#111-what-a-rationale-grounds) needs them apart. `@kind` qualifies, because the readout is
ordered by it. `@glossary` qualifies, because [§3.3](./vocabulary.md#33-a-changed-glossary-is-a-yellow-row) puts a
row on the readout when a glossary moves, and without the directive crux could only tell a glossary by its
filename — which is a convention rather than a declaration, and which would force one file per area.

The target command, the judgment, the reason, and the rejected alternative do not. They are read only by an
intelligence, and an intelligence reads prose.

### 6.5 Scope, and who needs it

`@scope` has three consumers, and only the first is inferential-only:

1. **Verdict carry-forward.** Nothing in the scope changed, so the expensive judgment is reused. [§5.5](./claims.md#55-the-verdict).
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
  is closed, because [§8.4](./review.md#84-the-readout) orders the readout by it and an open set has no order. The scoping is the whole check: a
  naive version fires on every JSDoc `@kind class` in the repository.
- Two declarations of one slug is a **collision**. One catalog means one slug names one claim, and nothing can say
  which declaration a marker meant.
- A `@grounds` that names no declared claim is a **forward dangle**. [§11.5](./lifecycle.md#115-a-dangling-grounding-in-both-directions) says why this is an error where the
  backward case is not.

**Crux resolves slugs. It never resolves paths.** Every check above reads a directive and compares it with
another directive.
None of them looks at where a file sits, so a repository may put its catalog, its glossaries, and its rationale
wherever it likes. [§3.4](./vocabulary.md#34-there-are-no-projects-and-the-trail-that-got-here) records the three
designs that would have made position load-bearing, and why each was dropped.

**A form error must be fixable by the person who caused it, at the moment they caused it.** That is the line
between this list and the two reports below it, and it is the whole reason the forward dangle is an error while
the backward one is not. A forward dangle is preventable while writing. A backward dangle is created by a later
merge, and it reaches into a document that cannot be edited from there and that was true when it was written.

Two things are reported and are **not** errors:

- **A rationale that grounds a claim which no longer exists.** [§11.5](./lifecycle.md#115-a-dangling-grounding-in-both-directions).
- **A glossary that changed in this diff.** [§3.3](./vocabulary.md#33-a-changed-glossary-is-a-yellow-row).

#### A rename does not touch every mention of the slug

The slug is the hub ([§11.1](./lifecycle.md#111-what-a-rationale-grounds)), so a rename looks like a search and
replace over the repository. It is not. Two kinds of mention are identical to a machine and must be treated
oppositely:

- A **citation** names a claim that exists now — a `@grounds`, an `@attests`, a catalog declaration, an
  amendment's entry, one amendment referring to another's claim. Every one must move, or it dangles.
- A **record** names a slug as it was written at some past moment — the list of claims an enacted amendment
  deleted, a note saying what a claim was renamed from, a log of what a session did. These must **not** move,
  because the name is the fact being recorded.

**No mechanical property separates them.** Crux finds an **orphaned** marker, so it would find every stale
citation. It would also find every record and report it as the same error, because both are a slug-shaped string
naming nothing that exists.

The consequence for a tool is direct, and it is not _rename everything_: a rename tool that operates on slug
identity corrupts the record it was supposed to preserve. What a tool can honestly do is **the join** — show every
mention, grouped by file, and let a person mark the boundary once. That is the same shape as
[§12.1](./roadmap.md#121-cairn-keeps-its-state-outside-the-repository-it-serves)'s watcher, and that remedy is
safe only because it never proposes to write.

The cost runs both ways when nobody does it by hand. A stale citation is a dangling reference a reader chases and
loses. A rewritten record is worse and quieter: it reads as true, and what it falsifies is the account of what the
work actually cost.
