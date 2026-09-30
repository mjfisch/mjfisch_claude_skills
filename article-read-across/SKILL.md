---
name: article-read-across
description: >
  Reviews an external content piece (podcast episode transcript, article, blog
  post, essay, talk, or report) and produces a structured read-across memo
  comparing its core ideas to the user's own program of record: a roadmap,
  backlog, build register, status ledger, decision log, or a named design
  decision in progress. Use whenever the user shares an article or transcript
  and asks what it means for their operations or project, how it compares to
  something they are building, or whether any of it is worth adopting. Triggers
  on phrases like "review this article", "review this episode transcript",
  "compare this to our roadmap", "what does this mean for us", "is there
  anything here worth using", or any upload of external content followed by an
  operations, product, or project question. Output is a markdown memo with main
  ideas, a fit-rated comparison table anchored to the user's item identifiers,
  prioritized adaptable concepts with next steps, direct pushback, and a bottom
  line.
license: MIT
---

# Article read-across

Turn an external content piece into a structured read-across memo: what the
piece argues, how each argument maps to what the user is actually building or
running, which concepts are worth adopting and how, and where the piece is
weak.

Throughout this skill, "article" means any external content piece. The most
common input is a podcast episode transcript; written articles, blog posts,
essays, conference talks, newsletters, and vendor white papers follow the same
process.

Why a memo and not a summary: a summary tells the user what someone else said.
A read-across tells them what it means for their own program, idea by idea,
with a fit rating and either a next step or an explicit gap. That is what stops
an interesting piece from turning into unplanned work, or from being forgotten
entirely.

## Inputs

Two things are needed:

1. **The article.** Pasted text, an attached file, a URL to fetch, or a
   transcript export.
2. **A comparison target.** The user's program of record (see below), a
   narrower named item within it, or a design decision in progress.

## The program of record

The comparison only has value when it is anchored to something concrete. Look
for the user's program of record: the documents that describe what is being
built, what is queued, what is blocked, and what has already been decided.
Typical shapes:

- a roadmap, backlog, or build register with stable item identifiers
- a status ledger with workstreams, next actions, and blockers
- an intake queue of groomed, staged, or in-flight items
- a decision log or architecture decision records
- a requirements or design document for a decision in progress

Where to look, in order: files attached to the conversation, the project's
knowledge base or shared documents, then the repository (`ROADMAP.md`,
`BACKLOG.md`, `docs/decisions/`, `docs/adr/`, a status or planning folder).
Read the status and register documents in full before analyzing; pull queue or
sequencing documents only when the article touches prioritization, sequencing,
or queued work. These files supersede anything reconstructed from chat memory
or from older copies.

If the user has forked this skill for their team, the "Configure for your
environment" section at the end names the files directly. Start there.

If no program of record can be found, ask one short question: what should the
article be compared against? If the user is not available to answer, compare
against the operations or project described in the request, state that
assumption in the memo header, and tag every equivalent in the comparison
table `INFERRED`.

## Step 1: fix the comparison target

**A narrower target is named** (an item identifier, a workstream, a decision in
progress): still ground in the full program of record first, so the memo does
not miss adjacent items, then focus the memo on the named target.

**No target is named:** default to the active program as the program-of-record
files describe it. State the assumed target in the memo header.

Anchor every row of the comparison table (Step 3) to item identifiers and
workstream names from these files. A parallel that cannot be anchored is
written as a gap, not invented.

## Step 2: extract main ideas

Read the whole article. Produce a **Main ideas** section:

- 3 to 6 numbered core claims or arguments
- each stated in one or two sentences, neutral, with no evaluation yet
  (evaluation belongs in Steps 3 to 5; mixing it in here biases the comparison)
- if the article has a framing device or metaphor that organizes its argument,
  name it explicitly; it will anchor the comparison table

Extract structural arguments, not the narrative arc. "The author argues that
review gates should sit at the point of maximum information rather than at
fixed calendar intervals" is an idea. "The author then tells a story about a
failed launch" is narrative.

### Podcast and talk transcripts

Transcripts are the most common input and have their own failure modes:

