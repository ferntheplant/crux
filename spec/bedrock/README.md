# Bedrock

Bedrock gives a place to work and an agent to do the work. It knows no claim, no witness, and no amendment ([No claim words below orchestration](../interfaces.md#no-claim-words-below-orchestration)). Belay is its first caller, but each caller that needs a workspace or an agent run can use it. The words of this file are in [the glossary of bedrock](CONTEXT.md).

## Workspaces

| Operation | Does |
| --- | --- |
| provision | makes a workspace from a repository, a commit, and an image or a snapshot |
| status | tells the phase of a workspace |
| exec | runs a command, and gives its exit code, its output, and the files that it names |
| diff, push | gives the changes from a commit; pushes a branch |
| snapshot | saves a workspace, when the provider can |
| destroy | deletes a workspace after it makes sure that no work is lost |

## Runs

| Operation | Does |
| --- | --- |
| start | starts an agent run in a workspace, with a harness, a model, instructions, and a budget |
| send | adds a message to the queue of a run; it never interrupts |
| interrupt | stops the current turn, and confirms that it stopped |
| state | tells the state of a run |
| events | gives the events of a run after a cursor, and streams new events |

> [!CLAIMS]
>
> ### Completion once
>
> Each run reports its completion exactly once, with one outcome: `succeeded`, `failed`, `cancelled`, `stuck`, or `budget spent`.
>
> ### Running cost
>
> A run reports its cost as a running total. A cost report that arrives two times changes nothing.
>
> ### No repeats
>
> Bedrock never starts a run, sends a message, or runs a command again on its own. The caller decides each repeat.
>
> ### Capabilities
>
> Each provider of workspaces tells which optional operations it supports, for example `snapshot`.
>
> ### Archive
>
> Bedrock keeps the events of each run after its workspace is destroyed.
>
> ### Unknown is not failed
>
> When bedrock does not know the result of an operation, it reports the result as unknown. It never reports an unknown result as a failure.

**Why the caller decides each repeat.** A loop that repeats work is orchestration. The OpenHands agent server ([References](#references)) keeps a goal loop and stuck detection inside execution, so orchestration is in two places. background-agents refuses to replay an interrupted prompt with no permission from a person. Bedrock takes the second rule for each operation.

**Why a running total.** An event can arrive two times after a reconnect. A total that only moves up makes the second copy harmless.

**Why unknown is a result.** A provider can lose the response to a request that it did. If bedrock reported a failure, the caller could provision a second workspace or start a second run.

> [!NOTE] **Open: the form of each operation.** The tables name the operations, not their arguments and results. Write the forms when a second provider or a second harness exists, so that the forms come from two cases and not one. Start from the forms of the [References](#references).

## References

Bedrock is thin on purpose. Two open systems are its references for the design of each operation:

- [OpenHands software-agent-sdk](https://github.com/OpenHands/software-agent-sdk): a workspace abstraction with local, Docker, and remote implementations, an agent server with a REST and WebSocket API for conversations and events, a classification of failures with a retry flag, and cost metrics for each model.
- [background-agents](https://github.com/ColeMurray/background-agents): a sandbox provider interface with capability flags, a protocol for agent harnesses with exactly one outcome for each turn, filesystem snapshots and prebuilt images, and a cost total that only moves up.

Read them for the form of an operation, not for the boundary. Both mix some orchestration with execution.
