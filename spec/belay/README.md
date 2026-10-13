# Belay

Belay takes an amendment and makes a pull request that is cheap to rule on. It is one process that makes a readout, and it keeps the claims that each such process keeps ([Making a readout](../readout.md#making-a-readout)). The rest of this file is belay's own. You can change it, and no other tool changes.

1. The operator gives belay an amendment.
2. Belay tells a **builder** to implement it.
3. **At any time, the builder can escalate.** It gives an escalation ([Escalation](../interfaces.md#escalation)), and it stops. The operator accepts or refuses the escalation, and the build continues.
4. Belay runs carabiner, and it gives the questions to a **reviewer** ([The questions for the reviewer](../carabiner/README.md#the-questions-for-the-reviewer)). Carabiner makes the readout from the reports and the answers.
5. Belay gives the amendment and the readout to the operator for the ruling.

> [!CLAIMS]
>
> ### Escalation stops the build
>
> When the builder escalates, belay stops the build until the operator accepts or refuses the escalation.
>
> ### Version check
>
> Before each handoff to the reviewer and to the operator, belay compares the version of the amendment with the version that the operator gave. When they are different, belay stops the work until the operator decides.
>
> ### Handoff when green
>
> Belay starts the reviewer only when each computational witness in the canvass of the builder affirms.
>
> ### New reviewer each round
>
> Belay starts a new reviewer for each round, with no context from the last round.
>
> ### Round budget
>
> Belay stops the loop when the round budget is spent, and gives the last readout to the operator.
>
> ### Restart loses nothing
>
> The supervisor calculates each fact again from the branch at HEAD. A restart loses no state.
>
> ### In the pull request
>
> Belay posts each readout as a pull request comment that names its commit.

**Green is a handoff. It does not mean that the work is done.** Before the handoff, the builder breaks each witness that can deny, and makes sure that it denies. A witness that has never denied has not been tested.

**The supervisor holds a loop and no facts.** A reviewer with context from the last round has already heard the arguments of the builder.

**The version check replaces a copy of the issue.** A design for belay kept its own copy of the text of each issue and compared strings before each step. The version of the amendment is a hash of the same text ([Version](../interfaces.md#version)), so belay compares two hashes and keeps no copy.

If a builder escalates on each claim, it got fog, not an amendment ([Fog](../practice.md#fog)).

> [!NOTE] **Open: facts that HEAD does not hold.** The round count, the version that the operator gave, and the decisions on escalations are not in the branch. [Restart loses nothing](#restart-loses-nothing) then needs a record that belay keeps. Plain files are the default ([Principles](../tools.md#principles)).

## Words

Belay can use its own words inside itself, for example the words of a teaching hospital: a patient, a fellow, an attending, and a discharge. They go in a `CONTEXT.md` in this directory when belay uses them. They never go into a message to a different tool ([Backbone words](../interfaces.md#backbone-words)), and they never define again a word of the root glossary.

**The step that admits an amendment is not a triage.** _Triage_ is a root word: the pass in which the judge selects witnesses to ask again. The step in which belay accepts, splits, or refuses an amendment needs a different word, for example _admission_.
