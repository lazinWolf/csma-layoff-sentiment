# Solidarity or Blame: master project reference

*Last updated 25 November 2026. This is the single page that describes the whole project. It points to the detailed files; it does not replace them. When something changes, update it here first.*

---

## 1. At a glance

| | |
|---|---|
| **Course project** | CSMA (computational social media analysis), team project |
| **Title** | *Solidarity or Blame*: how Reddit communities discuss disruption in technology employment |
| **Data** | About 2.97M Reddit posts and comments from r/cscareerquestions, r/technology and r/programming, Jan 2025 – Sep 2026 |
| **Stage** | Midterm delivered. H1–H3 now have **supported** results. The human label check, which is needed before results count as findings, is **not started** |
| **Next action** | Fill in `outputs/12_replies/check_sheet.csv` (240 rows, yes/no). Two coders are needed for agreement |
| **Unknown** | The final deadline and the final report requirements are not recorded anywhere. Add them here when known |

## 2. The problem

**What we are trying to understand.** Since 2023, technology work has been disrupted by layoffs, a hard hiring market, AI changing jobs and restructuring. People talk about this online. We study how they **make sense of it together**:
- whether they share their own experiences;
- whom they blame: executives, offshoring, foreign workers, AI, or themselves;
- how others respond, with comfort, advice, agreement or pushback.

**Why it is interesting.** Most studies reduce such talk to sentiment, meaning positive or negative. We look at what a comment *does* and how the community *responds*. That is the difference between a community that offers **solidarity** and one that turns to **blame**.

**Research gap** (from the midterm report). There is no controlled comparison across communities that joins human-coded solidarity and blame to reply structure. Related work:
- Bose (2026) found an amplification gap on X that did not replicate on Reddit.
- Ton (2025) found displacement concern and adaptation in subreddits for specific professions.
- Yin, Wong and Reddy (2025) found that people seek different kinds of support and may get mismatched replies.

**Scope.** This is a pilot comparing communities within Reddit. It is **not** cross-platform, it is **not** a representative sample of tech workers, and it makes **no** causal claims.

## 3. Objectives and hypotheses

**Midterm objective.** Develop validated solidarity–neutral–blame stance and theme labels through human coding. Then connect them to reply networks and to trends over time.

| | Hypothesis (midterm wording) | Status at midterm (Oct) | Status now (Nov) |
|---|---|---|---|
| **H1** Community concentration | Employment-disruption language differs between r/cscareerquestions and r/technology in the same observation window | Preliminary support | **Supported**. Layoffs are ambient in cscq and event-driven in r/technology |
| **H2** Discourse function | Career discussion contains more worker-centred support and experience narratives; technology discussion has more news and industry framing | Not tested | **Supported, then narrowed.** Who gets blamed comes with the person, not the community (D1) |
| **H3** Reply structure | Validated solidarity and blame labels are associated with different reply and engagement patterns | Not tested | **Supported.** Becomes a *Finding* after the human label check |

The broader research questions, from `docs/RESEARCH_DESIGN.md`, cover experiences, explanations and imagined futures, social responses, interaction structure, and change over time against external events. H1–H3 are the parts we are actually testing.

## 4. Team and roles

| Member | Role |
|---|---|
| Vanshit Ahuja | Research framing, hypotheses, shared-window comparison, statistics, literature review, report and slides, QA |
| Vaibhav Jain | Reddit data collection (Arctic Shift), provenance, coverage checks |
| Tanya | Data cleaning, thread linkage, descriptive analysis, community comparison, figures |
| Krish | Lexical and stance baselines, reading sample, coding framework, NLP evaluation. **Owns stance labels:** check with him before presenting label-based results |
| Dividh Vijay | All Gephi and network work: exports, edge direction, weights, scope, validation. **Owns the reply-structure part of H3** |

## 5. Data

### Source and size
- Collected with **Arctic Shift** by Vaibhav. The original search settings are **unknown**.
- **115,213 posts** and **2,859,037 comments**, which is 2,974,250 records.

### Coverage is uneven
This is the most important caveat about the data.

| Community | Comments from | Comments to | Comments | Note |
|---|---|---|---:|---|
| r/cscareerquestions | 2025-01 | **2026-09** (23 Sep) | 962,337 | Its original posts are **absent** |
| r/technology | 2025-01 | **2025-06** (9 Jun) | 1,657,900 | |
| r/programming | 2025-01 | **2025-12** (12 Dec) | 238,800 | |

- **Comparisons between communities** use the shared window:
  - the midterm used **Jan–May 2025**;
  - the November analyses use **Jan–Jun 2025**, which includes r/technology's partial June (see section 9).
