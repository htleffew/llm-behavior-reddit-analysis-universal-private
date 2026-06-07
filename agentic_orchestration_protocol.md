# Agentic Orchestration Protocol

**Purpose.** This document specifies how the procedural method (`community_reported_llm_behavior_method.md`) and methods library (`methods_library.md`) are *executed* under single-operator constraints with agent assistance. The procedural method tells you *what* to do at each phase; the methods library tells you *which technique to choose*; this protocol tells you *how to dispatch work and when to pause for human judgment*.

It is the third load-bearing artifact for any project that adopts the universal method. The procedural method is the spine; the methods library is the toolkit; this protocol is the production system.

---

## 1. Roles

### 1.1 The human (Dr. Leffew)

Irreducibly human concerns:

- **Phase 0 seed encounters.** The researcher's lived experience is data, not background. Cannot be delegated.
- **Phase 6 precision-at-N hand-coding.** The 50-item TP/FP coding that validates each dictionary. Agents prepare samples and the recording interface; the judgment is the researcher's.
- **Phase 7 construct naming.** Agents present evidence; the researcher names the constructs.
- **Phase 9 reflexive synthesis.** The writeup is the researcher's voice. Agents can produce drafts; the final synthesis is hers.
- **All AskUserQuestion checkpoints.** Every "do these clusters look real?", "should these be in the stop-word list?", "is this dictionary boundary correct?" is a researcher decision.

### 1.2 The orchestrator (Claude Opus 4.7, this assistant)

What I do:

- **Dispatch agents** with precise briefs against the procedural method and methods library.
- **Select models per dispatch.** Haiku 4.5 for clerical/descriptive runs (cheap, fast); Sonnet 4.6 for general analysis and excavation; Opus 4.7 (myself) for integration and decision-synthesis work that consumes the agent outputs.
- **Background vs. foreground.** Long-running scrapes and parallel analytical passes run in background. I am notified on completion; I do not poll.
- **Integration.** When agent outputs return, I integrate findings into the methods library, the project notebooks, the deliverables, and the audit trail.
- **Checkpoint construction.** I build the AskUserQuestion at each procedural-method decision point, with previews that show the evidence the researcher needs to decide on.

### 1.3 Dispatched agents

What they do:

- **Excavation agents** read prior sources (notebooks, repos, dissertations) and return structured findings against schemas the orchestrator provides. Default: Explore subagent, Sonnet 4.6.
- **Execution agents** run scrapers, analytical scripts, and notebook cells against the active corpora. Default: general-purpose subagent, Sonnet 4.6, sometimes Haiku 4.5 for descriptive-only work.
- **Plan agents** design implementation steps for specific complex phases. Default: Plan subagent, Opus 4.7.

What they do not do:

- Make construct claims.
- Decide whether a cluster is "real."
- Name constructs.
- Hand-code the precision-at-N sample.
- Write the case characterization.

---

## 2. Procedural method × orchestration map

For each phase of `community_reported_llm_behavior_method.md`, the orchestration role is specified.