- Work from the full transcript when it is supplied. Show notes and chapter
  lists compress and reorder the argument; use them only for navigation, or
  when the transcript is unavailable, and say so (see "Transcript basis is
  explicit" under quality controls).
- Auto-generated transcripts mis-render proper nouns. Normalize them (for
  example "Clod" to "Claude", "Cooper Netties" to "Kubernetes", "Sequel" to
  "SQL") and record each normalization in the memo's source basis so the
  reader can check them.
- Use the episode's segment or chapter structure to anchor extraction.
- Treat sponsor reads, promotional segments, and host banter as framing to
  discard, not ideas to extract.
- Statistics and claims the speakers relay secondhand ("I read that 70 percent
  of teams...") are unverified. Carry them into the memo only with a
  `NEEDS REVIEW` tag.

## Step 3: build the comparison table

| Article concept | Program equivalent | Fit | Gap or action |
|---|---|---|---|

Fit values:

- `Strong fit`: a direct structural or functional parallel already exists in
  the program
- `Partial fit`: the concept applies but needs translation or scoping to fit
- `Aspirational`: the concept points at something not yet built or decided
- `Not applicable`: the concept does not transfer to this environment, and the
  row says why

Populate one row for every idea from Step 2. The "Program equivalent" cell
names the item identifier and workstream from the program of record. If
nothing corresponds, write `No current equivalent, gap` rather than stretching
a weak parallel. A clean gap is more useful to the reader than a forced match,
because a gap is something they can act on.

## Step 4: concepts worth adapting

A numbered list in priority order, 3 to 5 entries. For each:

- the article's idea, in one sentence
- the translation into a specific place in the user's program ("apply to the
  acceptance criteria on item X", not "apply to quality generally")
- one concrete next step the user could take in their next working session

Do not pad. Three strong entries beat five with two weak ones. The reader acts
on the top of the list and loses trust in the whole memo if the bottom is
filler.

## Step 5: pushback

Three to six sentences, direct. Cover:

- where the article overstates, generalizes from a single case, or assumes
  resources, scale, or tooling the user does not have
- concepts that sound compelling but are impractical in this environment, and
  why
- what the article actually demonstrates versus what it merely asserts or
  finds directionally interesting

Do not soften criticism of weak source material. Much external content is a
fundraising narrative, a product launch, or thought-leadership marketing. The
reader relies on this section to separate the structural idea from the pitch.

## Step 6: bottom line

One paragraph, three to five sentences:

- whether the article validates, challenges, or is neutral on the program's
  current direction
- the single most actionable takeaway
- a directional call: **adapt now**, **watch and revisit**, or **not relevant**

## Step 7 (optional): rendered companion

Some teams keep a rendered companion (an HTML page, a PDF, a styled document)
alongside every markdown memo, generated through their own template or
document skill. If the user's environment has one:

1. Generate the companion only after the markdown memo is final.
2. Map the memo's sections onto the companion's navigation in this order:
   overview and sources, main ideas, comparison table, concepts worth adapting,
   pushback, bottom line.
3. Render claim tags and fit values as the template's tag or status components,
   not as bracketed text.
4. Run whatever validation the template provides, and fix every reported issue
   before delivery.
5. Give the companion the memo's base name with the companion's extension, and
   deliver both files together.

The markdown memo is always the source of truth; the companion is a rendering
of it. If the memo is revised, regenerate the companion. If no template or
document skill exists, deliver markdown only; do not hand-build a styled page.

## Claim tags

Tag statements in the memo so the reader knows how much weight each one
carries:

- `CONFIRMED`: stated in the article itself, or verified against the program
  of record
- `INFERRED`: a reasonable reading that neither the article nor the program
  files state outright
- `RECOMMENDED`: the memo's own suggestion
- `NEEDS REVIEW`: a relayed statistic, a secondhand claim, or anything the user
  should verify before reuse

Tag comparison-table equivalents and adaptable-concept translations at minimum.
In prose, tag only where the weight is not obvious from context.

## Output format

Deliver the memo as a markdown file named
`Article_Review_<short-source-slug>.md` (plus its companion per Step 7 when
applicable). Use this structure:

```markdown
## Article review: [article or episode title]
**Source:** [publication or show, author or hosts, date, URL if available]
**Source basis:** [full text | full transcript | partial transcript | show notes and chapters only], plus any name normalizations applied
**Comparison target:** [named item, workstream, decision, or "active program as described in <files>"]
**Date reviewed:** [YYYY-MM-DD]

---

### Main ideas
1. ...
2. ...

---

### Comparison to [target]
| Article concept | Program equivalent | Fit | Gap or action |
|---|---|---|---|
| ... | ... | ... | ... |

---

### Concepts worth adapting
1. **[Concept name]**: [article idea]. In this program: [translation]. Next step: [concrete action].

---

### Pushback
[3 to 6 sentences]

---

### Bottom line
[3 to 5 sentences, ending with adapt now / watch and revisit / not relevant]
```

## Worked example

Input: a transcript of a podcast episode in which two engineering leaders argue
that AI coding agents should be given a per-repository "definition of done"
file, that human review should move from every change to sampled audits once
error rates are known, and that most teams over-invest in prompt tuning and
under-invest in evaluation harnesses. The user's program of record is a backlog
with items `INF-12` (agent runner), `QA-4` (acceptance checklist, kept on a
wiki page), and `QA-9` (evaluation harness, not started).

Comparison rows that follow from this:

| Article concept | Program equivalent | Fit | Gap or action |
|---|---|---|---|
| Per-repository "definition of done" file for agents | `QA-4` acceptance checklist (wiki page, not in the repo) | Partial fit | Move `QA-4` into the repo as a machine-readable file the `INF-12` runner reads at start |
| Sampled audits replace per-change review once error rates are known | `No current equivalent, gap` | Aspirational | Needs the `QA-9` harness to produce error rates first; sequence after `QA-9` |
| Teams over-invest in prompt tuning, under-invest in evals | `QA-9` evaluation harness (not started) | Strong fit | Validates prioritizing `QA-9`; no new item needed |

The pushback for this piece would note that both speakers sell agent tooling,
that the sampled-audit claim rests on one company's internal numbers
(`NEEDS REVIEW`), and that the definition-of-done idea is sound but presented
as novel when it restates long-standing acceptance-criteria practice.

## Quality controls

- **No hallucinated parallels.** Every program equivalent is anchored to the
  program of record, a documented workflow, or an explicit statement by the
  user. If none exists, write `No current equivalent, gap`.
- **No false urgency.** Recommend adapting a concept only when there is a
  specific, near-term place for it in the program. "Sounds good" is not a
  reason.
- **Separate the concept from the pitch.** Extract the structural idea, discard
  the promotional framing, and name the commercial interest in the pushback
  when there is one.
- **Confidentiality by default.** Do not carry client or customer names,
  account identifiers, credentials, personal data, or other confidential
  details from the program of record into the memo, even when the source files
  contain them. Use generic references ("a high-complexity account", "the
  close checklist") unless the user directs otherwise. The memo may travel
  further than the files it was built from.
- **Flag weak source material.** Heavy reliance on anecdote, unnamed sources,
  or unverifiable claims goes in the pushback section. Relayed statistics carry
  `NEEDS REVIEW`.
- **Transcript basis is explicit.** State in the header whether the source was
  the full text or transcript, a partial transcript, or show notes and
  chapters only. Ideas extracted from anything less than the full source are
  tagged `INFERRED`. When the full source arrives later, rerun the skill and
  note in the new memo that it supersedes the earlier one.
- **Companion parity.** When a rendered companion is produced, it carries the
  same content as the markdown, section for section, and ships only after the
  template's validation passes.

## Configure for your environment

Teams that fork this skill should fill in this block so the skill starts from
known files instead of searching for them. Delete the lines you do not use.

```yaml
program_of_record:
  status_ledger: docs/STATUS.md        # workstreams, next actions, blockers; read in full
  build_register: docs/BACKLOG.md      # prioritized items with stable identifiers; read in full
  intake_queue: docs/QUEUE.md          # queued, staged, in flight; read when sequencing is in play
  decision_log: docs/decisions/        # locked decisions; a memo may cite one, never re-open one
identifier_pattern: "[A-Z]{2,5}-[0-9]+"  # how items are referenced, for example QA-9
output_directory: memos/article-reviews/
companion:
  enabled: false
  skill_or_template: ""                # name of your HTML, PDF, or document skill, if any
confidentiality:
  never_name: [clients, customers, accounts, individuals outside the team]
```
