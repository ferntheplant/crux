# The readout

How belay measures a change against the catalog. Belay takes an amendment, asks each witness for a verdict, has an independent reviewer audit the instruments, and gives one page to a human for the ruling.

## Four questions

Crux asks four questions about each claim. They are different questions on purpose.

| Question      | Answered by         | Cost               | Made void by                                                            | Default                           |
| ------------- | ------------------- | ------------------ | ----------------------------------------------------------------------- | --------------------------------- |
| **existence** | a form check        | free               | nothing — crux calculates it on each run                                | calculated each time              |
| **verdict**   | an adapter or judge | free, or expensive | computational: each canvass. Inferential: a diff that the judge selects | **asked**; triaged if inferential |
| **standing**  | an auditor          | always expensive   | a change to the instrument or to the claim text                         | **kept**; audited after a change  |
| **coverage**  | an auditor          | always expensive   | a change to one of the instruments of the claim, or to its claim text   | **kept**; audited after a change  |

A computational verdict is cheap, so each canvass asks it. An inferential verdict is expensive, so a judge does a triage of the diff first. A standing and a coverage stay the same until a change, because only an intelligence can set them.

## Canvass

To canvass is to ask each witness for a verdict. The tool adapters answer the computational witnesses, and the judge answers the remaining witnesses.

> [!CLAIMS]
>
> ### Complete
>
> A canvass asks each witness in the index. It never samples.
>
> ### Silence is a result
>
> A witness that nobody answered is silent. A canvass reports it and does not fail.

A canvass asks, and it does not execute. Belay runs the sources that `.belay/witnesses.toml` names, and the adapters read their reports.

### Adapters

An adapter changes the report of one tool into verdicts for the witnesses that crux indexed. Adapters are in belay, never in the core. The join uses a handle that the tool reports and that the extent of the witness contains.

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
> Each witness that no tool adapter reports on goes to the judge.
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

## Verdict

| Verdict                  | Colour | Meaning                                                             |
| ------------------------ | ------ | ------------------------------------------------------------------- |
| **affirms**              | green  | Somebody asked the witness, and the subject satisfies the claim.    |
| **affirms (unaffected)** | green  | A judge read the diff and found that it cannot change this verdict. |
| **silent**               | yellow | Nobody answered. No tool ran the witness, or no judge did a triage. |
| **denies**               | red    | Somebody asked the witness, and the subject does not satisfy it.    |

> **By induction, each verdict on the main branch is _affirms_. Thus a judge reads only the diff.**

