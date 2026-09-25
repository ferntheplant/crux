# Crux — the specification

**Crux is a format that records what a codebase promises. A machine can find each promise. A person can then
decide if the work satisfies it.**

This document is the whole of crux. Each rule has one line of reason beside it. The long form had the full
arguments and the history. It was retired after commit `27db6c6`, and `git show 27db6c6:docs/README.md` shows it.

This document uses ASD-STE100 Simplified Technical English. Each technical word has one meaning, and §2 defines it.

**Section numbers are stable.** A section number is the citation form in commits, trackers, and other
repositories. A missing number was merged into a different section or removed. A section marked _retracted_ tells
why.

**Changes from the long form:**

- **Rationale is not part of crux.** `@grounds` is deleted. See §11.
- **The word _marker_ is deleted.** An `@attests` block identifies a witness. See §5.1.
- **`@glossary` and `@kind` are deleted.** A glossary is a convention that crux does not see. The file that
  declares a claim is its group. See §3 and §3.4.
- **`@scope` is deleted.** Nothing declares a subject. A judge reads the diff and selects the inferential
  witnesses to ask again. See §5.5.
- **The format has two directives: one noun and one verb.** See §6.1.

**Status:** the model is stable enough to build. No code implements it yet.

---

## 1. Purpose

> Organise the requirements of a project so that it is cheap to find if the codebase satisfies them.

An agent can build the wrong thing. An agent can also build the right thing badly. These are the same failure:
somebody accepted the work, and nothing existed to measure it against. Crux makes that measure a first-class
artifact.

There is one artifact: the **catalog**. It tells what the codebase promises now.

## 2. Vocabulary

| Word           | Meaning                                                                                                   |
| -------------- | --------------------------------------------------------------------------------------------------------- |
| **claim**      | A short statement that the codebase satisfies now. It is falsifiable, and it has a stable slug.           |
| **catalog**    | The set of all claims in the repository.                                                                  |
| **witness**    | A part of the repository that can show if the codebase satisfies a claim.                                 |
| **directive**  | One `@name` and its token, on one line.                                                                   |
| **block**      | A contiguous sequence of directive lines.                                                                 |
| **extent**     | The lines that a block owns: from the block to its terminator, to the next block, or to the end of file.  |
| **attest**     | The relation that a witness records. Witness X attests claim Y.                                           |
| **subject**    | The code that a witness examines. The witness observes it and does not take its contract from it.         |
| **instrument** | The witness itself: its `@attests` block, its extent, and the text of the claim that it attests.          |
| **existence**  | If a claim has a witness. A form check finds it.                                                          |
| **verdict**    | What a witness tells about the subject: **affirms**, **affirms (unaffected)**, **denies**, or **silent**. |
| **standing**   | If one instrument supports one claim: **sound**, **unsound**, or **unaudited**.                           |
| **coverage**   | If the witnesses of a claim uphold it together: **covered**, **under-covered**, or **unaudited**.         |
| **amendment**  | The set of claim changes that one unit of work proposes.                                                  |
| **fog**        | Material that you want, but cannot yet write as a claim.                                                  |
| **canvass**    | To ask each witness for a verdict.                                                                        |
| **adapter**    | A converter from a verdict source into verdicts. The source is the report of a tool, or a judge.          |
| **judge**      | The intelligence that answers the inferential witnesses. It is the default adapter.                       |
| **audit**      | To read instruments and set their standings, and then set the coverage of their claims.                   |
| **auditor**    | The intelligence that does the audit.                                                                     |
| **readout**    | The result of a canvass and an audit: one block for each claim.                                           |
| **ruling**     | The decision that a human makes at the merge.                                                             |

### 2.1 The naming rule

Use a formal word for a **thing**. Use a plain word for an **event**. A person learns a thing once, so it gets a
precise word. The objects in a sentence already describe an event. Thus the operations of an amendment are
**add**, **change**, and **delete**.

### 2.2 Rejected words

Record these words so that nobody proposes them again.

| Word                   | Rejected because                                                                                                |
| ---------------------- | --------------------------------------------------------------------------------------------------------------- |
| **marker**             | It named the same thing as _witness_. One thing gets one word.                                                  |
| **rationale**          | The practice stays. Crux does not track it, because nothing mechanical reads it (§11).                          |
| **project**            | It was tried in three forms, and all three failed. The file that declares a claim is its group (§3.4).          |
| **run**                | You cannot run a prose witness.                                                                                 |
| **poll**               | A poll samples. A canvass is complete. _Poll_ is the fallback if _canvass_ is too easy to misspell.             |
| **passes** / **fails** | _The witness fails_ can mean that the code is bad or that the witness is bad.                                   |
| **ADR**                | An ADR holds the decision and the reasoning together. The catalog holds the decision.                           |
| **complete**           | This was a name for _covered_. No audit can prove that nothing is missing.                                      |
| `@witness`             | `@attests` already opens a witness, so this directive added no information.                                     |
| `@kind`                | Nothing used its closed set. The readout is in the order of the claims (§8.4).                                  |
| `@tag`                 | Free tags change without a signal. Declared tags need a registry. The declaring file is the group (§3.4).       |
| `@glossary`            | Its only function was a readout row. A new word in a glossary gave a row for many claims (§3).                  |
| `@scope`               | It was a manual list of the dependencies of a witness. A judgment replaces its one use (§6.5).                  |
| a bare `@end`          | It is a keyword in Objective-C and Texinfo. A stray one cuts a witness short. Each terminator names its opener. |

### 2.3 Tool names

The tools are **crux**, **belay**, **cairn**, and **beacon**. A tool name has no meaning in the vocabulary. Thus
you can rename a tool, and no migration is necessary.

## 3. The glossary — _retracted from crux_

