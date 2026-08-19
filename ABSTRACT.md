# Crux — claims, witnesses, and the ruling

**Crux is a format for writing down what a codebase promises, so that a machine can find every promise and a
person can rule on whether the work satisfies them.**

The documentation is written in ASD-STE100 Simplified Technical English. Start at
[the contents](./docs/README.md).

---

## 1. What the system is for

> Organise the requirements of a project so that it is cheap to judge whether the codebase satisfies them.

Two failures motivate this. An agent builds the wrong thing. An agent builds the right thing badly. Both are the
same failure: work was accepted, and there was nothing to judge it against. The repair is to make the thing that
work is judged against into a first-class artifact.

## The three artifacts

Each answers a different question, and none of them answers another's.

| Artifact      | Says                                                  | Lives in                                      |
| ------------- | ----------------------------------------------------- | --------------------------------------------- |
| **catalog**   | what the codebase promises **now**                    | [claims](./docs/claims.md)                    |
| **glossary**  | what the words in a promise mean                      | [vocabulary](./docs/vocabulary.md)            |
| **rationale** | why a promise reads as it does, and what was rejected | [lifecycle](./docs/lifecycle.md#11-rationale) |

A conventional ADR holds the decision **and** the reasoning. Crux takes the decision away and puts it in the
catalog as a present-tense claim with a witness. What is left in the document is the reasoning alone.

## The mechanism, in one pass

A **claim** is a short falsifiable statement with a stable slug. A **witness** is a mechanism that judges whether
some part of the codebase satisfies a claim, and a witness exists only as a **marker** — a comment block carrying
`@attests` and the slug it attests. There is no registry and no identity to migrate: delete the test, and the
marker goes with it.

Three questions are asked about every claim, and they are deliberately not the same question.

| Question     | Asks                                             | Answered by            |
| ------------ | ------------------------------------------------ | ---------------------- |
| **verdict**  | does the code satisfy the claim?                 | a tool, or a judge     |
| **standing** | does this instrument support this claim?         | always an intelligence |
| **coverage** | do the claim's witnesses **together** uphold it? | always an intelligence |

To **canvass** is to ask every witness in scope for a verdict. To **audit** is to read the instruments and set the
standings and the coverage. The two acts produce one **readout**, and a human makes the **ruling** at the merge.
The gate is applied by somebody who did not build.

Work that changes what the catalog promises is proposed as an **amendment**. Material you want but cannot yet
state as a claim is **fog**, and it is cleared rather than accumulated.

## What crux refuses to do

The refusals are load-bearing, and each one keeps a cost out of the core.

- **It never learns the comment syntax of any language.** A directive is a name, whitespace, and one
  whitespace-free token. The core is a line scanner.
- **It never resolves a path.** Every check compares one directive against another, so where a file sits is a
  house rule that a repository may decline.
- **It never reads a glossary's definitions.** It reads one directive there and nothing else.
- **It runs nothing and it stores nothing.** Every index it needs is derived on each run.
- **It judges the repository only.** A claim whose state is not rederivable from a checkout is not a claim.

## Status

The vocabulary and the mechanism are settled. No code implements them.
[The roadmap](./docs/roadmap.md) holds what gets built first, what is still open, and what is being watched.

This revision is shaped by a project outside crux — the first to adopt the vocabulary without being crux itself.
Parts of the model exist because something was tried and did not survive contact with the format, and other parts
exist because that project then **built** what it had designed, and the build corrected the design.
