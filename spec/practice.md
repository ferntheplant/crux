# Practice

How to decide what becomes a claim, and how work moves from a wish to the catalog. Nothing in this file is a claim. It is judgment, and a tool must never make it ([Dogfooding](tools.md#dogfooding)).

## Writing a claim

**Write a claim only if something can possibly violate it.** Each claim costs a witness, an audit, and a readout row for all time. A claim that nothing can break has this cost and gives no protection. _We use React_ is a claim, because somebody can add a Svelte component. _We use a monorepo_ is not a claim. This test has no mechanical form, because the correct answer is frequently the unexpected one.

**A decision that nothing can violate is prose.** Write it beside the claims that it explains ([Prose](catalog.md#prose)).

**A review rule is not a claim.** _Do not add unnecessary abstraction_ examines a change, not the codebase. The base commit already has much abstraction, so the induction has no state to start from. Write it again as a state, for example _no interface has exactly one implementation outside tests_. Or keep it in the reviewer runbook, outside the catalog.

**A claim reads alone.** Read it with the glossary, but without its heading and the prose around it. If it is not true or false alone, it depends on context that no audit reads ([Prose](catalog.md#prose)).

## Claims and examples

> **A claim is a rule. A witness is an example of the rule, or an instrument that reaches each case of it.**

Two older practices give a shape to the sentences of crux. [EARS](https://alistairmavin.com/ears/), the Easy Approach to Requirements Syntax, gives the shape of a claim. [Gherkin](https://cucumber.io/docs/gherkin/reference/) gives the shape of an example. Crux takes their sentence shapes and nothing more. No tool checks the shape, because crux never reads meaning ([What crux refuses to do](../ABSTRACT.md#what-crux-refuses-to-do)). The shape helps the writer, the builder, and the reviewer.

### The shape of a claim

Write claim text in the clause order of EARS. Only the system and the response are necessary.

| Clause | Starts with | Holds | Example |
| --- | --- | --- | --- |
| feature | _Where_ | a feature or a configuration that the repository holds | _Where single sign-on is enabled for the tenant,_ |
| state | _While_ | a state that is true before the event | _while the session is active,_ |
| event | _When_ | the event that starts the response | _when a request carries an expired session token,_ |
| unwanted event | _If … then_ | an event that must not cause harm | _if the payment client times out, then_ |
| system | _the_ | the part of the product that a reader sees | _the API_ |
| response | a verb in the present tense | what the system does | _returns 401 and changes no stored data._ |

A complete claim: _When a request carries an expired session token, the API returns 401 and changes no stored data._

- **Present tense, not _shall_.** EARS writes _the system shall_, because a requirement becomes true later. A claim is true now.
- **Name the system.** The system clause makes the claim read alone ([Writing a claim](#writing-a-claim)). A claim that starts with _It_ has no system.
- **The system sets the altitude.** A reader sees the API, the report, or the dashboard. When the system clause names a configuration, a module, or a service inside the product, the claim describes its own witness ([Altitude](#altitude)).
- **An _If … then_ clause asks for a second polarity.** A witness must cause the unwanted event and observe that no harm occurs ([Polarity](witnesses.md#polarity)). A response that says _no X occurs_ asks for the same witness.
- **A _Where_ clause names what the repository holds.** When the feature is a configuration, name the configuration, not its value ([Rederivable](#rederivable)).
- **One response for each claim, at the altitude of a reader.** EARS puts one response in each requirement. Altitude groups defects by the failure that a reader sees. The two rules agree: three defects that give one visible failure are one response and three witnesses, not three clauses.

### The shape of an example

Gherkin has the _Rule_: one business rule that the product keeps. It shows each rule with _Examples_. A claim is a rule, and a witness is an example of it. Gherkin then joins each example to code through step definitions. Crux does not, because a witness is already code, or prose that a judge reads.

Write the _Valid when_ condition of an inferential witness as one example in three parts:

- **Given** the state that the example starts from. Make it from the checkout: a fixture, a seed, or a configuration ([Rederivable](#rederivable)).
- **When** the event. It is the _When_ or the _If_ clause of the claim.
- **Then** what a reader observes, at the subject. Gherkin requires an observable outcome. Crux requires the point of observation ([Subject and instrument](witnesses.md#subject-and-instrument)).

The judge does the work of a step definition. It reads the example and finds if the subject makes it true. When a test runner permits it, name a test in the same three parts, so that an auditor reads the example in the name.

### Coverage by clause

An example shows one case. A claim covers each case of its clauses. Thus the auditor reads the clauses of the claim and asks which witness reaches each one ([Coverage](readout.md#coverage)):

- Which witness makes each _While_ state, and which witness makes the opposite state?
- Which witness causes each _When_ event, and which witness causes each _If_ event?
- Which witness observes the response at the system that the claim names, and not at a proxy?
- Does a type, a lint rule, or a test over many inputs reach each case, or do only examples reach some cases?

An answer of _under-covered_ names the clause that no witness reaches. Coverage is still a judgment. The clauses only give the judgment a list.

### What crux does not take

| From | Not taken | Because |
| --- | --- | --- |
| EARS | _shall_ | A claim is true now. |
| EARS | requirement identifiers | The slug is the identity of a claim. |
| Gherkin | step definitions and their runtime | Shared step code is a dependency of each instrument, and crux does not track it ([Reading](readout.md#reading)). |
| Gherkin | tags | Free tags change with no signal ([Groups](catalog.md#groups)). |
| both | a check of the sentence shape | Crux never reads meaning. |

## Settling the words

A claim is falsifiable only if its words are clear. Thus, settle the words before you write the claims that use them. Write the definitions. Select one word for each concept, and record the rejected synonyms. This is a convention, and crux does not check it.

**The glossary directive was deleted.** The long form had `@glossary`, and a change to a glossary gave a readout row. The row was usually noise. Add one word to a glossary, and each claim that can use that word gets a yellow row, but no claim changed its meaning. An operator learns to ignore a row like this, and an ignored row is worse than no row.

**The cost of the deletion.** A builder can make a definition narrower, and a claim becomes easier to satisfy. No claim and no witness changes, and nothing mechanical sees it. The reviewer reads the full diff, and the changed definition is in that diff. If a definition change alters what a claim promises, the claim text must also change, and changed claim text is already in the audit scope ([Claim change](checks.md#claim-change)).

**A tool directory can add words.** A directory in `spec/` that holds the specification of one tool can hold a `CONTEXT.md` with the words that only that tool uses ([Where a claim lives](interfaces.md#where-a-claim-lives)). It never defines again a word of the root `CONTEXT.md`. Its words never go into a message to a different tool, unless that tool owns the form of the message ([Backbone words](interfaces.md#backbone-words)). Thus belay can use the words of a hospital, and the root keeps one meaning for each word.

**Why scope a glossary by directory.** One meaning for each word is easy in one tool. Six tools need words that the others never use, and a tool that borrows a model, such as a teaching hospital, borrows its words too. A root glossary with each of these words grows without end, and two tools fight over a word such as _triage_. The root is the language between tools. A tool glossary is a dialect inside one tool.

## Rejected words

Crux uses its own convention ([Settling the words](#settling-the-words)). This table gives the rejected synonyms of the words of crux, and the reason for each.

| Word | Rejected because |
| --- | --- |
| **clause** | It named a part of a claim. A claim is already the smallest promise, so one thing got two words. |
| **marker** | It named the same thing as _witness_. One thing gets one word. |
| **project** | It was tried in three forms, and all three failed. A tool directory is a group, not a project. See [Groups](catalog.md#groups) and [Where a claim lives](interfaces.md#where-a-claim-lives). |
| **run** | You cannot run a prose witness. Bedrock uses _run_ for an agent run, in its own glossary. See [Bedrock words](bedrock/CONTEXT.md). |
| **poll** | A poll samples. A canvass is complete. _Poll_ is the fallback if _canvass_ is too easy to misspell. |
| **passes** / **fails** | _The witness fails_ can mean that the code is bad or that the witness is bad. |
| **cycle** | It named the same thing as _round_. One thing gets one word. |
| **shall** | It is the verb of EARS. A claim is true now, so it uses the present tense. See [The shape of a claim](#the-shape-of-a-claim). |
| **requirement** | It is the word of a three-layer design that came before these tools. A requirement is something that will be true. A claim is true now. |
| **check** | It named both the witness and the act of asking it. _Witness_ and _canvass_ separate them. |
| **issue copy** | Belay kept a copy of the text of an issue to see a change. The version of the amendment does this. See [Version](interfaces.md#version). |
| **ADR** | An ADR holds the decision and the reasoning in one record. The catalog holds claims, with the reasoning as prose. |
| **complete** | This was a name for _covered_. No audit can prove that nothing is missing. |
| **section number** | `§8.2` was a second identity for a heading, and it did not move with the heading. A link cites the slug. |
| **pin** | A ` ^id` at the end of a claim fixed its id. The heading of a claim now gives its slug, and GitHub and VS Code do not go to a pin. See [Claims](catalog.md#claims). |
| **id** | It named the part of a slug after `#^`. A claim now has a heading slug, which is a word that each viewer already uses. |
| **wikilink** | `[[file#heading]]` named a file by a short name, which can match several files. See [Links](catalog.md#links). |
| `@claim` | Markdown structure declares a claim. The directive was a second name for a thing that the label already names. |
| `@claims` | A directive with no token breaks the directive form. The `[!CLAIMS]` line does its work. See [Claims](catalog.md#claims). |
| `@crux` | A tool name in the format is a migration that you cannot run. See [Tool names](tools.md#tool-names). |
| `@witness` | `@attests` already opens a witness, so this directive added no information. |
| `@kind` | Nothing used its closed set. The readout is in the order of the claims. |
| `@tag` | Free tags change without a signal. Declared tags need a registry. See [Groups](catalog.md#groups). |
| `@glossary` | Its only function was a readout row, and the row was noise. See [Settling the words](#settling-the-words). |
| `@grounds` | It linked a rationale document to claims, and nothing mechanical read it. See [Prose](catalog.md#prose). |
| `@scope` | It was a manual list of the dependencies of a witness. A judgment replaces its one use. See [Scope](checks.md#scope). |
| a bare `@end` | It is a keyword in Objective-C and Texinfo. A stray one cuts a witness short. Each terminator names its opener. |

> [!NOTE]
>
> Open: the spelling of _canvass_; it blocks the command names of carabiner. **Poll** is the fallback.

## Rederivable

> **Crux examines the repository. If a checkout cannot show that a claim is true, it is not a claim.**

A judge can read only the diff ([Verdict](readout.md#verdict)), because the repository is the only thing that changes between merges. A claim about the live world stays green for all time. No diff touches it, but the world changes.

- **Declared infrastructure can be a claim. Live infrastructure cannot.** The limit moves with your tools. Adopt a framework that declares DNS records in the repository, and claims about those records become possible.
- **Runtime state, human actions outside the repository, and drift are never claims.** They are monitoring.
- **A command that connects to a service is a witness only if it runs fully locally.** Examples are a dry run and an emulator.
- **If the repository does not hold a value, make the claim more general.** If the domain is in configuration, do not claim _the sender is on the mail subdomain_. Claim _each sender address comes from the configured domain_. The general claim is also better. It finds a hardcoded address, which is the error that is likely to occur.

> [!NOTE] **Under watch: a tool requires a credential that no observation uses.** An infrastructure framework reads cloud credentials when it makes its client. This occurs before it knows that each resource is emulated. With no credentials, the witness fails with an authentication error before it runs. Four lines of placeholder environment values repaired it. Is a witness still rederivable from a checkout if it requires a credential that it never uses? The answer taken was yes, because no observation depends on the value. Seen once, with one tool and one provider.

> [!NOTE] **Under watch: a witness needs something from the process, not from the program.** A test deploys two stacks from a third directory, and a relative path in a resource declaration then points to nothing. The only repair was to change the working directory at the start of the test module. The line is easy to delete by accident. A dependency system models what a witness needs in the program. It models nothing about the process: working directory, environment variables, or the temporary directory of the run. Seen once, in one test harness.

## Altitude

> **Group claims by the failure that a reader sees. Do not group them by the check that finds it.**

Two properties are two claims when they can fail separately **and** a reader sees two different things. A weak generator, a `Math.random` call, and a short token are three defects. They give one visible failure: _somebody can guess the token_. Thus they are one claim with three witnesses.

**A claim that describes its own witness is a witness with the name of a claim.** _The configuration is a context service_ describes an instrument. No operator has an opinion about it. A catalog that nobody can rule on does not do its function.

**Two forces set the altitude.** This section moves a claim up, to what a reader sees. Coverage moves it down, to what the witnesses reach ([Coverage](readout.md#coverage)). A claim above the balance promises what nothing checks. A claim below it describes its own witness.

**Headings take the pressure off a claim.** A heading and its prose can describe a feature at the altitude of a reader, so a claim does not need to. Each claim must still name a failure that a reader can see. When a claim needs parts, it becomes a heading ([Claims](catalog.md#claims)).

> [!NOTE] **Under watch: one prose sentence, two claims, two altitudes.** One sentence of a design document became two claims at different altitudes. Neither claim implied the other, and no single witness covered both. One occurrence tells nothing yet about the correct altitude.

## Amendments

An amendment is the set of claim changes that one unit of work proposes. Its operations are **add**, **change**, and **delete**. An add names its claim **and** the witness that will attest it. A delete removes the claim and its witnesses in one merge. The entries and the prose of an amendment have a fixed form ([Amendment](interfaces.md#amendment)). The witness plan is in the prose: write one example in three parts for each witness that the builder will write, and name its rung ([The shape of an example](#the-shape-of-an-example)).

**The design occurs when you name the witness.** _The dashboard never writes_ is only a wish until you ask what will examine it. The answer is a database handle with no write method, and that answer changes the design.

**An amendment is a specification. It does not stop changes.** When you write the witness, you complete the claim. This frequently shows that the claim did not say what it meant. This is correct, and the builder escalates it ([Escalation](interfaces.md#escalation)).

- **Set coverage while the amendment is still text.** An amendment that regroups witnesses makes new sets of witnesses. Early, this costs one paragraph. After the build, it cost two audit rounds and rework.
- **Read the amendment as one thing.** Two claims can each be correct and be impossible together. All other checks examine one claim at a time. The contradiction usually shows first in the coverage prose.
- **Name the seam, and tell what it replaces.** The production work that a claim needs has no artifact. _This has no claim_ does not mean _this is small_.
- **Tell when a witness is written before its subject.** A witness can deny a name that a later amendment will make. The witness cannot tell this, so the amendment tells it.
- **The amendment stays proposed until the merge.** The merged code shows what somebody built. Only the amendment shows what somebody wanted.

> [!NOTE] **Under watch: an amendment has no version.** Three amendments were written under a rule that was later retracted, and nobody enacted them. Nothing marked them as old. Somebody remembered, and revised them three sessions later. This stops being important when crux becomes stable.

## Fog

Fog is material that you want but cannot yet write as a claim. It is not a verdict and not a colour, because it is not in the catalog.

> **Fog is clear when you can write the claim and select its witness.**

| State | Can you write the claim? | Held by | Exit |
| --- | --- | --- | --- |
| **fog** | no | cairn | examine the checkout, or examine the world |
| **proposed amendment** | yes | branch | the merge |

- **A fog item is a claim with an empty clause.** When you cannot write the system, the event, or the response in words of the glossary, the claim is fog ([The shape of a claim](#the-shape-of-a-claim)). The item names the empty clause.
- **A fog item records what will clear it.** One search and a temporary deployment have very different costs. This field lets you sort the queue.
- **Fog is inability, not unwillingness.** A claim that you can write is an amendment that you did not write yet. _Not now_ makes no artifact.
- **Fog can clear into nothing.** It can become a decision that nothing can violate. It can also become a question that an existing claim already answers. In both cases, it closes, and nothing enters the catalog.
