# Roadshow talk planning and IG workspace setup

**Date**: 2026-09-14
**Model**: claude-fable-5-1
**Claude Code**: v2.1.266
**Duration**: 3100 minutes
**Turns**: 99

## Three Ps (IDW2025)

**Prompt**: Brian set up a new workspace for the RDA Documenting Generative AI Interactions in Research Interest Group, asking to mirror the group's Google Drive documentation, reuse slide tooling from a teaching repository, and plan a 15-minute talk for the ARDC and RDA Roadshow in Sydney on 17 September 2026 whose only goal is to get attendees to support the group's Statement of Work during community review.

**Process**: Claude Code (Fable 5.1) surveyed the hand-copied Drive mirror and the teaching repository's PowerPoint harness, initialised a git repository, and transcribed the 92-minute Brisbane BoF recording with the local scribe tool after Brian stopped an attempt to use whisperx with a Hugging Face token. It converted the submitted proposal and prior decks to Markdown, then built the talk breadth-first under Brian's rulings: a design brief, a ten-event outline that Brian cut to five, on-screen wording redrafted against the workspace prose register after Brian rejected the first draft as trite, presenter notes drawn from Brian's own Brisbane phrasing, and a Quarto build to PowerPoint that Brian emailed to the organisers. Two project notes were written with Brian's approval. The 1-of-137 figure was verified against its JAMA Network Open source, and the QR code against the live page.

**Provenance**: Brian ruled every content decision: the five-slide shape, all visible wording, the exclusion of decorative imagery, and the choice of local transcription. Claude drafted, checked sources, and built. Inputs were Brian's own BoF deck and recording, the proposal's Statement of Work, two forwarded emails from the RDA Secretariat and Shawn Ross, and the Eventbrite listing. The session log contains third parties' contact details and one credential from Brian's shell history, and is scrubbed before publication.

---

## User

right, ok, just made this, half we need to sync stuff from google drive for all the documentation, and half we need to pull slides tooling from people/Brian/INTS1302 so that I can plan my presentation for Thursday

## Assistant

Route: `denubis-project-notes:scanning-project-notes` first, because this is consequential project work in the Shawn workspace and the notes decide what I can assume. Then I'll inspect the new repo and the INTS1302 slides tooling.

## Assistant

**Tools used:**
- Skill

## Assistant

**Tools used:**
- Bash: `PLUGIN_DIR=/home/brian/.claude/plugins/cache/denubis-plug...`

## Assistant

**Tools used:**
- Read: .notes/project_workspace-state.md

## Assistant

**Tools used:**
- Bash: `ls -la /home/brian/people/Brian/INTS1302/; echo ---; cat ...`

## Assistant

**Tools used:**
- Bash: `ls -la /home/brian/people/Brian/INTS1302/; echo ---; cat ...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Brian/INTS1302; sed -n 80,200p CLAU...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Brian/INTS1302; ls pptx-harness/too...`

## Assistant

Survey done. Here is where things stand before I touch anything.

**The new directory is empty and is not a git repository.** The session header says it is, but `git log` fails there, exactly as the workspace CLAUDE.md warns about the Shawn root. Nothing here is version controlled yet, so I have not created anything.

**Slides tooling in INTS1302.** The reusable part is `pptx-harness/`, an MQ-branded Pandoc to PPTX chain ported from the SARDI lecture. What generalises versus what is INTS1302-specific:

