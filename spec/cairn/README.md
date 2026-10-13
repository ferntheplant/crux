# Cairn

Cairn holds the work that is not yet a branch: fog, and amendments that nobody enacted. It owns each part of an amendment except its entries, its prose, and its version ([Amendment](../interfaces.md#amendment)).

> [!CLAIMS]
>
> ### State outside the repository
>
> Cairn keeps its records outside the repository, and it never writes into the repository.
>
> ### Gives amendments
>
> Cairn gives each amendment in the form that [Amendment form](../interfaces.md#amendment-form) tells, with the version of its current text.
>
> ### Records decisions
>
> Cairn records the decision of the operator on each escalation. After an accepted escalation, cairn gives the proposed amendment as the amendment.
>
> ### Enactment
>
> Cairn finds that an entry is enacted when the index of the main branch shows it: an `add` or a `change` when the slug has the claim text of the entry, and a `delete` when the slug names no claim.
>
> ### Watcher
>
> When the main branch changes, the watcher of cairn reads the machine index of crux and warns about each slug of an existing claim in a cairn record that names no claim.
>
> ### Witness supply
>
> When a change to the set of possible witnesses merges, cairn proposes a list of claims and fog to examine again. It never rewrites them.

**Why outside.** An amendment exists before a branch exists. Fog in the repository adds noise to its history. The cost is that a rename does not change the tracker at the same time. The watcher repairs that cost, and it holds no facts of its own.

**Why enactment comes from git.** Belay does not tell cairn that work merged. The merge changes the index of the main branch, and cairn reads it. Thus cairn works with each belay, and with no belay ([Layers](../interfaces.md#layers)).

**A record holds a slug, and claim text only in an entry.** A record of cairn names an existing claim by its slug, and the person or tool with the checkout resolves it. The proposed text of an `add` or a `change` entry is the one exception, because the claim does not exist on the main branch yet. The first version of this rule allowed no claim text at all, and an `add` could not exist.

**Why the witness supply matters.** When you adopt a type-aware linter or a declarative infrastructure framework, no claim changes. But the set of **possible** claims changes ([The ladder](../witnesses.md#the-ladder)). Cairn records the plan, and the merge is the signal to look again.

**What cairn can change freely.** Where an amendment is kept, how a person writes it, who approves it, how fog is sorted, and how the prose is organised. A first cairn can be a Linear project that gives the JSON form at the interface.