- **Trends over time** use cscq only.

### Preparation
- 94.61% of comment bodies are usable.
- Text was cleaned:
  - HTML was decoded;
  - quoted lines were separated, so `own_text` is the author's own words;
  - deleted and removed text was marked.
- Authors were replaced with stable pseudonyms (`author_key`), and bots are flagged (`likely_bot`).
- The reply structure was rebuilt from parent links. Missing parents are kept and marked.
- Details: `docs/METHODOLOGY.md`, `docs/DATA_DICTIONARY.md`.

### Where the data lives

| What | Path |
|---|---|
| Working database, opened read-only by every analysis | `data/processed/reddit.sqlite` |
| Main tables | `records`, `features` (text, author, bot flag), `disruption_screen` (6 employment screens), `reply_edges`, `threads` |
| Flat exports | `outputs/03_tables/` |
| Raw files | `raw/` (full project only) |

### Employment screens and markers
- **Six keyword screens** flag employment candidates (177,808 records): layoffs, hiring conditions, AI and work, restructuring, economic policy, and career transition. **A match is a retrieval aid, not a theme.**
- **Comment-type markers** are rule-based regex patterns in `scripts/explore_leads.py`, in the dict `M`.

| Marker | What it catches | What it really is | Provisional precision |
|---|---|---|---|
| `experience` | "I was laid off…" | Speech act (telling your own story) | 39/40 on layoff comments; about 11/20 on all employment comments |
| `mentions_execs` | executives, shareholders, greed | **Topic**, not stance | n/a |
| `mentions_offshore` | offshoring, H-1B, visas | **Topic.** Only 7/70 blame foreign workers | n/a |
| `ai_excuse` | "AI is just the excuse" | Topic or claim | n/a |
| `ai_replaces` | "AI will replace…" | Catches rejections too (7/30 endorse) | n/a |
| `doom` | "the market is cooked" | Claim | n/a |
| Reply: `sympathy`, `advice`, `agree`, `challenge` | Reply function | Speech act | Not yet checked |
| Layoffs screen | | | About 95% |
| Hiring-conditions screen | | | **About 22%**: do not quote its numbers |

All precision figures so far are *Claude-provisional* and need a human check.

## 6. Method: how we work

1. **Question first.** Each analysis starts from a question tied to H1–H3 and has a rival explanation that a number can rule out. There is no open-ended exploration and no close reading of whole threads.
2. **Interpretation routine** (`docs/INTERPRETATION_ROUTINE.md`), applied to every result:
   - **say it** as what people did;
   - compare it with what you **expected**;
   - list **rival** explanations, including the boring ones (R1–R8: label, denominator, a few threads, coverage, timing, length, bots, outside events);
   - **check** the other number each rival predicts;
   - **read** 5–10 examples;
   - **name** the concept from the literature.
3. **Claim labels:**
   - **Lead:** one number;
   - **Supported:** it survived the rivals and one independent measure agrees;
   - **Finding:** three or more measures converge and the labels are human-checked.
4. **Statistics:**
   - lift, the share of a reply type divided by its share for unmarked comments;
   - thread-clustered bootstrap confidence intervals;
   - robustness checks within a length band;
   - within-author comparisons;
   - shared-window denominators.
5. **Network.**
   - The author network has edges from replier to parent author.
   - The "type network" has nodes for kinds of comment and kinds of reply, with lift as edge weight.
   - Interpretation is delayed until labels are validated, because a reply is contact, not agreement.

## 7. Progress timeline

| When | What | Where |
|---|---|---|
| Early Oct 2026 | Data audit, preprocessing, SQLite database, tables, descriptive analysis, 5 figures | `outputs/01`–`04`, `docs/PROCESS_REPORT.md` |
| Oct | Six employment screens and a 200-record reading sample, with blank labels | `outputs/03_tables`, `outputs/05_sample`, `annotations/` |
| Oct | Gephi author network: 14,507 users, 15,427 edges. Metrics verified in Python | `gephi/analysis/FINDINGS.md` |
| Oct | **Midterm:** report, 15-slide deck, H1 shared-window comparison (11.36×), lexical stance baseline | `deliverables/`, `outputs/08_midterm/` |
| Nov 14–15 | Exploratory leads: markers, precision samples, H-1B spike | `outputs/09_leads/` |
| Nov | Language and dynamics: distinctive vocabulary, adoption, votes, linked sources | `outputs/10_language/`, `outputs/11_dynamics/` |
| Nov 19 | **H3 reply analysis:** kind of comment → kind of reply | `outputs/12_replies/REPLY_NOTES.md` |
| Nov 19 | Interpretation routine written and applied. Checks for H1 and AI-excuse | `docs/INTERPRETATION_ROUTINE.md`, `outputs/13_interpretation/` |
| Nov 19 | **Findings by hypothesis** (archived Nov 25; replaced by the layered reference) | `outputs/archive/FINDINGS_BY_HYPOTHESIS.md` |
| Nov 19 | Four directions: same author, sympathy and return, H-1B shock, core vs periphery | `outputs/14_directions/DIRECTIONS_NOTES.md` |
| Nov 19 | Project tidy-up: old plan archived, outputs index added | `outputs/README.md`, `outputs/archive/` |
| Nov 25 | **R4 author-type network:** centrality, mixing, communities, rivals (cscq, Jan–Jun 2025) | `outputs/15_network/NETWORK_NOTES.md` |

