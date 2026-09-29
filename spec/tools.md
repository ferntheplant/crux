---
catalog: 1
---

# Tools

The tools that read the format, the order in which they get built, and the rules that apply to all of them.

## Tool set

The format is the product. You can replace the tools.

|      | Name       | Does                                                                    |
| ---- | ---------- | ----------------------------------------------------------------------- |
| core | **crux**   | reads claims and witnesses; reports form errors ([checks](./checks.md)) |
| 1    | **belay**  | takes an amendment; makes a PR that is cheap to rule on                 |
| 2    | **cairn**  | holds fog and amendments that are not enacted; gives amendments         |
| 3    | **beacon** | moves the operator between fog work and rulings                         |

> [!CLAIMS] Claims
>
> - **Crux stands alone.** The crux package depends on no package of belay, cairn, or beacon.

**The format goes into repositories that you do not control.** Thus a change to the format is a migration that you cannot run, and a tool that the format depends on cannot change freely.

**A tracker stores slugs. It never stores claim text.** The person or tool with the checkout resolves the slug. The interface is one thing: a tracker gives an amendment.

> [!NOTE] **Open: beacon.** It has no owner, and it adds no data model.

## Cairn

Cairn holds the work that is not yet a branch: fog, and amendments that nobody enacted.

> [!CLAIMS] Claims
>
> - **State outside the repository.** Cairn keeps its records outside the repository, and it never writes into the repository.
> - **Watcher.** When the main branch changes, the watcher of cairn reads the machine index of crux and warns about each slug in a cairn record that names no claim.
> - **Witness supply.** When a change to the set of possible witnesses merges, cairn proposes a list of claims and fog to examine again. It never rewrites them.

**Why outside.** An amendment exists before a branch exists. Fog in the repository adds noise to its history. The cost is that a rename does not change the tracker at the same time. The watcher repairs that cost, and it holds no facts of its own.

**Why the witness supply matters.** When you adopt a type-aware linter or a declarative infrastructure framework, no claim changes. But the set of **possible** claims changes ([[witnesses#the-ladder]]). Cairn records the plan, and the merge is the signal to look again.

## Build order

**1. Crux.** It runs nothing and stores nothing. Its claims are in [[checks#form-errors]], [[checks#index]], and [[checks#scope]], and the format that it reads is in [[catalog#catalog-files]], [[catalog#claims]], [[catalog#links]], and [[witnesses#witness-blocks]].

**2. A belay MVP.** It runs the sources that `.belay/witnesses.toml` names. It joins their reports to the witness index, makes the readout, and posts it to the pull request. It runs the builder and a new reviewer in a loop, within a budget ([[readout#belay]]).

**Cairn comes after belay.** Belay makes the catalog useful. Cairn makes it easy to use.

**This specification is its own first catalog.** No code implements crux yet, so each claim in it is unattested. The first build of crux can run on this repository, and its form errors are the work list.

## Dogfooding

While you build tools 1 and 2, record each problem when it occurs. A manual step that feels **clerical** is a missing feature. A manual step that feels like **thinking** must never be automated. Two items in this second group are settled: _is this worth a claim?_ ([[practice#writing-a-claim]]) and _is this claim covered?_ ([[readout#coverage]]). A complaint that a catalog is tiring to read is evidence about its altitude ([[practice#altitude]]).

**An observation that occurred once is an anecdote.** One occurrence changes nothing, and a second occurrence is evidence. The specification keeps a single observation as an _Under watch_ note beside the rule that it questions, and it does not make it a rule. An entry leaves in one of three ways. It occurs again and becomes a rule. Somebody finds that it is wrong, and it is deleted with a line that tells why. Or it stays, because it occurred once and never again. A runbook step costs one question, so one observation is sufficient for a runbook step.

## Principles

These rules apply to each tool and each part of the format. Each specific rule in this specification comes from one of them, or from the purpose.

- Nothing in the repository is built without a claim, and nothing merges before somebody measures it against one.
- A claim is complete when something that is not you can falsify it.
- Use artifacts that are clear in both directions: guidance in, and feedback out.
- Evidence has an index by claim. Tools make it, and nobody collects it by hand.
- Spend human attention only on authority: set a claim, rule on a result, stop the work. Never spend it on status.
- Easy re-entry is more important than deep autonomy.
- Durable state is plain files. Each tool calculates its view and stores nothing.
- The machine checks form. A human or a model checks truth.
