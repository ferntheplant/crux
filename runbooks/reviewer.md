# Runbook — the reviewer

Part of crux. [Specification](../SPEC.md) · [Abstract](../ABSTRACT.md)

You did not build this work. That is all of your authority, and it is the reason for this role. A builder that
writes its own witness sets its own measure of success
([§8.6](../SPEC.md#86-why-the-canvass-and-the-audit-stay-separate)).

You do two acts and make one page. **Canvass** each witness for a verdict. **Audit** the instruments in scope to
set their standings and the coverage of each claim. Both go into one readout ([§8.4](../SPEC.md#84-the-readout)).

This runbook is procedure. It has no history and no rejected alternatives. Follow a link for the reason.

---

## 1. Set the audit scope before you read

> **Audit scope** = witnesses whose instrument changed in this diff **∪** witnesses that attest a claim whose text
> changed in this diff.

The first term is the most important. It finds a builder that added a witness to a claim that is **not** in the
amendment. That is where a builder sets its own measure of success ([§8.5](../SPEC.md#85-the-audit)).

Compare claim text by slug. A claim that moved to a different file did not change.

A change to the subject does **not** put a witness in the audit scope. It asks the verdict again, and does nothing
more ([§5.3](../SPEC.md#53-subject-and-instrument)).

## 2. Do the triage of the inferential witnesses

Do one pass. Read the diff and the _Valid when_ line of each inferential witness. Return the witnesses that the
diff can affect, and answer them ([§5.5](../SPEC.md#55-the-verdict)).

- Ask if HEAD still satisfies the claim, because the base did. Do not ask if the diff is acceptable alone.
- When you are not sure, ask the witness. A false red costs minutes. A false green is the failure.
- If the diff is too large for a correct triage, refuse it. Each inferential witness is then silent.

## 3. Work claim by claim, never witness by witness

Collect all the witnesses of one claim. Set a standing for each witness in scope. Then read the full set together
and set the coverage. The order of the witnesses is the wrong order, because it does not show coverage
([§5.8](../SPEC.md#58-coverage)).

Coverage needs **all** the witnesses of the claim, and this includes the witnesses that did not change. Read them.
Do not set a standing on them.

## 4. For each witness: does this instrument support this claim?

Read the instrument and the code that it examines ([§5.6](../SPEC.md#56-the-standing)).

- **Sound does not mean sufficient.** A witness can support part of a claim and be sound. The full claim is the
  next question.
- **Is its contract its own?** A type derived from the subject, a snapshot recorded again, or a fixture made from
  the current output takes its expected value from the subject. That witness is unsound.
- **What does the instrument depend on?** Name each shared helper or type utility that gives the witness its
  meaning. Decide if it is stable enough to trust.
- **Examine what it observes, not only what it asserts.** An invocation result, an HTTP status, or a job outcome
  can be a proxy that reports success when the subject failed
  ([§5.3](../SPEC.md#53-subject-and-instrument)). Ask where the observation occurs.
- **A type witness must bind its subject.** Can the code go around the type? If yes, the witness tells nothing.
- **A standing belongs to a witness and a claim together.** One witness that attests three claims has three
  standings, and they can be different. To repair one bad pair, remove that one `@attests`.

You can set **unsound** on your own authority. To find a bad witness needs no authority.

## 5. For each claim: do the witnesses reach it together?

- **Do this check first on a claim whose witnesses are all prohibitions.** A witness that removes one way to fail
  does not affirm the way to succeed ([§5.8](../SPEC.md#58-coverage)).
- **Ask: if somebody adds a new one, which witness sees it?** A new handler, route, or renderer in a new file is
  outside each existing witness. A lint rule, a type, or a test that examines all instances reaches it.
- **Look for the opposite mismatch.** A witness that reaches **more** than its claim means that the claim is too
  small. Repair the claim, not the witness.
- **Set the coverage again for each regrouped set.** If the amendment joined, split, or moved witnesses, each
  claim that it touched has a new set. The old coverage is not valid for it
  ([§7.1](../SPEC.md#7-the-amendment)).
- **Where a claim crosses two stateful systems**, find the commit point that its witnesses observe. Ask what is
  true between the two systems. If the claim cannot be true with the declared primitives, it is not under-covered.
  Make the claim smaller ([§16](../SPEC.md#16-under-watch), W3).

You can set **under-covered** on your own authority. You **propose** covered, and the operator confirms it.

## 6. Read against the work

The tests assert the usual path. The claims promise the edges. The runner and the repository gate cannot find
that gap. Both ask the subject, and the failure is in the instrument.

- Force the mechanism that the claim names. Do not accept any passing path.
- Test each limit from both sides.
- Prove that a refusal had no side effect.
- **Test the framework, not only the code.** _What does this dependency do when my code fails in it?_ has no
  place in the model, and a proxy observation hides there.
- **Break the mechanisms that the build added.** A generated identifier, a retry, or an index can be load-bearing,
  and no witness observes it. The audit goes from claims to witnesses, and never in the other direction
  ([§16](../SPEC.md#16-under-watch), W4). Aim this at the diff, not at the file.

## 7. Expect more than one round

A repair changes the instrument, and a changed instrument is in scope again. A second audit is normal
([§8.5](../SPEC.md#85-the-audit)). The first build needed two rounds on one package, and the second round found a
real gap.

## 8. Give the operator one page

Put it in the order of the claims: the claim text first, then the result. Do not sort by rung or by colour
([§8.4](../SPEC.md#84-the-readout)).

A red item never goes to the operator. It is a stop, and the work goes back to the builder. The ruling gets only
three types of open item: silent verdicts, standings that you proposed as sound, and coverage that you proposed as
covered ([§9.5](../SPEC.md#95-what-the-human-decides)).
