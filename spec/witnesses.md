# Witnesses

A witness is a part of the repository that can show if the codebase satisfies a claim. This file tells how a witness is marked, and how to write one that tells the truth.

## Witness blocks

> **A witness is the part of the repository that one `@attests` block and its extent identify.**

Crux finds each witness with a line scanner, in each file that git tracks. It never learns the comment syntax of a language.

```ts
/** @attests catalog/closing.md#ordered-not-atomic    crux-ignore */
describe("close", () => { … })
```

> [!CLAIMS]
>
> ### One directive
>
> `@attests` is the only directive. It is `@attests`, whitespace, and one token with no whitespace. Crux ignores the remaining text on the line.
>
> ### Blocks
>
> A block is a contiguous sequence of directive lines. The first line with no directive ends it.
>
> ### Several slugs
>
> A token can hold several slugs, separated by commas. Several `@attests` lines in one block are one witness of each slug.
>
> ### Extent
>
> A block owns the lines from itself to `@attests:end`, to the next block, or to the end of the file, whichever is first.
>
> ### Not a directive
>
> A token that contains `<`, `>`, or a backtick is not a directive.
>
> ### Ignored lines
>
> A line that contains `crux-ignore` has no directive. The match is case-sensitive.
>
> ### Claims only
>
> A witness attests claims. An `@attests` token has the form of a claim slug, `path#heading`, and never names a section heading.

Thus the core ignores `*/`, `-->`, and `#`, and it does not know what they are. The rule about `<`, `>`, and the backtick protects prose that explains the format, for example the `AGENTS.md` of each repository that uses crux. `crux-ignore` is unusual on purpose, because it is the only permitted way to hide a real directive.

**There is no registry and no witness identifier.** Crux makes the index of witnesses again on each run. One block is one witness. Two blocks for one claim in one file are two witnesses.

- **Existence enforces itself.** Delete the test, and its block goes with it. The claim becomes unattested.
- **A witness needs no stable identity.** A diff already records a moved file.
- **A witness has two users.** An adapter changes it into a verdict. An auditor changes it into a standing.

