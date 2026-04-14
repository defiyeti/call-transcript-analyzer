# 📞 Call Transcript Analyzer — Claude Skill

> A Claude skill that reads your sales call transcripts from Google Drive, scores every deal on MEDDPICC, surfaces your top objections, flags at-risk pipeline, and delivers a polished weekly intelligence report — automatically.

---

## What it does

Every week, this skill connects to a Google Drive folder containing your call transcripts, reads every one, and produces a structured Google Doc report that tells you exactly what happened across your pipeline and what to do about it.

It's designed for Account Executives who want more than a CRM snapshot — it gives you the *conversation-level* intelligence that lives inside calls but rarely makes it into Salesforce.

### Per-call analysis

For every transcript in your folder, the skill extracts:

- **MEDDPICC score** (0–16) — each element scored individually so you know exactly where the gaps are, not just whether a deal is "qualified"
- **Objections** — verbatim, with notes on whether they were resolved, deflected, or left open
- **Buying signals** — classified as Strong / Moderate / Weak / Negative, with the specific moments that drove the rating
- **Competitor mentions** — who came up, how, and in what context
- **Pricing & budget signals** — what was said, by whom, and how it landed
- **Action items & next steps** — what you committed to, what the prospect committed to, and a flag if no next meeting was confirmed
- **Product feedback** — feature gaps, integration concerns, and things prospects wish the product did differently
- **Coaching observations** — one thing you did well, one specific thing to try differently next time

### Aggregated weekly view

Across all your calls, the report surfaces:

- **Top 5 objections** ranked by frequency, with suggested talk tracks for each
- **Competitor leaderboard** — who's coming up most and how you're positioned against them
- **Deals to prioritize** — ranked by buying signal strength + MEDDPICC score combined
- **At-risk flags** — deals with low MEDDPICC scores, no confirmed next steps, or negative sentiment
- **Product feedback themes** — grouped across all calls, traceable back to source conversations
- **Personal coaching summary** — your pattern strengths and one focused area to work on next week

---

## Sample report output

```
📊 Executive Summary
• 6 calls analyzed. Acme Corp and Vertex Health show strongest buying intent this week.
• "We already have a solution for that" came up in 4/6 calls — unresolved in 3 of them. Needs a talk track.
• No next steps confirmed on 2 calls (DataSync, Horizon Retail) — follow up today.
• Salesforce mentioned in 3 calls, all in "evaluating alongside" context.

🔥 Deals to Prioritize
Company         | MEDDPICC | Signal   | Next Step              | Risk
Acme Corp       | 13/16    | Strong   | Legal review by Fri    | —
Vertex Health   | 11/16    | Strong   | Champion intro to VP   | Paper process unknown

⚠️ At-Risk Deals
• DataSync (4/16) — No economic buyer surfaced, no next steps confirmed
• Horizon Retail (6/16) — Went cold after pricing discussion

🚫 Top 5 Objections This Week
1. "We already have something for that" (4 calls, resolved 1/4)
   → Talk track: "That's common — most teams we work with had a point solution before us.
     What's been the biggest gap you've noticed with what you have today?"
...
```

---

## Requirements

| Requirement | Details |
|---|---|
| **Claude app** | [Cowork](https://claude.ai) desktop app (where skills run) |
| **Google Drive MCP** | Must be connected in Cowork — the skill reads your transcript Docs and writes the report back to Drive |
| **Transcript format** | Google Docs in a shared Drive folder. Works with Gong exports, Otter.ai, Zoom auto-transcripts, and manual notes |
| **Sales framework** | Built around MEDDPICC — works best if your team already uses this, but useful even if you don't |

---

## Installation

### 1. Download the skill

Download `call-transcript-analyzer.skill` from this repo (Releases tab, or directly from the root).

### 2. Install in Cowork

Open the Cowork desktop app, then either:
- Drag and drop the `.skill` file into the app, or
- Click the skill file — it will prompt you to install

### 3. Connect Google Drive

In Cowork, connect the **Google Drive MCP** if you haven't already:
- Go to Settings → Connectors
- Find Google Drive and click Connect
- Authorize with the Google account that has access to your transcript folder

### 4. Run it

Just talk to Claude naturally:

> *"Analyze my call transcripts in the 'Q2 Calls' folder and give me this week's report."*

Claude will find the folder, read every transcript, and write the report as a new Google Doc in the same folder (or a "Weekly Reports" subfolder if one exists).

---

## Setting up weekly automation

Once you've confirmed the skill works with your transcripts, you can schedule it to run automatically every Monday morning:

> *"Set up the call transcript analyzer to run every Monday at 8am on my Q2 Calls folder."*

Claude will configure a recurring task. You'll get a fresh report waiting for you at the start of every week.

---

## Folder structure

```
call-transcript-analyzer/
├── SKILL.md                  # Core skill instructions
└── references/
    └── meddpicc.md           # MEDDPICC scoring criteria (0–2 per element)
```

---

## Customizing the skill

The skill is a plain Markdown file (`SKILL.md`) — no code, no build step. You can open it in any text editor and adjust:

- **Sales framework** — swap MEDDPICC for BANT or a custom framework by editing the scoring section
- **Report sections** — add, remove, or reorder sections in the "Write the report" step
- **Coaching criteria** — adjust what counts as good/bad talk ratio, question quality, etc.
- **Folder behavior** — change where the output report is saved

After editing, repackage by zipping the folder with a `.skill` extension and reinstalling.

---

## How it works (for the curious)

This is a **Claude skill** — a Markdown file that gives Claude a specific set of instructions and context when triggered. When you ask Claude to analyze your calls, Cowork loads `SKILL.md` into Claude's context alongside the Google Drive MCP tools. Claude then:

1. Lists all Google Docs in your specified folder
2. Reads each transcript in full
3. Runs the per-call analysis pass across all of them
4. Aggregates findings into the weekly view
5. Writes the final report as a new Google Doc via the Drive MCP

No data is stored outside of your Google Drive. No external servers are involved beyond Anthropic's Claude API and Google's Drive API.

---

## Contributing

PRs welcome. Useful directions:

- Support for additional transcript formats (Chorus, Fireflies, etc.)
- CRM-aware deal stage mapping (e.g., pull current stage from Salesforce to compare against transcript signals)
- Multi-AE team mode (aggregate across a team folder, not just one AE's calls)
- Slack digest output alongside the Google Doc

---

## License

MIT — use freely, modify for your team, share with your org.