# The catalog

The catalog tells what the codebase promises now. It is the one artifact of crux. This file tells how a catalog is written and how crux reads it.

The catalog is ordinary Markdown. Crux reads its structure — headings, claims blocks, links, and an optional frontmatter key — and never the meaning of its sentences. A catalog file at `catalog/sending.md` looks like this:

```md
## Schedule

Prose that tells what this part of the product does and why.

> [!CLAIMS]
>
> #### Retry
>
> A failed attempt is tried again on each later fire until one returns or the local date ends.
>
> #### Send hour
>
> The first fire at or after the configured send hour attempts the prompt, and no earlier fire does.
```

The two claims have the slugs `catalog/sending.md#retry` and `catalog/sending.md#send-hour`. A witness attests the first as `@attests catalog/sending.md#retry`. Prose in `catalog/` cites it as `[Retry](sending.md#retry)`, and prose in a different directory uses a longer relative path.

## Condition

> **A claim is in the catalog only if its witnesses affirm it, each witness is sound, and together they cover it.**

A claim can fail in three places. A witness can deny it. A witness can support nothing. The set of witnesses can be too small. Thus the condition has three parts: the [Verdict](readout.md#verdict), the [Standing](readout.md#standing), and the [Coverage](readout.md#coverage).

- **A claim enters the catalog in the merge that makes it true.** It never enters earlier. There is no `pending` status. Intent is an amendment ([Amendments](practice.md#amendments)) or fog ([Fog](practice.md#fog)).
- **The condition holds by induction.** The claim satisfies it when it enters. Each merge keeps it, because each merge audits what it changed ([Scope](checks.md#scope)). Thus a reviewer reads only the diff.

Belay enforces the condition. A red item stops the work before a human sees it ([Red stops](readout.md#red-stops)).

## Catalog files

A catalog file is an ordinary document. It has a title, then prose that tells what the file contains, then its sections. That opening prose is the charter of the file. A reader can compare a section with the charter and decide if the section belongs.

> [!CLAIMS]
>
> ### Declared by a claims block
>
> A Markdown file that git tracks is a catalog file when it holds a claims block. Crux never selects a catalog file by its path or by its frontmatter.
>
> ### Default version
>
> A catalog file with no `catalog` key in its frontmatter uses version 1 of the format.
>
> ### Format version
>
> When the frontmatter of a Markdown file has the key `catalog`, its value is a positive integer: the version of the format that the file uses.
>
> ### Versions can mix
>
> One repository can hold catalog files of different versions. Crux reads each file by its own version.
>
> ### The title declares nothing
>
> The H1 of a catalog file and the text before its first H2 hold no claim.
>
> ### File path
>
> The file part of a slug is the path of the catalog file from the root of the repository, with `.md`.
>
> ### Unique headings
>
> The slug of each heading is unique in its file. This includes the headings of claims.

**A claims block declares a catalog file.** The first version of this file declared a catalog file with the frontmatter key `catalog`. A file that held no claims needed the key too, only so that other files could link to its headings, and the glossary of this specification was such a file. A link target is now any Markdown file ([Links](#links)), so the key has only one job left: to name a version. A claims block is already the thing that crux reads, so it is sufficient to declare the file. The frontmatter key is a word of the format, not the name of a tool ([Tool names](glossary.md#tool-names)).

**The path, not the file name.** The first version of this file used the file name alone, so that a directory move renamed nothing. The cost was a collision rule: two catalog files with one name, in any directories, were an error, and an ordinary name such as `README` could hold claims only once in a repository. A path cannot collide. A directory move now renames each claim in the moved files, but it does not change their claim text, so rename detection finds it and it opens no audit ([Rename](checks.md#rename)).

**Headings are unique in a file** because a fragment names only one heading, not the headings above it. GitHub gives a second heading with the same slug a suffix, for example `rename-1`. That suffix changes when somebody adds a heading above it, so crux reports the duplicate and does not accept the suffix.

### Versions

> **This specification defines version 1.** The files in this repository have no `catalog` key, so they use version 1.

The format goes into repositories that you do not control ([Tool set](tools.md#tool-set)), so a change to the format is a migration that somebody else must run. A version in each file tells a tool which rules to read it by, and it lets a repository migrate one file at a time.

**The version is in the file, not in a configuration file.** Crux reads nothing that does not come from a file it already examines, and a version that sits beside the claims moves with them.

**No key means version 1, for all time.** A later version does not change the default. Thus a file with no key never changes its meaning when a new version appears, and only a file that uses a later version must say so. The cost is one rule that a reader must know: an absent key is a version.

**The version names the format, not a tool.** A crux release can read several versions. Its release number is not in the format, for the same reason as each tool name ([Tool names](glossary.md#tool-names)).

**Increase the version only for a change that alters how an existing file reads.** Examples are a change to the slug of a heading, to the form of a slug, or to what a claims block holds. Each is a change after which the same file gives a different slug, a different claim, or a different form error. A new rule that no existing file can break does not increase the version.

**The witness side has no version.** An `@attests` token is a slug, and the catalog file that declares the slug sets the rules that make it. A version change that changes slugs is thus a rename, and rename detection applies to it ([Rename](checks.md#rename)).

| Version | Change                                                                                  |
| ------- | --------------------------------------------------------------------------------------- |
| 1       | The first version: claims blocks of headings, `path#heading` slugs, and Markdown links. |

## Claims

A claim is one heading in a claims block and the text under it. It is the only thing in the catalog that promises anything.

> [!CLAIMS]
>
> ### Claims block
>
> A blockquote whose first line starts with `[!CLAIMS]`, in any letter case, is a claims block.
>
> ### Title
>
> The text after `[!CLAIMS]` on the first line is an optional title. It is prose, and it declares nothing.
>
> ### Claim heading
>
> Each heading in a claims block starts one claim. The text of the heading is the label of the claim. Crux ignores the level of the heading.
>
> ### Claim text
>
> The claim text of a claim is the paragraphs after its heading, to the next heading in the block or to the end of the block. A claim has one or more paragraphs.
>
> ### Only claims
>
> A claims block holds only claims. The first block after its first line is a heading.
>
> ### Flat
>
> Claim text holds only paragraphs. It holds no list, no blockquote, and no code fence.
>
> ### Slug
>
> The slug of a claim is the path of its file, `#`, and the slug of its heading. For example, `spec/checks.md#rename`.
>
> ### Fences declare nothing
>
> A claims block in a code fence is an example. It declares no claim.

A section can hold several claims blocks, and it can hold none. Section headings nest as deep as the writing needs, and no section heading is part of a slug. Thus you can change and move section headings in a file, and no claim changes its slug. Give a claim heading the level below its section, so that an outline shows each claim under its section.

**Why a claim is a heading.** A citation must go to the claim, not to the top of its file, in the places where people read claims: GitHub and VS Code. CommonMark gives no element an id. Renderers add an id to each heading by convention, and they add one to no other element. Headings in a blockquote get the same id, on GitHub and in the VS Code language service. Thus a heading is the only anchor that each reader resolves with no extension and no HTML. These options were rejected:

- **A bold label on a list item, with a derived id or an Obsidian pin (` ^id`).** This was the first version of this file. Only Obsidian, markdown-oxide, Foam, and Dendron go to a pin. GitHub and VS Code open the file at the top, and GitHub shows the pin as literal text.
- **An attribute, such as `{#id}`.** CommonMark has no attribute syntax. Pandoc, kramdown, markdown-it-attrs, and Djot each put it in a different place on a list item, and GitHub shows it as literal text.
- **An HTML anchor, `<a id="rename"></a>`, at the start of a list item.** It works on GitHub and in the VS Code editor. The VS Code preview opens a link to it from a different file at the top. It also puts HTML in each claim, and it needs a second marker to separate a claim from an ordinary anchor.

**The cost of a heading.** Each claim takes one more line than a list item, and each claim shows in the outline of the file. A change to a label changes the slug, so it is a rename ([Rename](checks.md#rename)). Crux must also make each heading slug as GitHub makes it ([Heading link](#heading-link)).

**Why `[!CLAIMS]`.** It is the callout form of Obsidian: `[!type]`, then an optional title. Obsidian and the viewers that follow it accept a type that they do not know, and they show it as a callout. With no title, they show the type name. GitHub renders only five alert types: `NOTE`, `TIP`, `IMPORTANT`, `WARNING`, and `CAUTION`. `CLAIMS` is not one of them, so GitHub shows a blockquote whose first line is the literal text. Both are visible, and the quote bar separates the claims from the prose around them.

**Why not `[!IMPORTANT]`.** A standard alert renders as a box on GitHub. But prose authors use each standard alert for other things. A `[!WARNING]` that tells a reader what a claim does not promise would become a claim. GitHub also accepts no title after a standard alert: `> [!NOTE] Claims` is a plain blockquote.

**The title is optional.** With no title, GitHub shows `[!CLAIMS]` and Obsidian shows _Claims_. A title such as `> [!CLAIMS] Delivery` is permitted, and crux ignores it.

**Why the label is not claim text.** A rename must be possible without a change of meaning ([Rename](checks.md#rename)). Thus the claim text must carry the full promise. If a change to a label changes what a reader understands the claim to promise, the promise was in the label, and the claim text is incomplete.

**A claim that needs parts becomes a section.** When a claim holds two promises that can fail separately and that a reader sees as different failures ([Altitude](practice.md#altitude)), make it a section heading with its own claims block. Give the new claims new labels. The old slug then names no claim, so each of its witnesses is orphaned and must select the new claim that it attests. This is correct, because each part needs its own witness ([Several claims](witnesses.md#several-claims)).

**The long form had `@claim slug`.** The slug was then a second name for a thing that the text already described. A catalog of forty directives read like a ledger, and nobody could read it as a specification. Structure gives each claim a name that a reader already sees, and prose between the claims gives the context that a ledger could not.

## Prose

> **Prose never promises.** If a sentence of prose is false, no claim is broken.

Each part of a catalog file that is not in a claims block is prose.

> [!CLAIMS]
>
> ### Prose has no slug
>
> Crux gives prose no slug, and no witness attests it.

Prose can explain, argue, give an example, and tell what somebody rejected. It must not tell what the codebase does in a way that nothing checks. A promise in prose has no witness, so nothing can deny it. Crux cannot find such a promise, because it does not read meaning. The judge finds it in the triage of each diff that changes prose ([Prose triage](readout.md#prose-triage)).

A section heading has a slug, and a link can cite it. But it is not a claim, so no witness attests it ([Claims only](witnesses.md#claims-only)).

**A claim must be true or false when you read it alone.** Read it with the glossary, but without the prose around it. This rule makes it safe to keep prose out of the audit scope ([Prose reopens nothing](checks.md#prose-reopens-nothing)). A claim that starts with _It_ or _Required._ depends on the heading or the prose, and it breaks the rule.

**The reasoning stays beside the claim.** Write why a claim is as it is, and name the rejected option, because the code cannot show it. Write this only for a decision that is hard to reverse, surprising without the reasoning, and the result of a real trade-off.

**Rationale was a separate document.** The long form had a rationale directory and a `@grounds` directive that linked it to claims. Nothing mechanical read them, and a directive exists only for what the core must resolve without intelligence ([Witness blocks](witnesses.md#witness-blocks)). The practice was correct, but its home was wrong. A rationale in a different file is an argument split from the rule that it supports. Now it is prose beside its claims, and crux still does not read it.

## Links

A link cites a file, a heading, or a claim. Its form is the CommonMark link, which each Markdown viewer resolves.

> [!CLAIMS]
>
> ### Link
>
> A link is a CommonMark link, inline or by reference, whose destination has no URI scheme. Crux ignores a link whose destination has a scheme, such as `https:`.
>
> ### Relative path
>
> The path of a link is relative to the file that holds the link. A link with only a fragment names its own file.
>
> ### Any tracked file
>
> A link can name each file that git tracks, and each directory that holds such a file. The file does not have to be a catalog file or a Markdown file.
>
> ### Heading link
>
> A link to a Markdown file with a fragment cites a heading in that file. The fragment is the slug of the heading: its text in lowercase, with each character that is not a letter, a digit, a space, `-`, or `_` removed, and each space as `-`.
>
> ### Claim link
>
> A heading link to the heading of a claim cites the claim. Its path and fragment, resolved from the root of the repository, are the slug of the claim.
>
> ### Code is not a link
>
> A link form in a code span or in a code fence is an example, not a link.

Crux checks each link in each Markdown file that git tracks ([Dangling link](checks.md#dangling-link)). It checks a fragment only when the link names a Markdown file. A link to a source file, for example `[the retry loop](../src/sending.ts)`, is a citation of that file, and crux checks that the file exists.

**A link and a slug name one thing.** `[Rename](checks.md#rename)` in `spec/` resolves to `spec/checks.md#rename`, and that is the token of an `@attests` line. Thus one search for `checks.md#rename` finds each witness and each citation of that claim. In a commit or in a tracker, write the slug.

**Why Markdown links and not wikilinks.** The first version of this file used `[[file#^id]]`. A wikilink names a file by a short name, and a short name can match several files. Each resolver then needs a collision rule or a guess, and each tool guesses differently. GitHub also shows a wikilink as literal text. A CommonMark link names one file with no guess. GitHub, VS Code, Obsidian, markdown-oxide, and lat all resolve it, and it is a link on GitHub.

**Crux now resolves a path.** The first version of crux refused to resolve a path. It still reads no file system: it compares the text of a path with the list of files that git tracks. The cost of a path is that a file move changes each link to that file. VS Code can update the links when you move a file, and crux reports each link that it did not update.

**The slug rule is the rule of GitHub.** Each viewer makes heading slugs, and they do not all agree on punctuation and on letters outside ASCII. GitHub is the viewer that most readers use, and VS Code uses a compatible rule. Write labels in plain words, and the rules agree.

**A link replaces the section number.** `§8.2` was a second identity for a heading. It did not move with the heading, and nothing checked it. A link names what it cites, and a rename changes it or makes it dangling ([Rename](checks.md#rename)).

**A record is not a link.** A _citation_ names a claim that exists now, and it must move when the claim moves. A _record_ names a slug as it was at a time in the past, for example the claims that an enacted amendment deleted. It must not move. Write a record in a code span: `spec/old.md#retry`.

## Groups

> **The file of a claim is part of its identity. Its section headings are not.**

A claim belongs to the file that holds it. The path of the file is its group and the first half of its slug. The section headings inside the file arrange the claims for a reader, and nothing depends on them.

- **Directories nest groups.** `catalog/billing/invoices.md` is a group in a group.
- **A move to a different file or directory is a rename.** Crux detects it when the claim text does not change ([Rename](checks.md#rename)).
- **A change to a section heading renames nothing.** Only a heading link that cites it changes.

**The three failed designs put the group into the identity.** A declared prefix made each reorganisation rename each slug. A position rule let the group control where a witness can be, so a product in `packages/` and `apps/` could not share one group. The long form then separated the two completely: the file grouped the claim, and a free slug identified it.

**Heading paths were tried next, and they were deleted.** A slug made from the section headings above a claim was readable, but each heading change renamed each claim under it. A fragment names one heading, so no viewer can resolve a heading path. The heading of the claim itself is different: it is the label, and a change to it is a rename of that claim only.

**A group across many files is a search, not a structure.** _All security claims_ is a query on the catalog. Each design that gave a claim two structural groups needed a registry to keep them consistent.

**Incoherence is an old problem.** Two files that overlap are the same problem as two code folders that overlap. Developers already solve it when they read the tree. A catalog that is tiring to read is evidence at the ruling ([Dogfooding](tools.md#dogfooding)).
