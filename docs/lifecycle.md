# Fog, amendment, rationale

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

## 7. The amendment

An amendment is the set of claim changes that one unit of work proposes. It is the specification.

Its operations are **add**, **change**, and **delete**. Each entry names a claim, and for an add it also names the
witness that will attest it.

A delete removes the claim and its witnesses in the same merge.

**Naming the witness is not bookkeeping. It is where the design happens.** An add that names its claim and leaves
the witness for later has recorded a wish, and the wish is usually wrong in a way only the witness reveals. _The
dashboard never writes_ is a sentence about intent until you ask what would judge it, and the answer is a decision
about who constructs the database handle — a type with no write method, which is a kind 1 witness and a change to
the design. _The token is random_ is trivially statable and untestable as stated. In both cases the claim as
written would have shipped something worse.

So the witness field is the step that finishes the claim, and §10's exit gate already says so: fog is clear when
you can write the claim **and** assign its witness. An amendment is authorable at exactly that moment and not
before.

**An amendment is a specification, not a freeze.** Naming a witness starts the design. **Writing** the witness
finishes it, and that happens during the build. The first package built under this vocabulary entered with eleven
claims and left with fourteen, and every change came from the same place: somebody wrote the witness and found
that the claim did not say what it meant.

| What the build found                                        | What that build did | What [§5.8](./claims.md#58-coverage) says to do |
| ----------------------------------------------------------- | ------------------- | ----------------------------------------------- |
| a prohibition cannot affirm a provenance                    | split the claim     | add a witness                                   |
| _required_ has a compile-time and a runtime reading         | split the claim     | add a witness                                   |
| _expires after seven days_ did not settle the exact instant | reword the claim    | reword the claim                                |

Read the last two columns together. The **finding** was right all three times, and the build was working. The
**remedy** was wrong twice, because the rule of the day forced a split where a second witness was the honest
answer. [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see) records that correction. What matters here is the first column: all three were invisible until
somebody wrote the witness.

The third row is the plainest. The test could not be written until somebody ruled whether the boundary instant is
inside or outside. That is fog (§10) that the amendment did not know it held, and it surfaced when a person tried
to write the assertion.

**So a build that changes its amendment is the design working, not the entry gate failing.** What it needs is a
route: the builder cannot make that change alone, because [§9.5](./review.md#95-what-the-human-decides) gives the choice of claims to the operator. [§9.1](./review.md#91-the-sequence)
carries the route. This is half of why belay receives an amendment rather than a branch with failing witnesses — a
gate demanding a complete and correct amendment asks for something no design phase can deliver
([§15](./roadmap.md#15-open-threads), closed).

### 7.1 Judge coverage while the amendment is still text

An amendment that **regroups** existing witnesses — combining two claims, splitting one, moving a witness from one
claim to another — creates a new witness set for every claim it touches, and every new set needs a coverage
judgment ([§5.8](./claims.md#58-coverage)). Take that judgment in the amendment, before it freezes the shape of the
work.

The cost is asymmetric, and it was measured in both directions.

**Taken late**, on an amendment that combined six claims into two and scheduled the coverage audit after the gate:
two audit rounds and rework. The first found a boundary value that passed every witness. The second found two
claims whose witnesses observed a test-only path rather than the production one. The repository edits had happened
before the judgment that constrained them.

**Taken early**, on the next two amendments: a paragraph each. It still found three gaps before a line was
written — a claim whose witnesses were all prohibitions ([§5.8](./claims.md#58-coverage)), a witness that
enumerated which routes exist where the claim was about what they return, and a shared marker that was the sole
proof of two claims ([§5.7](./claims.md#57-witnesses-that-attest-several-claims)).

This moves a required judgment earlier. It does not automate it.

### 7.2 Read the amendment as a whole

**Two claims of one amendment can contradict each other, and nothing in the model looks for it.** Coverage asks
whether a claim's witnesses reach that claim. A standing asks whether one instrument supports one claim. The form
checks are scoped to one directive. Every question is scoped to a single claim — and the amendment, which is the
natural unit, is the one artifact with no consistency question attached to it at all.

One amendment held _the send happens at the send hour and at no other hour_ beside _a failed send is recorded_,
whose coverage note spoke of suppressing every retry for that local date. Under a strict hourly gate there is no
retry inside the local date, so the second claim's own reasoning was false. Under a loose gate the first claim's
second clause was false. Both were defensible alone. Neither could be built without deciding the other.

**The tell was in the coverage prose, not in either claim.** _Every retry for that local date_ presupposes retries
that the sibling claim forbids. Coverage prose is where an amendment writes down its assumptions about its
neighbours, and it is nobody's job to read it against them.

[§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see) records the same failure one level up: claims
individually defensible and collectively wrong, found by an operator who had to read the catalog. This is the
amendment-sized version, and the escalation route settled it — the builder stated the proposed change and stopped,
and one exchange produced a better claim than either reading. Two of the three options on the table would have
shipped a real defect.

### 7.3 Name the seam, and say what it displaces

An amendment specifies claims. The production work that a specification implies has no artifact, and that is
correct: nothing about a service split is falsifiable that the claims do not already say. The failure is reading
_this carries no claim_ as _this is small_.

Two amendments each noted that a capability seam "carries no claim of its own" and would land in whichever merged
first. Building it moved a type off the package's public entrypoint, rewrote every existing witness in that
package to go through the new services, rebuilt a table so a row could exist in a state it previously could not,
dropped a stored column in favour of a derived value, changed how the package's exports resolve, and added a
migration. None of it changed a claim. All of it was required before two of six claims could be witnessed at all.

The repair is a line in the amendment rather than a new artifact: **name the seam, and name what it displaces.**

### 7.4 A witness may be written against an artifact that does not exist yet

Amendments are sequenced, so this happens in any repository with more than one of them open. A claim promising
that a set of routes exposes no history was witnessed by an import restriction naming the entrypoint that carries
the list operation — and that entrypoint belongs to the **next** amendment.

| Option                                  | Verdict                                                                       |
| --------------------------------------- | ----------------------------------------------------------------------------- |
| create the artifact now, empty          | rejected — dead code, and the repository's own tooling is right to say so     |
| defer the witness to the later merge    | rejected — the claim would enter the catalog under-covered on purpose         |
| deny what exists **and** what is coming | taken — restrict the entrypoint that exists, name the identifiers that do not |

A restriction on a name that nothing exports yet is legal and silent until the day it is not. That is the right
trade, because the rule holding a witness in place is worth least on the day it is written and most on the day
somebody else adds the thing it forbids.

Crux has a word for a claim that cannot yet be stated (§10) and for one stated but unbuilt (an amendment). It has
none for a witness whose subject is in the future, and it needs none — but **an amendment says plainly when a
witness is written forward**, because the marker cannot say it.

### 7.5 The amendment is proposed until the merge

**The merge enacts it.** Do not merge an amendment to the main branch on its own. The rule that decides this:

> Keep a pre-work artifact only if it carries information that the post-work artifact cannot reconstruct.

An amendment qualifies. The merged code shows what was built. Only the amendment shows what was **wanted**.

## 10. Fog

Fog is material you want, but cannot yet state as a claim.

**Fog is not a verdict, and it is not a fourth colour.** A verdict is a report about a claim. Fog has no claim, so
there is nothing to report. The colour scale measures the catalog, and fog is not in the catalog.

But fog is not outside this framework either. The framework owns two things about it:

- **The definition.** Fog is the negative space of the catalog. You cannot define it without the definition of a
  claim.
- **The exit gate.** Fog is clear when you can write the claim **and** assign its witness. That is the same test as
  _is the amendment authorable_. It is the main exit and not the only one; see §11.6.

Cairn owns the rest: where fog is stored, how it is cleared, and the record of the route taken.

**There are exactly two states outside the catalog:**

| State                  | Can you write the claim? | Who holds it | Exit                               |
| ---------------------- | ------------------------ | ------------ | ---------------------------------- |
| **fog**                | no                       | cairn        | ask the checkout, or ask the world |
| **proposed amendment** | yes                      | the branch   | the merge                          |

So a tracker needs no third object. Fog discharges into an amendment, or — when what it resolves into is settled
by construction — into a rationale. The amendment discharges into the catalog. **A rationale is not a third
state**, because it is answered rather than pending, and it leaves the tracker for the repository at the moment
it is written.

**The exit cut is _inside the checkout or outside it_, and not _judgment or evidence_.** An earlier draft used the
second pair and it predicted nothing, because the two are not alternatives: a real item is often both, with an
agent gathering the facts and a human making the call. What varies by orders of magnitude is where the fact lives.

| Fog item                                  | Cleared by                            | Cost         |
| ----------------------------------------- | ------------------------------------- | ------------ |
| _is this dependency already pinned?_      | one read of the lockfile              | seconds      |
| _which of these two tools do we adopt?_   | reading two repositories, then a call | two fetches  |
| _does this handler support that API?_     | deploying a throwaway service         | hours        |
| _can this subdomain be onboarded safely?_ | a rehearsal on a spare domain         | an afternoon |

The top two are answerable from a checkout and the bottom two are not, and that line sorts the queue when
_judgment or evidence_ leaves all four looking alike. This is [§4.1](./claims.md#41-the-subject-must-be-rederivable-from-the-repository)'s boundary arriving a third time, from an
unrelated direction.

**So a fog item records what would clear it.** Not as documentation — as the field that makes the queue sortable.
Without it, an item answerable in one grep and an item requiring a deployment are indistinguishable, and the human
picks by guessing. Cairn holds the field; this document only says why it must exist.

**Fog is defined by inability, not by absence of effort.** A claim you can write but have not written yet is not
fog. It is an unwritten amendment. If you allow that mistake, fog becomes a backlog, and it stops being a signal
that a human is needed.

**And _not now_ produces nothing at all.** It is not fog, because the test is inability rather than unwillingness.
It is not an amendment and it is not a rationale, because §11.2's three tests reject writing one for a refusal —
it is cheap to reverse, nobody would be surprised, and no alternative was examined. A framework that emitted a
ticket here would be worse. The absence of an artifact is the feature.

## 11. Rationale

The catalog says what the system promises. It does not say why. A **rationale** is the document that does.

**A conventional ADR holds both halves** — the decision (_we will use React_) and the reasoning (_because X,
having rejected Svelte for Y_). Crux takes the first half away and puts it in the catalog as a present-tense
claim with a witness. What is left in the document is only the second half.

So the artifact is not an ADR, and it does not carry that name. See [§2.2](./vocabulary.md#22-words-that-were-rejected-and-why).

### 11.1 What a rationale grounds

> **`@grounds <slug>` — this rationale grounds that claim.**

Repeatable. One rationale often grounds several claims that were decided together.

The direction is deliberate, and it is the direction everything else already points. A marker names its claims; a
rationale names its claims; the catalog names nothing. **The claim slug is the hub.** Three results:

- **The catalog stays undecorated.** A claim is falsifiable prose and a slug, and it does not carry a list of
  documents that would rot.
- **The link sits on the stable side.** Rationale is written once. Claims churn.
- **The reverse index is derived**, so _show me this claim's reasoning_ costs nothing to maintain.

The query it answers is **why is this claim the way it is, and what was rejected on the way?** It is asked at the
ruling, and it is asked at amendment time, before proposing a change to a claim that somebody already reasoned
about.

**Prose that mentions a slug is not a grounding.** A rationale may discuss a claim in passing, or contrast it with
another. The directive is what separates _this document is about that claim_ from _this document mentions it_, and
resolving that without intelligence is the [§6.4](./format.md#64-what-is-not-a-directive) test.

**A `@grounds` list that grows over time is a signal**, and it is the reason §11.2 says to cite only the claims
that exist rather than every claim you expect. A document that accumulates groundings across several merges is one
whose decision turned out to be load-bearing across the system. Writing the full list upfront destroys that
information, and it is not recoverable afterwards.

### 11.2 When to write one, and when to ship it

All three must be true. If any is missing, skip it.

1. **Hard to reverse** — the cost of changing your mind later is real. If it is cheap to reverse, you will just
   reverse it.
2. **Surprising without the reasoning** — a future reader will look at this and ask why. If nobody would wonder,
   nobody will read the document.
3. **The result of a real trade-off** — there were genuine alternatives, and you picked one for stated reasons. If
   there was no alternative, there is nothing to write beyond _we did the obvious thing_.

**And it names what was neglected.** This is a requirement on content, not on form. The rejected option is the
half that the code cannot show you, so a rationale that omits it has recorded only what was already visible.

**A rationale ships with the claims it grounds, and never ahead of them.** A rationale explains why a claim _reads
as it does_. If the claim does not exist, the document is explaining a decision nobody has enacted — which is
intent, and intent is the amendment's job. A rationale committed ahead of its claim is doing the amendment's work
in the wrong artifact, so [§6.6](./format.md#66-form-errors) makes a `@grounds` that names no declared claim a form error.

The mechanism needs nothing new. A claim is **declared** by `@claim` on the branch, and [§4](./claims.md#4-claims-and-the-catalog)'s condition — a sound
witness affirms it — is enforced at the merge rather than at declaration. So the claim, its witness, and its
rationale are authored on one branch and land in one merge, and `@grounds` resolves the whole time.

**When a rationale grounds claims that land in different merges, write it as soon as the first one needs it.** This
is common rather than exotic: one thought often spans a layer boundary, and the packages on either side are built
separately. The alternative — hold the document until the last claim it grounds — keeps the check strict and hides
the reasoning during exactly the window when a reader goes looking, because some of its claims are already live. A
later amendment adds its own `@grounds` line to the existing document. Nothing dangles, the check stays strict, and
the reasoning is available from the first merge.

**The failure this accepts, stated plainly.** The later amendment's author may not find the earlier rationale, and
then the document under-grounds: it is about that claim, does not cite it, and _why is this claim the way it is?_
returns nothing when an answer exists.

This is uncheckable by construction. §11.1 already says a machine cannot tell _this document is about that claim_
from _this document mentions it_ — that is why `@grounds` exists at all. No form check reaches it and no audit
reaches it, because a rationale is not an instrument. It is survivable because §11.3 already accepts that some
rationales are found by reading the directory rather than by index, so under-grounding degrades to a baseline this
document already tolerates. The case to watch is the layer boundary, since that is where correlated claims reliably
land in different merges.

### 11.3 Settled by construction, and the index that does not exist

**A claim exists to be checked.** That is its whole purpose — [§4](./claims.md#4-claims-and-the-catalog) admits it to the catalog only when sound
witnesses affirm and cover it, and [§5.2](./claims.md#52-the-four-kinds) pushes each one as far up the ladder of witnesses as it honestly goes. So
there is no point claiming something that nothing can violate, because the check can never do any work.

Most decisions leave a claim behind. _We will not do X_ is usually _we do Y instead_, and Y is claimable. But one
class is not, and it is not an edge case:

> **Claim it if something could plausibly violate it. Otherwise write only the rationale.**

**This is the gap a rationale fills.** Something that should be recorded, and that no witness could ever
usefully check.

| Decision               | Could anything violate it?      | Result                  |
| ---------------------- | ------------------------------- | ----------------------- |
| we use React           | someone adds a Svelte component | claim + lint witness    |
| squash merges only     | someone lands a merge commit    | claim + ruleset witness |
| no AWS, for compliance | someone adds the SDK            | claim + lint witness    |
| we use a monorepo      | no                              | **rationale only**      |
| _canvass_, not _poll_  | no                              | **rationale only**      |

This is a stricter bar than [§14](./roadmap.md#14-the-properties) property 2, which asks only that a claim be falsifiable. Falsifiable **in principle**
and **plausibly violable** are different, and the catalog pays for the second one: every claim needs a witness,
and every witness carries audit cost forever ([§8.5](./review.md#85-the-audit-and-why-it-needs-no-cache)) and a row on every readout. A claim that nothing can break
buys a permanent cost with no protection.

**And the question has no mechanical form. Do not build one.** _Claim, or settled by construction?_ goes the
counter-intuitive way often enough that any rule you write will be wrong. _Use two workers, not one_ reads like a
claim and is not — nothing drifts into one worker by accident. _The dashboard reads data only_ reads like
documentation and is the most important claim in its project. Getting the pair backwards produces a catalog of
unfalsifiable statements that costs audit time forever, while missing the one property worth protecting. It is a
judgment, it stays a judgment, and [§13.3](./roadmap.md#133-the-dogfooding-rule) files it permanently under _never automate_.

So a decision that is settled by construction gets a rationale and no claim. [§2.2](./vocabulary.md#22-words-that-were-rejected-and-why) of this document is one. So is
the deleted file-level marker in [§6.2](./format.md#62-the-extent-of-a-marker) and the deleted failure classifier in [§9.2](./review.md#92-the-builders-exit-condition).

**These rationales have no index, and that is deliberate.** A rationale with no `@grounds` is invisible to crux.
There is nothing to key it on — the whole point is that no claim exists — and a shared bucket to hold them all
would be a pile rather than an index, no cheaper to search than the directory itself.

**They are found by reading.** The retrieval mechanism for a settled-by-construction rationale is an agent
reading `docs/rationale/` at the start of work, over descriptive filenames, in a directory that §11.2 keeps small
by construction. Do not build an index for this. Two facts make reading sufficient:

- The set is small, because most decisions do leave a claim.
- The question it answers — _has anyone already considered X?_ — is a **search**, not a lookup. No index keyed on
  claims could ever have answered it.

### 11.4 The format

A rationale is a Markdown file. There is no numbering: uniqueness comes from the filename and ordering comes from
git, and _scan the directory and add one_ collides the moment two branches both write `0038`.

```md
> @grounds belay/close/ordered-not-atomic crux-ignore

# Closing is ordered, not atomic

Closing deletes before it cleans, and a partial close leaves the blockers behind.

The rejected option was a transaction around both steps. It was dropped because the blocker store is not in the
same database, so the transaction would have been a lie that only showed itself under load.

## Consequences

A failed close is re-runnable, so every caller must tolerate a second call.
```

**The whole document can be one paragraph.** The value is that the reasoning was written and that the neglected
option was named. It is not in filling in sections. Add `## Consequences` only when a downstream effect is not
obvious. Add a status only when a rationale is superseded, and then it names its successor and stays where it is.

The title states the decision in the present tense, which is the tense of a claim.

### 11.5 A dangling grounding, in both directions

A `@grounds` can name a claim that does not exist for two opposite reasons, and they are not the same failure.

| Direction           | Cause                         | Result                                             |
| ------------------- | ----------------------------- | -------------------------------------------------- |
| **forward dangle**  | the claim is not declared yet | **form error.** [§6.6](./format.md#66-form-errors) |
| **backward dangle** | the claim was deleted later   | **reported, not an error**                         |

**The backward case is reported and it is not a form error.** Making it fail would force editing history, and the
document is still true — it records reasoning that was correct when it was written. What the report says is that a
decision was reversed and nothing yet records the reversal, which is the one decay in this artifact that a machine
can see.

**The forward case is an error**, for the reason [§6.6](./format.md#66-form-errors) states generally: a form error must be fixable by the person
who caused it, at the moment they caused it. A forward dangle is preventable while writing, and §11.2 says how —
the claim, its witness, and its rationale land in one merge. A backward dangle is created by a later merge that
cannot reach back into a document that was true when written.

### 11.6 Fog can discharge into a rationale

§10 gives fog one exit: write the claim and assign its witness. There is a second, and it is the minority path.

> **Fog that resolves into a decision nothing can violate discharges into a rationale.**

Without it, fog can only leave by becoming a claim, so anything investigated and settled by construction stays in
the fog forever — and §10 says that is how fog turns into a backlog and stops being a signal that a human is
needed.

Both exits need the same authority. Deciding is a specification decision, and [§9.5](./review.md#95-what-the-human-decides) says those belong to the human.
