# Context

The words of crux. Each technical word has one meaning. The specification and the runbooks use each word only in that meaning, and they are written in ASD-STE100 Simplified Technical English.

These are the words between tools. A tool can add its own words in a `CONTEXT.md` in its directory of `spec/`, and it never defines again a word of this file ([Settling the words](spec/practice.md#settling-the-words)).

## Words

**adapter** — A converter from the report of one tool into verdicts for the witnesses that crux indexed.

**amendment** — The set of claim changes that one unit of work proposes: its entries, its prose, and its version.

**answers** — The inferential verdicts, the standings, and the coverage that an intelligence gives for one commit.

**attest** — The relation that a witness records. Witness X attests claim Y.

**audit** — To read instruments and set their standings, and then set the coverage of their claims.

**auditor** — The intelligence that does the audit.

**base** — The commit that a scope or a readout compares with. The caller names it.

**block** — A contiguous sequence of directive lines.

**builder** — The role that makes the claims of an amendment true and writes their witnesses.

**canvass** — To ask each witness for a verdict.

**catalog** — The set of all claims in the repository.

**catalog file** — A Markdown file that holds a claims block.

**claim** — One heading in a claims block and the paragraphs under it. A short statement that the codebase satisfies now. It is falsifiable.

**claim text** — The paragraphs of a claim after its heading.

**claims block** — A blockquote whose first line starts with `[!CLAIMS]`. It holds only claims.

**computational witness** — A witness that a tool adapter reports on. Types, tests, and lint rules are computational.

**coverage** — If the witnesses of a claim uphold it together: **covered**, **under-covered**, or **unaudited**.

**directive** — One `@attests` and its token, on one line.

**entry** — One operation of an amendment on one slug: **add**, **change**, or **delete**.

**escalation** — A proposed amendment, with a reason, that stops the work until the operator decides.

**existence** — If a claim has a witness. A form check finds it.

**extent** — The lines that a block owns: from the block to its terminator, to the next block, or to the end of file.

**fog** — Material that you want, but cannot yet write as a claim.

**heading slug** — The fragment that GitHub makes from the text of a heading. For example, `rename-in-a-witness`.

**index** — What crux knows about one commit: its catalog files, claims, witnesses, headings, and links.

**inferential witness** — A witness that no tool adapter reports on. A judge answers it.

**instrument** — The witness itself: its block, its extent, and the text of the claim that it attests.

**judge** — The intelligence that answers the inferential witnesses.

**label** — The heading text of a claim. It is the name of the claim, not a part of its text.

**layer** — One job in the system: requirements, measurement, orchestration, execution, or attention. One tool does the job of one layer.

**link** — A CommonMark link with a relative path. It cites a file, a heading, or a claim.

**operator** — The person who approves an amendment, decides an escalation, and makes the ruling.

**prose** — Each part of a catalog file that is not in a claims block.

**question** — An answer that a readout needs and does not have yet.

**readout** — The result of a canvass and an audit: one block for each claim.

**rename** — A slug that disappears in a diff, while a new slug appears with the same claim text.

**reviewer** — The role that does the triage and the audit of a diff that it did not build.

**round** — One pass of build and review on one amendment.

**ruling** — The decision that a human makes at the merge.

**rung** — The position of a witness on the ladder: **type**, **test**, **lint rule**, or **prose**.

**scope** — The witnesses whose instrument changed, and the witnesses of each claim whose text changed, from the base to HEAD.

**slug** — The identity of a claim: the path of its file from the root of the repository, `#`, and its heading slug. For example, `spec/checks.md#rename`.

**standing** — If one instrument supports one claim: **sound**, **unsound**, or **unaudited**.

**subject** — The code that a witness examines. The witness observes it and does not take its contract from it.

**triage** — The one pass in which the judge reads a diff and selects what to ask again.

**verdict** — What a witness tells about the subject: **affirms**, **affirms (unaffected)**, **denies**, or **silent**.

**version** — The SHA-256 of the entries and the prose of an amendment. It changes when either changes.

**witness** — A part of the repository that can show if the codebase satisfies a claim.

**witness reference** — The file and the line of a witness block, at one commit.

## Naming rule

Use a formal word for a **thing**. Use a plain word for an **event**. A person learns a thing once, so it gets a precise word. The objects in a sentence already describe an event. Thus the operations of an amendment are **add**, **change**, and **delete**, and a slug that changes is a **rename**.