The long form had a directive, `@glossary`. When a glossary changed, the readout showed a row. This directive is
deleted.

**The row was usually noise.** Add one word to a glossary, and each claim that can use that word gets a yellow
row. No claim changed its meaning. An operator learns to ignore a row like this, and an ignored row is worse than
no row.

**The practice stays as a convention.** A claim is falsifiable only if its words are clear. Thus, settle the words
before you write the claims that use them. Write the definitions. Select one word for each concept, and record the
rejected synonyms. Crux does not see this work.

**The cost of the deletion.** A builder can make a definition narrower, and a claim becomes easier to satisfy. No
claim and no witness changes, and nothing mechanical sees it. The reviewer reads the full diff, and the changed
definition is in that diff. If a definition change alters what a claim promises, the claim text must also change.
Changed claim text is already in the audit scope (§8.5).

### 3.4 There are no projects. The declaring file is the group

> **The location of a claim groups it. Its slug identifies it. Neither one depends on the other.**

A claim belongs to the file that declares it. A catalog file is an ordinary document. It has a heading, then prose
that tells what the file contains, then the claims. That prose is the charter of the group. A prefix cannot hold a
charter. A reader can compare a claim with the charter and decide if the claim belongs.

- **Each claim has one group, and no new check is necessary.** A slug is declared once. A second declaration is a
  _collision_ (§6.6).
- **Directories nest groups.** `catalog/billing/invoices.md` is a group in a group. Crux reports the path of the
  declaring file. It resolves nothing, and no check depends on the location of the file.
- **A move renames nothing.** Move a claim to a different file, and it moves to a different group. Each citation of
  its slug stays correct.
- **A slug prefix is permitted, and it has no meaning.** Authors will use prefixes to keep slugs unique. A
  repository can require that its prefixes agree with its files. It writes that as a claim with a lint witness.

**The three failed designs joined location and identity** (long form §3.4). A declared prefix put the group in the
identity, so a reorganisation renamed each slug. A position rule let the group control where a witness can be.
Thus a product in `packages/` and `apps/` could not share one group. Here, the group is only the location of the
declaration, and a witness can be anywhere.

**Incoherence is an old problem.** Two files that overlap are the same problem as two code folders that overlap.
Developers already solve it when they read the tree. A catalog that is tiring to read is evidence at the ruling
(§13.3). A prefix scheme can drift without a signal, because its claims are in many files. A file puts the full
group in front of one reader.

**A group across many files is a search, not a structure.** _All security claims_ is a query on the catalog. Each
design that gave a claim two structural groups needed a registry to keep them consistent.

## 4. Claims and the catalog

A claim is falsifiable prose with a slug. The slug is free-form and stable. The file that declares the claim is
its group (§3.4).

```md
> @claim core/token/cannot-be-guessed crux-ignore

Nobody can guess a session token from the information that the server publishes.
```

> **A claim is in the catalog only if its witnesses affirm it, each witness is sound, and together they cover it.**

A claim can fail in three places. A witness can deny it. A witness can support nothing. The set of witnesses can
be too small. Thus the condition has three parts.

- **A claim enters the catalog in the merge that makes it true.** It never enters earlier. There is no `pending`
  status. Intent is an amendment (§7) or fog (§10).
- **The condition holds by induction.** The claim satisfies it when it enters. Each merge keeps it (§8.5).

**Write a claim only if something can possibly violate it.** Each claim costs a witness, an audit, and a readout
row for all time. A claim that nothing can break has this cost and gives no protection. _We use React_ is a claim,
because somebody can add a Svelte component. _We use a monorepo_ is not a claim. This test has no mechanical form,
because the correct answer is frequently the unexpected one.

**A review rule is not a claim.** _Do not add unnecessary abstraction_ examines a change, not the codebase. The
base commit already has much abstraction, so the induction has no state to start from. Write it again as a state,
for example _no interface has exactly one implementation outside tests_. Or keep it in the [reviewer runbook](./runbooks/reviewer.md),
outside the catalog.

### 4.1 The subject must be rederivable from the repository

> **Crux examines the repository. If a checkout cannot show that a claim is true, it is not a claim.**

A judge can read only the diff (§5.5), because the repository is the only thing that changes between merges. A
claim about the live world stays green for all time. No diff touches it, but the world changes.

- **Declared infrastructure can be a claim. Live infrastructure cannot.** The limit moves with your tools. Adopt a
  framework that declares DNS records in the repository, and claims about those records become possible.
- **Runtime state, human actions outside the repository, and drift are never claims.** They are monitoring.
- **A command that connects to a service is a witness only if it runs fully locally.** Examples are a dry run and
  an emulator.
- **If the repository does not hold a value, make the claim more general.** If the domain is in configuration, do
  not claim _the sender is on the mail subdomain_. Claim _each sender address comes from the configured domain_.
  The general claim is also better. It finds a hardcoded address, which is the error that is likely to occur.

### 4.2 How the condition fails

| Failure                                                | Question  | Found by    | Result              |
| ------------------------------------------------------ | --------- | ----------- | ------------------- |
| no witness attests the claim — **unattested**          | existence | form check  | red — stop          |
| a witness **denies**                                   | verdict   | the canvass | red — stop          |
| a witness is **unsound**                               | standing  | the audit   | red — stop          |
| the claim is **under-covered**                         | coverage  | the audit   | red — stop          |
| each witness is **silent**                             | verdict   | the canvass | yellow — the ruling |
| a standing is unaudited, or proposed and not confirmed | standing  | the audit   | yellow — the ruling |
| coverage is unaudited, or proposed and not confirmed   | coverage  | the audit   | yellow — the ruling |

**A red item never goes to a human.** The work goes back to the builder. The readout that an operator gets has
only yellow items, and the ruling closes them.