| Phase | Orchestration mode | Human checkpoint type |
|---|---|---|
| §C.0 Triggering Observation | **Human-only.** Researcher writes the seed encounter and community-report scan. | n/a |
| §C.1 Corpus Definition & Scrape | **Mixed.** Researcher specifies subreddits, time window, seed terms (per §C.1 rules). Agent executes the scrape. | Approve seed term list before scrape; review scrape coverage report after scrape. |
| §C.2 Descriptive Engagement | **Agent-heavy.** Two parallel agents per corpus: frequency/n-gram/collocation pass and KWIC/co-occurrence/network pass. Each produces tables saved to `deliverables/`. | "Do top terms look like signal? Stop-words to add to the domain list? KWIC contexts that change your read of an anchor term?" |
| §C.3 Unit-of-Analysis Determination | **Agent + human.** Agent re-runs §C.2 analyses at candidate units. Researcher chooses the primary and secondary unit. | "Which unit shows the most coherent signal? Document the decision." |
| §C.4 Voice Segmentation | **Agent-heavy.** Agent implements regex + LLM-fallback segmentation per methods library §9, runs hand-validation sample preparation. Researcher hand-codes the 50–100 item validation set. | Hand-code segmentation validation; decide if ≥70% threshold met. |
| §C.5 Inductive Theme Discovery | **Agent-heavy.** Agent runs topic modeling at multiple k with mandatory stop-word vectorizer (per methods library §2.1) and sensitivity sweeps. Produces stability matrix. | "Of these clusters, which are themes vs. noise? Which to retain for §C.6?" — typically AskUserQuestion with previews showing top terms and KWIC samples per cluster. |
| §C.6 Lexicon Construction & Refinement | **Iterative agent + human.** Agent derives candidate dictionary from theme evidence, prepares hand-coding sample of 50 items, computes precision-at-N. Researcher hand-codes. Agent revises dictionary based on revealed misfires. Loop until precision ≥ 0.85. | Hand-code; review precision; approve/refine dictionary. |
| §C.7 Construct Formation | **Human-led.** Agent presents the evidence chain (themes + dictionaries + KWIC + foundational prior coherence) per construct. Researcher names. | Name the constructs. |
| §C.8 Inferential Analysis | **Agent-heavy.** Agent runs PMI, segmented regression / ITS, regressions per methods library §3 and §11. Each statistical claim is between two validated constructs. | Review effect sizes, sensitivity, and whether to report findings as central, supporting, or excluded. |
| §C.9 Reflexive Synthesis & Writeup | **Human-led.** Agent can produce a draft skeleton citing the audit trail. Researcher writes the case characterization. | Draft, revise, finalize. |
| §C.10 Cross-Corpus Comparison | **Far downstream.** After both LCR and sleep complete §C.9. Same orchestration pattern applies but across two completed studies. | n/a until both studies complete. |

---

## 3. Checkpoint patterns

### 3.1 Pattern A — "Theme vs. noise" checkpoint

After a §C.5 topic-model run, the AskUserQuestion presents each candidate cluster with:
- Preview: top 10 terms + 5 random KWIC contexts from the cluster's most distinctive term.
- Options: `Theme-eligible / Noise (stop-word smear) / Unstable / Defer`.
- Multi-select where applicable.

### 3.2 Pattern B — "Stop-word addition" checkpoint

After §C.2 frequency analysis reveals high-frequency content-bearing or domain-bearing terms that may be noise:
- Preview: term, frequency, 3 KWIC contexts, suggested classification (signal / domain stop-word / standard stop-word miss).
- Options: `Add to domain stop-words / Keep as signal / Inspect further`.
- Multi-select.

### 3.3 Pattern C — "Dictionary boundary" checkpoint

During §C.6 dictionary refinement, after the agent has surfaced candidate terms for inclusion:
- Preview: candidate term, frequency in corpus, external lexicon membership (LIWC / NRC / mental-health-datasets cross-walk), 5 KWIC contexts showing the term in use.
- Options: `Include / Exclude / Split (multiple senses) / Refer to literature first`.

### 3.4 Pattern D — "Construct naming" checkpoint

After §C.7 evidence chain assembly:
- Preview: theme top terms, validated dictionary, KWIC exemplars, coherence with foundational phenomenological prior (e.g., the Medium article for LCR).
- Options: text input. The researcher names. The agent records.

### 3.5 Pattern E — "Phase advancement" checkpoint

At every phase boundary, before advancing:
- Preview: a one-page summary of what the phase produced, the decision rule outcome, and the next phase's first step.
- Options: `Advance / Return to current phase to revise / Pause`.

### 3.6 Pattern F — "Tier of evidence" checkpoint

Per [method §C.4] tier rule, each row of the corpus is assigned to Tier 1 (segmentable) or Tier 2 (unitary) based on its voice-segmentation span breakdown. For ambiguous rows where the automated tier assignment is uncertain:
- Preview: the row's text with its span breakdown shown inline using labeled chunks (per the *show content, not meta-description* discipline). Include the automated tier assignment and the rationale.
- Options: `Tier 1 (supports discrete attribution analyses) / Tier 2 (whole-unit qualitative only) / Tier 1 with corrected spans (custom text) / Defer to researcher walk-through`.
- Used during Phase 4 hand-validation; the validation sample's `researcher_label` column captures the tier verdict per row plus any span corrections.

---

## 4. Audit trail conventions

Every checkpoint outcome is recorded to disk so the audit trail is reproducible and the agent can hand off to a future session.

### 4.1 Where decisions live

In each project repo, under `notebooks/audit_trail/`:

