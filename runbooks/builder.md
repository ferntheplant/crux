# Runbook — the builder

Part of crux. [Specification](../spec/) · [Abstract](../ABSTRACT.md)

You have an **amendment**: the set of claim changes that this unit of work proposes. Make each claim in it true. Leave witnesses that will continue to show it after you go.

This runbook is procedure. It has no history and no rejected alternatives. Follow a link for the reason.

---

## 1. Read the amendment as one thing

Before you write code, read all entries together. Ask if they can all be true at the same time. Crux does not check this, because all other questions examine one claim at a time ([[practice#amendments]]).

- Read the **coverage prose**, not only the claim text. An entry records its assumptions about other entries there, and a contradiction shows there first.
- If two entries cannot both be true, stop and escalate. Do not select one.

## 2. Write the witness before you believe the claim

A claim is a wish until something can examine it. The witness completes the design ([[practice#amendments]]). It is also where a claim frequently shows that it does not say what it means.

Move each claim as high on the ladder as it can correctly go ([[witnesses#the-ladder]]). Ask these questions:

- **Can a rule exist here?** Do not ask _does a rule exist_. If your linter has no rule that fits, a plugin can be forty lines. The instrument that you build keeps the witness one rung higher than a test.
- **Where does the observation occur?** A witness that observes a wrapper, an invocation result, or a job outcome observes a proxy, not the subject ([[witnesses#subject-and-instrument]]). Name the point that you observe. Observe the thing that the claim is about.
- **Is the contract its own?** Do not take the expected value from the subject ([[witnesses#stated-contract]]). Write the allowed shape of a type. Do not derive it with `Omit` from the type that you examine. Do not record a snapshot again to make a test pass.
- **Is the subject bound to the type?** A type witness proves nothing if the code can go around the type.

For an inferential witness, write a _Valid when_ line. Tell what must be true and where to look. The judge reads this line for its triage ([[readout#verdict]]).

## 3. Escalate when the claim is wrong

You cannot change the amendment on your own authority. The operator selects the claims ([[readout#belay]]). Tell the proposed change, and stop ([[readout#^escalation_stops_the_build]]).

This is a normal step. If you escalate on **each** claim, you got fog, not an amendment ([[practice#fog]]).

## 4. Break each witness and make sure that it denies

> **A witness that has never denied has not been tested.**

A type and a lint rule have no failing state before the build ([[witnesses#the-ladder]]). Thus they pass on their first run, if they work or not. **You cannot see the difference in the green result.**

Break each witness on purpose, and make sure that it fails. Write the import that the rule forbids. Add the write that the type prevents. Put in the literal.

In one unit of work, five structural witnesses were all green on the first run. Each was broken and run again. Four denied. The fifth did not. The linter replaces the options of a rule in a directory override, and it does not merge them. Thus a package restriction removed the patterns of the base configuration without a signal.

**Nobody expected the broken one.** The check found it only because it was applied to all five witnesses. Thus this is a step, not a decision.

**This step is yours, not the reviewer's.** It needs none of the independence of an audit ([[readout#separation]]). The witness fails or it does not. The person who wrote it can see this.

## 5. Hand off when the canvass is green

Green is a **handoff**. It does not mean that the work is done ([[readout#^handoff_when_green]]). It tells that the subject satisfies the witnesses. It tells nothing about whether the witnesses support the claims.

Before you hand off:

- [ ] Each promise is a claim in a claims block. No prose that you wrote promises anything ([[catalog#prose]]).
- [ ] Each claim is true or false when you read it alone, without its heading ([[practice#writing-a-claim]]).
- [ ] Each claim in the amendment has a minimum of one witness that attests it alone ([[witnesses#several-claims]]).
- [ ] Each witness that **can** deny has denied once (step 4).
- [ ] Each inferential witness has a _Valid when_ line.
- [ ] The amendment names each witness that you wrote against an artifact that a later amendment will make ([[practice#amendments]]).
- [ ] The amendment names each seam that this build replaced ([[practice#amendments]]).

Somebody who did not build decides if the work is done.