## 5. Witnesses

### 5.1 A witness is an `@attests` block

> **A witness is the part of the repository that one `@attests` block and its extent identify.**

There is no registry and no witness identifier. Crux makes the index of witnesses again on each run. One block is
one witness. Two `@attests` blocks for one claim in one file are two witnesses.

- **Existence enforces itself.** Delete the test, and its block goes with it. The claim becomes unattested.
- **A witness needs no stable identity.** A diff already records a moved file.
- **A witness has two users.** An adapter changes it into a verdict. An auditor changes it into a standing.

### 5.2 The ladder

Move each claim as high in this list as it can correctly go.

|     | Rung      | Verdict from     | Reason for its position                                               |
| --- | --------- | ---------------- | --------------------------------------------------------------------- |
| 4   | type      | the type checker | It reaches each use, new uses also. It can require and it can forbid. |
| 3   | test      | the runner       | It observes behaviour. It has a failing state before the build.       |
| 2   | lint rule | the linter       | It reaches each file, new files also. It can only forbid.             |
| 1   | prose     | a judge          | It is expensive to ask. It is only as good as the judge who reads it. |

Types, tests, and lint rules are **computational**: a tool answers them. Prose is **inferential**: a judge
answers it.

**A test is above a lint rule because of polarity.** A lint rule removes one way to fail. It cannot affirm the way
to succeed (§5.8). A test can do both. But a lint rule examines each file, and this includes files that people add
later. A test usually does not. Thus a claim that must hold for new code frequently needs a lint rule **and** a
test.

**A type is at the top because it has both properties.** It reaches each use, as a lint rule does. It can require
a shape, as a test does. If a type system can encode a behaviour, _it compiles_ becomes a verdict about that
behaviour.

> **A witness that no tool adapter reports on is inferential.**

Crux cannot identify a test or a request handler, and it does not need to. The source that answers sets the rung.
Nothing declares it. An adapter reports verdicts for witnesses, never for files (§8.2). The judge gets each
witness that no adapter reports on.

**A type witness must bind its subject.** A `ReadOnlyDb` type proves nothing if the dashboard can still get `Db`
directly. The project compiles, and the witness affirms, but it tells nothing. A type has no failing state before
the build. Thus the builder breaks it: the builder adds the write that the type forbids, and makes sure that the
build fails.

- **Your ecosystem and your budget set the top of the ladder, and the claim does not set it alone.** A library
  with type-aware lint rules raises the top for all claims at once. Ask _can a rule exist here_, not _does a rule
  exist_. A custom rule can be forty lines.
- **A static witness needs a declarative subject.** Nobody can read an imperative deploy script for the state
  that it makes. If a claim stays at prose, ask if you can make its subject declarative.

An inferential witness names its target and tells when it is valid. It can be a witness file:

```md
> @attests report/reads-at-a-glance crux-ignore

Run `demo report --fixture test/fixtures/mixed-status`. The rendering code is in `src/report/`.

Valid when: a reader can tell a failing row from a skipped row without the labels.
```

It can also be on the code that it examines. This is the correct first rung for a codebase that has no tests:

```ts
/**
 * @attests checkout/never-charges-an-expired-cart    crux-ignore
 *
 * Valid when: the handler compares the expiry with the server-side timestamp
 * before it makes the payment client, and an expired cart returns 409 with
 * no call to payments.
 */
export async function checkout(req: Request) { … }
// @attests:end    crux-ignore
```

**Write the condition, not only the assertion.** _This endpoint satisfies the claim_ gives an auditor nothing to
examine. The _Valid when_ line is the instrument. It also tells where to look, and the triage of the judge reads it
(§5.5).

**The upgrade is one merge.** When a test exists, add its `@attests` and delete the inline block. The claim does
not change, and the readout shows the higher rung.

### 5.3 Subject and instrument

> **The subject is what a witness observes and must not consult.**

Nothing declares a subject, but the subject is real. The rule for a sound witness defines it (§5.6). The witness
uses the subject, and it takes none of its contract from the subject.

> **The verdict is about the subject. The standing and the coverage are about the instrument. Only a change to an
> instrument or to the text of a claim opens a standing or a coverage again.**

A change to the subject asks the verdict again, and does nothing more. This is the TDD position, and it is true for
each rung. If the witness still affirms and the instrument did not change, the audit stays valid.

- **A witness in its subject follows the subject automatically.** The extent of an inline witness is the code that
  it examines. Thus an edit to that code is an edit to the instrument, and the auditor examines the standing again.
  The cost is an audit after each such edit.
- **A standing belongs to a witness and a claim together.** A witness that attests three claims has three
  standings. To repair one bad pair, remove one `@attests`. Do not remove the witness.
- **Tell where the observation occurs.** A test can observe a wrapper and pass. A scheduler that changes each
  failure into success makes _the job completed_ true when the send was refused. A witness that does not name its
  point of observation has not told what it observes.

### 5.4 The four questions

| Question      | Answered by         | Cost               | Made void by                                                            | Default                           |
| ------------- | ------------------- | ------------------ | ----------------------------------------------------------------------- | --------------------------------- |
| **existence** | a form check        | free               | nothing — crux calculates it on each run                                | calculated each time              |
| **verdict**   | an adapter or judge | free, or expensive | computational: each canvass. Inferential: a diff that the judge selects | **asked**; triaged if inferential |
| **standing**  | an auditor          | always expensive   | a change to the instrument or to the claim text                         | **kept**; audited after a change  |
| **coverage**  | an auditor          | always expensive   | a change to one of the instruments of the claim, or to its text         | **kept**; audited after a change  |

