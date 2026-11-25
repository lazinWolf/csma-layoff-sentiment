# CSMA: Technology employment disruption on Reddit

This project studies how online communities discuss layoffs, hiring difficulties, AI and changing work, restructuring, and the future of technology employment. We examine experiences, explanations, uncertainty, support, and interactions between participants.

**Current stage (19 November 2026):** data preparation, the midterm ("Solidarity or Blame") and the exploration rounds are done. H1–H3 now have **supported** results. The remaining step is a human check of the keyword labels (`outputs/12_replies/check_sheet.csv`).

## Start here

| What you want | Open this |
|---|---|
| **Everything on one page: problem, data, team, progress, open tasks** | [Master project reference](docs/PROJECT_REFERENCE.md) |
| **The current results, by hypothesis H1–H3** | [Findings by hypothesis](outputs/04_analysis/FINDINGS_BY_HYPOTHESIS.md) |
| How to turn a number into a finding (the routine we use) | [Interpretation routine](docs/INTERPRETATION_ROUTINE.md) |
| H3: what kind of comment gets what kind of reply | [Reply notes](outputs/12_replies/REPLY_NOTES.md) |
| The human label check (to do) | [`outputs/12_replies/check_sheet.csv`](outputs/12_replies/check_sheet.csv) |
| What every output folder is | [outputs/README.md](outputs/README.md) |
| Midterm report and slides | [`deliverables/`](deliverables/) |
| Network (Gephi) findings | [Author-network findings](gephi/analysis/FINDINGS.md) and the [Gephi guide](gephi/README.md) |
| Background exploration | [Language notes](outputs/10_language/LANGUAGE_NOTES.md), [dynamics notes](outputs/11_dynamics/DYNAMICS_NOTES.md) |
| Project overview as of October | [Project summary](docs/PROJECT_BRIEF.md), [process report](docs/PROCESS_REPORT.md), [research design](docs/RESEARCH_DESIGN.md) |
| Run or extend the Python workflow | [Running guide](docs/RUNNING.md) |

## Corpus facts (from the October data audit)

- **The collection is large:** 115,213 posts and 2,859,037 comments across three communities.
- **Coverage is uneven:** technology comments end in June 2025, programming comments in December 2025, and career-question comments in September 2026. Pooled trends need care.
- **Most comment text is usable:** 94.61% of comment bodies are available. Many post bodies are empty because they are link posts; titles remain available.
- **Explicit layoff language is concentrated in career discussions:** r/cscareerquestions contributes 84.90% of directly matching comments, but its original submissions are absent.
- **The broader study has material to review:** six employment-disruption screens retrieve 177,808 distinct candidates. Matching terms are retrieval aids, not confirmed themes or causes.
- **Engagement is skewed:** median scores are 1 for both record types, while means are 655.2 for posts and 13.61 for comments.

The results describe this collected corpus. Collection settings and completeness are unknown. Qualitative interpretations, population-level conclusions, and causal explanations have not been established.

## Files for the next research steps

**Reading and coding:** begin with `outputs/05_sample/employment/reading_sample.csv`. It contains 200 varied examples. Read `reading_packet.jsonl` alongside it when context is needed, then edit `annotations/reading_labels_employment.csv`. Join records by `(kind,id)`; multiple codes use semicolons. Current labels are blank.

**Gephi:** all related material is in [gephi/](gephi/README.md), including guides, scripts, original exports, the user's metrics CSV, rankings, and analysis. The author network has 14,507 nodes and 15,427 directed weighted edges; the conversation network has 13,866 nodes and 13,206 reply edges. Both exports retain their original layoff scope. The user's `gephi/input/authors_keyword-comments.csv` contains Gephi-calculated author metrics. Python verifies it against the original network and produces [findings, rankings, group tables, and four figures](gephi/analysis/FINDINGS.md). The conversation network has not yet been analyzed.

**Methods and definitions:** use [METHODOLOGY.md](docs/METHODOLOGY.md) for processing decisions and [DATA_DICTIONARY.md](docs/DATA_DICTIONARY.md) for fields and outputs.

## Share with a collaborator

Send **`share/csma-review.zip`**. Note: the current ZIP is a 6 October snapshot and does not include the November findings; rebuild it before sharing. After extraction, open **`csma-review/README.md`**.

The ZIP includes the overview, research/methods reports, five corpus plots and four network plots in PNG and SVG, summary tables, candidate screen results, the current reading sample and working labels, original Gephi CSVs, the user's Gephi metrics export and its Python analysis, Python scripts, and tests. It omits raw JSON, SQLite, the large full-corpus CSVs, caches, logs, and the local Python environment. Reports document the full project; `package_manifest.json` inside the ZIP identifies exactly what the snapshot contains and omits.

To refresh the ZIP after changing results or working labels:

```bash
python scripts/build_share_package.py
```

Building the ZIP does not send or publish anything. Keep each collaborator's edited label file separately and merge coding by record key.

## Working-project layout

| Folder | Purpose |
|---|---|
| `docs/` | Research design, methods, process, coding, and running instructions |
| `outputs/` | All generated results; see [outputs/README.md](outputs/README.md) for what each folder is |
| `gephi/` | Network exports, Gephi input, network analysis/figures, guides, scripts, tests, and saved projects |
| `annotations/` | Editable human coding |
| `scripts/`, `tests/` | Reproducible processing and checks |
| `raw/`, `data/processed/` | Original collection and structured working database; full project only |
| `share/` | The generated collaborator ZIP |
| `.cache/`, `.venv-eda/` | Local logs/cache and optional plotting environment; excluded from sharing |

For complete processing commands and optional plotting setup, continue with the [running guide](docs/RUNNING.md).
