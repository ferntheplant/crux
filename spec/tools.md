# Tools

The tools that read the format, the order in which they get built, and the rules that apply to all of them.

## Tool set

The format is the product. You can replace the tools. Each tool does one job in one layer, and it meets the other tools only through messages ([Interfaces](interfaces.md)).

| Layer | Name | Does |
| --- | --- | --- |
| requirements | **crux** | reads claims and witnesses; reports form errors; gives the index and the scope ([checks](./checks.md)) |
| requirements | **cairn** | holds fog and amendments that are not enacted; gives amendments ([cairn](cairn/README.md)) |
| measurement | **carabiner** | joins tool reports and answers to witnesses; makes the readout ([carabiner](carabiner/README.md)) |
| orchestration | **belay** | takes an amendment; makes a PR that is cheap to rule on ([belay](belay/README.md)) |
| execution | **bedrock** | gives workspaces and agent runs ([bedrock](bedrock/README.md)) |
| attention | **beacon** | moves the operator between fog work and rulings |

> [!CLAIMS]
>
> ### Tools stand alone
>
> The package of each tool depends on no package of a different tool.

**The format goes into repositories that you do not control.** Thus a change to the format is a migration that you cannot run, and a tool that the format depends on cannot change freely.

**Tools stand alone was _Crux stands alone_.** The first version kept only the crux package free of the others. Now each tool can change on its own, so each package is free of the others. A tool that needs a different tool calls it, or reads a file that it wrote, in the form that [Messages](interfaces.md#messages) tells.

> [!NOTE] **Open: beacon.** It has no owner, and it adds no data model.

## Tool names

The tools are **crux**, **cairn**, **carabiner**, **belay**, **bedrock**, and **beacon** ([Tool set](#tool-set)). A tool name has no meaning in the vocabulary. Thus you can rename a tool, and no migration is necessary.

For the same reason, no part of the format uses a tool name. The directive is `@attests`, the frontmatter key is `catalog`, the block marker is `[!CLAIMS]`, and the file of sources is `canvass.jsonc`. The format goes into repositories that you do not control, and a tool name in it would make a rename into a migration that you cannot run.

## Build order

**1. Crux.** It runs nothing and stores nothing. Its claims are in [Form errors](checks.md#form-errors), [Index](checks.md#index), and [Scope](checks.md#scope), and the format that it reads is in [Catalog files](catalog.md#catalog-files), [Claims](catalog.md#claims), [Links](catalog.md#links), and [Witness blocks](witnesses.md#witness-blocks).

**2. Carabiner.** It reads the reports of the sources that `canvass.jsonc` names, joins them to the witness index, and makes the readout from them and from the answers ([Carabiner](carabiner/README.md)). With crux and carabiner, a person can make a readout by hand.

**3. A belay MVP on a bedrock MVP.** Belay runs the builder and a new reviewer in a loop, within a budget, and posts the readout to the pull request ([Belay](belay/README.md)). Bedrock gives one kind of workspace and one harness ([Bedrock](bedrock/README.md)).

**Cairn comes after belay.** Belay makes the catalog useful. Cairn makes it easy to use. Until cairn exists, the operator writes the amendment file ([Layers](interfaces.md#layers)).

**This specification is its own first catalog.** No code implements crux yet, so each claim in it is unattested. The first build of crux can run on this repository, and its form errors are the work list.

## Dogfooding

While you build the first tools, record each problem when it occurs. A manual step that feels **clerical** is a missing feature. A manual step that feels like **thinking** must never be automated. Two items in this second group are settled: _is this worth a claim?_ ([Writing a claim](practice.md#writing-a-claim)) and _is this claim covered?_ ([Coverage](readout.md#coverage)). A complaint that a catalog is tiring to read is evidence about its altitude ([Altitude](practice.md#altitude)).

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