A computational verdict is cheap, so each canvass asks it. An inferential verdict is expensive, so a judge does a
triage of the diff first. A standing and a coverage stay the same until a change, because only an intelligence
can set them.

### 5.5 The verdict

| Verdict                  | Colour | Meaning                                                             |
| ------------------------ | ------ | ------------------------------------------------------------------- |
| **affirms**              | green  | Somebody asked the witness, and the subject satisfies the claim.    |
| **affirms (unaffected)** | green  | A judge read the diff and found that it cannot change this verdict. |
| **silent**               | yellow | Nobody answered. No tool ran the witness, or no judge did a triage. |
| **denies**               | red    | Somebody asked the witness, and the subject does not satisfy it.    |

> **By induction, each verdict on the main branch is _affirms_. Thus a judge reads only the diff.**

The condition of §4 was true at the merge. Thus the question for the judge is: can **this diff** change the
verdict? For a small diff, this is cheap. A good code reviewer already works this way. Crux stores no verdicts.

**Triage is one pass.** The judge gets the diff and the _Valid when_ line of each inferential witness. It returns
the witnesses to ask again. The other witnesses affirm, unaffected. Then the judge answers the witnesses that it
returned.

**The judge asks if HEAD satisfies the claim, because the base did.** It never asks if the diff is acceptable
alone. The two questions give different answers when small changes add up. Twenty diffs can each be acceptable,
and together they can break the claim. A judge that examines each diff alone starts again at zero each time.

Three rules keep the triage correct:

- **The reviewer does the triage. The builder never does it.** _Unaffected_ is how a builder can avoid a witness
  (§8.6).
- **The readout shows each result.** _Affirms (unaffected)_ and _affirms_ are different lines, so an operator can
  see a wrong _unaffected_ quickly. It needs no confirmation. A confirmation step puts a yellow row on each witness
  in each PR.
- **A judge can refuse a diff that is too large for a correct triage.** Then each inferential witness is silent.
  This is safe, and it is a reason to keep PRs small.

A computational witness never gets a triage. To ask it is cheaper than to decide if you must ask it. Only a
skipped test is silent.

### 5.6 The standing

| Standing      | Meaning                                                                         |
| ------------- | ------------------------------------------------------------------------------- |
| **sound**     | An auditor read the instrument, and it supports this claim.                     |
| **unsound**   | It does not support this claim. Remove the `@attests`, repair it, or delete it. |
| **unaudited** | Nobody has read it against this claim.                                          |

> **A sound instrument states its contract. It does not derive its contract from its subject.**

This rule makes the induction of §8.5 safe. If the contract of a witness is its own, a change to the subject
cannot move it. Thus a subject change only asks the verdict again. If the contract comes from the subject, the
subject can make the witness weaker, and each verdict stays green.

```ts
// Derived: Db gets `insert`, and the witness becomes weaker.
type ReadOnlyDb = Omit<Db, "write">;

// Stated: no new method of Db can go through it.
interface ReadOnlyDb {
  read(q: Query): Row[];
}
```

Tests can fail in the same way. Examples are a snapshot that you record again after each output change,
`expect(f(x)).toEqual(f(x))`, and a fixture that you make again from the current output. Each one takes its
expected value from the thing that it examines.

The auditor examines three things for each standing:

- **Does it support the claim?** This is the usual question.
- **Is its contract its own?** Nothing that it asserts comes from the subject.
- **What does the instrument depend on?** A shared test helper or a type utility is part of the meaning of the
  instrument. It is outside the extent. Name it, and decide if it is stable enough to trust (§8.5).

**Sound does not mean sufficient.** A sound witness supports its claim, but it can reach only part of the claim.
Coverage asks about the remaining part.

**An agent can set _unsound_ alone. An agent only proposes _sound_, and a human confirms it.** To find a bad
witness needs no authority. To say that a witness is good enough needs authority.

### 5.7 Witnesses that attest several claims

**Each claim needs a minimum of one witness that attests it alone.** If not, its verdict stays tied to a different
claim. A shared witness is acceptable as an addition. It costs one standing for each claim, and a change opens all
of them again. Thus share a witness only between claims that succeed and fail together.

### 5.8 Coverage

> **Coverage is if the witnesses of a claim uphold it together. If each witness is sound, the claim is not
> necessarily covered.**

| Coverage          | Meaning                                                                         |
| ----------------- | ------------------------------------------------------------------------------- |
| **covered**       | An auditor read the witnesses together, and they reach the full claim.          |
| **under-covered** | No witness reaches part of the claim. Add a witness, or make the claim smaller. |
| **unaudited**     | Nobody has read the witnesses together.                                         |

**A witness that removes one way to fail does not affirm the way to succeed.** A lint rule that forbids
`Math.random` is sound for _nobody can guess the token_. But a weak generator that somebody writes by hand passes
it. Add a second witness with the opposite polarity: a test that observes the real path get its bytes from the
approved source. Do this check first on a claim whose witnesses are all prohibitions.

**If a witness reaches more than its claim, the claim is too small.** Repair the claim.

An agent can set _under-covered_ alone. It only proposes _covered_. Coverage has no mechanical form, and it will
never have one.

### 5.9 Altitude

> **Group claims by the failure that a reader sees. Do not group them by the check that finds it.**

Two properties are two claims when they can fail separately **and** a reader sees two different things. A weak
generator, a `Math.random` call, and a short token are three defects. They give one visible failure: _somebody can
guess the token_. Thus they are one claim with three witnesses.

**A claim that describes its own witness is a witness with the name of a claim.** _The configuration is a context
service_ describes an instrument. No operator has an opinion about it. A catalog that nobody can rule on does not
do its function.

**Two forces set the altitude.** This section moves a claim up, to what a reader sees. Coverage moves it down, to
what the witnesses reach. A claim above the balance promises what nothing checks. A claim below it describes its
own witness.

