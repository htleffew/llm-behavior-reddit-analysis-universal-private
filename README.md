# Universal Methodology for Community-Reported LLM Behavior Research

A topic-agnostic procedural method and methods library for the **phenomenological investigation of community-reported behaviors of deployed large language models**, using observational social-discourse data, by a single-operator independent researcher.

**Heather Leffew, PhD** &middot; [HAIIQU](https://haiiqu.com/)

---

## What this repository contains

Three load-bearing methodological artifacts, plus a quarantined archive of the prior attempts they retire.

### 1. `community_reported_llm_behavior_method.md` — the procedural method

The spine. A topic-agnostic phase-by-phase procedure with explicit checkpoints, decision rules, and failure modes. It generalizes the structure of the author's doctoral dissertation methodology (corpus identification → linguistic analysis → clustering → cluster verification → regression on cluster membership) to a domain — community-reported LLM behavior — where prior theoretical literature is absent or thin and constructs must emerge from the corpus rather than be pre-specified.

Designed for: a single researcher, no academic affiliation, no funding, no API budget, no second human coder. The constraints shape the method; the method is not a watered-down version of an academic-lab protocol.

### 2. `methods_library.md` — the methods library

The toolkit. A reference catalog of analytical techniques, organized so that when descriptive engagement reveals a structural feature of the phenomenon — temporal, lexical, semantic, network, affective, demographic, behavioral-sequence — the relevant techniques, decision logic, single-operator feasibility notes, anti-patterns, and worked-example pointers are retrievable as a self-contained section.

Designed for agentic discovery. Each technique entry uses a consistent schema (Use when / Do not use when / What it produces / Single-operator feasibility / Choose this over alternatives when / Anti-patterns / Provenance / Notes for agentic execution). Entries containing `TODO[excavate: <source>]` mark places where future agent sessions need to read named prior projects and populate the slots.

### 3. `agentic_orchestration_protocol.md` — the orchestration protocol

The production system. Specifies how the procedural method gets executed under single-operator constraints with multi-agent assistance and human-in-the-loop checkpoints. Names the role split (what the human does, what the orchestrator does, what dispatched agents do), the model selection defaults per work type (Haiku 4.5 for clerical, Sonnet 4.6 for excavation and analysis, Opus 4.7 for integration), the checkpoint patterns for AskUserQuestion at each phase boundary, and the audit-trail conventions.

The procedural method is the spine. The methods library is the toolkit. The orchestration protocol is the production system. The three together are the load-bearing architecture.

### 4. `archive/contaminated_frame/` — quarantined prior attempts

The earlier `universal_pipeline_master_plan.md` and `resources_and_approaches.md` are preserved here for provenance. They labelled themselves "abductive" but pre-named target constructs and provided no checkpoints, no stop-word discipline, and no decision rules. They are the disease the active artifacts retire. Their `README.md` explains why they should not be read as methodological guidance.

---

## Who uses this repository

Two audiences:

- **Project repositories** that instantiate this method (currently `claude-lcr-analysis` and `claude-sleep-analysis`). Each has an `AGENTS.md` that points back here and a thin set of project-specific instantiation notes (seed encounter, corpus, candidate seed terms, foundational phenomenological prior, hard constraints).
- **Agents (human and AI)** acting on those project repositories. The recommended read order is documented in each project's `AGENTS.md`.

---

## Reading order

```
1. This README
2. community_reported_llm_behavior_method.md
3. methods_library.md
4. (then, for project work) The project's AGENTS.md and README
```

---

## Repository structure

```
llm-behavior-reddit-analysis-universal/
├── README.md                                    this file
├── community_reported_llm_behavior_method.md    procedural method (spine)
├── methods_library.md                           techniques catalog (toolkit)
└── archive/
    └── contaminated_frame/                      retired prior attempts
        ├── README.md
        ├── universal_pipeline_master_plan.md
        ├── resources_and_approaches.md
        ├── ideation_conversation_export.md
        ├── conversation_transcript.txt
        ├── user_messages_only.txt
        └── src/
            └── universal_anomaly_engine.py
```

---

## Methodological lineage

The active method generalizes from:

- **Leffew (Doctoral dissertation),** *Instrumental and Affective Mass Murder*. The procedural template: corpus → linguistic analysis → clustering → cluster verification → regression on cluster membership. Defensible there because instrumental vs. affective offender typology has robust theoretical literature pre-specifying cluster structure.
- **Leffew (2025),** *Gaslighting in the Name of AI Safety* (Medium). The phenomenological posture: self as instrument, refusal of premature closure, triangulation across kinds of evidence, concept formation as deliverable.
- **The tradition of reflexive single-analyst qualitative inquiry.** Smith's Interpretative Phenomenological Analysis, Charmaz's constructivist grounded theory, Braun & Clarke's reflexive thematic analysis. Rigor grounded in positionality, reflexivity, audit trail, and transparent stance — not in inter-rater reliability.

The active method's contribution is the bridge: a procedurally specified pipeline that preserves the phenomenological posture while supplying the checkpoint discipline, sensitivity ablations, external lexical cross-walks, and hand-coded precision-at-N rigor mechanisms that substitute for academic-lab resources the design does not have.

---

## What this repository does *not* contain

- Source code. The procedural method and methods library reference Python libraries (`nltk`, `gensim`, `sklearn`, `bertopic`, `networkx`, `statsmodels`, etc.) and external repositories (forks of `pleonasty`, `MEHv2`, `Contextualizer-keyword-incontext`, `ContentCoder-Py-LIWCish`, `RIOTLite-dictbased`, `archetypes-boyd`, `mental-health-datasets`, `depression-datasets-nlp`) but does not vendor them. Project repositories instantiate the method using the libraries they need.
- Data. The corpora live in the project repositories that scraped them.
- Paper drafts. The methodology supports peer-reviewable writeups but is not itself a paper.

---

## License

- Methodology documents: CC BY 4.0. Cite as the work is used.
- Archived prior attempts: same license, preserved for provenance, not for guidance.

---

## Contact

[HAIIQU](https://haiiqu.com/) &middot; heatherleffew@forevueinsights.com
