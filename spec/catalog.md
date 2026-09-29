---
catalog: 1
---

# The catalog

The catalog tells what the codebase promises now. It is the one artifact of crux. This file tells how a catalog is written and how crux reads it.

The catalog is ordinary Markdown. Crux reads its structure — frontmatter, headings, claims blocks, and links — and never the meaning of its sentences. A catalog file named `sending.md` looks like this:

```md
## Schedule

Prose that tells what this part of the product does and why.

> [!CLAIMS] Claims
>
> - **Retry.** A failed attempt is tried again on each later fire until one returns or the local date ends.
> - **Send hour.** The first fire at or after the configured send hour attempts the prompt, and no earlier fire does. ^send_hour
```

The first claim has the slug `sending#^retry`, from its label. The second has the slug `sending#^send_hour`, from its pin. A witness attests them as `@attests sending#^retry`, and prose cites them as `[[sending#^retry]]`.

## Condition

> **A claim is in the catalog only if its witnesses affirm it, each witness is sound, and together they cover it.**

A claim can fail in three places. A witness can deny it. A witness can support nothing. The set of witnesses can be too small. Thus the condition has three parts: the [[readout#verdict]], the [[readout#standing]], and the [[readout#coverage]].

- **A claim enters the catalog in the merge that makes it true.** It never enters earlier. There is no `pending` status. Intent is an amendment ([[practice#amendments]]) or fog ([[practice#fog]]).
- **The condition holds by induction.** The claim satisfies it when it enters. Each merge keeps it, because each merge audits what it changed ([[checks#scope]]). Thus a reviewer reads only the diff.

Belay enforces the condition. A red item stops the work before a human sees it ([[readout#^red_stops]]).

## Catalog files

A catalog file is an ordinary document. It has a title, then prose that tells what the file contains, then its sections. That opening prose is the charter of the file. A reader can compare a section with the charter and decide if the section belongs.

> [!CLAIMS] Claims
>
> - **Declared by frontmatter.** A Markdown file is a catalog file only when its frontmatter has the key `catalog`. Crux never selects a catalog file by its path.
> - **Format version.** The value of `catalog` is a positive integer: the version of the format that the file uses. ^format_version
> - **Versions can mix.** One repository can hold catalog files of different versions. Crux reads each file by its own version.
> - **The title declares nothing.** The H1 of a catalog file and the text before its first H2 hold no claim.
> - **File name.** The file part of a slug is the name of the catalog file without `.md`. Two catalog files with one name are a collision, in any directories. ^file_name
> - **Unique headings.** The text of each heading is unique in its file, when compared in lowercase with each space as `-`. ^unique_headings

The frontmatter key is a word of the format, not the name of a tool ([[glossary#tool-names]]). A directory rule would make crux resolve a path, and crux never resolves a path.

### Versions

> **This specification defines version 1.** Each catalog file in this repository has `catalog: 1`.

The format goes into repositories that you do not control ([[tools#tool-set]]), so a change to the format is a migration that somebody else must run. A version in each file tells a tool which rules to read it by, and it lets a repository migrate one file at a time.

**The version is in the file, not in a configuration file.** Crux reads nothing that does not come from a file it already examines, and a version that sits beside the claims moves with them.

**The version names the format, not a tool.** A crux release can read several versions. Its release number is not in the format, for the same reason as each tool name ([[glossary#tool-names]]).

**Increase the version only for a change that alters how an existing file reads.** Examples are a change to the derivation of an id, to the form of a slug, or to what a claims block holds. Each is a change after which the same file gives a different slug, a different claim, or a different form error. A new rule that no existing file can break does not increase the version.

**The witness side has no version.** An `@attests` token is a slug, and the catalog file that declares the slug sets the rules that make it. A version change that changes slugs is thus a rename, and rename detection applies to it ([[checks#^rename]]).

| Version | Change                                                                              |
| ------- | ----------------------------------------------------------------------------------- |
| 1       | The first version: claims blocks, labels and pins, `file#^id` slugs, and wikilinks. |

**The file name, not the path**, because a directory move then renames nothing. Markdown-oxide and Obsidian resolve a link by the file name in the same way.

**Headings are unique in a file** because a link to a heading names only one heading, not the headings above it. Markdown-oxide compares heading text in lowercase with each space as `-`, and crux uses the same rule. Thus `[[witnesses#stated-contract]]` resolves in an editor and in crux.

## Claims

A claim is one item in a claims block. It is the only thing in the catalog that promises anything.

> [!CLAIMS] Claims
>
> - **Claims block.** A blockquote whose first line starts with `[!CLAIMS]` is a claims block. Each item of its list is one claim.
> - **Title.** The text after `[!CLAIMS]` on the first line is a title. It is prose, and it declares nothing.
> - **Only a list.** A claims block holds one list and nothing else.
> - **Label.** Each claim starts with bold text, its label. The label is a name. The claim text is the sentences after it. ^label
> - **Flat.** A claim holds no nested list.
> - **Pin.** A claim whose last line ends with a space, `^`, and an id of letters, digits, and `_` is pinned. The pin is its id. ^pin
> - **Derived id.** The id of a claim with no pin is its label, in lowercase, with its final period removed and each run of other characters as one `_`. ^derived_id
> - **Slug.** The slug of a claim is its file name, `#^`, and its id. Each id is unique in its file. ^slug
> - **Fences declare nothing.** A claims block in a code fence is an example. It declares no claim.

A section can hold several claims blocks, and it can hold none. Headings nest as deep as the writing needs, and no heading is part of a slug. Thus you can change and move headings in a file, and no claim changes its slug.

**Why a pin is optional.** A pin is the block id of Obsidian, and markdown-oxide and Obsidian can go to a claim only when it has one. On GitHub and in most other viewers, a pin shows as literal text at the end of the claim. Thus pin a claim that prose cites, or a claim whose label will change. Leave the others unpinned.

**Why the derived id uses `_`.** A block id can hold only letters, digits, and `_`. The derived id has the same form, so to pin a claim is to write its derived id at the end of its last line. Its slug does not change, and a pin is never a rename.

**Why `[!CLAIMS]`.** It is the callout form of Obsidian: `[!type]`, then an optional title. Obsidian and the viewers that follow it accept a type that they do not know, and they show it as a callout with the default style and the title. GitHub renders only five alert types, and `CLAIMS` is not one of them, so GitHub shows a blockquote whose first line is the literal text. Both are visible, and a reader who must cite a claim can see where the claims are. A standard alert such as `[!IMPORTANT]` renders better on GitHub, but authors use it for other things, and they would declare claims by accident.

**Write the title `Claims`.** With no title, Obsidian shows the type name, but some viewers show a generic label. `> [!CLAIMS] Claims` shows the same word in each viewer. A different title is permitted, for example `> [!CLAIMS] Delivery`, and crux ignores it.

**Why the label is not claim text.** A rename must be possible without a change of meaning ([[checks#^rename]]). Thus the claim text must carry the full promise. If a change to a label changes what a reader understands the claim to promise, the promise was in the label, and the claim text is incomplete.

**A claim that needs parts becomes a heading.** When a claim holds two promises that can fail separately and that a reader sees as different failures ([[practice#altitude]]), make it a heading with its own claims block. Give the new claims new ids. The old slug then names no claim, so each of its witnesses is orphaned and must select the new claim that it attests. This is correct, because each part needs its own witness ([[witnesses#several-claims]]).

**The long form had `@claim slug`.** The slug was then a second name for a thing that the text already described. A catalog of forty directives read like a ledger, and nobody could read it as a specification. Structure gives each claim a name that a reader already sees, and prose between the claims gives the context that a ledger could not.

## Prose

> **Prose never promises.** If a sentence of prose is false, no claim is broken.

Each line of a catalog file that is not in a claims block is prose.

> [!CLAIMS] Claims
>
> - **Prose has no slug.** Crux gives prose no id, and no witness attests it.

Prose can explain, argue, give an example, and tell what somebody rejected. It must not tell what the codebase does in a way that nothing checks. A promise in prose has no witness, so nothing can deny it. Crux cannot find such a promise, because it does not read meaning. The judge finds it in the triage of each diff that changes prose ([[readout#^prose_triage]]).

**A claim must be true or false when you read it alone.** Read it with the glossary, but without the prose around it. This rule makes it safe to keep prose out of the audit scope ([[checks#^prose_reopens_nothing]]). A claim that starts with _It_ or _Required._ depends on the heading or the prose, and it breaks the rule.

**The reasoning stays beside the claim.** Write why a claim is as it is, and name the rejected option, because the code cannot show it. Write this only for a decision that is hard to reverse, surprising without the reasoning, and the result of a real trade-off.

**Rationale was a separate document.** The long form had a rationale directory and a `@grounds` directive that linked it to claims. Nothing mechanical read them, and a directive exists only for what the core must resolve without intelligence ([[witnesses#witness-blocks]]). The practice was correct, but its home was wrong. A rationale in a different file is an argument split from the rule that it supports. Now it is prose beside its claims, and crux still does not read it.

## Links

A link cites a heading or a claim. Its form is the wikilink form that markdown-oxide and Obsidian resolve.

> [!CLAIMS] Claims
>
> - **Heading link.** `[[file#heading]]` cites a heading. The heading part is the heading text in lowercase, with each space as `-`.
> - **Claim link.** `[[file#^id]]` cites a claim. Its target is the slug of the claim.
> - **Alias.** A link can have an alias after `|`: `[[file#^id|text]]`.
> - **Code is not a link.** A link form in a code span or in a code fence is an example, not a link.

Crux checks each link in each Markdown file that git tracks ([[checks#^dangling_link]]). A claim link has the same form as the token of an `@attests` line. Thus one search for `checks#^rename` finds each witness and each citation of that claim.

**A claim link resolves in an editor only when the claim is pinned** ([[catalog#^pin]]). Crux resolves it in both cases. Pin each claim that prose cites, and the editor and crux agree.

**A link replaces the section number.** `§8.2` was a second identity for a heading. It did not move with the heading, and nothing checked it. A link names what it cites, and a rename changes it or makes it dangling ([[checks#^rename]]).

**GitHub shows a link as literal text.** This is the cost of one link form for editors, for crux, and for `@attests`.

**A record is not a link.** A _citation_ names a claim that exists now, and it must move when the claim moves. A _record_ names a slug as it was at a time in the past, for example the claims that an enacted amendment deleted. It must not move. Write a record in a code span: `old#^retry`.

> [!NOTE] **Open: other wiki links.** A repository that also uses `[[...]]` for a different tool gets a dangling link for each one that names no catalog heading or claim. Nobody has seen this yet.

## Groups

> **The file of a claim is part of its identity. Its headings are not.**

A claim belongs to the file that holds it. The file is its group and the first half of its slug. The headings inside the file arrange the claims for a reader, and nothing depends on them.

- **Directories nest groups.** `catalog/billing/invoices.md` is a group in a group. Crux reports the path of the file. No slug contains the directory, and no check depends on it.
- **A move between directories renames nothing.** The file name stays the same.
- **A move to a different file is a rename.** Crux detects it when the claim text does not change ([[checks#^rename]]).
- **A change to a heading renames nothing.** Only a heading link that cites it changes.

**The three failed designs put the group into the identity.** A declared prefix made each reorganisation rename each slug. A position rule let the group control where a witness can be, so a product in `packages/` and `apps/` could not share one group. The long form then separated the two completely: the file grouped the claim, and a free slug identified it.

**Heading paths were tried next, and they were deleted.** A slug made from the headings above a claim was readable, but each heading change renamed each claim under it. Editors cannot resolve a heading path, because a wikilink names one heading. The file name is a coarser group, and a file changes its name much less frequently than a heading. Rename detection makes the remaining cost an edit to the witnesses, not an audit.

**A group across many files is a search, not a structure.** _All security claims_ is a query on the catalog. Each design that gave a claim two structural groups needed a registry to keep them consistent.

**Incoherence is an old problem.** Two files that overlap are the same problem as two code folders that overlap. Developers already solve it when they read the tree. A catalog that is tiring to read is evidence at the ruling ([[tools#dogfooding]]).