## 6. The format

### 6.1 Directives and blocks

> **A directive is `@name`, then whitespace, then exactly one token with no whitespace. Crux ignores the remaining
> text on the line.**

Thus the core never learns the comment syntax of a language. It ignores `*/`, `-->`, and `#`, and it does not
know what they are.

```ts
/** @attests closing/ordered-not-atomic    crux-ignore */
describe("close", () => { … })
```

There are two directives: one noun and one verb.

| Directive         | Shape    | Token                   | Repeatable |
| ----------------- | -------- | ----------------------- | ---------- |
| `@claim <slug>`   | **noun** | one slug                | no         |
| `@attests <slug>` | **verb** | slugs (comma-separated) | yes        |

Each directive has a terminator: `@claim:end` and `@attests:end`.

> **The claim is the only noun, because it is the only thing with a stable identity.**

A noun binds a name. `@claim` binds a slug to the prose below it, and all other artifacts cite that slug. A verb
refers to a name that is already bound. `@attests` is a verb, because a witness has no identity to bind (§5.1). A
witness is a relation from a part of the repository to a claim. A name for each witness costs one more line on each
witness for all time. It also makes a registry for a thing that a diff already records.

The same rule sets which directive can repeat. A verb repeats, because one witness can attest several claims. A
noun does not, because one body of prose cannot be two claims.

- **A block is a contiguous sequence of directive lines.** The first line with no directive ends it. Several
  `@attests` lines in one block are one witness of several claims.
- **A block with both directives is a form error.** Nothing can tell what it is.
- **A token that contains `<`, `>`, or a backtick is not a directive.** This rule protects prose that explains the
  format. This includes the `AGENTS.md` of each repository that uses crux.
- **A line that contains `crux-ignore` has no directive.** This rule applies to one line. It is case-sensitive,
  and it is unusual on purpose, because it is the only permitted way to hide a real directive.

### 6.2 The extent

> **A block owns the lines from itself to its terminator, to the next block, or to the end of the file.**

| Block opened by | Its extent is  |
| --------------- | -------------- |
| `@claim`        | the claim text |
| `@attests`      | the instrument |

**An extent that is too large is safe. An extent that is too small is not safe.** Too large causes more audits. Too
small lets an unsound witness stay. Thus the large default is never wrong, only expensive, and a terminator is
optional. Use `@attests:end` where one witness is among lines that no witness owns, for example one rule in a lint
configuration.

**There is no witness for a full file, and blocks do not nest.** A line scanner cannot find "before the code"
without the comment syntax. When a claim covers a full file, write the slug on each block.

### 6.3 Markdown, and files with no comments

Markdown uses the same rule. Use the blockquote form, `> @claim slug`, because it is visible when rendered. An
HTML comment hides the slug that a reader must cite. Do not put a directive in a code span: the closing backtick
becomes part of the token. An example in a fence is a real directive, so add `crux-ignore` to it.

For a file that takes no comments, such as JSON, write a witness file that names it. First, examine the parser:
`tsconfig.json` is JSONC, and it takes comments.

### 6.4 What is not a directive

> **A directive exists only for what the core must resolve without intelligence. All other text is prose.**

There is no `@run`. The core runs nothing, and an agent reads a command as prose. The condition, the reason, and
the rejected option are prose for the same reason.

### 6.5 Scope — _retracted_

The long form had `@scope <globs>`, which named the subject of a witness. It is deleted.

**A judgment replaces its one mechanical use.** Crux used `@scope` to keep an inferential verdict when the diff
touched no glob. Now the judge does a triage of the diff (§5.5). Thus, by §6.4, the directive has no function. Its
other users were the reading list of the auditor and the claims that a diff affects. An intelligence always read
those, and the prose of the witness supplies them.

**A manual dependency list was the wrong instrument.** The code already tells what it depends on. A list beside
the code becomes incorrect in the unsafe direction: a helper that nobody listed changes, and the verdict stays
green.

**The cost of the deletion.** A kept verdict was arithmetic. _Affirms (unaffected)_ is a judgment. A wrong
judgment is quiet, but people examine a wrong verdict. Thus the readout shows each result of the triage. The
_dead scope_ form error is also deleted.

### 6.6 Form errors

A machine finds all of these errors with no tool, no language, and no intelligence. **The person who caused a form
error must be able to repair it when they cause it.**

- **unattested** — a claim that no witness attests.
- **orphaned** — an `@attests` that names no claim.
- **mixed** — a block that has both directives.
- **collision** — two declarations of one slug.

**Crux resolves slugs. It never resolves a path.** Each check compares one directive with a different directive.

**A rename does not change each mention of a slug.** A _citation_ names a claim that exists now, and it must move.
A _record_ names a slug as it was at a time in the past, for example the claims that an enacted amendment deleted.
It must not move. No mechanical property shows which is which. Thus a rename tool shows each mention, and a person
marks the limit.

## 7. The amendment

An amendment is the set of claim changes that one unit of work proposes. Its operations are **add**, **change**,
and **delete**. An add names its claim **and** the witness that will attest it. A delete removes the claim and its
witnesses in one merge.

**The design occurs when you name the witness.** _The dashboard never writes_ is only a wish until you ask what
will examine it. The answer is a database handle with no write method, and that answer changes the design.

**An amendment is a specification. It does not stop changes.** When you write the witness, you complete the claim.
This frequently shows that the claim did not say what it meant. This is correct, and the builder escalates it
(§9.1).

- **7.1 Set coverage while the amendment is still text.** An amendment that regroups witnesses makes new sets of
  witnesses. Early, this costs one paragraph. After the build, it cost two audit rounds and rework.