The condition of the catalog was true at the merge ([Condition](catalog.md#condition)). Thus the question for the judge is: can **this diff** change the verdict? For a small diff, this is cheap. A good code reviewer already works this way. Crux stores no verdicts.

**Triage is one pass.** The judge gets the diff and the _Valid when_ line of each inferential witness. It returns the witnesses to ask again. The other witnesses affirm, unaffected. Then the judge answers the witnesses that it returned.

> [!CLAIMS]
>
> ### HEAD, not the diff
>
> The judge asks if HEAD satisfies the claim. It never asks if the diff is acceptable alone.
>
> ### Triage by the reviewer
>
> Belay gives the triage to the reviewer. The builder never does a triage.
>
> ### Triage shown
>
> The readout shows _affirms (unaffected)_ and _affirms_ on different lines.
>
> ### Too large to triage
>
> The judge can refuse a diff that is too large for a correct triage. Then each inferential witness is silent.
>
> ### No triage for tools
>
> A computational witness never gets a triage. Each canvass asks it.
>
> ### Prose triage
>
> When a diff changes prose in a catalog file, the judge asks if the change adds a promise or changes what a claim means. If it does, the work goes back to the builder.

**Why HEAD.** The two questions give different answers when small changes add up. Twenty diffs can each be acceptable, and together they can break the claim. A judge that examines each diff alone starts again at zero each time.

**Why the reviewer.** _Unaffected_ is how a builder can avoid a witness ([Separation](#separation)).

**Why each result shows.** An operator can then see a wrong _unaffected_ quickly. It needs no confirmation. A confirmation step puts a yellow row on each witness in each pull request.

**Why refusal is safe.** Silence is yellow, and the ruling sees it. It is also a reason to keep pull requests small.

**Why tools are not triaged.** To ask a computational witness is cheaper than to decide if you must ask it. Only a skipped test is silent.

**Why prose is triaged.** Prose never promises ([Prose](catalog.md#prose)), and crux cannot read meaning to check that. Prose is out of the audit scope ([Prose reopens nothing](checks.md#prose-reopens-nothing)), so this question is the only check on it. It costs one question for each diff that touches prose.

> [!NOTE] **Open: how frequently the triage is wrong.** This blocks nothing. A wrong _unaffected_ is quiet, and the readout only makes it visible. Record each one that you find. If it occurs again, add a mechanical minimum under the triage.

## Audit

To audit is to read the instruments and set their standings, and then set the coverage of their claims. The instruments in the audit scope are the ones to read ([Scope](checks.md#scope)).

### Standing

| Standing      | Meaning                                                                                       |
| ------------- | --------------------------------------------------------------------------------------------- |
| **sound**     | An auditor read the instrument, and it supports this claim.                                   |
| **unsound**   | It does not support this claim. Remove the slug from its `@attests`, repair it, or delete it. |
| **unaudited** | Nobody has read it against this claim.                                                        |

The auditor examines three things for each standing:

- **Does it support the claim?** This is the usual question.
- **Is its contract its own?** Nothing that it asserts comes from the subject ([Stated contract](witnesses.md#stated-contract)).
- **What does the instrument depend on?** Name each shared helper or type utility, and decide if it is stable enough to trust.

**Sound does not mean sufficient.** A sound witness supports its claim, but it can reach only part of the claim. Coverage asks about the remaining part.

> [!CLAIMS]
>
> #### Unsound alone
>
> An agent can set _unsound_ with no confirmation.
>
> #### Sound is proposed
>
> An agent only proposes _sound_. The readout shows it as proposed until a human confirms it.

To find a bad witness needs no authority. To say that a witness is good enough needs authority.

### Coverage

> **Coverage is if the witnesses of a claim uphold it together. If each witness is sound, the claim is not necessarily covered.**

| Coverage          | Meaning                                                                         |
| ----------------- | ------------------------------------------------------------------------------- |
| **covered**       | An auditor read the witnesses together, and they reach the full claim.          |
| **under-covered** | No witness reaches part of the claim. Add a witness, or make the claim smaller. |
| **unaudited**     | Nobody has read the witnesses together.                                         |

> [!CLAIMS]
>
> #### Under-covered alone
>
> An agent can set _under-covered_ with no confirmation.
>
> #### Covered is proposed
>
> An agent only proposes _covered_. The readout shows it as proposed until a human confirms it.

Coverage has no mechanical form, and it will never have one. Check a claim whose witnesses are all prohibitions first ([Polarity](witnesses.md#polarity)). **If a witness reaches more than its claim, the claim is too small.** Repair the claim.

> [!NOTE] **Under watch: exactly-once language hid a commit point.** A claim promised a maximum of one send for each local date, and each witness affirmed it. An audit asked what is true between the send and the write that records it. They share no transaction, and the send has no idempotency key. A send that succeeds before a failed write looks the same as a send that never occurred. The repair was to make the claim smaller. Seen once, in one external side effect. It is a reviewer runbook step.

### Reading

- **Coverage makes the reading larger, not the scope.** To set the coverage of a claim again, the auditor reads each of its witnesses, changed or not. It sets standings only on the witnesses in scope.
- **The invariant holds by induction.** Each merge audits its scope and changes nothing more. Thus each witness on the main branch that no diff touched is presumed sound. Each claim that no diff touched is presumed covered. There is nothing to store and nothing to find.
- **One pass is not always sufficient.** A repair changes an instrument, and that instrument is in scope again. Plan for two rounds.
- **Do not add a `last audited` record.** A date cannot tell if the instrument changed.
- **The base case.** In an existing repository, all witnesses start unaudited. The readout shows this in yellow.
- **The accepted blind spot.** The dependencies of an instrument, for example a shared test helper or a type utility, are outside its extent. A dependency can change its meaning, or do nothing, and the instrument does not change. The auditor names these dependencies when it sets a standing. The reviewer sees a diff to one of them. Crux does not track them. An import graph cannot separate the dependencies of the instrument from the dependencies of the subject, and to audit again after each subject change is what [Subject and instrument](witnesses.md#subject-and-instrument) forbids.

> [!NOTE] **Under watch: a mechanism that nothing observed.** A fix added a per-attempt identifier: a migration, a unique index, and a generated value. No witness named it. A constant string in place of the generated value passed all twenty-three tests. The audit goes from claims to witnesses. Nothing goes in the other direction to ask _what here is load-bearing and not observed_. Seen once, in one amendment with three cases. It is a reviewer runbook step. If it occurs again, make it a step in the model.

## Separation

The canvass asks the subject. The audit asks the instrument. A builder that writes its own witness sets its own measure of success. Thus the canvass alone has no value. A builder that does the triage of its own inferential witnesses also selects which witnesses somebody asks.

The first real build passed each test and the repository gate. Then an independent audit set six claims to red.

> **Somebody who did not build applies the gate.**

## Readout

The readout has one block for each claim. The claim and its coverage are first. Then there is one line for each witness.

```
catalog/token.md#unguessable                                              covered (proposed)
  token.test.ts:14                    web crypto bytes     affirms                 sound
  vite.config.ts:31                   no Math.random       affirms                 sound

catalog/report.md#reads-at-a-glance                                                  covered
  witnesses/report-legibility.md:1    the summary view     affirms (unaffected)    sound

catalog/checkout.md#never-charges-an-expired-cart                                    covered
  src/api/checkout.ts:12              expiry before charge affirms                 unaudited
```

The third block is an inline witness. The diff changed the handler, so the judge asked the witness again. The edit also changed the instrument, so the standing waits for the audit.

| Failure                                                | Question  | Found by    | Result              |
| ------------------------------------------------------ | --------- | ----------- | ------------------- |
| no witness attests the claim — **unattested**          | existence | form check  | red — stop          |
| a witness **denies**                                   | verdict   | the canvass | red — stop          |
| a witness is **unsound**                               | standing  | the audit   | red — stop          |
| the claim is **under-covered**                         | coverage  | the audit   | red — stop          |
| the prose triage finds a promise                       | verdict   | the judge   | red — stop          |
| each witness is **silent**                             | verdict   | the canvass | yellow — the ruling |
| a standing is unaudited, or proposed and not confirmed | standing  | the audit   | yellow — the ruling |
| coverage is unaudited, or proposed and not confirmed   | coverage  | the audit   | yellow — the ruling |

> [!CLAIMS]
>
> ### Catalog order
>
> The readout lists the claims in catalog order, never by rung or by colour.
>
> ### Red stops
>
> Belay gives no readout that has a red item to the operator. The work goes back to the builder.
>
> ### In the pull request
>
> Belay posts each readout as a pull request comment that names its commit.
>
> ### Void when stale
>
> A readout whose commit is not HEAD is void, and belay marks it so.
>
> ### Not committed
>
> The machine form of a readout is a build artifact. Belay never commits it.

The operator gets the amendment and the readout together. The amendment tells what somebody wanted. The readout tells what the witnesses say. The amendment goes in the pull request description.

## Belay

Belay takes an amendment and makes a pull request that is cheap to rule on.

1. A human operator gives belay an amendment.
2. Belay tells a **builder** to implement it.
3. **At any time, the builder can escalate.** It tells the proposed change to the amendment, and it stops. The operator accepts or refuses the change, and the build continues.
4. Belay tells a **reviewer** to canvass and to audit. The result is the readout.
5. Belay gives the amendment and the readout to the operator for the ruling.

> [!CLAIMS]
>
> ### Escalation stops the build
>
> When the builder escalates, belay stops the build until the operator accepts or refuses the proposed change.
>
> ### Handoff when green
>
> Belay starts the reviewer only when each computational witness in the canvass of the builder affirms.
>
> ### New reviewer each cycle
>
> Belay starts a new reviewer for each cycle, with no context from the last cycle.
>
> ### Cycle budget
>
> Belay stops the loop when the cycle budget is spent, and gives the last readout to the operator.
>
> ### Restart loses nothing
>
> The supervisor calculates each fact again from the branch at HEAD. A restart loses no state.

**Green is a handoff. It does not mean that the work is done.** Before the handoff, the builder breaks each witness that can deny, and makes sure that it denies. A witness that has never denied has not been tested.

**The supervisor holds a loop and no facts.** A reviewer with context from the last cycle has already heard the arguments of the builder.

If a builder escalates on each claim, it got fog, not an amendment ([Fog](practice.md#fog)).

**What the human decides.** The human makes a specification decision: **were these the correct claims?** The human closes the yellow items: silent verdicts, standings that nobody confirmed, and coverage that nobody confirmed. Only the operator can say that the witnesses reach a claim far enough.