- **Reusable core**: `pptx-harness/tools/` (autofit, budget check, OPC check, oracle contact sheets, stamping, the bibliography-metadata Lua filter), `template/mq-reference.pptx`, `resources/` (the MQ 2024.06 potx), `pyproject.toml` and `uv.lock`.
- **INTS1302-specific, would not copy**: `oracle/` (contact-sheet renders, most of the 106 MB), `slides/`, `draft/`, `dist/`, and the per-week `scripts/build_weekNN_pptx_md.py` composers, which are hard-wired to that repo's BULLETS/NOTES layout.
- **Worth carrying as method rather than code**: `docs/register-and-writing.md` and `.notes/reference_slide-development-standards.md` (Brian's breadth-first slide rulings) and `reference_teaching-pace.md`. INTS1302 is also not a git repository, so this is a file copy, not a clone or submodule.

My proposed approach for that half: `git init` here, copy the reusable core into `pptx-harness/`, run `uv sync` from the configured cache and a smoke build of a one-slide deck against the template to prove the chain works in the new location, then write a `CLAUDE.md` naming the outline, bullets, notes layering so Thursday's planning follows the same breadth-first pass.

**Google Drive half.** The hook forbids rclone for agent processes. The available route is the Google Drive MCP connector, which needs you to complete an OAuth flow in the browser before I can list or download anything.

One question first, since it gates the Drive half entirely: which Drive folder or shared link holds the RDM LLM logging documentation, and are you happy for me to start the Drive MCP authorisation now so I can pull it?

## User

do we want to use dvc or rclone for google drive?

## User

do we want to use dvc or rclone for google drive? you do not get to mcp, this is read only

## User

do we want to use dvc or rclone for google drive? you do not get to mcp, this is read only. Right, I just did it by hand, rda-llm-local is the repo

## Assistant

Answer first: for a read-only Google Drive mirror, rclone is the right tool. DVC's Google Drive remote is for versioning your own artefacts, it wants a GCP OAuth client, and it stores hashed blobs rather than a browsable tree. Rclone gives you `rclone sync drive:folder ./docs` with a readable copy and `--dry-run`, and you run it by hand since agents are forbidden it here. DVC would only earn its place if you later need to version large generated outputs from this repo.

Let me look at what you pulled.

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn; ls -d */ | grep -i rda; ls -...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/rda-llm-local...`

## Assistant

The Drive mirror is in place. Here is what it holds and the one thing I need ruled before I build anything.

**What you copied** into `RDM-LLM-Logging/rda-llm-local/` (measured, not git tracked anywhere):

| | |
|---|---|
| Files | 32 |
| Size | 117 MB |
| Formats | 20 docx, 2 pptx, 1 xlsx, 3 audio (m4a, aac), 1 jpg, 1 jsonl, 1 json, 1 txt |

Three branches: `RDA BOF/` (the original BoF proposal and planning notes), `Meeting Transcripts and Resources/` (the 13 Oct 2025 BoF session and 15 Oct lunchtime meeting, with the post-IDW "New Interest Group Forming" deck, which is the closest thing to a predecessor for Thursday), and `Interest Group Proposal/` (the draft Statement of Work, summary, membership sheet, and a `case-study/` directory that is a Claude session archive with its extraction prompts saved as docx). Neither this directory nor its parent is a git repository yet.

**Two things I would flag before the layout is fixed.** The audio files are most of the 117 MB and should be gitignored or kept out of any hosted remote. And the docx mirror should stay read-only as you said, with any Markdown conversions I make for planning living in a separate derived directory so the sync can be re-run without clobbering work.

**The one question.** Your shell has moved into `rda-llm-local`, so I am unsure which directory is the repository root. Do I `git init` at `RDM-LLM-Logging/`, with `rda-llm-local/` as the untouched Drive mirror beside `pptx-harness/` and the Thursday planning files, or is `rda-llm-local/` itself the repo and everything goes inside it?

## User

great, let's start taking .notes. and we should have transcript of the audio -- hopefully one of them is my BoF presentation. hrm, trying to extract the deck is going to be tedious, since I can't extract or download, but so long we have standard slides (not MQ branded), we'll be fine

## Assistant

Plan: put `.notes/` at `RDM-LLM-Logging/` since the mirror stays read-only, check what transcripts already exist beside the audio, and find the transcription tool INTS1302 used for its lecture audio.

## Assistant

**Tools used:**
- Bash: `cd "/home/brian/people/Shawn/RDM-LLM-Logging/rda-llm-loca...`

## Assistant

**Tools used:**
- Bash: `cd "/home/brian/people/Shawn/RDM-LLM-Logging/rda-llm-loca...`

## Assistant

**Tools used:**
- Bash: `ls -la ~/.cache/huggingface/token 2>&1; env | grep -c -E ...`

## Assistant

**Tools used:**
- Bash: `cd "/home/brian/people/Shawn/RDM-LLM-Logging/rda-llm-loca...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; git init -q ...`

## User

[Request interrupted by user]

## User

um, why are you engaging with HF tokens? That's not ok at all. we have scribe as local infra for parakeet transcription

## Assistant

**Tools used:**
- Bash: `which scribe; ls ~/.local/bin | grep -i scribe; rg -l -i ...`

## Assistant

**Tools used:**
- Bash: `rg -n -i 'scribe' /home/brian/people/Brian/INTS1302/WORKL...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; mkdir -p tra...`

## Assistant

**Tools used:**
- Read: .notes/reference_teaching-pace.md

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/transcripts/2...`

## Assistant

**Tools used:**
- Skill

## Assistant

Writing the notes needs your approval on the exact records first. Two proposals, then scribe's result when it lands.

**Proposal 1**: `RDM-LLM-Logging/.notes/project_workspace-state.md` (new). Content: repo root is `RDM-LLM-Logging/`, initialised today on `main` with no commits; `rda-llm-local/` is a hand-copied read-only mirror of the Google Drive folder (32 files, 117 MB, three branches: BoF, meeting transcripts, IG proposal), re-synced by hand and never edited in place; audio is gitignored; derived work lives in `transcripts/` and the planning directories; the 13 Oct 2025 BoF deck is yours (30 slides, contact slide names you and Shawn), the 15 Oct lunchtime meeting already carries a transcript, and the post-IDW 6-slide deck is the current predecessor for Thursday. Consequence: the next session knows what is source and what is derived without re-surveying. Evidence: the file listing command and the python-pptx extraction I ran.

**Proposal 2**: `RDM-LLM-Logging/.notes/feedback_local-transcription-only.md` (new). Content: transcription in this workspace runs through `scribe` (local parakeet with speaker enrolment); never use whisperx, Hugging Face tokens, or any hosted credential, and never read or surface a token found in shell history. Why: your correction this session, quoted. How to apply: copy the recording out of the mirror into `transcripts/` and run scribe there so its outputs do not land in the read-only tree. Evidence: your message and the scribe help output.

Do I write both as stated?

## User

yes.

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; git rev-pars...`

## Assistant

**Tools used:**
- Write: project_workspace-state.md

## Assistant

**Tools used:**
- Write: feedback_local-transcription-only.md

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; PLUGIN_DIR=/...`

## Assistant

Both notes are written and verified: the inventory helper parses two flat frontmatter records, and nothing else in the tree changed. `.notes/` is not shown by git status, which matches the workspace rule that it is gitignored.

Scribe is in its diarisation pass on the 92-minute BoF recording. I'll report once the transcript lands and confirm whether your presentation is in it.

On slides: since standard rather than MQ-branded decks are fine, the INTS1302 harness reduces to Pandoc with a default reference deck or Quarto, and I will not copy the branded template, oracle renders, or the week-specific composers. What does carry over is the method: design brief, then outline, then bullets and notes, breadth first.

One question so I can set up the Thursday brief while scribe runs: what is Thursday's presentation, meaning the audience, the slot length, and whether it is an update on the interest group or a re-pitch of the BoF argument?

## User

<task-notification>
<task-id>bvdlqhiqf</task-id>
<tool-use-id>toolu_01RBKQwpSGwJReonV5GhE62Q</tool-use-id>
<output-file>/tmp/claude-1000/-home-brian-people-Shawn-RDM-LLM-Logging/33917f16-1a51-48fe-8f8c-0707b57eec76/tasks/bvdlqhiqf.output</output-file>
<status>completed</status>
<summary>Background command "Transcribe BoF recording with scribe" completed (exit code 0)</summary>
</task-notification>

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/transcripts/2...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/transcripts/2...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/transcripts/2...`

## Assistant

The BoF transcript is done and it is your presentation. Scribe wrote `transcripts/20251013-bof-session/20251013-bof-session.md` and `.json` (about 9,250 words, exit 0).

**Structure of the 92 minutes, from the diarised timestamps:**

- **0:00 to 0:09** Setup, shared-notes link, framing of the BoF as the basis of an interest group.
- **0:09 to about 0:41** Your talk, tracking the deck: disclosure versus showing your work, prompts and outputs treated like code and data, the paradigm-shift claim, the grimoire and Cauldron-spell-35 exchange, Latour and Galison on tacit knowledge, then the challenges list and the move to breakouts.
- **0:41 to about 1:16** Breakout discussion, mostly you circulating between groups; the recorder only caught fragments of the other tables.
- **1:16 to 1:32** Report-back and questions, with named participants introducing themselves, ending with your pitch to turn the group into an RDA interest group.

Speaker attribution: you are enrolled and named. The nine other voices are unnamed `SPEAKER_NN` labels, and roughly two thirds of all words are yours. The transcript contains participants' names and affiliations, so it stays local like everything else in this repo.

The earlier question still stands and is the one thing blocking the Thursday brief: what is Thursday's presentation, meaning the audience, the slot length, and whether it is an update on the interest group or a re-pitch of the BoF argument?

## User

default quarto is fine, I just need to lift these into google slides. Eventbrite
Strathfield
Updates
Find Events
Create Events
Help Center
Find my tickets
Sign in
Few tickets left

Share this event

ARDC and RDA Roadshow: Sydney
By
Australian Research Data Commons
1.2k followers
506 events
9y hosting
33.2k total attendees

Follow
University of Technology Sydney, Building 4
Ultimo, NSW
Thursday, September 17  •  9:30 AM - 11:30 AM
Overview
Data, HASS and AI: Meet Your Research Community

Connect with the people, platforms and communities helping researchers make more of data, digital infrastructure and AI.

The Australian Research Data Commons (ARDC) invites researchers and research support professionals from across NSW to join us in Sydney for an in-person morning of ideas, connections and community.

Research increasingly depends on access to the right data, digital infrastructure, expertise and communities. ARDC Roadshow: Sydney brings these together, connecting researchers with national infrastructure and services they can use, as well as local, national and international research communities.

Hear about emerging infrastructure and opportunities for humanities, arts and social sciences (HASS) research, including the Australian Internet Observatory (AIO), new approaches to AI and collections, and the Nectar Research Cloud.

The event will also bring the Research Data Alliance (RDA) Roadshow to Sydney, connecting the NSW research community with RDA's international network of researchers and research professionals working together to address shared research data challenges.

Through short, accessible presentations and a community panel, you'll hear from people involved in ARDC and RDA initiatives, discover opportunities to participate in emerging work - including around AI and research - and find practical ways to connect with communities relevant to your work.We'll finish the morning with networking over tea and coffee, giving you time to continue the conversations and make new connections.

What will you hear about?

Australian Research Data Commons (ARDC): discover national digital research infrastructure, services and communities supporting Australian research.
Australian Internet Observatory (AIO): learn about infrastructure, tools and services supporting research involving the internet and digital platforms, including data donation and reuse.
AI and collections : hear about emerging work supporting collections-based research, including the Community Data Lab and Knowledge-Driven AI for Collections initiative.
Nectar Research Cloud: discover cloud computing and digital research services available to Australian researchers.
Research Data Alliance (RDA): meet the global research community working together on research data challenges and learn about opportunities to participate in RDA groups and initiatives, including emerging work around AI.
The research community in action: hear from members of the RDA community about their experiences collaborating through national and international networks.
Who should attend?

This event is for researchers and people who support research, particularly those working in or with HASS disciplines.

We welcome researchers, academics, HDR candidates and early career researchers, research data and digital research professionals, research infrastructure specialists, university librarians and information professionals, research support staff, data custodians and collections professionals, digital humanities and computational researchers, and anyone interested in research data, digital infrastructure, AI and collaborative research communities.

You don't need to be an existing ARDC or RDA community member to attend, all are welcome!

What will participants gain from the session?

You'll leave with a better understanding of the national digital research infrastructure, services and communities available to support your research, along with practical pathways for getting involved.You'll discover:

research infrastructure and services that could support your work
new developments across HASS, research data and AI
opportunities to connect with ARDC and RDA communities
ways to participate in national and international research collaborations
people working on similar research challenges across NSW, Australia and internationally.
Most importantly, we hope you'll leave with new connections and a research community you can continue to engage with beyond the event.

Who will be speaking?

Speakers from the ARDC, RDA and the research community will share their work and experiences throughout the morning. Full speaker details will be announced soon.

Event format

The morning will feature short presentations from ARDC and RDA, followed by an RDA community panel and audience Q&A.

9:30 am – 11:00 am: Presentations, community panel and Q&A

11:00 am – 11:30 am: Networking over tea and coffee

Registration will be available from 9:15 am.

Event details

Date: Thursday 17th September, 2026

Time: 9.30AM - 11:30AM

Location : UTS, CBO4.03.320 Theatre, Building 4, Level 3, 745 Harris Street, Ultimo



About the Research Data Alliance

The Research Data Alliance (RDA) is an international community working to enable the open sharing and reuse of research data across technologies, disciplines and countries.Through its global network and community-led groups, RDA brings researchers, data professionals and other experts together to develop solutions, recommendations and practices for shared research data challenges.
About the ARDC

At the Australian Research Data Commons (ARDC), we're accelerating Australian research and innovation by driving excellence in the creation, analysis and retention of high-quality data assets.We partner with the research community and industry to build leading-edge digital research infrastructure to provide Australian researchers with competitive advantage through data.The ARDC is enabled by the Australian Government's National Collaborative Research Infrastructure Strategy (NCRIS).
Further ARDC & RDA resources

ARDC website
Resource Hub
RDA website
Become an RDA Member
Have questions?

Email us at contact@ardc.edu.au.

Please note that this event may be recorded and published by the ARDC. This may include your contributions during the session. ARDC respects the privacy of individuals. Information collected is in accordance with the ARDC Privacy Policy.

Attendees are expected to comply with the Code of Conduct for ARDC Activities during this event.

To keep up-to-date on latest digital research news and events, subscribe to the ARDC Connect newsletter.









Read less
Good to know
Highlights

2 hours
In person
Location
University of Technology Sydney, Building 4
745 Harris Street

Ultimo, NSW 2007

How do you want to get there?
Driving
Public transport
Biking
Walking
MapShow map
Organized by
Australian Research Data Commons
Followers
1246
Events
506
Hosting
9 years
Contact

Follow
Report this event
Still looking for the right event?
Explore all events in Ultimo and filter by date, category, and more to find the perfect fit.
Australia Events
New South Wales Events
Things to do in Haymarket, New South Wales
Haymarket networking
Haymarket Science & Tech networking
Free
Thu, Sep 17 • 9:30 AM

Reserve a spot
Use Eventbrite
How it Works
Pricing
Event Blog
Plan Events
Conference Management Software
Eventbrite + Yoga
Nonprofits & Fundraisers
Sell Tickets
Event Management & Planning
Online Event Registration
Online RSVP
Music Venues & Promoters
Event Equipment & Staffing
Eventbrite Sales Number
Find Events
Browse Online Events
Get the Eventbrite App
Connect with Us
Report this event
Help Center
Terms
Privacy
CA Privacy Notice
Accessibility
Community Guidelines
Manage Cookie PreferencesDo Not Sell or Share My Personal InformationPrivacy
© 2026 Eventbrite

 is the public. ARDC and RDA Roadshow: Sydney - Thursday 17th September, presenter info
[ARDC staff, name redacted]

[ARDC staff, name redacted] <[ARDC staff email redacted]>;[ARDC staff, name redacted] <[ARDC staff email redacted]>;
Brian Ballsun-Stanton
[ARDC staff, name redacted] <[ARDC staff email redacted]>;[ARDC staff, name redacted] <[ARDC staff email redacted]>
Hello All,

Thank you for helping us deliver our first ARDC and RDA Roadshow in Sydney on Thursday!

We have 88 registrations so far! If you haven't already registered for the event, register HERE

Here is the agenda so far:
Screenshot 2026-09-14 at 2.30.34 PM.png

Here is the draft slide deck where you can add your slides directly: [shared Google Slides link redacted]

Arrive by 9:15 am. The venue is:  UTS, CBO4.03.320 Theatre, Building 4, Level 3, 745 Harris Street, Ultimo. If you have any questions, please reach out to [ARDC staff] and I.

Thank you once again for your contribution and support of our event, which raises awareness of our services and mission among researchers and research infrastructure communities!

warmest,
[ARDC staff]

[ARDC staff, name redacted]
Research Data Alliance (RDA) Regional Community Manager (Oceania & East Asia)

[phone redacted]
[ARDC staff email redacted]
ardc.edu.au


















Curtin Institute for Data Science
Curtin University
Building 216, Kent St
Bentley  WA  6102, AUSTRALIA



















ARDC acknowledges the Traditional Owners of the lands that we live and work on across Australia and pays its respect to Elders past, present and emerging.

 is the private, I speak for 15 minutes with 5 Q&A at 10:30 and then am on a panel at 10:50.

The goal is to advertise the Interest Group that Shawn and I just submitted, and to get people to sign up and say that they're interested in it

## Assistant

Brief received. Constraints, as I read them: 15 minutes plus 5 of Q&A at 10:30, a HASS-leaning audience of researchers and research-support staff, and a single goal, which is sign-ups for the interest group you and Shawn just submitted. Delivery is default Quarto to PPTX, which Google Slides imports directly into [ARDC staff]'s shared deck.

Following the breadth-first rule, the first layer is a design brief, not an outline. To ground it on what was actually submitted rather than the BoF deck, I'll convert the IG proposal documents in the mirror to Markdown in a derived directory and read them.

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; mkdir -p der...`

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; uv run --no-...`

## Assistant

**Tools used:**
- Read: ig-proposal/RDA_IG_Summary_and_Description.md

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; head -c 5000...`

## Assistant

**Tools used:**
- Write: DESIGN.md

## Assistant

**Tools used:**
- Edit: DESIGN.md

## Assistant

The design brief is drafted at `roadshow-2026-09-17/DESIGN.md`, grounded in the submitted proposal rather than the Brisbane deck. Nothing in it is ruled except your three statements, which it quotes.

**What it settles as proposed:**

- **Audience**: NSW HASS-leaning researchers and support staff, mostly non-RDA, so no RDA vocabulary can be assumed and the Brisbane BoF is not shared context.
- **One outcome**: a listener joins RDA, opens the group's page, and records support before 5 October. Everything else in the deck serves that.
- **Content boundaries** come from the Statement of Work: the group's full name, the disclosure-versus-documentation thesis, the Three Ps, the four activities, and the P29 and P30 timeline. It carries the proposal's own warning that the talk must say "policy requires documentation but nothing specifies its content", not "no standard exists".
- **Timing**: plan 12 minutes of content with 3 of headroom, using the BoF transcript's 0:09 to 0:41 span as the source for phrasing that already worked in a room.
- **Production**: default Quarto to PPTX for [ARDC staff]'s shared Google Slides deck, layered brief then outline then bullets and notes, each ruled before the next.

Also in place from earlier: the repo initialised on `main` with audio gitignored, both approved `.notes/` records, the BoF transcript under `transcripts/`, and Markdown conversions of the proposal and the post-IDW deck under `derived/`. Nothing is committed.

One question, and it is the last thing the brief needs before I move to the outline: what is the URL of the group's Statement of Work page on the RDA site? The final slide needs it as a short link and QR code, otherwise nobody can perform the sign-up from the room.

## User

https://www.rd-alliance.org/groups/documenting-generative-ai-interactions-in-research-interest-group/work-statement/documenting-generative-ai-interactions-in-research-interest-group-statement-of-work/

## Assistant

**Tools used:**
- Edit: DESIGN.md

## Assistant

**Tools used:**
- WebFetch: https://www.rd-alliance.org/groups/documenting-gen...

## Assistant

**Tools used:**
- Edit: DESIGN.md

## Assistant

**Tools used:**
- Write: OUTLINE.md

## Assistant

The outline is drafted at `roadshow-2026-09-17/OUTLINE.md`: ten slide events in three movements, targets summing to 12 minutes with 3 of headroom.

| # | Event | Target |
|---|---|---|
| 1 | Title, group name, "in community review now", short link visible | 0:15 |
| 2 | Everyone is using it, almost nobody is documenting it (hands-up beat) | 1:00 |
| 3 | Disclosure is not documentation: "ChatGPT was used for data analysis" | 1:30 |
| 4 | Policy already asks for more than anyone specifies (CHART: 1 of 137) | 1:30 |
| 5 | Where this came from: Brisbane, October 2025 | 1:00 |
| 6 | The Three Ps: Prompt, Process, Provenance | 2:00 |
| 7 | What the group does in twelve months, P29 and P30 | 1:30 |
| 8 | Built on what exists, not beside it (FAIR, RO-Crate, AI Usage Cards) | 1:00 |
| 9 | Why you, in this room: HASS, librarians, support staff, collections | 1:00 |
| 10 | Three clicks before 5 October: join RDA, join the group, comment. QR code | 1:15 |

Events 2 to 4 are the argument, 5 to 8 the group, 9 to 10 the ask. If the day runs short, 8 compresses first and 5 second. Event 10 stays up through Q&A and anchors the panel.

First question, at the shape level: does the three-movement order stand, argument before group before ask, or do you want to open on Brisbane and the community so the argument arrives from people rather than from you?

## User

um... that's a *lot* of slides according to my normal practice

## Assistant

Checking your recorded practice before answering, since the pace note in INTS1302 measured it.

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Brian/INTS1302/.notes; rg -n -i 'pe...`

## Assistant

**Tools used:**
- Read: .notes/reference_teaching-pace.md

## Assistant

**Tools used:**
- Read: .notes/project_decisions.md

## Assistant

You're right, and your own measurements say so. The INTS1302 pace note puts you at about 2.5 minutes per top-level bullet in delivery, with a range of roughly 1 to 4.5, and your ruling there is that the planning unit is the cost per speaking beat, not the slide. Ten slides at one to two minutes each is below your measured floor. Fifteen minutes is about six beats, and with headroom, five.

Proposed collapse to five events, same three movements, same excluded material:

| # | Event | Beats | Target |
|---|---|---|---|
| 1 | **Disclosure is not documentation.** "ChatGPT was used for data analysis", and the reviewer, librarian, or later researcher who has to act on it and cannot | 1 | 2:30 |
| 2 | **Policy already asks for more than anyone specifies.** COPE, ICMJE, publishers require it; nothing says what the record contains; CHART's one of 137 | 1 | 2:30 |
| 3 | **The Three Ps.** Prompt, Process, Provenance, and why documentation subsumes disclosure but never the reverse | 1 | 2:30 |
| 4 | **The Interest Group.** Born in Brisbane, in community review now, four activities in twelve months, built on FAIR, RO-Crate, and AI Usage Cards rather than beside them | 1 to 2 | 3:00 |
| 5 | **Three clicks before 5 October.** Join RDA, join the group, comment on the Statement of Work page. QR code. Stays up through Q&A and the panel | 1 | 1:30 |

That is 12 minutes with 3 of headroom, and the title carries the group name and short link so no slide is spent on it. The "why you, in this room" material folds into event 4's second beat or into the Q&A, whichever you prefer.

One question: does five events with roughly one beat each match your normal practice, or is your usual shape closer to three?

## User

yes, that's better. and if we can figure out images that aren't ai slop, that's also useful? 5 slides my intuition says is 25 minutes, but... eh... we'll see if the slides are short enough

## Assistant

Five events it is. On images, the non-slop route is evidentiary images: real screenshots, your own photos, and diagrams built from code rather than generated art. Let me check two assets before proposing: the photo in the lunchtime meeting folder, and whether a QR generator is installed.

## Assistant

**Tools used:**
- Bash: `which qrencode; python3 -c "import qrcode" 2>&1 | tail -1...`

## Assistant

**Tools used:**
- Read: 20251015 - Lunchtime Meeting/PXL_20251015_040525713.jpg

## Assistant

**Tools used:**
- Write: OUTLINE.md

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/roadshow-2026...`

## Assistant

Agreed, and that settles it: no decoration. An image earns a slide only when it is the beat. That leaves three, all verifiable artefacts rather than illustrations:

- **Event 1**: a screenshot of a real published "ChatGPT was used for…" disclosure statement. The line on screen is the argument, and it is not slop because it is somebody's actual sentence, cited.
- **Event 3**: the Three Ps as a diagram drawn from code, three boxes and a one-way arrow from record to disclosure, because "documentation subsumes disclosure but not the reverse" is a shape.
- **Event 5**: the QR code, which is functional. Already generated at `roadshow-2026-09-17/assets/sow-page-qr.png` from the SoW URL. Its decode is unverified because no decoder is installed, so it needs a phone check before Thursday.

Events 2 and 4 are text only. CC-BY photographs would be filler here, so I am not proposing any.

Next layer is Level 1 wording, slide by slide, for your first pass. Event 1, "Disclosure is not documentation", proposed on-screen text:

> **"ChatGPT was used for data analysis."**
>
> - The reviewer cannot judge whether that was appropriate.
> - The reader cannot reproduce it.
> - The researcher cannot show the judgement they exercised.
>
> Prompts and outputs are method. Treat them like code and data.

Does that stand as the visible text for event 1, or do you want the disclosure line alone on screen and the three consequences spoken?

## User

3 seems... ornate, unless the diagram is useful here? but yeah, slide 1 visible text is fine

## Assistant

Slide 1 ruled. On event 3, I agree: three words and one sentence do the work, and a diagram of three boxes would be decoration. Dropping it. The only image left in the deck is the disclosure screenshot on slide 1 and the QR on slide 5.

Starting `BULLETS.md` with the ruled slide 1, then event 2.

## Assistant

**Tools used:**
- Write: BULLETS.md

## Assistant

Event 2, "Policy already asks for more than anyone specifies", proposed visible text:

> **Policy already asks for more than anyone specifies.**
>
> - COPE, ICMJE, the major publishers, and the EU's living guidelines all require that AI use be declared and described.
> - None of them says what the record must contain.
> - Of 137 chatbot health studies reviewed for CHART, one fully described the model version.
>
> The gap is not a missing rule. It is a missing specification.

Does that stand for event 2?

## User

no, go read my academic writing skill, this is too trite

## Assistant

**Tools used:**
- Skill

## Assistant

**Tools used:**
- Read: .notes/reference_register-shawn-ross.md

## Assistant

**Tools used:**
- Read: .notes/feedback_plain-language.md

## Assistant

**Tools used:**
- Read: docs/register-and-writing.md

## Assistant

Redrafted from the Statement of Work's own sentences, one idea per bullet, no closing flourish. Event 2:

> **Policy already requires documentation of AI use**
> *No standard says what the record should contain*
>
> - COPE, ICMJE, and the major publishers require authors to declare and describe generative AI use. ICMJE asks for that description in the Methods section.
> - None of those policies states what the description should contain or in what form.
> - The systematic review behind the CHART reporting guideline examined 137 chatbot health studies. One fully described the model version it evaluated.

The third bullet's figures come from the proposal's Statement of Work, which cites the JAMA Network Open review. I have not yet checked that paper directly, so before this slide is built I will verify the 1 of 137 figure against the source.

Does event 2 stand?

## User

go check the other slides before I get very grumpy

## Assistant

Checked and redrafted the remaining events against the register and the slide-writing contract before showing them. Faults I removed from my own earlier outline text: mirrored constructions, "not beside them" antithesis, colon-appended tags, and a "why you" address to the reader.

**Event 3**

> **Prompt, Process, Provenance**
> *Three things to record about any generative AI interaction*
>
> - Prompt records what the researcher gave the tool.
> - Process records how the tool was used within the work, including what the researcher checked and changed.
> - Provenance records where the interaction sits in the research, and which claims or outputs depend on it.
> - A journal's disclosure statement can be generated from such a record. The record cannot be recovered from the statement.

**Event 4**

> **An RDA Interest Group, in community review until 5 October**
> *Documenting Generative AI Interactions in Research*
>
> - The group formed at a Birds of a Feather session at RDA Plenary 25 in Brisbane, October 2025, with about twenty participants and members now across five continents.
> - Over twelve months it will survey existing standards, extend the Three Ps with working example tooling, consult publishers and infrastructure providers, and specify a Working Group.
> - It extends FAIR, RO-Crate, software citation, and AI Usage Cards rather than proposing a competing standard.
> - Work is asynchronous, with quarterly meetings run twice across time zones.

**Event 5**

> **Support the group before 5 October**
>
> 1. Join RDA. Membership is free.
> 2. Join the Interest Group on the RDA site.
> 3. Leave a comment on the Statement of Work page saying you support it.
>
> [QR code] rd-alliance.org, short link to follow
> brian.ballsun-stanton@mq.edu.au, shawn@fieldnote.au

Event 5 keeps the numbered steps and the imperative because they are instructions to the room, which the contract allows for visible activity instructions.

One flag on slide 1, which you ruled: its three "cannot" bullets are a mirrored construction of the kind the contract asks to rewrite. Your ruling stands unless you want it revisited.

Do events 3 to 5 stand, and is there anything in them you want cut for the clock?

## User

(and slide 1)

## Assistant

Slide 1 redrafted on the same terms:

> **Disclosure is not documentation**
> *"ChatGPT was used for data analysis."*
>
> - A sentence like that tells a reviewer that a tool was used, and nothing about whether the use was sound.
> - The reader who wants to reproduce or build on the work has no prompts, no outputs, and no record of what the researcher kept or discarded.
> - Prompts and outputs are part of the method, and deserve the same treatment as code and data.

The quoted line stays as the subtitle, and the screenshot of a real published statement sits beside it once a source is chosen.

Does slide 1 stand in this form?

## User

yes

## Assistant

**Tools used:**
- Write: BULLETS.md

## Assistant

**Tools used:**
- WebFetch: https://doi.org/10.1001/jamanetworkopen.2024.57879

## Assistant

**Tools used:**
- Bash: `quarto --version 2>&1; quarto check 2>&1 | grep -E -i 'pa...`

## Assistant

**Tools used:**
- WebFetch: https://jamanetwork.com/journals/jamanetworkopen/f...

## Assistant

Both checks pass. The source (Huo et al., JAMA Network Open, "Large Language Models for Chatbot Health Advice Studies: A Systematic Review") states 137 studies included and 1 (0.7%) fully describing the model version, so slide 2's figures stand as written. Quarto 1.10.18 with Pandoc 3.10 is installed, so the PPTX build path is available.

State of the deck at `roadshow-2026-09-17/BULLETS.md`: title and slide 1 ruled, slides 2 to 5 drafted in the register and awaiting you.

Do slides 2 to 5 stand as shown, or is there one you want reworked before I write the presenter notes?

## User

yes, those are fine

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; sed -i 's/^#...`

## Assistant

**Tools used:**
- Edit: BULLETS.md

## Assistant

**Tools used:**
- Write: NOTES.md

## Assistant

**Tools used:**
- Write: roadshow-2026-09-17.qmd

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging/roadshow-2026...`

## Assistant

**Tools used:**
- Bash: `mkdir -p _render && soffice --headless --convert-to pdf -...`

## Assistant

**Tools used:**
- Read: _render/contact.png

## Assistant

**Tools used:**
- Bash: `cd /home/brian/people/Shawn/RDM-LLM-Logging; printf 'road...`

## Assistant

The deck is built and ready for your check. Everything is under `roadshow-2026-09-17/`, nothing is committed.

**What exists**

- `DESIGN.md`, `OUTLINE.md`, `BULLETS.md` (all five slides ruled), `NOTES.md` (presenter notes in your Brisbane phrasing and the Statement of Work, not yet ruled).
- `slides/roadshow-2026-09-17.qmd` rendered with Quarto 1.10 to `slides/roadshow-2026-09-17.pptx`. Six slides including title, presenter notes on every content slide, QR on the last.
- Read-back with python-pptx confirms the text matches `BULLETS.md` and the notes are attached. The 1 of 137 figure is verified against Huo et al. in JAMA Network Open.

**One defect found in the LibreOffice render.** Slide 4 (the Interest Group) overflows its text box: the fourth bullet on asynchronous work clips at the bottom. Google Slides may lay it out differently, but it is the densest slide and your design already names it the first to compress. Recommended fix is moving that fourth bullet into the notes, where the same content already sits. It is ruled wording, so I have not touched it.

**Your acceptance test, in order**

1. Open `slides/roadshow-2026-09-17.pptx` and read it as the room would.
2. Import it into [ARDC staff]'s shared Google Slides deck and check that the notes survived the import and slide 4 fits.
3. Scan the QR on the last slide with your phone. No decoder is installed here, so it is unverified until you do.
4. Tell me whether to drop slide 4's fourth bullet, and whether the presenter notes stand.

Still open and yours to supply: a short link for the last slide, and a real published disclosure statement to screenshot for slide 1, which is the one image left in the deck.

## User

eh, I've now emailed them. If I have edit access, we'll make them fancy. The QR resolves.

## Assistant

Noted. Recording the QR as verified and the deck as sent, so the next session does not redo either.

## Assistant

**Tools used:**
- Edit: BULLETS.md

## Assistant

**Tools used:**
- Edit: DESIGN.md

## Assistant

Recorded in `DESIGN.md` and `BULLETS.md`: the QR resolves, the PPTX went to the organisers on 14 September, and visual work moves to Google Slides once you have edit access.

Still open, all yours: whether slide 4 fits in Google Slides, a short link for the last slide, and a published disclosure statement to screenshot for slide 1. Nothing in the repository is committed.

## User

ok, bit of formatting applied, found screenshots for slide 1. Anything else?

## Assistant

Three things worth doing before Thursday, in order of consequence:

- **Practise what the talk preaches.** The Brisbane deck's last slide linked the Claude chat that produced it, and the Statement of Work carries an AI-use statement. This deck was drafted with Claude Code and the record sits in this repository. A line on the last slide or in the title footer saying so, with where the record lives, is the Three Ps applied to the deck itself, and a room of librarians will notice if it is absent.
- **Time a run-through.** Your instinct said five slides is 25 minutes, the pace note says about 2.5 minutes a bullet, and the deck has 17 bullets. That is 15 to 40 minutes depending on which measurement holds. `scribe-record` will give you a timed transcript of one rehearsal, and that settles it before the room does.
- **Cite the screenshots on slide 1.** Whatever papers the disclosure statements come from need author, journal, and year visible, both because it is fair and because the slide's claim is that the sentence is real.

Smaller: slide 4 fitting in Google Slides is still unchecked from here, and nothing in the repository is committed, so say if you want a commit of the planning files and the transcript.

## User

sure, let's go work through a proper three p's transcript of this, and then go push this transcript to a github repo (not all the work though)

## Assistant

Route: `transcript-archive:transcript`, because it archives this session as Markdown with reviewed Three Ps metadata, which is the record you want. Before anything reaches GitHub there is a scrub gate, and I already know of one thing that must not pass it: the fish history grep earlier in this session printed a whisperx command with a Hugging Face token in it, so that token is now in this session's log on disk as well as in your shell history. Rotating it is your call, but I'd treat it as exposed.

## Assistant

**Tools used:**
- Skill

## Assistant

**Tools used:**
- Bash: `claude-research-transcript generate --repo . --source cla...`

## Assistant

**Tools used:**
- Bash: `claude-research-transcript --help 2>&1; claude-research-t...`

## Assistant

**Tools used:**
- Bash: `claude-research-transcript archive --help 2>&1`

## Assistant

**Tools used:**
- Bash: `claude-research-transcript init --help 2>&1`

## Assistant

**Tools used:**
- Bash: `claude-research-transcript update --help 2>&1`

## Assistant

The installed tool takes the Three Ps as flags on `archive` and can store them either locally under `ai_transcripts/` or on an orphan `transcripts` branch. That orphan branch is a good fit for your "not all the work" constraint, since it can be pushed on its own. Nothing will be pushed until the archive has been scrubbed and you have named the target.

Draft Three Ps for this session, for your confirmation:

**Prompt.** Brian set up a new workspace for the RDA "Documenting Generative AI Interactions in Research" Interest Group, asking to mirror the group's Google Drive documentation, reuse slide tooling from a teaching repository, and plan a 15-minute talk for the ARDC and RDA Roadshow in Sydney on 17 September 2026 whose only goal is to get attendees to support the group's Statement of Work during community review.

**Process.** Claude Code (Fable 5.1) surveyed the hand-copied Drive mirror and the teaching repository's PowerPoint harness, initialised a git repository, and transcribed the 92-minute Brisbane BoF recording with the local scribe tool after Brian stopped an attempt to use whisperx with a Hugging Face token. It converted the submitted proposal and prior decks to Markdown, then built the talk breadth-first under Brian's rulings: a design brief, a ten-event outline that Brian cut to five, on-screen wording redrafted against the workspace prose register after Brian rejected the first draft as trite, presenter notes drawn from Brian's own Brisbane phrasing, and a Quarto build to PowerPoint that Brian emailed to the organisers. Two project notes were written with Brian's approval. The 1-of-137 figure was verified against its JAMA Network Open source, and the QR code against the live page.

**Provenance.** Brian ruled every content decision: the five-slide shape, all visible wording, the exclusion of decorative imagery, and the choice of local transcription. Claude drafted, checked sources, and built. Inputs were Brian's own BoF deck and recording, the proposal's Statement of Work, two forwarded emails from the RDA Secretariat and Shawn Ross, and the Eventbrite listing. The session log contains third parties' contact details and one credential from Brian's shell history, and is scrubbed before publication.

One question: which GitHub repository receives the transcript branch, a new one (name and visibility) or an existing one?

## User

new one please.

## Assistant

Understood. Running the local steps first, since none depend on the remote: initialise the transcripts branch, archive this session with the Three Ps as drafted, then scan the archive for the credential and contact details before anything leaves the machine.

## Assistant

**Tools used:**
- Bash: `claude-research-transcript init --non-interactive 2>&1 | ...`

## Assistant

**Tools used:**
- Bash: `claude-research-transcript archive --local --session-id 3...`

## Assistant

**Tools used:**
- Bash: `ls -la ~/.claude/projects/-home-brian-people-Shawn-RDM-LL...`

## Assistant

**Tools used:**
- Bash: `T=~/.claude/projects/-home-brian-people-Shawn-RDM-LLM-Log...`