- **7.2 Read the amendment as one thing.** Two claims can each be correct and be impossible together. All other
  checks examine one claim at a time. The contradiction usually shows first in the coverage prose.
- **7.3 Name the seam, and tell what it replaces.** The production work that a claim needs has no artifact. _This
  has no claim_ does not mean _this is small_.
- **7.4 Tell when a witness is written before its subject.** A witness can deny a name that a later amendment will
  make. The witness cannot tell this, so the amendment tells it.
- **7.5 The amendment stays proposed until the merge.** The merged code shows what somebody built. Only the
  amendment shows what somebody wanted.

## 8. The canvass, the readout, and the audit

### 8.1 The canvass

To canvass is to ask each witness for a verdict. The tool adapters answer the computational witnesses, and the
judge answers the remaining witnesses. A canvass is complete. It asks, and it does not execute. No answer is a
normal result, and that result is silence.

### 8.2 The join

> **An adapter reports `(witness, verdict)` pairs. It never reports on a file.**

An adapter changes the report of one tool into verdicts for the witnesses that crux indexed. Adapters are in
belay, never in the core. The join uses a handle that the tool reports and the extent of the witness contains:

| Rung      | Handle              | The extent of the witness contains     |
| --------- | ------------------- | -------------------------------------- |
| type      | the full project    | a type declaration                     |
| test      | file and line range | the test that the runner ran           |
| lint rule | the rule id         | the configuration line that enables it |
| prose     | file and line range | nothing to join — the judge answers    |

**The judge gets each witness that no adapter reports on.** The judge is the default adapter (§5.2).

**A tool that runs on a file does not give a verdict for that file.** A linter and a type checker run on each
file. If that were a verdict, they would report on each inline witness. Each witness would then affirm when the
project compiled. Thus a type adapter reports only on witnesses whose extent contains a type declaration. Adapters
know the language, so they make this decision.

**The type join.** A type error occurs where the subject breaks the type, not in the extent of the type. Thus no
error joins to one witness. If the project compiles, each type witness affirms. If it does not compile, each type
witness denies. This is over-attribution, which is permitted. A failed type check stops the work in any case.

**The test join.** A result belongs to the witness whose extent contains its line. A failure denies. All passed
affirms. A skip with no failure is silent. The join needs only file paths and line numbers.

**A lint rule that reports nothing affirms. A test that does not run is silent.** This is the only rule that is
different for one rung, and it assumes that the linter ran the rule. Thus:

> **An adapter reports if the instrument ran, not only what it said.** A rule id that the tool does not recognise
> is silent, or worse.

**Over-attribution is permitted. Under-attribution is not.** A false red costs a builder some minutes. A false
green is the failure that crux exists to prevent.

### 8.3 A lint witness

The `@attests` block is on the rule, or on the configuration line that enables it. Delete the line, and the claim
is unattested. Set it to `warn`, and the instrument changes, so the auditor sets it to unsound. Violate the rule,
and the adapter reports a denial. **Do not add a test that asserts that the rule is enabled.** The position of the
block already does this.

### 8.4 The readout

The readout has one block for each claim, **in the order of the claims**. The claim and its coverage are first.
Then there is one line for each witness.

```
core/token/cannot-be-guessed                                        covered (proposed)
  token.test.ts:14                    web crypto bytes     affirms                 sound
  vite.config.ts:31                   no Math.random       affirms                 sound

report/reads-at-a-glance                                                         covered
  witnesses/report-legibility.md:1    the summary view     affirms (unaffected)    sound

checkout/never-charges-an-expired-cart                                           covered
  src/api/checkout.ts:12              expiry before charge affirms                 unaudited
```

The third block is an inline witness. The diff changed the handler, so the judge asked the witness again. The edit
also changed the instrument, so the standing waits for the audit.

The operator gets the amendment and the readout together. The amendment tells what somebody wanted. The readout
tells what the witnesses say.

### 8.5 The audit

> **Audit scope** = witnesses whose instrument changed in this diff **∪** witnesses that attest a claim whose text
> changed in this diff.

The first term is the most important. A builder that adds a test to a claim outside the amendment sets its own
measure of success.

**Crux compares claim text by slug, not by position.** The text of a claim changed only if the text under its slug
is different between the merge base and HEAD. A claim that moves to a different file is a deletion and an addition
in the diff. This must not put each witness of that claim in the audit scope.

- **Coverage makes the reading larger, not the scope.** To set the coverage of a claim again, the auditor reads
  each of its witnesses, changed or not. It sets standings only on the changed witnesses.
- **The invariant holds by induction.** Each merge audits its scope and changes nothing more. Thus each witness on
  the main branch that no diff touched is presumed sound. Each claim that no diff touched is presumed covered.
  There is nothing to store and nothing to find.
- **One pass is not always sufficient.** A repair changes an instrument, and that instrument is in scope again.
  Plan for two rounds.
- **Do not add a `last audited` directive.** A date cannot tell if the instrument changed.
- **The base case.** In an existing repository, all witnesses start unaudited. The readout shows this in yellow.
- **The accepted blind spot.** The dependencies of an instrument, for example a shared test helper or a type
  utility, are outside its extent. A dependency can change its meaning, or do nothing, and the instrument does not
  change. The auditor names these dependencies when it sets a standing (§5.6). The reviewer sees a diff to one of
  them. Crux does not track them. An import graph cannot separate the dependencies of the instrument from the
  dependencies of the subject. To audit again after each subject change is what §5.3 forbids.

### 8.6 Why the canvass and the audit stay separate

The canvass asks the subject. The audit asks the instrument. A builder that writes its own witness sets its own
measure of success. Thus the canvass alone has no value. A builder that does the triage of its own inferential
witnesses also selects which witnesses somebody asks. The first real build passed each test and the repository
gate. Then an independent audit set six claims to red.

