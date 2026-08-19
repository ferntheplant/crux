# Runbook — the builder

Part of the crux framework documentation. [Contents](../README.md) · [Abstract](../../ABSTRACT.md)

You have been given an **amendment**: the set of claim changes this unit of work proposes. Your job is to make
every claim in it true, and to leave behind witnesses that will still say so when you are gone.

This runbook is procedure. It carries no history and no rejected alternatives — follow a link when you want the
argument.

---

## 1. Read the amendment as one thing

Before writing code, read every entry together and ask whether they can all be true at once. Nothing in crux
checks this: every other question is scoped to a single claim
([§7.2](../lifecycle.md#72-read-the-amendment-as-a-whole)).

- Read the **coverage prose**, not only the claim text. That is where an entry writes down its assumptions about
  its neighbours, and it is where a contradiction shows first.
- If two entries cannot both hold, stop and escalate. Do not pick one.

## 2. Write the witness before you believe the claim

A claim is a wish until something can judge it. Writing the witness is what finishes the design
([§7](../lifecycle.md#7-the-amendment)), and it is where a claim usually turns out not to say what it meant.

Push each claim as far up the ladder as it honestly goes
([§5.2](../claims.md#52-the-four-kinds)), and ask two questions rather than one:

- **Can a rule exist here?** Not _does one exist_. If your linter ships nothing that fits, a plugin may be forty
  lines. Building the instrument keeps the witness a rung higher than settling for a test.
- **Where does the observation happen?** A witness that watches a wrapper, an invocation result, or a job outcome
  is watching a proxy and not the subject ([§5.3](../claims.md#53-subject-and-instrument)). Name the point you
  observe, and observe the thing the claim is about.

## 3. Escalate when the claim is wrong

You cannot change the amendment on your own authority; the choice of claims belongs to the operator
([§9.5](../review.md#95-what-the-human-decides)). State the proposed change and stop
([§9.1](../review.md#91-the-sequence), step 3).

This is a first-class step and not an escape hatch. Escalating on **every** claim means you were given fog rather
than an amendment ([§10](../lifecycle.md#10-fog)).

## 4. Break every witness and watch it deny

> **A witness that has never denied is a witness nobody has tested.**

Three of the four kinds have no failing state before the build
([§5.2](../claims.md#52-the-four-kinds)), so a structural witness — a type assertion, a lint denial — passes on
its first run whether it is working or doing nothing at all. **The two cases are indistinguishable from the
green.**

So break each one deliberately and watch it fire. Write the import the rule forbids. Add the write the type is
supposed to prevent. Paste in the literal.

Five structural witnesses in one unit of work were all green on the first run, and four of the five would have
been green doing nothing. Each was broken and re-run. Four denied. The fifth did not: the linter replaces a rule's
options in a directory override rather than merging them, so a package-specific restriction had silently dropped
the patterns the base configuration set.

**The one that was broken is the one nobody would have thought to check.** It was found only because the check was
applied to all five rather than to the ones that looked risky. That is why this is a step and not a judgment call.

**It belongs to you and not to the reviewer.** Breaking your own witness needs none of the independence an audit
needs ([§8.6](../review.md#86-why-the-canvass-and-the-audit-stay-separate)) — the witness either fires or it does
not, and the person who wrote it can watch it fire.

## 5. Hand off when the canvass is green

Green is a **handoff**, not doneness ([§9.2](../review.md#92-the-builders-exit-condition)). It says the subject
satisfies the witnesses. It says nothing about whether the witnesses support the claims, and an unsound witness or
an under-covered claim is red at the ruling.

Before you hand off:

- [ ] Every claim in the amendment has at least one witness that attests it alone
      ([§5.7](../claims.md#57-witnesses-that-attest-several-claims)).
- [ ] Every witness that **can** deny has been shown to deny (step 4).
- [ ] Every `@scope` matches a file that exists.
- [ ] Any witness written against an artifact a later amendment will create is called out in the amendment
      ([§7.4](../lifecycle.md#74-a-witness-may-be-written-against-an-artifact-that-does-not-exist-yet)).
- [ ] Any seam this build displaced is named in the amendment
      ([§7.3](../lifecycle.md#73-name-the-seam-and-say-what-it-displaces)).

Doneness is decided by somebody who did not build.
