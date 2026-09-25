# Runbook — the reviewer

Part of the crux framework documentation. [Contents](../README.md) · [Abstract](../../ABSTRACT.md)

You did not build this. That is the whole of your authority and the reason the role exists: a builder that writes
its own witness decides its own success ([§8.6](../review.md#86-why-the-canvass-and-the-audit-stay-separate)).

You do two acts and produce one page. **Canvass** every witness in scope for a verdict. **Audit** the instruments
in scope to set their standings and each claim's coverage. Both go into one readout
([§8.4](../review.md#84-the-readout)).

This runbook is procedure. It carries no history and no rejected alternatives — follow a link when you want the
argument.

---

## 1. Fix the scope before you read anything

> **Audit scope** = markers whose instrument changed in this diff **∪** markers that attest a claim whose text
> changed in this diff.

The first term is the one that matters most: it catches a builder that added a witness to a claim **not** in the
amendment, which is exactly where a builder defines its own success
([§8.5](../review.md#85-the-audit-and-why-it-needs-no-cache)).

A change inside a witness's `@scope` does **not** put it in the audit scope. That voids a verdict, not a standing
([§6.5](../format.md#65-scope-and-who-needs-it)).

## 2. Work claim by claim, never marker by marker

Collect every witness of one claim. Set a standing for each one in scope, then read the whole set together and set
the coverage. Marker order is the wrong order, because coverage is not visible from it
([§5.8](../claims.md#58-coverage)).

Reading coverage needs **every** witness of the claim, including the ones that did not change. You read them; you
set no standing on them.

## 3. For each witness — does this instrument support this claim?

Read the instrument and the code in `@scope`. Read the glossary that governs its words beside them
([§3.2](../vocabulary.md#32-crux-does-not-read-it)).

- **Sound does not mean sufficient.** A witness may support part of a claim and be sound
  ([§5.6](../claims.md#56-the-standing)). Reaching the whole claim is the next question, not this one.
- **Check what it observes, not only what it asserts.** A witness that watches an invocation result, an HTTP
  status, or a job outcome may be watching a proxy that reports success when the subject failed
  ([§5.3](../claims.md#53-subject-and-instrument)). Ask where the observation happens.
- **A standing belongs to a marker and a claim together.** One marker attesting three claims has three standings,
  and they can differ. The repair for one bad pair is to remove that one `@attests`.

You may set **unsound** on your own authority. Finding a bad witness needs none.

## 4. For each claim — do the witnesses together reach it?

- **Run this check first on any claim whose witnesses are all prohibitions.** A witness that closes one way to
  fail does not affirm the way to succeed, and the set of wrong implementations is unbounded
  ([§5.8](../claims.md#58-coverage)).
- **Look for the reverse mismatch too.** A witness that reaches **further** than its claim is invisible to
  coverage and means the claim is under-stated. Repair the claim, not the witness.
- **Re-cover every regrouped set.** If the amendment combined, split, or moved witnesses between claims, each
  affected claim has a new set and an old coverage judgment that no longer applies
  ([§7.1](../lifecycle.md#71-judge-coverage-while-the-amendment-is-still-text)).
- **Where a claim crosses two stateful systems**, find the commit point its witnesses observe and ask what is true
  between them. A guarantee that is not implementable with the declared primitives is not an under-covered claim —
  it is a claim to lower.

You may declare **under-covered** on your own authority. You **propose** covered; the operator confirms it.

## 5. Adversarial reading

The tests assert the ordinary path. The claims promise the edges. Neither the runner nor the repository gate finds
that gap, because both ask the subject and the failure is in the instrument.

- Force the mechanism the claim names, rather than accepting any passing path.
- Hit every boundary from both sides.
- Prove that a refusal had no side effect.
- **Run the adversarial case against the framework, not only against the code.** _What does this dependency do
  when my code fails inside it?_ has no home in the model, and it is where a proxy observation hides.
- **Break the mechanisms the build added.** Machinery introduced during the build — a generated identifier, a
  retry, an index — can be load-bearing and attested by nothing, because the audit walks from claims to witnesses
  and never the other way. Aim this at the diff, not at the file.

## 6. Expect more than one round

A repair changes the instrument, and a changed instrument is back in scope. A second audit is the normal case, not
a defect ([§8.5](../review.md#85-the-audit-and-why-it-needs-no-cache)). The first build needed two rounds on one
package, and the second round found a real gap.

## 7. Hand the operator one page

Order it by claim: the claim text first, the outcome second. Do not sort by witness kind or by colour
([§8.4](../review.md#84-the-readout)).

A red item never reaches the operator — it is a stop, and the work returns to the builder. What reaches the ruling
is exactly three kinds of open item: yellow verdicts, standings you proposed as sound, and coverage you proposed
as covered ([§9.5](../review.md#95-what-the-human-decides)).