> **Somebody who did not build applies the gate.**

## 9. Belay

### 9.1 The sequence

1. A human operator gives belay an amendment.
2. Belay tells a **builder** to implement it.
3. **At any time, the builder can escalate.** It tells the proposed change to the amendment, and it stops. The
   operator accepts or refuses the change, and the build continues.
4. Belay tells a **reviewer** to canvass and to audit. The result is the readout.
5. Belay gives the amendment and the readout to the operator for the ruling.

If a builder escalates on each claim, it got fog, not an amendment.

### 9.2 The builder hands off when the canvass is green

> **The builder hands off when the canvass is green.**

Green is a handoff. It does not mean that the work is done. Before the handoff, the builder breaks each witness
that can deny, and makes sure that it denies. A witness that has never denied has not been tested.

### 9.3 The supervisor

The supervisor holds a loop and no facts. It calculates each fact again from the branch at HEAD, so a restart
loses nothing. It starts a **new reviewer for each cycle**. A reviewer with context from the last cycle has
already heard the arguments of the builder. The supervisor also enforces a **cycle budget**.

### 9.4 Where the readout goes

The amendment goes in the PR description. Each readout goes in a PR comment that names its commit. A readout
whose commit is not HEAD is void. The machine form is a build artifact. Never commit it.

### 9.5 What the human decides

The human makes a specification decision: **were these the correct claims?** The human closes the yellow items:
silent verdicts, standings that nobody confirmed, and coverage that nobody confirmed. Only the operator can say
that the witnesses reach a claim far enough.

## 10. Fog

Fog is material that you want but cannot yet write as a claim. It is not a verdict and not a colour, because it is
not in the catalog.

> **Fog is clear when you can write the claim and select its witness.**

| State                  | Can you write the claim? | Held by | Exit                                       |
| ---------------------- | ------------------------ | ------- | ------------------------------------------ |
| **fog**                | no                       | cairn   | examine the checkout, or examine the world |
| **proposed amendment** | yes                      | branch  | the merge                                  |

- **A fog item records what will clear it.** One search and a temporary deployment have very different costs. This
  field lets you sort the queue.
- **Fog is inability, not unwillingness.** A claim that you can write is an amendment that you did not write yet.
  _Not now_ makes no artifact.
- **Fog can clear into nothing.** It can become a decision that nothing can violate (§4). It can also become a
  question that an existing claim already answers. In both cases, it closes, and nothing enters the catalog.

## 11. Rationale — _retracted from crux_

The long form had a rationale document and a `@grounds` directive that linked it to claims. Both are deleted from
crux.

**Nothing mechanical read them.** By §6.4, a directive exists only for what the core must resolve without
intelligence. The readers of a rationale are the operator at the ruling and the author of an amendment. They are
intelligences, and they can search for a slug. The check for a `@grounds` with no claim examined only `@grounds`
itself. The report for a grounded claim that was deleted was the only independent value, and it was small.

**The practice stays as a convention.** Write the reasoning beside the thing that it supports. Name the rejected
option, because the code cannot show it. Write a rationale only for a decision that is hard to reverse, surprising
without the reasoning, and the result of a real trade-off. Crux does not see this work.

## 12. The tools

The format is the product. You can replace the tools.

|      | Name       | Does                                                            |
| ---- | ---------- | --------------------------------------------------------------- |
| core | **crux**   | reads claims and witnesses; reports form errors                 |
| 1    | **belay**  | takes an amendment; makes a PR that is cheap to rule on         |
| 2    | **cairn**  | holds fog and amendments that are not enacted; gives amendments |
| 3    | **beacon** | moves the operator between fog work and rulings                 |

**Crux depends on none of the other tools.** The format goes into repositories that you do not control. Thus a
change to the format is a migration that you cannot run.

**A tracker stores slugs. It never stores claim text.** The person or tool with the checkout resolves the slug.
The interface is one thing: a tracker gives an amendment.

### 12.1 Cairn keeps its state outside the repository

An amendment exists before a branch exists. Fog in the repository adds noise to its history. The cost is that a
slug rename does not change the tracker at the same time. Thus cairn runs a **watcher**. The watcher gets the main
branch, runs crux, and warns about incorrect references in the records of cairn. It holds no facts. It uses the
machine-form index of crux. It never writes into the repository.

### 12.2 Cairn holds the work that changes the witness supply

When you adopt a type-aware linter or a declarative infrastructure framework, no claim changes. But the set of
**possible** claims changes. Cairn records the plan. When the change merges, cairn proposes a list of claims and
fog to examine again. It never rewrites anything.

## 13. Build order

**1. Crux.** It runs nothing and stores nothing. It:

- lists claims with their witnesses, grouped by the file that declares them
- lists witnesses with their claims and extent
- reports the form errors of §6.6
- gives the witness index as a machine form, for adapters and for cairn
- has a one-page summary of the format, for agents to read at the start of a session

**2. A belay MVP.** It runs the sources that `.belay/witnesses.toml` names. It joins their reports to the witness
index, makes the readout, and posts it to the PR. It runs the builder and a new reviewer in a loop, within a
budget.

**Cairn comes after belay.** Belay makes the catalog useful. Cairn makes it easy to use.

### 13.3 The dogfooding rule

While you build tools 1 and 2, record each problem when it occurs. A manual step that feels **clerical** is a
missing feature. A manual step that feels like **thinking** must never be automated. Two items in this second
group are settled: _is this worth a claim?_ (§4) and _is this claim covered?_ (§5.8). A complaint that a catalog is
tiring to read is evidence about its altitude.

## 14. The properties

Do not renumber this list. Add new properties at the end.