**A witness attests claims, not section headings.** The long form let a witness attest a group of claims. Then a new claim in that group was attested on the day it was written, by a witness that nobody had read against it. That is under-attribution ([Over-attribution](readout.md#over-attribution)).

**An extent that is too large is safe. An extent that is too small is not safe.** Too large causes more audits. Too small lets an unsound witness stay. Thus the large default is never wrong, only expensive, and a terminator is optional. Use `@attests:end` where one witness is among lines that no witness owns, for example one rule in a lint configuration.

**There is no witness for a full file, and blocks do not nest.** A line scanner cannot find "before the code" without the comment syntax. When a claim covers a full file, write the slug on each block.

**A directive exists only for what the core must resolve without intelligence.** All other text is prose. There is no `@run`: the core runs nothing, and an agent reads a command as prose. The condition, the reason, and the rejected option are prose for the same reason.

**The name stays `@attests`.** It is a verb, and it tells what the line means: this block attests that claim. A tool name in its place would put a migration into repositories that you do not control ([Tool names](glossary.md#tool-names)).

### Markdown and files with no comments

A witness in Markdown uses the same rule. A prose witness file is ordinary Markdown with an `@attests` line. It holds no claims block, so it is not a catalog file. Do not put a directive in a code span: the closing backtick becomes part of the token. An example in a fence is a real directive, so add `crux-ignore` to it.

For a file that takes no comments, such as JSON, write a witness file that names it. First, examine the parser: `tsconfig.json` is JSONC, and it takes comments.

## The ladder

Move each claim as high in this list as it can correctly go.

|     | Rung      | Verdict from     | Reason for its position                                               |
| --- | --------- | ---------------- | --------------------------------------------------------------------- |
| 4   | type      | the type checker | It reaches each use, new uses also. It can require and it can forbid. |
| 3   | test      | the runner       | It observes behaviour. It has a failing state before the build.       |
| 2   | lint rule | the linter       | It reaches each file, new files also. It can only forbid.             |
| 1   | prose     | a judge          | It is expensive to ask. It is only as good as the judge who reads it. |

Types, tests, and lint rules are **computational**: a tool answers them. Prose is **inferential**: a judge answers it.

**A test is above a lint rule because of polarity.** A lint rule removes one way to fail. It cannot affirm the way to succeed ([Polarity](#polarity)). A test can do both. But a lint rule examines each file, and this includes files that people add later. A test usually does not. Thus a claim that must hold for new code frequently needs a lint rule **and** a test.

**A type is at the top because it has both properties.** It reaches each use, as a lint rule does. It can require a shape, as a test does. If a type system can encode a behaviour, _it compiles_ becomes a verdict about that behaviour.

> **A witness that no tool adapter reports on is inferential.**

Crux cannot identify a test or a request handler, and it does not need to. The source that answers sets the rung. Nothing declares it ([Adapters](readout.md#adapters)).

**A type witness must bind its subject.** A `ReadOnlyDb` type proves nothing if the dashboard can still get `Db` directly. The project compiles, and the witness affirms, but it tells nothing. A type has no failing state before the build. Thus the builder breaks it: the builder adds the write that the type forbids, and makes sure that the build fails.

- **Your ecosystem and your budget set the top of the ladder, and the claim does not set it alone.** A library with type-aware lint rules raises the top for all claims at once. Ask _can a rule exist here_, not _does a rule exist_. A custom rule can be forty lines.
- **A static witness needs a declarative subject.** Nobody can read an imperative deploy script for the state that it makes. If a claim stays at prose, ask if you can make its subject declarative.

### Inferential witnesses

An inferential witness names its target and tells when it is valid. It can be a witness file:

```md
> @attests catalog/report.md#reads-at-a-glance crux-ignore

Run `demo report --fixture test/fixtures/mixed-status`. The rendering code is in `src/report/`.

Valid when: a reader can tell a failing row from a skipped row without the labels.
```

It can also be on the code that it examines. This is the correct first rung for a codebase that has no tests:

```ts
/**
 * @attests catalog/checkout.md#never-charges-an-expired-cart    crux-ignore
 *
 * Valid when: the handler compares the expiry with the server-side timestamp
 * before it makes the payment client, and an expired cart returns 409 with
 * no call to payments.
 */
export async function checkout(req: Request) { … }
// @attests:end    crux-ignore
```

**Write the condition, not only the assertion.** _This endpoint satisfies the claim_ gives an auditor nothing to examine. The _Valid when_ line is the instrument. It also tells where to look, and the triage of the judge reads it ([Verdict](readout.md#verdict)).

**The upgrade is one merge.** When a test exists, add its `@attests` and delete the inline block. The claim does not change, and the readout shows the higher rung.

### Lint witnesses

The block is on the rule, or on the configuration line that enables it. Delete the line, and the claim is unattested. Set it to `warn`, and the instrument changes, so the auditor sets it to unsound. Violate the rule, and the adapter reports a denial. **Do not add a test that asserts that the rule is enabled.** The position of the block already does this.

> [!NOTE] **Open: a new lint rule denies before the build.** A new rule denies on code that nobody touched. Two options are an allowlist, or to merge the rule with the refactor.

## Subject and instrument

> **The subject is what a witness observes and must not consult.**

Nothing declares a subject, but the subject is real. The rule for a sound witness defines it ([Stated contract](#stated-contract)). The witness uses the subject, and it takes none of its contract from the subject.

> **The verdict is about the subject. The standing and the coverage are about the instrument. Only a change to an instrument or to the text of a claim opens a standing or a coverage again.**

A change to the subject asks the verdict again, and does nothing more. This is the TDD position, and it is true for each rung. If the witness still affirms and the instrument did not change, the audit stays valid.

- **A witness in its subject follows the subject automatically.** The extent of an inline witness is the code that it examines. Thus an edit to that code is an edit to the instrument, and the auditor examines the standing again. The cost is an audit after each such edit.
- **A standing belongs to a witness and a claim together.** A witness that attests three claims has three standings. To repair one bad pair, remove one slug from its `@attests`. Do not remove the witness.
- **Tell where the observation occurs.** A test can observe a wrapper and pass. A scheduler that changes each failure into success makes _the job completed_ true when the send was refused. A witness that does not name its point of observation has not told what it observes. A witness that observes a proxy for its subject tells nothing about the subject.

### Stated contract

> **A sound instrument states its contract. It does not derive its contract from its subject.**

This rule makes the induction of the audit safe. If the contract of a witness is its own, a change to the subject cannot move it. Thus a subject change only asks the verdict again. If the contract comes from the subject, the subject can make the witness weaker, and each verdict stays green.

```ts
// Derived: Db gets `insert`, and the witness becomes weaker.
type ReadOnlyDb = Omit<Db, "write">;

// Stated: no new method of Db can go through it.
interface ReadOnlyDb {
  read(q: Query): Row[];
}
```

Tests can fail in the same way. Examples are a snapshot that you record again after each output change, `expect(f(x)).toEqual(f(x))`, and a fixture that you make again from the current output. Each one takes its expected value from the thing that it examines.

**What the instrument depends on is part of its meaning.** A shared test helper or a type utility is outside the extent. Name it when you set a standing, and decide if it is stable enough to trust. Crux does not track it ([Reading](readout.md#reading)).

## Several claims

**Each claim needs a minimum of one witness that attests it alone.** If not, its verdict stays tied to a different claim. A shared witness is acceptable as an addition. It costs one standing for each claim, and a change opens all of them again. Thus share a witness only between claims that succeed and fail together.

## Polarity

> **A witness that removes one way to fail does not affirm the way to succeed.**

A lint rule that forbids `Math.random` is sound for _nobody can guess the token_. But a weak generator that somebody writes by hand passes it. Add a second witness with the opposite polarity: a test that observes the real path get its bytes from the approved source. Do this check first on a claim whose witnesses are all prohibitions.

A witness that has never denied has not been tested. Before a handoff, the builder breaks each witness that can deny, and makes sure that it denies ([Handoff when green](readout.md#handoff-when-green)).
