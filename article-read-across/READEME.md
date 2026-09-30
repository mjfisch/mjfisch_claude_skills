# Article read-across

A Claude skill that turns an external content piece (a podcast episode
transcript, article, blog post, essay, talk, or report) into a structured
read-across memo against your own roadmap, backlog, or a decision you are in
the middle of making.

Most "summarize this article" prompts produce a summary of what someone else
said. This skill produces the document you actually need afterwards: each of
the piece's core ideas mapped to a named item in your program of record, rated
for fit, with a concrete next step or an explicit gap, followed by direct
pushback on the piece's weak points and a one-paragraph call to adapt now,
watch and revisit, or move on.

## What it produces

One markdown memo, `Article_Review_<source>.md`, with six sections:

| Section | What it contains |
|---|---|
| Header | Source, source basis (full transcript, partial, show notes only), comparison target, date |
| Main ideas | 3 to 6 numbered structural arguments, stated neutrally |
| Comparison table | One row per idea: program equivalent, fit rating, gap or action |
| Concepts worth adapting | 3 to 5 prioritized concepts, each with a specific translation and a next step |
| Pushback | 3 to 6 sentences on where the piece overstates, sells, or does not transfer |
| Bottom line | Validates, challenges, or neutral, plus the single most actionable takeaway |

Fit ratings are `Strong fit`, `Partial fit`, `Aspirational`, or
`Not applicable`. Statements carry claim tags (`CONFIRMED`, `INFERRED`,
`RECOMMENDED`, `NEEDS REVIEW`) so a reader can see how much weight each one
holds. Optionally, teams with their own document template or rendering skill
can have the skill emit a rendered companion (HTML, PDF) alongside the
markdown.

## When it triggers

Share an article, transcript, or URL and ask an operations, product, or
project question about it. Examples:

- "Review this episode transcript against our backlog."
- "Here's a post about evaluation harnesses. Is there anything here worth
  using for what we're building?"
- "Compare this article to the review-gate decision we're working on."
- "What does this talk mean for our close process?"

## Installation

**Claude Code.** Copy the `article-read-across/` folder into your personal
skills directory (`~/.claude/skills/`) or a project's `.claude/skills/`
directory. The skill is available in the next session.

**Claude.ai.** Zip the `article-read-across/` folder and upload it through the
skills section of your settings (requires custom skills to be enabled for your
account or organization).

**Claude API.** Upload the folder through the Skills API and reference the
skill in your requests. See the Claude platform documentation for the current
upload and invocation details.

## Configuring for your team

The skill looks for your program of record (status ledger, backlog or build
register, intake queue, decision log) in attached files, project knowledge,
and common repository locations. Forks should fill in the
"Configure for your environment" block at the end of `SKILL.md` so the skill
starts from known files instead of searching:

```yaml
program_of_record:
  status_ledger: docs/STATUS.md
  build_register: docs/BACKLOG.md
  intake_queue: docs/QUEUE.md
  decision_log: docs/decisions/
identifier_pattern: "[A-Z]{2,5}-[0-9]+"
output_directory: memos/article-reviews/
companion:
  enabled: false
  skill_or_template: ""
confidentiality:
  never_name: [clients, customers, accounts, individuals outside the team]
```

If your team has a house document template or an HTML or PDF rendering skill,
set `companion.enabled: true` and name it. The markdown memo remains the source
of truth; the companion is a rendering of it.

## Design notes

- **Anchored, not associative.** Every "program equivalent" must name a real
  item identifier or workstream from your files. If nothing corresponds, the
  memo says so. A clean gap is more actionable than a stretched parallel.
- **Transcripts first-class.** Podcast and talk transcripts are the most common
  input and the messiest. The skill works from the full transcript over show
  notes, normalizes mis-rendered proper nouns and records the normalizations,
  discards sponsor reads, and tags relayed statistics `NEEDS REVIEW`.
- **Pushback is mandatory.** A lot of external content is a fundraising
  narrative or product marketing. The memo separates the structural idea from
  the pitch and says which is which.
- **Confidential by default.** Client names, account identifiers, credentials,
  and personal data in your program files never travel into the memo unless
  you say so.

## Repository layout

```
article-read-across/
├── SKILL.md      # the skill: frontmatter, process, output template, quality controls
├── README.md     # this file
└── LICENSE
```

The skill is self-contained in `SKILL.md`; there are no scripts or bundled
assets to install.

## Contributing

Issues and pull requests are welcome. Keep changes general: the skill should
work for any team with a written program of record, so prefer explaining why a
step matters over adding rules that fit one workflow. Run the skill on two or
three real articles against a real backlog before proposing a change to the
process steps.

## License

MIT. See `LICENSE`.
