# Roadshow talk, 17 September 2026: slides and the interaction record that produced them

Brian Ballsun-Stanton, Macquarie University, with Shawn Ross, Fieldnote.

A 15-minute talk for the ARDC and RDA Roadshow in Sydney, advertising the
proposed RDA **Documenting Generative AI Interactions in Research** Interest
Group, in community review until 5 October 2026:

<https://www.rd-alliance.org/groups/documenting-generative-ai-interactions-in-research-interest-group/work-statement/documenting-generative-ai-interactions-in-research-interest-group-statement-of-work/>

## Contents

- `slides/roadshow-2026-09-17.qmd` — Quarto source, on-screen text and
  presenter notes.
- `slides/roadshow-2026-09-17.pptx` — the build emailed to the organisers on
  14 September, before formatting applied in Google Slides.
- `slides/assets/sow-page-qr.png` — QR code for the Statement of Work page.
- `transcript/conversation.md` — the Claude Code session in which the deck was
  planned and drafted, user and assistant turns.
- `transcript/session.meta.json` — session metadata including the Prompt,
  Process, and Provenance summaries.

## What this record is, and what it is not

The talk argues that generative AI interactions are method and should be
documented like code and data. This repository is that practice applied to
the talk itself, in the form of the Three Ps.

**Prompt.** The user turns in `transcript/conversation.md` are the prompts,
verbatim apart from the redactions below.

**Process.** The assistant turns record what the model proposed and what
Brian ruled. Brian ruled every content decision: the five-slide shape, all
visible wording, the exclusion of decorative imagery, and the choice of local
transcription. The model drafted, checked sources, and built. The first
wording draft was rejected as trite and redrafted against a prose register;
a ten-slide outline was cut to five; an attempt to use a hosted transcription
credential was stopped. Those corrections are in the record.

**Provenance.** Inputs were Brian's own 2025 Brisbane Birds of a Feather deck
and recording, the group's submitted Statement of Work, two forwarded emails,
and the public event listing. The one statistic on the slides (1 of 137
studies) was checked against its source during the session.

## What was withheld, and why

- **The full tool log** (every command, file read, and tool result) is not
  published. It contains a credential exposed from shell history during the
  session, the names and affiliations of participants in the 2025 Brisbane
  session recording, and a photograph of a handwritten sign-up sheet with
  third parties' email addresses.
- **In the published conversation**, one mobile number, five ARDC staff names
  and email addresses, a Secretariat staff member's first name, and a shared
  editing link were replaced with bracketed placeholders. Nothing else was
  altered.
- **The planning documents** (design brief, outline, bullets, notes) and the
  Brisbane transcript are not published here.

The withheld material is retained locally. The redaction was done by
pattern substitution and verified by searching the published file for each
pattern; that check establishes the absence of those patterns, not of every
possible identifying detail.