- `phase_0_seed_encounter.md` — the seed encounter document.
- `phase_1_corpus_provenance.md` — subreddits, window, seed terms with provenance, scrape methodology, scrape coverage report.
- `phase_2_descriptive_engagement.md` — frequency tables index, stop-word ablation outcomes, KWIC read notes.
- `phase_3_unit_decision.md` — chosen primary and secondary unit with rationale.
- `phase_4_segmentation_validation.md` — segmentation hand-validation precision-at-N.
- `phase_5_theme_decisions.md` — per-cluster theme-eligible / noise / unstable decisions.
- `phase_6_dictionary_revisions.md` — per-dictionary revision log, precision-at-N values, external cross-walks.
- `phase_7_constructs.md` — named constructs with evidence chains.
- `phase_8_inferential_results.md` — quantitative findings with sensitivity sweeps.
- `phase_9_synthesis_notes.md` — synthesis draft notes, reflexive memos.

### 4.2 Reflexive memo trail

Continuous reflexive memos are kept at `notebooks/reflexive_memos/` with timestamped filenames (`YYYY-MM-DD_HHMM_topic.md`). These are not commit messages; they are records of the analyst's thinking — hypotheses entertained and abandoned, surprises, reversals.

### 4.3 LLM-as-tool disclosure log

Where an LLM is used as a tool (segmentation, KWIC summarization, dictionary expansion suggestions), the prompt, model, date, and the LLM's role in the decision are recorded at `notebooks/llm_tool_log.md`. This is the transparency the methods library §D.4 requires.

---

## 5. Model selection conventions

| Work type | Default model |
|---|---|
| Reading prior sources for excavation | Sonnet 4.6 (Explore subagent) |
| Running scrapers / fetching data | Sonnet 4.6 (general-purpose) |
| Frequency/n-gram/collocation/KWIC table generation | Haiku 4.5 (general-purpose) |
| Topic modeling with multi-k sensitivity sweeps | Sonnet 4.6 (general-purpose) |
| Notebook construction (writing Phase-N notebooks against the procedural method) | Sonnet 4.6 (general-purpose) |
| Plan design for a complex phase | Opus 4.7 (Plan subagent) |
| Integration of agent outputs into library and audit trail | Opus 4.7 (orchestrator — me) |
| Reflexive memo drafting | Opus 4.7 (orchestrator — me) |

Model selection is documented in the LLM-as-tool log per §4.3.

---

## 6. Foreground vs. background

**Background:**
- Scrapes (long-running, rate-limited).
- Multi-k topic-model sensitivity sweeps.
- Excavation reads of large prior sources.
- Any work where the orchestrator has other work to do in parallel.

**Foreground:**
- Quick audits (file inventory, schema checks).
- Integration of returned agent outputs.
- AskUserQuestion construction.

Background agents return notifications when complete; I do not poll.

---

## 7. Anti-patterns

These moves are forbidden under this protocol.

- **Delegating Phase 6 precision-at-N hand-coding to an agent.** Agents can prepare the sample; they cannot do the coding.
- **Delegating Phase 7 construct naming to an agent.** Agents present evidence; they do not name constructs.
- **Skipping the AskUserQuestion at a phase boundary.** Phase advancement requires the researcher's explicit decision.
- **Running scrapers without a pre-specified provenance record.** Scrape methodology must be documented in `phase_1_corpus_provenance.md` before the scrape runs.
- **Dispatching parallel agents to the same files.** When agents would step on each other, run them sequentially or use worktree isolation.
- **Treating LLM-segmentation output as a validator.** It is a tool. Hand-validation per §C.4 is required.
- **Running ITS or PMI before the constructs being correlated have cleared §C.6 validation.** This is the failure that retired the prior pipelines.

---

## 8. Project-by-project state

This protocol is durable across all projects that instantiate the universal method. Project-specific state (which phase the project is in, which checkpoints are pending, which audit-trail files exist) lives in the project's `notebooks/audit_trail/` directory, not here.

---

## 9. Versioning

**Version 1.0** (2026-05-17) — Initial codification, alongside the procedural method and methods library. Establishes the human/orchestrator/agent role split, the procedural-method-to-orchestration map, the checkpoint patterns, the audit trail conventions, model selection defaults, and the foreground/background discipline.