## 8. Current results

Full write-ups: `docs/PROJECT_REFERENCE_2.md` (layers 4–7), `outputs/14_directions/DIRECTIONS_NOTES.md` and `outputs/15_network/NETWORK_NOTES.md`.

**H1. Layoffs are ambient in cscq and occasional news in r/technology.** (Supported)
- 2.45% vs 0.20% of comments (Jan–Jun 2025).
- Even inside threads that mention layoffs at all, the rate is 4.06% vs 0.49%.
- 67% of large cscq threads contain layoff talk, against 21% in r/technology.

**H2. Different evidence cultures.** (Supported, then narrowed)
- cscq gives personal accounts: own stories 8.9% vs 3.1%. It talks about consequences and links to labour statistics (FRED, BLS).
- r/technology uses corporate and political framing and links to news sites.
- r/programming talks about the craft and its tools.
- **Narrowing (D1).** We compared the same 160 authors in both communities. Their mentions of executives are about equal (6.6% vs 5.8%), so **blame framing is about who posts**. Their AI talk still differs (+9.4 points), so **topics are shaped by the community**.

**H3. Replies follow what a comment does, not whom it blames.** (Supported)
- In cscq, telling your own layoff story gets sympathy replies **2.8×** as often as unmarked comments (5.1% vs 1.8%) and gets less pushback.
- Comments about executives, offshoring or AI as an excuse all get the same pattern: a little more agreement and less support.
- Doom draws advice (×1.4).
- Checks against rivals:
  - advice does not rise, so stories are not just requests for help;
  - the effect holds within a length band;
  - the CI is thread-clustered;
  - votes agree.
- **Concept:** emotional vs informational support. **Solidarity follows disclosure, not a shared enemy.**

**Other results**
- **"AI as an excuse" did not grow.** It only looked that way because AI talk overall doubled (a denominator effect). This corrects an earlier note.
- **H-1B announcement (19 Sep 2025)** (D3). Offshoring talk jumped from about 13% to 43% for one week, then fell below the earlier level. Its framing did not change: a shock, not a shift.
- **Return after a story** (D2, lead). Newcomers who get *any* reply come back more often (50% vs 39%). Sympathy itself is too rare to judge.
- **Core vs periphery** (D4). Story-sharers and blamers have the same tenure, but they are different people.
- **R4. Author-type network (Nov 25, `15_network`)** on the cscq reply graph (30,976 authors in the LWCC, 97%):
  - **Position follows activity, not role** (contradicts the expected "central helpers → peripheral story-sharers"). Once activity is matched, story-sharers are not on the periphery and helpers are not hubs. This covers story-sharers who post comments only; people who post their story as a post are not in the data.
  - **Sympathy flows to story-sharers** (lead). They get 1.6–1.9× more comfort, whoever is replying, and helpers supply most of it.
  - **Blame-talkers form an argument group** (supported). They reply to each other 1.7× more than chance (z = 8, against a within-thread null), and they challenge rather than agree.
  - Louvain communities (Q = 0.43) are **not** sorted by type.
- **Author network (Oct, earlier Gephi export with a different scope).** It is fragmented: 64% of users are in the largest component and 97% of pairs exchange only one reply. Attention is concentrated: the top 1% of users get 17.8% of replies.

**One-sentence answer to the title:** in cscq, solidarity goes to people who say what happened to them. Blame, in any direction, starts debates. Who blames whom is something people bring with them, not something a community produces.

## 9. Known problems and caveats

