# The readout

How a change is measured against the catalog. A readout is the result: each witness gets a verdict, an independent reviewer audits the instruments, and a human gets one page for the ruling. Any process can make a readout. Belay makes one with agents ([Belay](belay/README.md)). A person makes one with carabiner and their own answers ([Carabiner](carabiner/README.md)). This file tells what a readout holds, and what each process must keep true when it makes one.

## Four questions

Crux asks four questions about each claim. They are different questions on purpose.

| Question | Answered by | Cost | Made void by | Default |
| --- | --- | --- | --- | --- |
| **existence** | a form check | free | nothing — crux calculates it on each run | calculated each time |
| **verdict** | an adapter or judge | free, or expensive | computational: each canvass. Inferential: a diff that the judge selects | **asked**; triaged if inferential |
| **standing** | an auditor | always expensive | a change to the instrument or to the claim text | **kept**; audited after a change |
| **coverage** | an auditor | always expensive | a change to one of the instruments of the claim, or to its claim text | **kept**; audited after a change |

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

A canvass asks, and it does not execute. Carabiner reads the reports of the sources that `canvass.jsonc` names. Its adapters join each report to the witnesses that crux indexed ([Adapters](carabiner/README.md#adapters)). The rung of a witness comes from the source that answers it ([The ladder](witnesses.md#the-ladder)).

## Verdict

| Verdict | Colour | Meaning |
| --- | --- | --- |
| **affirms** | green | Somebody asked the witness, and the subject satisfies the claim. |
| **affirms (unaffected)** | green | A judge read the diff and found that it cannot change this verdict. |
| **silent** | yellow | Nobody answered. No tool ran the witness, or no judge did a triage. |
| **denies** | red | Somebody asked the witness, and the subject does not satisfy it. |

> **By induction, each verdict on the main branch is _affirms_. Thus a judge reads only the diff.**

The condition of the catalog was true at the merge ([Condition](catalog.md#condition)). Thus the question for the judge is: can **this diff** change the verdict? For a small diff, this is cheap. A good code reviewer already works this way. Crux stores no verdicts.

**Triage is one pass.** The judge gets the diff and the _Valid when_ condition of each inferential witness. It returns the witnesses to ask again. The other witnesses affirm, unaffected. Then the judge answers the witnesses that it returned.

> [!CLAIMS]
>
> ### HEAD, not the diff
>
> The judge asks if HEAD satisfies the claim. It never asks if the diff is acceptable alone.
>
> ### Triage by the reviewer
>
> The reviewer does the triage of the inferential witnesses. The builder never does a triage.
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

| Standing | Meaning |
| --- | --- |
| **sound** | An auditor read the instrument, and it supports this claim. |
| **unsound** | It does not support this claim. Remove the slug from its `@attests`, repair it, or delete it. |
| **unaudited** | Nobody has read it against this claim. |

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

| Coverage | Meaning |
| --- | --- |
| **covered** | An auditor read the witnesses together, and they reach the full claim. |
| **under-covered** | No witness reaches part of the claim. Add a witness, or make the claim smaller. |
| **unaudited** | Nobody has read the witnesses together. |

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

> [!CLAIMS]
>
> ### Independent reviewer
>
> The reviewer that answers for a readout did not build the diff that the readout measures.

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

| Failure | Question | Found by | Result |
| --- | --- | --- | --- |
| no witness attests the claim — **unattested** | existence | form check | red — stop |
| a witness **denies** | verdict | the canvass | red — stop |
| a witness is **unsound** | standing | the audit | red — stop |
| the claim is **under-covered** | coverage | the audit | red — stop |
| the prose triage finds a promise | verdict | the judge | red — stop |
| an entry is not enacted, or a catalog change has no entry | amendment match | the comparison | red — stop |
| each witness is **silent** | verdict | the canvass | yellow — the ruling |
| a standing is unaudited, or proposed and not confirmed | standing | the audit | yellow — the ruling |
| coverage is unaudited, or proposed and not confirmed | coverage | the audit | yellow — the ruling |

> [!CLAIMS]
>
> ### Catalog order
>
> The readout lists the claims in catalog order, never by rung or by colour.
>
> ### Red stops
>
> No process gives a readout that has a red item to the operator for a ruling. The work goes back to the builder.
>
> ### Void when stale
>
> A readout names its commit. When its commit is not HEAD, the readout is void.
>
> ### Not committed
>
> The machine form of a readout is a build artifact. No tool commits it.

The operator gets the amendment and the readout together. The amendment tells what somebody wanted. The readout tells what the witnesses say.

**Where the readout goes is a choice of the process.** The first version of this section had belay post each readout as a pull request comment. That is one rendering of the machine form, and belay still does it ([In the pull request](belay/README.md#in-the-pull-request)). A different process can show the same document in a different place.

### Machine form

> [!CLAIMS]
>
> #### Readout form
>
> The machine form of a readout is a JSON document. It names its commit, its base, and the version of its amendment when it has one.
>
> #### Claim entries
>
> The machine form has one item for each claim, in catalog order. Each item has the slug of the claim, its coverage, and one row for each witness of the claim.
>
> #### Witness rows
>
> Each witness row has the witness reference, the verdict, and the standing. A verdict of _affirms (unaffected)_ is a different value from _affirms_.
>
> #### Proposed is marked
>
> Each _sound_ standing and each _covered_ coverage is marked as proposed or as confirmed.
>
> #### Witness reference
>
> A readout names a witness by its file and the line of its block, at the commit of the readout. The reference has no meaning at a different commit.

**A witness reference is temporary on purpose.** A witness has no identity ([Witness blocks](witnesses.md#witness-blocks)). It is the extent that a block owns, and the slugs on the block tie it to its claims. A file and a line name it exactly at one commit, and a readout is valid at one commit only ([Void when stale](#void-when-stale)). Thus nothing needs a stable name for a witness.

### Against the amendment

> [!CLAIMS]
>
> #### Amendment match
>
> When a readout has an amendment, it compares the entries with the catalog changes from its base to its commit. Each entry that the diff does not make is red. Each catalog change that no entry names is red.
>
> #### Text match
>
> An `add` or a `change` entry is made when the claim text at the commit is the text of the entry, after the whitespace is normalised.
>
> #### No amendment
>
> A readout with no amendment lists the catalog changes from its base to its commit, and it compares nothing.

**Why a change with no entry is red.** A builder that changes a claim outside the amendment sets its own measure of success. The audit scope already finds the witness side of this ([Scope](checks.md#scope)). The comparison finds the claim side. The repair is an escalation, not a ruling ([Escalation](interfaces.md#escalation)).

**Why exact text.** The operator approved the words of each entry. A builder that rewords a claim to make it easier to satisfy changes the promise. A rewording that keeps the promise costs one escalation. A comparison of meaning would need an intelligence, and this check is cheap because it needs none.

## Answers

Answers are what an intelligence gives to a readout: the verdicts of the inferential witnesses, the standings, and the coverage. A judge and an auditor give them. That can be an agent that belay starts, or a person.

> [!CLAIMS]
>
> ### Answers form
>
> Answers are a JSON document that names its commit. Each verdict and each standing is keyed by a witness reference and a slug. Each coverage is keyed by a slug.
>
> ### Answers from any intelligence
>
> A readout takes answers from an agent or from a person, in the same form.
>
> ### Only a person confirms
>
> An answer from an agent can propose _sound_ or _covered_. Only an answer from a person can confirm them.
>
> ### Stale answers
>
> Answers whose commit is not the commit of the readout are not used.

**The questions come from the readout.** Before the answers exist, carabiner lists the answers that the readout needs ([Questions](carabiner/README.md#questions)). An intelligence answers that list, and it does not need to find the scope itself.

## Making a readout

Each process that makes a readout for a ruling keeps these claims: [Independent reviewer](#independent-reviewer), [Triage by the reviewer](#triage-by-the-reviewer), [Red stops](#red-stops), and [Only a person confirms](#only-a-person-confirms). The rest of the process is its own. Belay keeps them with agents in a loop ([Belay](belay/README.md)). A person can keep them with carabiner and a colleague.

**What the human decides.** The human makes a specification decision: **were these the correct claims?** The human closes the yellow items: silent verdicts, standings that nobody confirmed, and coverage that nobody confirmed. Only the operator can say that the witnesses reach a claim far enough. The merge confirms each proposed item that the ruling accepts, so no confirmation is kept anywhere else.

**Belay was in this file.** The first version of this file told the loop of belay here, and the loop was a set of claims of the format. Then a change to belay was a change to crux. The loop moved to [Belay](belay/README.md). Only the claims that each process must keep stayed.
