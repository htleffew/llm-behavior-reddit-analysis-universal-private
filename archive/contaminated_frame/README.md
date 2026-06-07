# Archived: Contaminated Frame

**Status:** Preserved for provenance only. Do NOT treat any artifact in this directory as methodological guidance.

## What this is

The files here were the prior universal "abductive pipeline" attempt. They share the same disease as the LCR and sleep project pipelines they were meant to generalize:

- They label the method **"abductive"** but pre-name the target constructs (*Pathologizing, Care Taking, Hard Directive, Soft Directive*) in advance. The clusters are then expected to be "mapped into" those names. That is construct imposition with abductive labeling, not actual abduction.
- They claim that K-means with a priori `k` proves "the theoretical constructs naturally exist in the dataset" — but this is the dissertation move (defensible when there is robust theoretical literature pre-specifying `k`, as in instrumental vs. affective offender typologies) being misapplied to a domain where no such theoretical literature exists.
- They conflate two distinct things under the phrase "solves the single-coder methodological problem." External pre-validated dictionaries (e.g., from `mental-health-datasets`, `depression-datasets-nlp`, LIWC clinical categories) *do* provide **external lexical validation**: the terms themselves have been adjudicated by people outside this study, so the researcher is not the sole authority on whether a given clinical term belongs in a clinical-pathologizing lexicon. That is real and useful. What they do *not* do is provide **inter-rater reliability for coding decisions**: a second human coder applying the dictionary to ambiguous cases. The prior plan promised the second by pointing at the first. The active method handles these as separate concerns. See Phase 6 of `../../community_reported_llm_behavior_method.md`.
- They specify no checkpoints between phases, no decision rules for advancing, no failure modes for any phase, and no stop-word discipline. The stop-word-cluster failure that occurred in the LCR pipeline was structurally invisible because nothing in the plan required looking.
- They reach for a long tail of external repos (`pleonasty`, `MEHv2`, `Contextualizer`, `ContentCoder`, `RIOTLite`, `archetypes-boyd`, `mental-health-datasets`) as if integration of these is the methodology. Tools are not method.

## What replaces it

`../../community_reported_llm_behavior_method.md` is the active procedural method. It is the load-bearing artifact for this repository and for any project that instantiates it.

## What to do with these files

Read them only to understand the methodological history. Do not import their phase structure, their construct names, their tool list, or their phrasing into any active work.
