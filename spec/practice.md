# Practice

How to decide what becomes a claim, and how work moves from a wish to the catalog. Nothing in this file is a claim. It is judgment, and a tool must never make it ([Dogfooding](tools.md#dogfooding)).

## Writing a claim

**Write a claim only if something can possibly violate it.** Each claim costs a witness, an audit, and a readout row for all time. A claim that nothing can break has this cost and gives no protection. _We use React_ is a claim, because somebody can add a Svelte component. _We use a monorepo_ is not a claim. This test has no mechanical form, because the correct answer is frequently the unexpected one.

**A decision that nothing can violate is prose.** Write it beside the claims that it explains ([Prose](catalog.md#prose)).

**A review rule is not a claim.** _Do not add unnecessary abstraction_ examines a change, not the codebase. The base commit already has much abstraction, so the induction has no state to start from. Write it again as a state, for example _no interface has exactly one implementation outside tests_. Or keep it in the reviewer runbook, outside the catalog.

**A claim reads alone.** Read it with the glossary, but without its heading and the prose around it. If it is not true or false alone, it depends on context that no audit reads ([Prose](catalog.md#prose)).

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

An amendment is the set of claim changes that one unit of work proposes. Its operations are **add**, **change**, and **delete**. An add names its claim **and** the witness that will attest it. A delete removes the claim and its witnesses in one merge.

**The design occurs when you name the witness.** _The dashboard never writes_ is only a wish until you ask what will examine it. The answer is a database handle with no write method, and that answer changes the design.

**An amendment is a specification. It does not stop changes.** When you write the witness, you complete the claim. This frequently shows that the claim did not say what it meant. This is correct, and the builder escalates it ([Escalation stops the build](readout.md#escalation-stops-the-build)).

- **Set coverage while the amendment is still text.** An amendment that regroups witnesses makes new sets of witnesses. Early, this costs one paragraph. After the build, it cost two audit rounds and rework.
- **Read the amendment as one thing.** Two claims can each be correct and be impossible together. All other checks examine one claim at a time. The contradiction usually shows first in the coverage prose.
- **Name the seam, and tell what it replaces.** The production work that a claim needs has no artifact. _This has no claim_ does not mean _this is small_.
- **Tell when a witness is written before its subject.** A witness can deny a name that a later amendment will make. The witness cannot tell this, so the amendment tells it.
- **The amendment stays proposed until the merge.** The merged code shows what somebody built. Only the amendment shows what somebody wanted.

> [!NOTE] **Under watch: an amendment has no version.** Three amendments were written under a rule that was later retracted, and nobody enacted them. Nothing marked them as old. Somebody remembered, and revised them three sessions later. This stops being important when crux becomes stable.

## Fog

Fog is material that you want but cannot yet write as a claim. It is not a verdict and not a colour, because it is not in the catalog.

> **Fog is clear when you can write the claim and select its witness.**

| State                  | Can you write the claim? | Held by | Exit                                       |
| ---------------------- | ------------------------ | ------- | ------------------------------------------ |
| **fog**                | no                       | cairn   | examine the checkout, or examine the world |
| **proposed amendment** | yes                      | branch  | the merge                                  |

- **A fog item records what will clear it.** One search and a temporary deployment have very different costs. This field lets you sort the queue.
- **Fog is inability, not unwillingness.** A claim that you can write is an amendment that you did not write yet. _Not now_ makes no artifact.
- **Fog can clear into nothing.** It can become a decision that nothing can violate. It can also become a question that an existing claim already answers. In both cases, it closes, and nothing enters the catalog.
