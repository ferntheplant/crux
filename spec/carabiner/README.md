# Carabiner

Carabiner makes a readout. It joins the reports of tools and the answers of intelligences to the witnesses that crux indexed. It runs no agent and keeps no state. A person who builds by hand can use it with crux and no other tool.

> [!CLAIMS]
>
> ### Inputs
>
> Carabiner makes a readout from the index of its commit, the scope from its base, the reports of its sources, the answers, and the amendment when there is one.

**Why carabiner is not in belay.** The adapters were first in belay. Then a team that did not use agents could not make a readout, and a different belay had to copy the adapters. A readout needs reports and answers. It does not need a loop. Belay calls carabiner like each other caller ([Belay](../belay/README.md)).

## Sources

> [!CLAIMS]
>
> ### Canvass file
>
> The file `canvass.jsonc` at the root of the repository names each source: the command that makes its report, the path of the report, and the adapter that reads it.
>
> ### Reads reports
>
> Carabiner reads the report of each source. It runs the command of a source only when its caller asks, in the checkout where carabiner runs.

**Why `canvass.jsonc`.** The first name was `.belay/witnesses.toml`. A tool name in a file in a repository that you do not control makes a rename of the tool into a migration ([Tool names](../tools.md#tool-names)). _Canvass_ is a word of the vocabulary, and the file names what it is for. The file is JSONC because each message between tools is JSON ([Messages](../interfaces.md#messages)), and a configuration file needs comments to tell why a source is there.

**Why carabiner reads and does not run.** CI already runs the tests. Belay runs the commands in a workspace of bedrock. A person runs them in a terminal. If carabiner had to run each command, it would need the execution layer. It reads the reports that each of these leaves.

## Adapters

An adapter changes the report of one tool into verdicts for the witnesses that crux indexed. Adapters are in carabiner, never in the core. The join uses a handle that the tool reports and that the extent of the witness contains.

| Rung      | Handle              | The extent of the witness contains     |
| --------- | ------------------- | -------------------------------------- |
| type      | the full project    | a type declaration                     |
| test      | file and line range | the test that the runner ran           |
| lint rule | the rule id         | the configuration line that enables it |
| prose     | file and line range | nothing to join — the judge answers    |

> [!CLAIMS]
>
> #### Witnesses, not files
>
> An adapter reports `(witness, verdict)` pairs. It never reports a verdict for a file.
>
> #### The judge gets the rest
>
> Each witness that no tool adapter reports on is a question for the judge.
>
> #### Type join
>
> If the project compiles, each witness whose extent contains a type declaration affirms. If it does not compile, each of them denies.
>
> #### Test join
>
> A test result belongs to the witness whose extent contains its line. A failure denies. All passed affirms. A skip with no failure is silent.
>
> #### Lint join
>
> A lint rule that ran and reported nothing affirms. A report of the rule denies.
>
> #### Ran, or silent
>
> An adapter reports if the instrument ran, not only what it said. A rule id that the tool does not recognise is silent.
>
> #### Over-attribution
>
> When an adapter cannot tell which witness a result belongs to, it gives the result to each witness that can own it. It never gives a result to none.

**A tool that runs on a file does not give a verdict for that file.** A linter and a type checker run on each file. If that were a verdict, they would report on each inline witness, and each witness would affirm when the project compiled. Thus a type adapter reports only on witnesses whose extent contains a type declaration. Adapters know the language, so they make this decision.

**The type join over-attributes.** A type error occurs where the subject breaks the type, not in the extent of the type. Thus no error joins to one witness. A failed type check stops the work in any case.

**A lint rule that reports nothing affirms, and a test that does not run is silent.** This is the only rule that is different for one rung, and it assumes that the linter ran the rule. That is why the adapter must report if the instrument ran.

> **Over-attribution is permitted. Under-attribution is not.** A false red costs a builder some minutes. A false green is the failure that crux exists to prevent.

## The questions for the reviewer

> [!CLAIMS]
>
> ### Questions
>
> Carabiner lists each answer that a readout needs: each inferential witness for the triage, each standing in the audit scope, and the coverage of each claim that has a witness in the audit scope.
>
> ### Silent without an answer
>
> An inferential witness with no answer is silent in the readout. A standing or a coverage with no answer is unaudited.

**The list is the brief of the reviewer.** A reviewer that gets the questions does not have to find the scope itself, and a different reviewer gets the same list. The answers go back in the form that [Answers](../readout.md#answers) tells.