| Issue | Effect | Handling |
|---|---|---|
| Unknown collection settings | We can't claim prevalence | Describe "the collected corpus" only |
| Uneven coverage by community | Trends get confused with coverage | Shared window for comparisons; trends in cscq only |
| Shared window differs: midterm Jan–May, November Jan–Jun 2025 | Small differences in numbers (2.32% vs 2.45%) | **To decide:** pick one window for the final report |
| No cscq posts (0 in the database, all months) | Thread topics are unknown. In the reply network, 108,722 replies to original posters (40%) have no target, and layoff stories told *as posts* are invisible | Use comments; top-level comments as a stand-in. R3 and R4 describe comment-to-comment conversation only |
| "Agree" reply marker fires on any reply starting with "This", including hostile ones (R4 reading) | Agreement is overstated | Report blame responses as *debate*, not agreement; add to the check sheet |
| Labels are keyword markers | Topic ≠ stance (the "blame sideways" label was about 95% just mentions) | Rename to topic labels; human check sheet |
| Hiring screen precision about 22% | The midterm's 18.7× ratio is unreliable | **Stop quoting it** |
| Reddit scores are snapshots, early arrival dominates | Vote analyses are biased | Within-thread percentiles; control for arrival time |
| Cross-posters are a special group (D1) | The within-author result may not generalise | Report n=160 and the caveat |

## 10. Open tasks

| # | Task | Who | Blocks |
|---|---|---|---|
| 1 | **Human check sheet:** 240 rows yes/no. Two coders give precision and agreement | Dividh plus one teammate | H3 → Finding, all label claims |
| 2 | Tell Krish about the H3 and D1 results before presenting them | Dividh | Team alignment on stance labels |
| 3 | Remove the 18.7× hiring figure from headlines | Whoever edits the final report | |
| 4 | Pick one shared window: Jan–May or Jan–Jun 2025 | Team | Consistent numbers |
| 5 | Gephi slide: 11-node type network (`outputs/12_replies/gephi/`), and author nodes coloured by "story vs offshore" | Dividh | Final presentation |
| 6 | Date the second offshoring spike (mid-Dec 2025) | Anyone | D3 |
| 7 | Code the 200-record reading sample (`annotations/reading_labels_employment.csv`), midterm promise | Krish and team | Stance classifier evaluation |
| 8 | Rebuild the share ZIP (`python scripts/build_share_package.py`); the current one is from 6 October | Before sharing | |
| 9 | Record the final deadline and report requirements here | Team | Planning |

The midterm also promised VADER and transformer stance evaluation, topic modelling and external labour indicators. **These have not been started.** Do them only if a hypothesis needs them.

## 11. File map

| Need | File |
|---|---|
| This overview | `docs/PROJECT_REFERENCE.md` |
| The project as layers 0–7 (collection → concepts), with status, concept verdicts and next steps | `docs/PROJECT_REFERENCE_2.md` |
| Results by hypothesis (archived) | `outputs/archive/FINDINGS_BY_HYPOTHESIS.md` |
| How to interpret a number | `docs/INTERPRETATION_ROUTINE.md` |
| What every output folder is | `outputs/README.md` |
| H3 details | `outputs/12_replies/REPLY_NOTES.md` |
| Four directions | `outputs/14_directions/DIRECTIONS_NOTES.md` |
| R4 author-type network (GEXF for Gephi) | `outputs/15_network/NETWORK_NOTES.md`, `network_profiled.gexf` |
| Tableau extracts for R1–R5 | `outputs/16_tableau/TABLEAU_GUIDE.md` |
| Network findings and Gephi guide | `gephi/analysis/FINDINGS.md`, `gephi/README.md` |
| Research design and questions | `docs/RESEARCH_DESIGN.md` |
| Processing decisions and fields | `docs/METHODOLOGY.md`, `docs/DATA_DICTIONARY.md` |
| Running the pipeline | `docs/RUNNING.md` |
| Midterm deliverables | `deliverables/` (report PDF/DOCX, slides, `MIDTERM_HANDOFF.md`) |
| Archived plans and notes | `outputs/archive/` |

**Analysis scripts.** All are read-only on the database. Run each with `.venv-eda/bin/python scripts/<name>.py`.

| Script | Output folder |
|---|---|
| `explore_leads.py` | `09_leads` |
| `explore_language.py` | `10_language` |
| `explore_dynamics.py` | `11_dynamics` |
| `explore_replies.py` | `12_replies` |
| `check_interpretations.py` | `13_interpretation` |
| `explore_directions.py` | `14_directions` |
| `explore_network.py` (needs networkx, scipy, scikit-learn) | `15_network` |
| `export_tableau.py` (run after `explore_network.py`) | `16_tableau` |

**Tests:**
```
python3 -m unittest discover -s tests
python3 -m unittest discover -s gephi/tests
```