1. Nothing in the repository is built without a claim, and nothing merges before somebody measures it against one.
2. A claim is complete when something that is not you can falsify it.
3. Fog has a name, somebody clears it on purpose, and it records what will clear it.
4. Use artifacts that are clear in both directions: guidance in, and feedback out.
5. Evidence has an index by claim. Tools make it, and nobody collects it by hand.
6. Spend human attention only on authority: set a claim, rule on a result, stop the work. Never spend it on
   status.
7. Easy re-entry is more important than deep autonomy.
8. Durable state is plain files. Each tool calculates its view and stores nothing.
9. The machine checks form. A human or a model checks truth.
10. Somebody who did not build applies the gate.
11. Over-attribution is permitted. Under-attribution is not.
12. A directive exists only for what the core must resolve without intelligence.
13. Settle the words before the claims that use them. This is a convention, and crux does not check it (§3).
14. _Retracted._ The catalog holds the decision. The reasoning is a convention that crux does not see (§11).
15. A claim is in the catalog only if something can possibly violate it.
16. Crux examines the repository. If a checkout cannot show it, it is not a claim.
17. A witness that removes one way to fail does not affirm the way to succeed.
18. A claim names a failure that a reader can see. A claim that describes its own witness has the wrong altitude.
19. If each witness is sound, the claim is not necessarily covered.
20. A witness that has never denied has not been tested.
21. A witness that observes a proxy for its subject tells nothing about the subject.
22. Read an amendment as one thing. Its claims can each be correct and be false together.
23. The ecosystem and the budget set the top of the witness ladder. The claim does not set it alone.
24. Only a change to an instrument or to the text of a claim opens an audit again. A change to the subject asks
    the verdict again.
25. A review rule examines a change. A claim examines the codebase.
26. A witness states its contract. It never derives the contract from its subject.
27. The location of a claim groups it. Its slug identifies it. Neither one depends on the other.

## 15. Open threads

- **How frequently the triage of a judge is wrong.** This blocks nothing. A wrong _unaffected_ is quiet, and the
  readout only makes it visible. Record each one that you find. If it occurs again, add a mechanical minimum under
  the triage.
- **How the audit scope gets its diff base.** This blocks belay. The merge base is the clear answer, and it needs
  git.
- **How a new lint rule denies before the build.** A new rule denies on code that nobody touched. Two options are
  an allowlist, or to merge the rule with the refactor.
- **The spelling of _canvass_.** This blocks the command names of belay. **Poll** is the fallback.
- **Beacon.** It has no owner, and it adds no data model.

## 16. Under watch

An observation that occurred **once** is an anecdote. One occurrence changes nothing, and a second occurrence is
evidence (§13.3). This section keeps single observations, and it does not make them rules.

**Nothing here is a rule.** An entry leaves this section in one of three ways. It occurs again and becomes a rule.
Somebody finds that it is wrong, and it is deleted with a line that tells why. Or it stays here, because it
occurred once and never again. Some entries are already steps in the runbooks. A runbook step costs one question,
so one observation is sufficient for it.

| #   | Observation                                                           | Seen                       | Now                           |
| --- | --------------------------------------------------------------------- | -------------------------- | ----------------------------- |
| W1  | a tool requires a credential that no observation uses                 | one tool, one provider     | argued by hand                |
| W2  | a witness needs a working directory that nothing gives it             | one test harness           | a comment, and a fragile line |
| W3  | an exactly-once claim hid a commit point between two stateful systems | one external side effect   | a reviewer runbook step       |
| W4  | somebody added a mechanism, argued for it, and nothing observed it    | one amendment, three cases | a reviewer runbook step       |
| W5  | an amendment has no version                                           | three amendments           | recorded only                 |
| W6  | one prose sentence became two claims at two altitudes                 | one sentence               | recorded only                 |

**W1 — A tool can run a local witness, but it refuses to start.** An infrastructure framework reads cloud
credentials when it makes its client. This occurs before it knows that each resource is emulated. With no
credentials, the witness fails with an authentication error before it runs. Four lines of placeholder environment
values repaired it. The question for §4.1: is a witness still rederivable from a checkout if it requires a
credential that it never uses? The answer taken was yes, because no observation depends on the value. This becomes
evidence if a second tool causes the same problem.

**W2 — A witness can need something from the process, not from the program.** A test deploys two stacks from a
third directory, and a relative path in a resource declaration then points to nothing. The only repair was to
change the working directory at the start of the test module. The line is easy to delete by accident. A
dependency system models what a witness needs in the program. It models nothing about the process: working
directory, environment variables, or the temporary directory of the run.

**W3 — Exactly-once language hid a commit point that nobody can observe.** A claim promised a maximum of one send
for each local date, and each witness affirmed it. An audit asked what is true between the send and the write that
records it. They share no transaction, and the send has no idempotency key. A send that succeeds before a failed
write looks the same as a send that never occurred. The repair was to make the claim smaller (§5.8). This becomes
evidence if it occurs again.

**W4 — Somebody added a mechanism, argued for it, and nothing observed it.** A fix added a per-attempt identifier:
a migration, a unique index, and a generated value. No witness named it. A constant string in place of the
generated value passed all twenty-three tests. The audit goes from claims to witnesses. Nothing goes in the other
direction to ask _what here is load-bearing and not observed_. If it occurs again, make it a step in the model, not
only a runbook step.

**W5 — An amendment has no version.** Three amendments were written under a rule that was later retracted, and
nobody enacted them. Nothing marked them as old. Somebody remembered, and revised them three sessions later. This
stops being important when crux becomes stable.

**W6 — One prose sentence, two claims, two altitudes.** One sentence of a design document became two claims at
different altitudes. Neither claim implied the other, and no single witness covered both. One occurrence tells
nothing yet about the correct altitude.
