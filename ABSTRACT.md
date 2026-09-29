# Crux — claims, witnesses, and the ruling

**Crux is a format that records what a codebase promises. A machine can find each promise. A person can then decide if the work satisfies it.**

The documentation uses ASD-STE100 Simplified Technical English. The full model is in [the specification](./spec/).

---

## 1. What the system is for

> Organise the requirements of a project so that it is cheap to find if the codebase satisfies them.

Two failures cause this work. An agent builds the wrong thing. An agent builds the right thing badly. These are the same failure: somebody accepted the work, and nothing existed to measure it against. The repair is to make that measure a first-class artifact.

## The artifact

There is one artifact: the **catalog**. It tells what the codebase promises **now**. A claim enters the catalog in the merge that makes it true, and never earlier.

The catalog is ordinary Markdown. Claims sit in claims blocks, and the **prose** around them tells what a part of the product does and why somebody chose it. Prose never promises, so crux reads it only to keep it out of the way. A **glossary** settles the words of the claims. It is a convention, and crux does not check it.

## The mechanism, in one pass

A **claim** is a short falsifiable statement: one heading in a `[!CLAIMS]` block and the paragraphs under it. Its slug is the path of its file and the slug of its heading, for example `catalog/sending.md#retry`, and an ordinary Markdown link `[Retry](catalog/sending.md#retry)` cites it. A **witness** is a part of the repository that can show if the codebase satisfies a claim. An `@attests` block identifies a witness. There is no registry and no identity to migrate. Delete the test, and its block goes with it.

Crux asks three questions about each claim. They are different questions on purpose.

| Question     | Asks                                                  | Answered by            |
| ------------ | ----------------------------------------------------- | ---------------------- |
| **verdict**  | does the code satisfy the claim?                      | a tool, or a judge     |
| **standing** | does this instrument support this claim?              | always an intelligence |
| **coverage** | do the witnesses of the claim **together** uphold it? | always an intelligence |

A tool asks each computational witness again on each canvass. A judge reads the diff and asks again only the inferential witnesses that the diff can affect. A standing and a coverage change only when an instrument or a claim text changes. The subject never makes them void.

To **canvass** is to ask each witness for a verdict. To **audit** is to read the instruments and set the standings and the coverage. The two acts make one **readout**, and a human makes the **ruling** at the merge. Somebody who did not build applies the gate.

Work that changes what the catalog promises is an **amendment**. Material that you want but cannot yet write as a claim is **fog**. You clear fog. You do not collect it.

## What crux refuses to do

The refusals are load-bearing. Each one keeps a cost out of the core.

- **It never learns the comment syntax of a language.** A directive is a name, whitespace, and one token with no whitespace. The core is a line scanner.
- **It never reads the file system.** Each check compares one thing that crux read with a different thing that it read. A path resolves against the list of files that git tracks. There are no projects: one repository, one catalog, and slugs made from a path and a heading.
- **It has one directive.** Markdown structure declares a claim. `@attests` is the only directive, and it is a verb.
- **It runs nothing and it stores nothing.** It calculates each index again on each run.
- **It examines the repository only.** If a checkout cannot show a claim, it is not a claim.

## Status

The vocabulary and the mechanism are stable. No code implements them. The specification is written in its own format, so each of its claims is unattested until crux exists. It tells what gets built first ([Build order](spec/tools.md#build-order)). Open questions and single observations are notes beside the rules that they question.

This revision comes from a project outside crux, the first to use the vocabulary. Some parts of the model exist because somebody tried a different design and it failed. Other parts exist because that project **built** its design, and the build corrected the design.
