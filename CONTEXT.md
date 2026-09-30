# Context

The words of crux. Each technical word has one meaning. The specification and the runbooks use each word only in that meaning, and they are written in ASD-STE100 Simplified Technical English.

## Words

**adapter** — A converter from a verdict source into verdicts. The source is the report of a tool, or a judge.

**amendment** — The set of claim changes that one unit of work proposes.

**attest** — The relation that a witness records. Witness X attests claim Y.

**audit** — To read instruments and set their standings, and then set the coverage of their claims.

**auditor** — The intelligence that does the audit.

**block** — A contiguous sequence of directive lines.

**canvass** — To ask each witness for a verdict.

**catalog** — The set of all claims in the repository.

**catalog file** — A Markdown file that holds a claims block.

**claim** — One heading in a claims block and the paragraphs under it. A short statement that the codebase satisfies now. It is falsifiable.

**claim text** — The paragraphs of a claim after its heading.

**claims block** — A blockquote whose first line starts with `[!CLAIMS]`. It holds only claims.

**coverage** — If the witnesses of a claim uphold it together: **covered**, **under-covered**, or **unaudited**.

**directive** — One `@attests` and its token, on one line.

**existence** — If a claim has a witness. A form check finds it.

**extent** — The lines that a block owns: from the block to its terminator, to the next block, or to the end of file.

**fog** — Material that you want, but cannot yet write as a claim.

**heading slug** — The fragment that GitHub makes from the text of a heading. For example, `rename-in-a-witness`.

**instrument** — The witness itself: its block, its extent, and the text of the claim that it attests.

**judge** — The intelligence that answers the inferential witnesses. It is the default adapter.

**label** — The heading text of a claim. It is the name of the claim, not a part of its text.

**link** — A CommonMark link with a relative path. It cites a file, a heading, or a claim.

**prose** — Each part of a catalog file that is not in a claims block.

**readout** — The result of a canvass and an audit: one block for each claim.

**rename** — A slug that disappears in a diff, while a new slug appears with the same claim text.

**ruling** — The decision that a human makes at the merge.

**slug** — The identity of a claim: the path of its file from the root of the repository, `#`, and its heading slug. For example, `spec/checks.md#rename`.

**standing** — If one instrument supports one claim: **sound**, **unsound**, or **unaudited**.

**subject** — The code that a witness examines. The witness observes it and does not take its contract from it.

**triage** — The one pass in which the judge reads a diff and selects what to ask again.

**verdict** — What a witness tells about the subject: **affirms**, **affirms (unaffected)**, **denies**, or **silent**.

**witness** — A part of the repository that can show if the codebase satisfies a claim.

## Naming rule

Use a formal word for a **thing**. Use a plain word for an **event**. A person learns a thing once, so it gets a precise word. The objects in a sentence already describe an event. Thus the operations of an amendment are **add**, **change**, and **delete**, and a slug that changes is a **rename**.
