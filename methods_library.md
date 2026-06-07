# Methods Library for Community-Reported LLM Behavior Research

**Purpose.** A reference catalog of analytical techniques applicable within the procedural method specified in `community_reported_llm_behavior_method.md`. Organized so that when Phase 2 (Descriptive Engagement) reveals a structural feature of the phenomenon — temporal, lexical, semantic, network, affective, behavioral-sequence, demographic — the relevant techniques, decision logic, single-operator feasibility notes, anti-patterns, and worked-example pointers are retrievable as a self-contained section.

**How an agent should use this document.**
- The procedural method (`community_reported_llm_behavior_method.md`) is the spine. It tells you *when* to consult this library. The library tells you *what to consider* and *how to choose*.
- Each entry is self-contained: read the section, you should be able to act. If you need more depth, follow the provenance pointers.
- Entries follow a consistent schema (see §0.3) so that pattern-matching across techniques is fast.
- Where an entry contains `TODO[excavate: <source>]`, a future agent session should read the named prior project or repository and populate the entry with the concrete patterns found there. The schema gives the agent the slot structure to fill.
- Cross-references to the procedural method use the form `[method §C.5]` (referring to Phase 5 in §C of the procedural method).

**Researcher constraints assumed throughout.**
- Single individual operator, no second coder, no team
- No academic affiliation, no funding, no API budget for large-scale programmatic calls
- No locally hosted model with tuning weights
- Time is the binding resource; compute is secondary

These constraints shape the *feasibility* field of every entry.

---

## 0. How to use this library

### 0.1 When to consult

| Procedural method phase | Library sections to consult |
|---|---|
| [method §C.0] Triggering Observation | none — this is human qualitative work |
| [method §C.1] Corpus Definition & Scrape | §10 (External lexical resources) for seed-term sanity check against established lexicons |
| [method §C.2] Descriptive Engagement | §1 (Lexical), §3 (Temporal) for initial frequency and time work; §9 (Voice segmentation) if early evidence suggests segmentation viability |
| [method §C.3] Unit-of-Analysis Determination | §1 + §4 (Network) for unit-level pattern comparison |
| [method §C.4] Voice Segmentation | §9 |
| [method §C.5] Inductive Theme Discovery | §2 (Semantic/thematic) primary; §1 supporting |
| [method §C.6] Lexicon Construction | §1 (dictionary construction patterns), §8 (Hand-coding), §10 (External validation) |
| [method §C.7] Construct Formation | none directly — this is synthesis work informed by all prior phases |
| [method §C.8] Inferential Analysis | §3 (Temporal regression), §11 (Feature attribution), and lightweight applications of §1's PMI |
| [method §C.9] Reflexive Synthesis | §8 for audit trail conventions |
| [method §C.10] Cross-Corpus Comparison | applies the full library across two completed studies |

### 0.2 How to choose between techniques within a section

Each technique entry contains a **Choose this over alternatives when** field. Agents reading this library should:
1. Identify the structural feature surfaced in Phase 2.
2. Open the corresponding section.
3. Read every entry's **Use when** and **Do not use when** before committing to one.
4. Consult the decision table at the top of each section for the most common branching logic.
5. Cross-reference any **Anti-patterns** before execution.

### 0.3 Schema for technique entries

Every technique entry uses this structure. Agents reading or writing entries should preserve the slot names verbatim so that retrieval and pattern-matching remain reliable.

```
### <Technique name>

**Use when.** <Trigger conditions from descriptive engagement, stated as observable features of the corpus>
**Do not use when.** <Conditions under which this technique misleads — must be specific>
**What it produces.** <Concrete output type: table, figure, fitted model, dictionary>
**Single-operator feasibility.** <Time cost, compute cost, dependency burden under stated constraints>
**Choose this over alternatives when.** <Decision logic vs nearby techniques in the same section>
**Anti-patterns.** <Specific known ways this technique is misused, with the failure observed>
**Provenance.** <Where in the researcher's prior work, the external repos, or the literature this technique appears>
**Notes for agentic execution.** <What an LLM agent needs to know to run this without producing lock-in artifacts: checkpoints, sensitivity ablations to run, outputs to validate before reporting>
```

### 0.4 Versioning

This library is revisable. When an agent session populates a `TODO[excavate: ...]` block or refines an existing entry, that change is appended to §13 (Revision log) with a one-line summary and date.

---

## 1. Lexical-structure techniques

**When this section applies.** Phase 2 descriptive engagement reveals that the phenomenon is anchored in specific words or phrases — that the community talks about it using a recognizable vocabulary, and that the analytical question concerns *what words appear, in what combinations, in what contexts*.

**Section decision table.**

| If the corpus shows… | Reach for… |
|---|---|
| Many candidate anchor words, unclear which matter | §1.1 Raw frequency, then §1.2 Collocation |
| Anchor words but unsure whether they appear together meaningfully | §1.2 Collocation, §1.3 Co-occurrence |
| Need to test whether disclosure-type A predicts model-output-type B | §1.4 Pointwise mutual information |
| Multi-word expressions matter (the phrase, not the words separately) | §1.5 N-gram and skipgram analysis |
| Need to construct a dictionary derived from the corpus | §1.6 Dictionary construction patterns |

### 1.1 Raw frequency analysis (unigram, bigram, trigram)

**Use when.** First descriptive pass on any new corpus. Always. Before any other lexical technique.

**Do not use when.** Never skipped, but never the *only* analysis — raw counts conflate signal and stop-words.

**What it produces.** Ranked frequency tables at unigram, bigram, trigram level. Comparison tables under different preprocessing (stop-word removal on/off, lemmatization on/off).

**Single-operator feasibility.** Trivial. Minutes of compute on tens of thousands of posts using `nltk.FreqDist` or `sklearn.feature_extraction.text.CountVectorizer`.

**Choose this over alternatives when.** It is not an alternative; it is the foundation. Every later lexical analysis presumes this work has been done and read.

**Anti-patterns.**
- Reporting top-K terms without inspecting them in context (KWIC, §1.7). Top-frequency terms can be misleading; their *function* in the corpus is what matters.
- Defaulting to stop-word-removed counts only. Run with and without. Stop-words sometimes *are* the signal (e.g., pronouns in dissertation work, modal verbs in directive analysis).
- Using a generic English stop-word list without adding domain-specific stop-words (e.g., *claude, gpt, prompt, llm, user, model*) discovered from the data.

**Provenance.** Standard practice across all of her prior projects. Specific patterns:
- Content-moderation telemetry notebook (`Pm_html/projects/Juggernaut/Juggernaut_Content_Moderation_Telemetry_txt.txt`): lines 260–350 demonstrate **custom domain-stopword extension** at runtime, adding platform-specific terms (e.g., *fyp, viral, greenscreen, 4u, nan*) to the standard NLTK English stopword list. Lines 1000–1100 demonstrate n-gram generation and per-policy frequency analysis. The pattern is: load standard stopwords → add domain-specific stops discovered from the data → re-run frequency analysis → report side-by-side.
- TikTok Elections: `Content Moderation Analysis and Reporting_txt.txt` lines ~680–720 demonstrate emoji/hashtag frequency extraction with stop-word handling. `Policy_Misinfo_Anthro_txt.txt` lines 528–560 demonstrate n-gram-by-policy extraction via `Counter()` and `most_common()` on a `cleaned_query` field. Lines 1200–1250 of `Policy_Misinfo_Anthro_txt.txt` show a `violation_detection_rate` function on search terms — note the distinct concept of *policy-conditional violation rate* (`rate(violation | term X) = count(rows with term X AND violation) / count(rows with term X)`) alongside raw frequency. These answer different questions: frequency = "how often does this term appear"; conditional rate = "how predictive is this term of the outcome."
- Dissertation: LIWC frequency-based variable construction (`Pm_html/projects/Instrumental_and_Affective_Mass_Murder/Dissertation.txt`).

**Stratification pattern (from TikTok Elections).** Where the corpus has a categorical structure (policy category hierarchy, subreddit, time window), compute frequency tables *separately at each level* of the structure. Save as `freq_unigram_{level}_{value}.csv`. Top-K terms within a stratum often differ substantially from top-K across the corpus; the difference is the signal.

**Notes for agentic execution.**
- Always run the analysis at all three n-gram orders (1, 2, 3). Patterns invisible at unigram emerge at bigram.
- Always produce a side-by-side comparison: with stop-words / without / with lemmatization / without. The comparison itself is the artifact, not any single column.
- Save the comparison as a markdown or CSV table to the project's `deliverables/` directory with a filename that encodes the preprocessing variants (e.g., `freq_unigram_stopword-off_lemma-on.csv`).

---

### 1.2 Collocation analysis (anchored windows)

**Use when.** Specific anchor terms have been identified (from §1.1 or from Phase 1 seed terms) and the question is *what surrounds these terms*.

**Do not use when.** No anchor terms identified. Run §1.1 first.

**What it produces.** For each anchor term, a ranked list of words appearing within a specified window (5, 10, 20 tokens typical). Often expressed as PMI-weighted or t-score-weighted collocates rather than raw frequencies, to surface specific rather than ubiquitous co-occurrents.

**Single-operator feasibility.** Minutes to tens of minutes per anchor term on tens of thousands of posts. `nltk.collocations` or custom window-counting.

**Choose this over alternatives when.** You care about *what is near* specific words, not which words appear together generally across the corpus (that is §1.3).

**Anti-patterns.**
- Reporting raw co-occurrence counts as collocates. Use a statistical association measure (PMI, log-likelihood, t-score, Dice). The choice affects which collocates surface; document the choice.
- Single window-size analysis. Always run at multiple windows (5, 10, 20) and compare. Collocates stable across windows are more trustworthy.
- Failing to filter stop-words from the *collocate side* of the table even when keeping them in the anchor side.

**Provenance.**
- TikTok Elections: TODO[excavate: Policy_Misinfo_Anthro and Content View Source Analysis notebooks for collocation patterns around policy violation terms]
- External: `Contextualizer-keyword-incontext` provides KWIC + collocation infrastructure; even if not used directly, its window logic is reusable.

**Notes for agentic execution.**
- Sensitivity sweep: report collocates at three window sizes. Note which collocates survive all three.
- Pair every collocation table with KWIC reads (§1.7) on at least the top 10 collocates per anchor. The numbers are only interpretable in context.

---

### 1.3 Co-occurrence analysis (unanchored, graph-style)

**Use when.** The question is which *terms* in the corpus tend to appear together across the corpus as a whole, without privileging any specific anchor. Useful for surfacing latent semantic clusters before topic modelling.

**Do not use when.** You have specific anchors to investigate (use §1.2). The corpus is small enough that simple frequency suffices (§1.1).

**What it produces.** A weighted term-term co-occurrence matrix or graph (often built with `networkx`). Visualizable as a network; analyzable via centrality, community detection (Louvain, Leiden).

**Single-operator feasibility.** Memory-bound. For 30k posts with vocabulary trimmed to top-N most-frequent content words, fits easily in memory. For larger corpora, use sparse representations or sample.

**Choose this over alternatives when.** You want to see structural relationships among many terms simultaneously, not pairwise. When a graph view will be interpreted (so that visualizing matters).

**Anti-patterns.**
- Building the matrix on the full vocabulary including rare terms — the matrix is dominated by noise. Trim to top-N (e.g., 500–2000) content-bearing terms.
- Visualizing without community detection. A 1000-node graph without grouping is unreadable.
- Treating community-detection output as theme discovery. Communities here are an artifact of the term graph; they may or may not correspond to themes a human would name. §2 is for theme discovery.

**Provenance.**
- Juggernaut: TODO[excavate: hashtag co-occurrence network construction patterns]
- TikTok Elections: TODO[excavate: hashtag/term co-occurrence patterns]
- External: NetworkX is the obvious dependency. Boyd-lab `archetypes-boyd` uses embedding-based co-occurrence — note the contamination history (§archive) but the underlying NetworkX patterns are reusable.

**Notes for agentic execution.**
- Always trim vocabulary before building the matrix; document the trim.
- Run community detection at multiple resolution parameters (`networkx.algorithms.community.louvain_communities` with `resolution=0.5, 1.0, 2.0`). Stable communities across resolutions are more trustworthy.
- Save the graph in `.gexf` format so it can be opened in Gephi for visual inspection.

---

### 1.4 Pointwise mutual information (PMI)

**Use when.** [method §C.8 Inferential Analysis] You want to quantify the strength of association between a user-side feature (e.g., a disclosure category) and a model-side feature (e.g., a payload category) — *and both sides have already cleared Phase 6 validation*.

**Do not use when.** Either side is an unvalidated lexicon. PMI between two unvalidated instruments is a noise correlation, not a finding.

**What it produces.** PMI value (positive: more co-occurrence than chance; zero: chance; negative: less than chance). Normalized variants (NPMI) constrain to [-1, 1] and are easier to compare across pairs.

**Single-operator feasibility.** Trivial compute once Phase 6 has produced validated dictionaries.

**Choose this over alternatives when.** You need a single interpretable statistic for a single pair. For multi-feature regression, use §11's logistic regression instead.

**Anti-patterns.**
- Computing PMI before Phase 6 validation. This is the failure that retired the prior LCR pipeline — PMI was computed across two unvalidated lexicons, one of which was imported wholesale from a sibling project.
- Reporting PMI without base rates. PMI alone can mislead at low base rates. Always report base rate and conditional probability alongside.
- Treating PMI as causal. It is a co-occurrence statistic.

**Provenance.** Standard NLP statistic; the *misuse* in the contaminated frame is documented at `archive/contaminated_frame/` in both project repos.

**Notes for agentic execution.**
- Report PMI together with: raw co-occurrence count, base rate of each side, conditional probability, total corpus size. The PMI alone is not interpretable.
- Use NPMI when comparing across many pairs.

---

### 1.5 N-gram and skipgram analysis (phrase-level)

**Use when.** Phase 2 evidence suggests that the phenomenon is carried by multi-word expressions rather than single words. Example signals: high-frequency phrases that lose meaning when decomposed (*professional help*, *take a step back*, *go to sleep*, *get some rest*).

**Do not use when.** Unigram analysis already exhibits clear signal; phrases would add noise without surfacing new pattern.

**What it produces.** Ranked tables of n-grams (typically 2 ≤ n ≤ 6) and skipgrams (allowing intervening tokens). Skipgrams capture phrasal patterns with variation (*take a [adj] step back*).

**Single-operator feasibility.** Trivial for n ≤ 4. Skipgram explosion at higher n; constrain.

**Choose this over alternatives when.** Unigrams under-specify. Specifically: when the same unigram appears in multiple contexts with different meanings, but the surrounding phrase disambiguates.

**Anti-patterns.**
- Reporting only contiguous n-grams. Skipgrams catch patterns n-grams miss (*"you should [adv] talk to a [profession]"*).
- Top-N filtering before stop-word handling. A phrase top-list dominated by *of the / in the / and the* is uninformative; trim domain stop-phrases.

**Provenance.**
- TikTok Elections: TODO[excavate: n-gram patterns by policy violation type]
- Sleep analysis archive: `archive/contaminated_frame/.../deliverables/skipgrams_top.csv` for an example output (read for format, not for methodological direction)

**Notes for agentic execution.**
- Run n-grams at n=2,3,4 and skipgrams at n=2 with skip-distance 1 and 2.
- Always pair phrase-level analysis with §1.7 KWIC reads on top phrases.

---

### 1.6 Dictionary construction patterns

**Use when.** [method §C.6 Lexicon Construction & Refinement] A surviving theme from §C.5 needs operationalization as a lexicon for downstream §C.8 inferential analysis.

**Do not use when.** The theme has not yet cleared §C.5 stability checks. The corpus has not been read in §C.2.

**What it produces.** A dictionary: a labelled set of terms and phrases that together operationalize a single theme, with documented precision-at-N (default acceptance ≥ 0.85) and an audit trail of revisions.

**Single-operator feasibility.** Hand-coding is the time-binding step. Budget ~30–60 minutes per dictionary for the initial 50-item precision sample. Revision iterations add more.

**Choose this over alternatives when.** You need a transparent, auditable, defensible lexical operationalization rather than a black-box classifier.

**Anti-patterns** (these are the failure modes documented in `archive/contaminated_frame/`):
- Constructing the dictionary in Phase 1 from seed encounters and freezing it. Dictionaries must be derived from the corpus in Phase 6, after themes have emerged.
- Skipping the external lexical cross-walk (§10). Loses external validation.
- Skipping precision-at-N coding. Without it there is no quantitative defense against the single-coder critique on coding decisions.
- Treating precision below 0.85 as a "sensitivity-specificity tradeoff" to be footnoted. Below 0.85 returns to refinement or to theme reconsideration.
- Single-meaning assumption: assuming every occurrence of a dictionary term means the same thing. KWIC will reveal where it doesn't; split the dictionary.

**Provenance.**
- **Critical worked example for corpus-derived refinement: the original phenomenological notebook** (`Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt`, lines 167–198). This is the pattern that *predates the contaminated frame*. Seed dictionaries (`harmful_lexicon`, `user_experience_lexicon`) are pre-specified at lines 167–169, but immediately refined per segment at lines 172–198 via TF-IDF + sentence-transformer cosine similarity to base-lexicon embeddings. The top-N most-similar new terms are added per segment. A bias-check narrative documents potential stigma-bias in the resulting dictionary at line 196. This is corpus-prior dictionary expansion, not construct-imposed dictionary freezing — the discipline the active method preserves.
- The dissertation's LIWC-summary-variable construction is the cleanest example of *validated pre-existing dictionary use*, but it relied on LIWC's pre-validation. The corpus-derived analog is what Phase 6 specifies and what the original phenomenological notebook exemplifies.
- TikTok Elections `Policy_Misinfo_Anthro_txt.txt` lines 1300–1400 show LLM-assisted thematic interpretation feeding into iterated dictionary boundaries.
- External validators: see §10.

**Notes for agentic execution.**
- Always document the dictionary in a structured file (`disclosure_lexicons.json` or similar) with the schema: `{theme_name: {included_terms: [...], excluded_terms_considered: [...], precision_at_n: float, sample_size: int, revision_log: [...] }}`.
- Save the precision-coding sample (the hand-coded 50 items with TP/FP labels) as a CSV in `deliverables/` so the audit trail is reproducible.

---

### 1.7 KWIC (Keyword-in-Context) reading

**Use when.** Always, in support of every other lexical technique. Whenever a term's prominence is established by counts, *read its actual occurrences in context* before drawing inference.

**Do not use when.** Never skipped.

**What it produces.** For a given keyword, a table of its contexts: the keyword centered, with N tokens of left and right context. Read by hand.

**Single-operator feasibility.** Trivial to generate; the time cost is in reading. Budget 10–30 minutes per anchor term for embodied familiarity.

**Choose this over alternatives when.** It is not an alternative — it is the validation layer on top of every other lexical technique.

**Anti-patterns.**
- Generating KWIC tables but not reading them.
- Reading only the first few entries instead of a random sample.
- Single window-size. Read at multiple widths.

**Provenance.**
- External: `Contextualizer-keyword-incontext` (Boyd-lab) provides KWIC infrastructure; adopt the windowing logic regardless of whether the full tool is used.

**Notes for agentic execution.**
- For each anchor term, sample at least 20 random hits and read them.
- When an LLM agent assists with KWIC review, it should produce a *summary of contextual patterns observed*, not a *judgment about the term's appropriateness*. The judgment belongs to the human researcher informed by the summary.

---

### 1.8 Sense-discovery via embedding-cluster on KWIC contexts

**Use when.** A seed term is polysemous (carries multiple distinct meanings in the corpus) and naive frequency or collocation analysis is misleading because surface counts conflate senses. Typical triggers: descriptive engagement reveals that the seed term's high-frequency contexts include programming-construct, idiomatic, medical, and behavioral uses that the researcher's question intends to separate.

**Do not use when.** The seed term is clinically marked vocabulary with narrow polysemy (e.g., *psychosis, dissociation, mania* in a Reddit corpus). The seed term's hit count is too low to support clustering (floor: roughly 50 occurrences; below that, KWIC reading by hand is more honest than clustering).

**What it produces.** Per polysemous seed term:
- A set of context clusters with sampled exemplars from each.
- Cluster stability evidence across embedding models, HDBSCAN parameter settings, and KWIC window widths.
- Cross-validation against syntactic-feature stratification (POS, imperative-mood, code-block presence).
- A researcher-labeled sense taxonomy *after* exemplar review.

**Single-operator feasibility.** Modest compute. Per seed term: extract all KWIC windows; embed with sentence-transformer (e.g., `all-MiniLM-L6-v2`, CPU-feasible); HDBSCAN at min-cluster-size 5, 10, 20; sample top-N centroid-nearest exemplars. Per second embedding model for stability (e.g., `all-mpnet-base-v2` or `paraphrase-MiniLM-L3-v2`). Wall-clock per seed: 10–30 minutes on tens of thousands of posts. Researcher exemplar review: 20–60 minutes per seed.

**Choose this over alternatives when.** Speed and efficiency are not the binding constraint; rigorous sense discovery is the binding constraint. The alternatives considered and the reasoning:
- Pre-named sense splits in preprocessing impose a taxonomy before evidence supports it; less rigorous than emergent.
- POS-tag stratification alone uses syntactic structure as a sense proxy; useful as cross-validation (see §1.8 step 6 below) but does not generalize to senses that share POS.
- Subreddit stratification handles one source of confusion (e.g., r/ClaudeCode for programming contexts) but does not address medical or idiomatic confusion within other subreddits.
- Deferral to Phase 6 precision-at-N is methodologically conformant but passive; it waits for the dictionary to fail before refining. Sense-discovery actively maps the senses upstream.

**Anti-patterns.**
- Naming senses before reading exemplars. The clusters surface structure; the researcher reads exemplars and labels.
- Treating cluster count as a sense count. HDBSCAN noise points and very small clusters may be artifacts, not senses. Require at least 5 exemplar contexts in a cluster before labeling it a sense.
- Single-embedding-model reliance. Run two embedding models; report which senses are stable across both.
- Skipping the syntactic-feature cross-validation. If embedding clusters do not correspond to any structural feature, that is informative (suggests semantic-context-only sense distinctions) but should be documented, not silently accepted.

**Procedure.**
1. Identify polysemous seed terms (from §C.2 descriptive engagement, particularly KWIC reads).
2. For each polysemous seed, extract every occurrence with ±20 token context (raw, non-lemmatized, stop-words preserved).
3. Embed each context with a sentence-transformer. Default: `all-MiniLM-L6-v2`.
4. Cluster the embeddings with HDBSCAN at `min_cluster_size = 5, 10, 20`. Save cluster assignments per parameter.
5. Repeat steps 3–4 with a second embedding model (e.g., `all-mpnet-base-v2`). Document which clusters appear stably under both models.
6. For each stable cluster, compute: POS distribution of the seed term's occurrences in those contexts, fraction of contexts that contain code blocks (triple-backtick or indented-code patterns), fraction with imperative-mood grammar at the seed term's position (e.g., "you should X", "go X", "try X"), subreddit distribution.
7. Sample 5–10 exemplar contexts per stable cluster, drawn nearest to the cluster centroid.
8. Researcher reviews exemplars and assigns a sense label per cluster. Document via a Pattern A (theme vs. noise) checkpoint applied at sense level.
9. Save the labeled sense map as a JSON/CSV resource for Phase 6 dictionary construction.

**Provenance.**
- New addition; introduced 2026-05-17 as the rigor-maximizing response to polysemy observations on sleep-nudge seed terms (*sleep*, *rest*, *break*, *tired*) during the first Phase 2 descriptive engagement pass on `claude-sleep-analysis`. Methods library entry added before the technique was first dispatched.
- Generalizes the §2.3 sentence-transformer + HDBSCAN pattern to the sub-corpus level (one polysemous seed's contexts as a sub-corpus) for sense rather than theme discovery.

**Falsification finding (2026-05-17, sleep-nudge phenomenon).** The technique was tested against the four primary sleep behavioral seeds (*sleep, rest, break, tired*) at three corpus scales and retrieval modes: 4,114 posts wholesale, 7,021 posts wholesale (expanded canonical), and 773 rows targeted (Pass 1b canonical). **Cross-model adjusted Rand index stayed in the 0.2 range across all three runs; no seed achieved mean ARI ≥ 0.40; no code-block-dominant cluster ever emerged.** The technique consistently failed to produce stable sense separation for this phenomenon. The bottleneck is not corpus size or retrieval mode; it is the structural fit between the technique and the phenomenon's linguistic surface. The phenomenon's directive vocabulary (*sleep, rest, break, tired*) is polysemous in a way that does not cleanly partition into semantically coherent clusters at the KWIC-context level. The technique remains in the library because it may work for other phenomena whose polysemy structure is more separable; for sleep-nudge specifically, polysemy is handled downstream via Phase 5 multi-k topic stability and Phase 6 precision-at-N hand-coding, which are the procedural method's already-specified mechanisms for this class of problem.

**Mixed-result finding (2026-05-17, LCR pathologizing phenomenon).** The technique was tested against five LCR polysemous seeds (`professional, mental, episode, concerned, worried`) on the 26,158-row Pass 1b canonical corpus. **Three seeds (`episode, concerned, worried`) failed in the same shape as sleep's primary seeds** — all-noise at mcs=10, no stable clusters, falsified. **Two seeds (`professional, mental`) returned technically valid clustering** (`professional` ARI 0.420; `mental` ARI 0.617), but inspection of cluster exemplars revealed the clusters were driven by document-duplication attractors rather than genuine sense separation. See the document-duplication-attractor failure mode below.

**Failure mode: document-duplication attractor (2026-05-17).** When a corpus contains many near-verbatim reproductions of the same text fragment (e.g., the LCR system-prompt text quoted in dozens of Reddit posts; a frequently-reposted user testimony; a viral quote), the embedding clustering will preferentially group those duplicate contexts together because their embeddings are nearly identical. The resulting cluster has:
- A very high within-cluster cosine similarity (because the contexts are near-identical).
- A high cross-model adjusted Rand index against the same configuration (because every model produces the same near-duplicate grouping).
- Top exemplars that look semantically coherent (because they ARE the same fragment).

The metric outputs look like a successful sense discovery. They are not. The cluster is a duplicate-text artifact, not a sense.

**How to detect document-duplication attractors:**
- Compute pairwise edit-distance or cosine-similarity within each candidate cluster on the raw KWIC text. Clusters where >50% of pairs exceed similarity 0.95 are duplicate-driven.
- Inspect exemplar contexts manually. If the top 10 exemplars are recognizably the same prose with minor punctuation differences, the cluster is duplicate-driven.
- Compute the number of distinct `post_id` values per cluster. Clusters where one or a few post_ids account for >50% of cluster membership are duplicate-driven (the same post or a few posts contributed all the contexts).

**What to do when a duplicate-attractor cluster is detected:**
- Document the cluster as a duplicate artifact, not a sense.
- Deduplicate the corpus by exact-or-near-text match BEFORE re-running §1.8.
- Alternatively, exclude the duplicate-fragment-bearing rows from the sense-discovery input and run on the remainder.
- For LCR specifically: the documented C4 cluster from the prior 22k wholesale pass on `professional` was a system-prompt duplication artifact (see `claude-lcr-analysis/notebooks/audit_trail/phase_2_5_anchor_strategy_professional.md`); the Pass 1b §1.8 verdicts of WORKS on `professional` and `mental` are downgraded to *duplicate-driven, not genuine polysemy separation*, and the operational anchor strategy (bigram primary + clinical-proximity secondary) remains the canonical Phase 6 input.

**Notes for agentic execution.**
- Save outputs under `deliverables/phase_2_5_sense_discovery/{seed_term}/`.
- Per seed term, produce: `embedding_clusters_{model}_mcs{n}.csv` (contexts × cluster_id), `cluster_exemplars_{model}_mcs{n}.csv` (top-N nearest-centroid contexts per cluster), `stability_crosstable.csv` (cluster overlap across models and parameters), `syntactic_features_per_cluster.csv` (POS distribution, code-block fraction, imperative-mood fraction per cluster).
- Per seed term, write `sense_discovery_notes_{seed}.md` documenting what was found at a structural level (without imposing sense labels — the researcher labels at the Pattern A checkpoint).
- Do NOT assign sense labels in the agent output. Sense labeling is a researcher decision at the next checkpoint.

---

### 1.9 Iterative seed-term refinement (bootstrapping retrieval)

**Use when.** A first-pass corpus retrieval yields a small but coherent set of positive cases of the phenomenon, and the phenomenon's full linguistic surface is broader than the initial seed-term list captures. Particularly applicable when the phenomenon is rare in untargeted retrieval but specific enough that positive cases carry recognizable linguistic signatures.

**Do not use when.** Initial retrieval has produced fewer than ~10 confirmed-positive cases (too few to mine signatures reliably). The phenomenon's linguistic surface is open-ended (any English speaker can describe it many ways with no characteristic vocabulary). The researcher has not yet read enough positive contexts to recognize what constitutes a positive case.

**What it produces.** A Round N+1 augmented seed-term list with explicit provenance per term (which positive cases produced it). A merged corpus tagged by retrieval round and source term. A saturation criterion to know when to stop.

**Single-operator feasibility.** Modest. Per round: 30–60 minutes to mine signatures from positives + 30 minutes to design and execute the augmented retrieval. Typically 2–4 rounds to saturation depending on phenomenon coherence.

**Choose this over alternatives when.** Initial retrieval is producing too few positives for downstream analysis (see [method §C.5] stability failure or [method §C.6] precision-at-N density issues), and the alternative is to abandon the phenomenon as unstudiable rather than to refine retrieval. Aligned with the procedural method's emergence-from-corpus principle: terms emerge from positives, not from theory.

**Anti-patterns** (rigorous use of this technique requires explicit safeguards against each):
- **Refining toward a hypothesis.** Each new term must be sourced from an observed positive case, not from theoretical expectation of what the phenomenon should look like. Document provenance per term.
- **Treating the iteratively assembled corpus as a population sample.** It is not. It is a corpus assembled by iterated relevance feedback, and the methods section must say so plainly.
- **Construct formation on the assembled corpus alone.** The retrieval procedure does not exempt the corpus from Phase 5 stability checks, Phase 6 precision-at-N validation, or Phase 7 evidence-chain construct defense. The technique improves retrieval; it does not change downstream rigor requirements.
- **Saturation by fiat.** Stop when the data tell you the round added little, not when you have what you wanted.

**Procedure.**

1. **Confirm Round 1 positives.** From the existing corpus, identify confirmed-positive cases. Sources: Phase 2.5 sense-discovery exemplars, KWIC observation notes flagged as phenomenon-relevant, manually-coded cases the researcher has already verified. Minimum: 10 positives to begin.

2. **Mine signatures from positives.** Extract:
   - **Phrasal patterns.** Multi-word expressions appearing in positives at higher rate than in negatives or in the wholesale baseline. Examples to look for: imperatives addressed to the user, model self-attributions, characteristic time-reference patterns.
   - **Grammatical patterns.** Syntactic structures characteristic of the phenomenon (e.g., second-person modal verbs, third-person model self-state attribution).
   - **Co-occurring vocabulary.** Terms that appear in the neighborhood of seed terms in positives but not in the wholesale comparator.
   - **User-reaction language.** Vocabulary characteristic of how users describe the experience of receiving the directive.

3. **Generate Round N+1 augmented seed-term list.** Each new term tagged with: provenance (which positive case it came from), pattern type (phrasal, grammatical, co-occurring, user-reaction), confidence (high if directly extracted from positive utterances, medium if derived from positive contexts, low if speculative).

4. **Execute Round N+1 retrieval.** Apply augmented list against:
   - The existing corpus (re-tag with new term matches; some posts will newly match because of newly added terms).
   - Fresh retrieval from the source archive for the augmented terms specifically.
   Each retrieved post is tagged with the round number and the specific term(s) that surfaced it.

5. **Inspect the new positives.** Quick KWIC reads on a random sample of the newly retrieved posts. Are they actually positive cases of the phenomenon? Or did the augmented terms drag in irrelevant content?

6. **Iterate or stop.** Stop when:
   - A new round adds fewer than ~10% additional positive cases to the corpus, OR
   - Hand-validation of the new positives reveals that the augmented terms are pulling in mostly noise.

7. **Document the full provenance trail** in `notebooks/audit_trail/iterative_retrieval_rounds.md`: per round, which terms were added, why, how many new posts they surfaced, what fraction of new posts were positive on hand-validation. The methods section of the eventual paper draws from this document directly.

**Provenance.**
- The original phenomenological notebook (`Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt`) lines 167–198 implement a TF-IDF + sentence-transformer-cosine-similarity variant of this pattern as part of its dictionary refinement procedure. **This is the pre-contamination version** of the technique and is the methodological lineage the active method preserves.
- Information retrieval literature: relevance feedback (Rocchio, 1971), query expansion, pseudo-relevance feedback. NLP bootstrapping literature: Yarowsky (1995) decision-list bootstrapping; Riloff & Jones (1999) mutual bootstrapping for extraction patterns.
- New library entry, introduced 2026-05-17 in response to wholesale-corpus insufficiency on the sleep-nudge phenomenon.

**Notes for agentic execution.**
- Save the augmented seed-term list as `notebooks/audit_trail/seed_terms_round_{N}.csv` with columns `[term, provenance_case_id, pattern_type, confidence, retrieval_action]`. The `retrieval_action` column specifies how the term will be used: exact match, fuzzy match, regex pattern, contextual co-occurrence.
- Save the per-round provenance memo at `notebooks/audit_trail/iterative_retrieval_round_{N}_memo.md` with: what was mined from the prior round, design choices for the new term set, hand-validation results on a sample of new positives.
- Do not assign sense labels or construct names. The augmented retrieval brings in candidate positives; researcher and Phase 5–7 work assign meaning.
- If a round produces a new term whose hand-validation precision is below 0.5 on a 20-item sample, drop the term and document the failure. Do not let weak terms accumulate.

---

## 2. Semantic and thematic-structure techniques

**When this section applies.** Phase 2 has stabilized and the question is whether *themes* (coherent clusters of meaning) exist in the corpus that go beyond individual lexical anchors.

**Section decision table.**

| If you want to… | Reach for… |
|---|---|
| Discover themes from scratch, fully unsupervised | §2.1 BERTopic, §2.2 LDA |
| Group documents by similarity in embedding space | §2.3 Sentence-transformer embeddings + HDBSCAN/K-Means |
| Test whether a theoretically pre-specified number of clusters exists | §2.4 Deductive K-means with a priori k (only when theoretical literature supports the k) |
| Verify that discovered clusters are not stop-word artifacts | §2.5 Cluster verification protocol |

### 2.1 BERTopic

**Use when.** Inductive theme discovery on documents short enough that contextualized embeddings make sense (Reddit posts, comments).

**Do not use when.** Corpus is below ~1000 documents (UMAP/HDBSCAN unstable at small N). Stop-word handling has not been configured (see Anti-patterns).

**What it produces.** A set of clusters with c-TF-IDF top terms per cluster and document-cluster assignments.

**Single-operator feasibility.** Memory and compute scale with embedding model size. `all-MiniLM-L6-v2` runs locally on CPU for tens of thousands of docs. Larger embedding models require GPU or trimming.

**Choose this over alternatives when.** Embeddings-driven clustering is appropriate (modern contextualized similarity matters more than bag-of-words). Easier off-the-shelf than rolling your own embedding+clustering stack.

**Anti-patterns** (these are the documented failure modes from the contaminated frame):
- Initializing BERTopic without a custom `CountVectorizer` that strips stop-words at the representation stage. The clustering step needs full sentences; the representation step does not. Without explicit stop-word handling in the vectorizer, top terms degenerate to *the / to / and / you*. This is the failure that produced the LCR project's apparent four-cluster validation that was actually two stop-word smears plus one signal cluster.
- Reporting cluster top-terms without inspecting them. If top-10 terms of a cluster are stop-words, it is not a theme.
- Running at a single k. Always sweep k.
- Treating BERTopic output as construct validation. It is descriptive evidence at best.

**Provenance.**
- Original instance of the anti-pattern: the original phenomenological notebook (`Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt`) lines 1214–1234 — BERTopic initialized with `nr_topics="auto"` but *without* a custom `vectorizer_model`. This is the same anti-pattern that subsequently produced the "two stop-word smears plus one signal cluster" finding documented in the contaminated LCR pipeline. The lesson: this anti-pattern is the *default behavior of the library*. Explicit configuration is required to avoid it.
- Misuse documented at `archive/contaminated_frame/.../notebooks/` in LCR repo.
- Correct configuration pattern: pass `CountVectorizer(stop_words="english", ngram_range=(1,2), min_df=5)` (or similar) as the `vectorizer_model` argument.

**Notes for agentic execution.**
- Mandatory configuration:
  ```python
  from bertopic import BERTopic
  from sklearn.feature_extraction.text import CountVectorizer
  vectorizer = CountVectorizer(stop_words="english", ngram_range=(1,2), min_df=5)
  topic_model = BERTopic(vectorizer_model=vectorizer, ...)
  ```
- After fitting, *immediately* inspect top terms of every cluster. If any cluster is stop-word-dominated, re-run with a more aggressive `min_df` and an extended stop-word list (add domain stop-words like *claude, gpt, model, ai, llm, user*).
- Run at k=3,5,8,12,20 (via `nr_topics` parameter or post-hoc `reduce_topics`). Build a stability matrix per [method §C.5].

---

### 2.2 LDA (Latent Dirichlet Allocation)

**Use when.** Themes are expected to mix continuously (a document can be 60% theme A + 40% theme B). Bag-of-words assumption is acceptable.

**Do not use when.** Themes are discrete (use BERTopic/HDBSCAN). Short documents (LDA struggles below ~50 tokens per doc).

**What it produces.** Per-topic word distributions and per-document topic distributions.

**Single-operator feasibility.** Trivial compute. `gensim.models.LdaModel` or `sklearn.decomposition.LatentDirichletAllocation`.

**Choose this over alternatives when.** You explicitly want the soft-assignment property (documents as mixtures). Or when contextualized embeddings are inappropriate for the corpus (heavy code-switching, jargon, very short documents).

**Anti-patterns.**
- Treating LDA topics as ground-truth themes without verification. Same anti-patterns as §2.1.
- Running at a single number of topics.
- No stop-word handling.

**Provenance.**
- Sleep analysis archive: `archive/contaminated_frame/.../deliverables/lda_themes.csv` for output format only.
- TODO[excavate: TikTok Elections notebooks for LDA patterns at policy violation level].

**Notes for agentic execution.** Same as §2.1: sweep topic count, configure stop-words, verify before reporting.

---

### 2.3 Sentence-transformer embeddings + HDBSCAN / K-Means

**Use when.** You want explicit control over the embedding and clustering steps. BERTopic is too opinionated; you need to swap embeddings, dimensionality reduction, or clustering algorithm.

**Do not use when.** BERTopic's defaults are adequate and you have no specific reason to roll your own.

**What it produces.** Document embeddings (high-dim), reduced embeddings (UMAP, typically to 5d), and cluster assignments (HDBSCAN or K-Means).

**Single-operator feasibility.** Same as §2.1 — embedding model is the cost driver.

**Choose this over alternatives when.** Different embedding model is required (multilingual, domain-specific). Different clustering algorithm is theory-motivated (density-based vs. partition).

**Anti-patterns.**
- Using K-Means on embeddings without comparing to HDBSCAN. K-Means forces every document into a cluster; HDBSCAN allows noise points. The latter is usually more honest for noisy text.
- Skipping UMAP dimensionality reduction before clustering on high-dim embeddings. Clustering in 384d is unreliable.

**Provenance.**
- External: `BUTTER_Client` and `archetypes-boyd` operate in this space; their *infrastructure* is reusable, their *construct-mapping* defaults are contaminated (see §12).

**Notes for agentic execution.**
- Always run both HDBSCAN and K-Means on the same reduced embeddings; compare assignments. Where they disagree, the disagreement is informative.

---

### 2.4 Deductive K-means with a priori k

**Use when.** Robust theoretical literature pre-specifies the cluster count `k`, grounded in a recognized taxonomy. Example from the dissertation: instrumental vs. affective violence typology (Meloy 1988; McEllistrem 2004; Dodge 1991; Kingsbury et al. 1997; Fontaine 2007; Hart & Dempster 1997) establishes k=2 *before* data analysis.

**Do not use when.** No such literature exists for the phenomenon under study. **Community-reported LLM behaviors typically do not yet have this literature.** Defaulting to a chosen k without citable grounding crosses the line the contaminated frame crossed when it imposed k=4 on the LCR/sleep phenomena.

**What it produces.** A K-means partition with assignments to theoretically-motivated clusters, cross-validation diagnostics showing cluster separation, discriminant-function loadings, and qualitative evaluation of cluster profiles against theoretical expectations.

**Single-operator feasibility.** Trivial compute (~seconds on hundreds of observations). The binding constraint is the literature-review work to *justify* k, not the execution.

**Choose this over alternatives when.** The theoretical literature is unambiguous about k. If ambiguous or absent, use §2.1 or §2.3 with multi-k stability sweeps instead.

**Anti-patterns.**
- Using a priori k without citable literature grounding.
- Reporting "K-means confirmed the constructs exist organically" when K-means was given the constructs. The validity claim must be that the predicted partition shows meaningful separation, not that k emerged from the data.
- Conflating post-hoc k-selection (after seeing the data) with a priori k (before). The a priori claim is fragile; if k was chosen after data inspection, say so.

**Provenance — the gold-standard worked example.**
- Dissertation: `Pm_html/projects/Instrumental_and_Affective_Mass_Murder/Dissertation.txt`
  - Lines 139–143, 224–230: theoretical grounding for k=2 with full citation chain.
  - Lines 277–284: clustering execution; feature specification (LIWC summary variables Analytic, Clout, Authentic, Tone chosen for face-valid theoretical opposition); Formann sample-size rule check (n=100 meets the 5(2k)=40 threshold).
  - Lines 308–322: cross-validation. 100% of original grouped cases correctly classified; 100% of cross-validated grouped cases correctly classified (leave-one-out).
  - Line 284: "the generated clusters were qualitatively evaluated for theoretical soundness." This is the explicit-codified qualitative step.
- Misuse: `archive/contaminated_frame/` in both project repos.

**Notes for agentic execution.**
1. **Document the theoretical claim for k before any clustering.** Cite the literature. If you cannot cite it, this technique is not available to you.
2. **Use theoretically-derived features**, not data-driven feature selection. The dissertation used LIWC summary variables selected because "face validity suggests that they will have opposing values for affective and instrumental offenders" (line 228).
3. **Check Formann's sample-size rule.** Minimum 2k cases, preferably 5(2k). Document the check.
4. **Run leave-one-out cross-validation** (LOOCV). Report classification accuracy.
5. **Run Linear Discriminant Analysis** as a secondary validation. Report classification rate.
6. **Qualitatively evaluate clusters against the literature.** Inspect cluster means/variances; compare to theoretical expectations. The dissertation does this at lines 587–630, walking through exemplars of each cluster and reading their language against the predicted typology.
7. **If qualitative evaluation reveals divergence from theory:** revise the construct or report the divergence honestly. Do not silently re-label clusters to match the theory.

---

### 2.5 Cluster verification protocol

**Use when.** Always, after any §2.1–§2.4 clustering output. Before reporting any cluster as a theme.

**Do not use when.** Never skipped.

**What it produces.** A go/no-go decision per cluster: theme-eligible or noise.

**Procedure.**
1. Read the top-10 terms of every cluster. If they are dominated by stop-words, the cluster is noise. Re-run §2.x with adjusted preprocessing.
2. KWIC-read 20 random documents assigned to each cluster. The cluster is theme-eligible only if the documents cohere semantically on reading.
3. Compare cluster assignments across two stop-word configurations and two k values. Clusters whose membership shifts wildly are unstable.
4. Document each cluster's status (theme-eligible, noise, unstable) before proceeding to [method §C.6].

**Anti-patterns.**
- The stop-word smear failure documented in the contaminated frame archive. The verification protocol exists to prevent it.

**Provenance.** Synthesized from the contaminated frame's documented failures.

---

## 3. Temporal-structure techniques

**When this section applies.** Phase 2 evidence suggests the phenomenon has a time dimension: bursts around an event, drift across a period, episodic vs. steady-state behavior.

**Section decision table.**

| If you want to… | Reach for… |
|---|---|
| Visualize volume over time | §3.1 Daily/weekly aggregations |
| Test whether a discrete event changed the rate | §3.2 Segmented regression / ITS |
| Detect change-points without specifying when they happened | §3.3 Change-point detection |
| Forecast or characterize autocorrelation | §3.4 ARIMA |
| Anchor analysis to a known event window | §3.5 Event-anchored windows |

### 3.1 Daily/weekly aggregations

**Use when.** First temporal pass on any corpus with timestamps. Always.

**Do not use when.** Corpus has no timestamps.

**What it produces.** Time-series tables of post counts, feature counts (e.g., payload-positive counts), or feature rates (payload-positive / total) per day or per week.

**Single-operator feasibility.** Trivial. `pandas.resample` or `groupby` on date.

**Choose this over alternatives when.** It is the foundation for §3.2–§3.5.

**Anti-patterns.**
- Reporting raw counts when rates are appropriate (the denominator matters when total post volume changes).
- Choosing day vs. week without considering data sparsity. Below ~10 events/day, weekly aggregation is more honest.

**Provenance.** Standard. Sleep archive deliverables show worked examples of format.

**Notes for agentic execution.** Always produce both count and rate series; the rate is more interpretable when the denominator varies.

---

### 3.2 Segmented regression / Interrupted Time Series (ITS)

**Use when.** [method §C.8] A discrete event (model release, policy change) is hypothesized to have changed the rate of a phenomenon, and the time series has enough observations pre- and post-event (≥ 20 each is a common rule of thumb).

**Do not use when.** No discrete event. Insufficient observations on either side. The outcome variable has not cleared Phase 6 validation.

**What it produces.** Fitted regression with intercept, pre-event slope, post-event level change, and post-event slope change. Newey-West HAC standard errors handle autocorrelation.

**Single-operator feasibility.** Trivial compute with `statsmodels.regression.linear_model.OLS` + `cov_type='HAC'`.

**Choose this over alternatives when.** A specific event is the question. The simpler before/after comparison (§3.5) suffices when slope changes don't matter.

**Anti-patterns.**
- Reporting only the level change and not the slope change. The slope change is often more interpretable (the level can shift transiently).
- Using ordinary OLS standard errors without HAC. Time-series autocorrelation makes them too small.
- Reporting ITS coefficients when the outcome variable's construct is unvalidated.

**Provenance.**
- Sleep archive: `archive/contaminated_frame/.../deliverables/its_results.csv` and `its_results_with_ci.csv` show output format.
- Procedural method §C.8 sets the validation precondition.

**Notes for agentic execution.**
- Always report: pre-slope, post-slope, slope-change, level-change, with 95% CIs. The whole table, not the headline.
- Plot the fitted lines over the daily aggregates so the eye can verify what the coefficients claim.

---

### 3.3 Change-point detection

**Use when.** A timing question exists but no specific event date is given. You want the data to tell you when something changed.

**Do not use when.** A specific event date is known — use §3.2 with that date as the cutpoint.

**What it produces.** Detected change-point dates with associated significance or Bayesian posterior.

**Single-operator feasibility.** Trivial with `ruptures` library (frequentist) or Prophet (Bayesian-ish).

**Choose this over alternatives when.** No prior event is given. Multiple change-points may exist.

**Anti-patterns.**
- Treating a detected change-point as causally explained without independent evidence about what happened on that date.
- Single-algorithm reliance. Different change-point algorithms detect different things; report at least two.

**Provenance.**
- The original gaslighting framework's Prophet use is documented but the procedural method retired Prophet in favor of §3.2 because the event date is known.

**Notes for agentic execution.** When in doubt about which algorithm, use `ruptures` with both `Pelt` and `Binseg` and report where they agree.

---

### 3.4 ARIMA

**Use when.** Autocorrelation structure or short-term forecasting matters. Rarely the primary tool for this kind of work, more useful as a diagnostic.

**Do not use when.** Cross-sectional questions dominate.

**What it produces.** Fitted ARIMA model with parameters and residual diagnostics.

**Single-operator feasibility.** Trivial with `statsmodels`.

**Choose this over alternatives when.** Specifically interested in the time-series structure itself, not in the effect of an event.

**Anti-patterns.** ARIMA-as-aesthetic — using it because it sounds rigorous rather than because the question demands it.

**Provenance.**
- Listed in the archived `resources_and_approaches.md` as part of the toolkit. Mostly diagnostic in this kind of work.
- **Worked example with a methodological caveat:** the original phenomenological notebook (`Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt`) lines 741–747 use `ARIMA(daily_sent, order=(1,1,1))` on daily-aggregated sentiment as a temporal diagnostic. This works as a diagnostic, but it is *not* the right tool for the question the corpus was assembled to answer (whether the September 29, 2025 Sonnet 4.5 release changed the rate of the phenomenon). §3.2 segmented regression / ITS with the release date as the cutpoint is what the procedural method specifies for that question. The lesson: ARIMA may be reached for as a default when ITS is what the question demands. Choose by question, not by familiarity.

**Notes for agentic execution.** Use sparingly. The procedural method's primary temporal tools are §3.1 and §3.2. If a discrete event is part of the question, §3.2 is the right tool, not §3.4.

---

### 3.5 Event-anchored windows

**Use when.** Comparing the corpus's behavior in a window before vs. after a known event, without needing slope estimates.

**Do not use when.** Slope changes are the question — use §3.2 instead.

**What it produces.** Pre- and post-event subcorpora with matched lengths, on which §1 lexical analyses can be run separately and compared.

**Single-operator feasibility.** Trivial.

**Choose this over alternatives when.** Lexical or thematic differences across a release are the question, not rate changes.

**Anti-patterns.**
- Mismatched window lengths.
- Failing to compare to a placebo window (same length, earlier period, no event).

**Provenance.** Sleep archive shows pre/post n-gram comparisons (`ngram_frequencies_pre.csv` vs `_post.csv`).

**Notes for agentic execution.** Always include a placebo window for sanity check.

---

## 4. Network and interactional-structure techniques

**When this section applies.** Phase 2 evidence suggests the phenomenon propagates through conversational structure (parent-comment-reply chains, repost patterns, user-to-user interaction).

**Excavation status.** The four worked-example sources excavated in the first-pass population (dissertation, content-moderation telemetry notebook, TikTok Elections trio, original phenomenological notebook) **do not demonstrate conversation-graph techniques** because their data models lack explicit threading (the telemetry notebook has `item_id` and `user_id` but no parent-reply relationships; TikTok content moderation operates on flat queries; the dissertation operates on individual pre-attack texts; the gaslighting notebook works on Reddit posts as flat units without exploiting comment-tree structure).

The active research projects (`claude-lcr-analysis`, `claude-sleep-analysis`) **do** have Reddit comment-tree structure available in their scrapes. This section will be populated as those projects' Phase 3 / Phase 5 work begins to exploit that structure.

### 4.1 Term-term co-occurrence networks
*See §1.3 — this technique is dual-listed; primary entry under Lexical.*

### 4.2 Conversation-thread graph construction
**Use when.** Reddit-style parent-reply structure exists in the corpus and the question concerns propagation of features (lexical, affective, behavioral) across the thread.

**What it would produce.** A directed graph where nodes are posts/comments and edges are parent-child links. Node attributes carry post features (timestamp, author, lexical features, sentiment).

**Single-operator feasibility.** Trivial with `networkx` once the parent_id field is populated (the LCR and sleep scrapers preserve this field).

**Provenance.** `TODO[populate from claude-lcr-analysis or claude-sleep-analysis when Phase 3 / Phase 5 first exploits comment-tree structure]`.

**Notes for agentic execution.** Construction is the easy part. The methodological work is deciding what node attributes matter — which is downstream of §C.5 theme discovery.

### 4.3 Centrality and community detection on conversation graphs
**Use when.** A conversation graph exists (§4.2) and the question concerns which posts/users carry disproportionate influence or which clusters of users discuss the phenomenon together.

**Single-operator feasibility.** Trivial compute via `networkx` community algorithms.

**Provenance.** `TODO[populate]`.

**Anti-patterns.** Treating graph community as substantive theme. Communities are an artifact of the link structure; semantic verification (KWIC reads on community members' posts) is required.

### 4.4 Emotion or feature propagation across parent-child chains
**Use when.** A conversation graph exists and the question concerns whether a feature (sentiment, lexical marker, behavioral pattern) shifts across thread depth or across reply chains.

**Single-operator feasibility.** Modest — feature aggregation by thread depth or by parent-child neighbor pair. `pandas` + `networkx` traversal.

**Provenance.** `TODO[populate]`. The Boyd-lab `NetworkX (Emotion Propagation)` reference listed in archived `resources_and_approaches.md` describes the conceptual pattern but is not adapted into a worked example yet.

**Anti-patterns.** Treating temporal shift across thread depth as causal. Threads are not random samples; later comments are written by users who self-selected into engagement with earlier comments.

---

## 5. Affective and sentiment-structure techniques

**When this section applies.** Phase 2 evidence suggests emotional content matters: users describe feeling specific ways, model output carries specific affective tone.

`TODO[excavate: dissertation LIWC affective dimensions; Juggernaut sentiment patterns; sleep archive for NRC emotion lexicon usage at deliverables/nrc_emotion_scores.csv format]`

### 5.1 VADER sentiment

**Use when.** Rapid, lexicon-based polarity classification on short text fragments. Corpus is not large enough or research question is not specialized enough to warrant model training. Reproducibility and interpretability are priorities.

**Do not use when.** Fine-grained emotion distinction is required (beyond positive/negative/neutral) — use §5.2 NRC. Text length exceeds ~500 tokens per sample. Domain has specialized sentiment vocabulary not in VADER's lexicon (risk: generic lexicon misclassifies platform-specific slang and sarcasm).

**What it produces.** Per-text sentiment scores: `(neg, neu, pos, compound)` where `compound ∈ [-1, 1]`. Aggregate statistics: mean/std of compound per segment, per category, per time window.

**Single-operator feasibility.** Negligible compute. `~2–5 minutes` to score tens of thousands of texts with `vaderSentiment.SentimentIntensityAnalyzer`.

**Choose this over alternatives when.** You need a baseline polarity reading before any deeper sentiment work. You want a single interpretable score per text with no model dependency.

**Anti-patterns.**
- Treating compound score as a true polarity measure without verifying on a sample of low/mid/high-scoring texts. VADER is heuristic-based and can misclassify sarcasm, negation, and domain-specific usage.
- Aggregating compound scores across heterogeneous categories without stratification. Violations in one category may have intrinsically different sentiment profiles than another; means across the pooled corpus are not comparable.
- Ignoring the component scores (`neg`, `neu`, `pos`) in favor of `compound` alone. The `neu` component is often more informative for moderation contexts than `compound`.

**Provenance.** Content-moderation telemetry notebook (`Pm_html/projects/Juggernaut/Juggernaut_Content_Moderation_Telemetry_txt.txt`) lines 3700–3750 (`process_nltk_sentiment_analysis`: VADER initialization, `polarity_scores` extraction, aggregate narrative). Lines 3900–3950 add details in advanced analyses.

**Notes for agentic execution.**
- Always compute and report: (a) compound mean/std, (b) mean of each component (`neg`, `neu`, `pos`), (c) distribution of compound binned by `[-1, -0.5]`, `[-0.5, 0]`, `[0, 0.5]`, `[0.5, 1]`.
- Stratify by relevant categorical structure. Note any segment with N<10 as unreliable.
- Spot-check 20 random texts from high/mid/low compound bins to document whether VADER's classifications match manual judgment on the sample.

---

### 5.2 NRC Emotion Lexicon

**Use when.** Eight-dimension affect mapping (anger, fear, anticipation, trust, surprise, sadness, joy, disgust) plus positive/negative valence is the descriptive question. Going beyond polarity to specific emotional textures.

**Do not use when.** Polarity alone suffices (use §5.1). Multilingual coverage matters (NRC has multilingual versions; verify before assuming English-only).

**What it produces.** Per-document scores for each of the eight emotions plus valence. Aggregate emotional profile tables by segment.

**Single-operator feasibility.** Trivial compute via `nrclex` or by loading the NRC lexicon as a dictionary and applying §1.6 dictionary patterns.

**Choose this over alternatives when.** Specific emotional granularity is the question. The phenomenon under study has reason to differentiate fear from sadness from disgust (e.g., user reactions to model behavior may differ across these dimensions in ways polarity collapses).

**Anti-patterns.**
- Treating the eight dimensions as orthogonal. They are correlated; fear and sadness often co-occur. Report correlations alongside marginals.
- Reporting raw counts when percentages are appropriate.

**Provenance.** Not currently demonstrated in the worked-example projects. `TODO[populate when first project applies NRC]`. The lexicon is available via `nrclex` or direct download from Saif Mohammad's site.

**Notes for agentic execution.** Treat NRC application as a §1.6-style dictionary cross-walk: each NRC term should appear in the corpus before being trusted, and KWIC reads should confirm the term's contextual function before its score is reported.

---

### 5.3 LIWC affective dimensions (Tone, Positive Emotion, Negative Emotion, Anger, Sadness, Anxiety)

**Use when.** Operationalizing affective constructs at the corpus level using validated psycholinguistic categories. When the research question concerns emotional tone, sentiment valence, or mood-related linguistic markers.

**Do not use when.** The phenomenon does not have a clear emotional component. The corpus is too small for reliable LIWC scoring — LIWC2015 requires `~50+` tokens per document for stable estimates; very short documents may produce unreliable category percentages.

**What it produces.** Per-document and per-document-group LIWC affective scores expressed as **percentage of word count**. Comparative tables across subgroups (e.g., cluster membership, time period, demographic stratum).

**Single-operator feasibility.** Trivial if LIWC is licensed (`~30 seconds` to score hundreds of documents). Alternatives: `ContentCoder-Py-LIWCish` (open-source LIWC-compatible, schema-loadable), or custom dictionary-based scoring per §1.6.

**Choose this over alternatives when.** The research question is about emotional or cognitive *style* rather than semantic content. LIWC summary variables (especially Tone) are designed to be construct-valid, not frequency-optimized, so they survive small-sample conditions better than ad-hoc lexicons.

**Anti-patterns.**
- Treating LIWC percentages as raw counts. Always use percentages of total words; raw counts confound document length with construct.
- Reporting only Tone without context. Tone is a 0–100 scale; the dissertation emphasizes (line 587) that "numbers near 50 suggest ambivalence or the absence of affective/emotive terms," so a Tone of 50 is not neutral — it is **semantically empty**.
- Ignoring that LIWC summary variables are composites. Tone is built from emotional language patterns; Authentic is built from pronouns, social words, negations; Analytic is built from articles, prepositions, etc. The categories overlap intentionally. Do not treat them as independent.

**Provenance — the gold-standard worked example.**
- Dissertation: `Pm_html/projects/Instrumental_and_Affective_Mass_Murder/Dissertation.txt`
  - Lines 224–230: rationale for LIWC summary variables. "By combining the examination of multiple LIWC variables assessing both function words, such as pronouns, and content words, LIWC's summary variables effectively capture differences in cognition, self-perception, and emotionality."
  - Lines 231–234: definitions of each summary variable from LIWC2015 Operator's Manual (Pennebaker et al., 2015).
  - Lines 587–588: substantive findings reporting convention. "The Instrumental Type of offender was found to achieve higher scores for Analytic, Clout, and Tone, with lower scores for Authentic, with the inverse being true of the Affective Type of offender."
  - Table 18 (~line 598): exemplar table with Analytic, Clout, Authentic, Tone scores per individual.

**Notes for agentic execution.**
1. **Scope.** The dissertation focuses on LIWC *summary* variables (Analytic, Clout, Authentic, Tone), not on individual emotion words. If the research question concerns specific emotional granularity, use the underlying LIWC emotion categories (PosEmo, NegEmo, Anxiety, Anger, Sadness). The dissertation reports Tone as having "moderate influence" relative to Analytic/Clout/Authentic (line 587).
2. **Interpretation conventions.**
   - High Analytic (>60): formal, logical, hierarchical thinking. Low Analytic (<40): narrative, personal, here-and-now.
   - High Clout (>60): confident, expert, commanding. Low Clout (<40): tentative, anxious, humble.
   - High Authentic (>60): honest, personal, disclosing. Low Authentic (<40): guarded, distanced.
   - High Tone (>70): positive, upbeat. Low Tone (<30): anxious, sad, hostile. Tone `~50`: emotionally neutral or **ambivalent / semantically empty**.
3. **Data preparation.** Transcribe all materials to digital text. Correct only significant misspellings per the LIWC Operator's Manual (dissertation line 276).
4. **Reporting.** Always report percentages, not raw counts. Use tables with means and standard deviations when comparing groups.

---

### 5.4 Custom corpus-derived affective dictionaries

*Cross-reference to §1.6 dictionary construction patterns; affective-domain specifics here.*

**Use when.** Domain contains specialized affect language not in standard lexicons (VADER, NRC, LIWC). Hand-coding a subset of texts has produced reliable polarity or emotion labels. Want to apply those labels at scale across new data from the same platform.

**Do not use when.** No hand-coded ground truth exists. Corpus is too small to derive a stable dictionary (floor: `~200` hand-coded examples, preferably stratified by category). Standard lexicons cover the relevant terms adequately — use §5.1/§5.2/§5.3 instead.

**What it produces.** A CSV or JSON mapping of `(token, polarity)` or `(token, emotion_vector)` derived from corpus data and/or manual annotation. Applied as a lookup during analysis, either for direct scoring or as an augmentation to a base lexicon.

**Single-operator feasibility.** Hand-coding `100–200` samples (`~1–2` hours). Dictionary compilation and validation against held-out sample (`~30` minutes).

**Choose this over alternatives when.** Standard lexicons miss platform-specific or domain-specific terms (e.g., *shadowban* as negative, *verification badge* as positive in social-media contexts). Interpretability is required (every token in the dictionary was hand-validated).

**Anti-patterns.**
- Deriving a dictionary from unvalidated frequency alone. Most-frequent terms ≠ most reliable polarity markers.
- Applying a dictionary trained on one stratum to another without re-validation. Affect language differs across strata.
- No documentation of inter-coder agreement (when multi-coder) or confidence of assignments. A dictionary without reliability metadata is untrustworthy.

**Provenance.**
- Content-moderation telemetry notebook (`Pm_html/projects/Juggernaut/...`) demonstrates the *pattern* of custom domain-stopword extension at runtime (lines 260–280: adding *fyp, viral, greenscreen, 4u, nan* to NLTK stopwords). The affective-dictionary analog is structurally identical: extend a base lexicon with domain-validated terms.
- Original phenomenological notebook (`Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt`) lines 167–198 demonstrate **corpus-derived dictionary refinement that predates the contaminated frame**: seed lexicons (`harmful_lexicon`, `user_experience_lexicon`) are expanded per segment via TF-IDF + sentence-transformer cosine similarity to base-lexicon embeddings. Top-N similar new terms are added. A bias-check narrative documents potential stigma-bias in the resulting dictionary (line 196). This is corpus-prior dictionary expansion, not construct-imposed dictionary freezing.

**Notes for agentic execution.**
1. If hand-coding is needed: stratified random sampling by relevant categorical structure. Code `≥100` samples per stratum.
2. Document the coding rules. If multi-coder: compute Cohen's κ or Krippendorff's α and resolve disagreements by adjudication or exclusion. Under single-coder constraints, use §8.3 precision-at-N instead.
3. Compile dictionary as CSV with `[token, polarity, confidence, freq_per_stratum]` columns. Include frequency per stratum to surface where the dictionary will be most reliable.
4. Validate on a held-out set (10–20 hand-coded texts not used in derivation). Report precision, recall, F1 per category.
5. Always include an explicit bias-check section in the dictionary documentation, following the pattern at line 196 of the original phenomenological notebook.

---

### 5.5 Sentiment time series
*Cross-reference to §3.1 for the aggregation pattern. Sentiment scores from §5.1–§5.4 are aggregated daily/weekly via `.resample('D')['sentiment'].mean()`.*

---

## 6. Demographic and group-structure techniques

**When this section applies.** Phase 2 evidence suggests subgroup differences matter (e.g., users disclosing different identities receive different model behavior).

`TODO[excavate: dissertation pronoun-differential patterns; Juggernaut/TikTok demographic inference patterns]`

### 6.1 Inferred group membership from disclosure

**Use when.** Corpus contains explicit self-disclosure markers (e.g., *"I'm a teacher"*, *"as a parent"*, *"fellow students"*) and you want to stratify analysis by inferred demographic or role membership without hand-coding every text.

**Do not use when.** Disclosure markers are absent or implicit. Hand-coding on a sample shows poor agreement on inferred membership. Risk of misclassification is high (e.g., quoted speech, sarcasm, hypotheticals).

**What it produces.** A new column on the corpus: `(text_id, inferred_membership, confidence)`. Stratified frequency tables, sentiment means, violation rates across inferred groups.

**Single-operator feasibility.** `~30 minutes` to design and test disclosure regex patterns. `~5–10 minutes` to apply across corpus. Plus `~1 hour` to validate on a 100-sample spot-check.

**Choose this over alternatives when.** Explicit group labels are absent. Disclosure markers are reliable and frequent enough (≥5% of corpus). The cost of hand-coding every text is prohibitive.

**Anti-patterns.**
- Inferring groups from weak signals (e.g., pronouns alone — *"we"* ≠ reliable group membership).
- Not validating inferred membership on a sample. Precision of inferred groups must be established before reporting stratified analyses.
- Treating inferred membership as ground truth. Always note it as an inference with confidence bounds.

**Provenance.** Pattern observable in content-moderation telemetry notebook structure (data model includes `user_id` and policy violation fields), though demographic disclosure inference is not explicitly performed there. `TODO[populate when first project applies this technique]`.

**Notes for agentic execution.**
1. Design regex patterns for common disclosures: `\b(as a|i'm a|i'm an|being a)\s+(teacher|parent|student|doctor)\b`. Test on a 500-sample manual review.
2. For each matched disclosure, assign confidence `∈ [0, 1]` based on pattern specificity: explicit first-person disclosure = high; hypothetical (*"if I were a teacher"*) = low.
3. Filter to confidence `≥ 0.7` for main analyses. Report separately for `< 0.7` as secondary or exploratory.
4. Document false positives and false negatives from the validation sample. Note systematic misclassifications.

---

### 6.2 Subgroup PMI / stratified §1.4 analysis

**Use when.** Corpus has been segmented by inferred or labeled group membership (§6.1), categorical structure, or temporal period. Want to measure association strength between a language feature and group membership, controlling for overall corpus frequencies.

**Do not use when.** Segment sizes are imbalanced (one group has `<10` documents). Underlying lexicons or dictionaries have not been validated (see §5.x, §1.6). Want to infer causality — PMI is co-occurrence, not causation.

**What it produces.** PMI matrix: rows = language features, columns = groups, cells = `PMI(feature, group) ∈ [-1, 1]` (normalized, NPMI). Tables of top features per group ranked by NPMI. Positive NPMI = feature more common in that group than expected by chance.

**Single-operator feasibility.** `~10 minutes` per analysis using `sklearn.metrics.mutual_info_score` or manual computation. Memory bound for very large feature × group matrices.

**Choose this over alternatives when.** You want a single interpretable statistic per `(feature, group)` pair across many features and groups. Baseline before regression or classification.

**Anti-patterns.**
- Computing PMI before validating group membership or language feature (§5.x, §6.1). PMI between two unvalidated instruments is a noise correlation.
- Reporting PMI without base rates and conditional probabilities. PMI alone can mislead at low base rates.
- Not controlling for multiple comparisons when the matrix is large.

**Provenance.**
- Content-moderation telemetry notebook lines 1000–1100: per-policy n-gram and co-occurrence analysis with implicit stratification by `policy_cat_0`, `policy_cat_1`, `item_policy_title`.
- TikTok Elections: per-policy stratification throughout `Policy_Misinfo_Anthro_txt.txt`.
- Cross-reference §1.4 PMI for the unstratified version.

**Notes for agentic execution.**
- For each group, compute base rate `P(feature)` and conditional probability `P(feature|group)`. Report alongside NPMI.
- Filter matrix to `|NPMI| ≥ 0.1` to surface meaningful associations.
- Document segment sizes; flag segments with `N < 10` as unreliable estimates.
- Save output: CSV with `[feature, group_1_NPMI, group_2_NPMI, ..., base_rate, max_conditional_prob]`. Include rank and top-10 features per group.

---

### 6.3 LIWC demographic markers (pronouns, age-coded language)

**Use when.** Research question concerns group identity or demographic-correlated linguistic patterns. Pronouns (especially first-person singular vs. plural) and age-coded language are operationalized through LIWC's existing categories.

**Do not use when.** Demographics are not theoretically relevant to the phenomenon. Corpus is too heterogeneous to allow meaningful pronoun comparison (e.g., mixed conversational genres conflate pronoun function).

**What it produces.** Per-document pronoun frequencies (I, we, me, us, my, our, etc.) and age-coded language category percentages.

**Single-operator feasibility.** Built into LIWC2015 as pre-calculated categories. No additional work beyond standard LIWC scoring (see §5.3).

**Choose this over alternatives when.** Demographic inference is a secondary question within a larger linguistic analysis. Pronoun use is validated across decades of psycholinguistic literature (Pennebaker, 2011; Kacewicz et al., 2014).

**Anti-patterns.**
- Assuming all first-person singular pronouns indicate narcissism or self-focus without controlling for document context. The same `I` appears in narrative, introspection, and technical exposition; context determines meaning.
- Treating age-coded language as a reliable demographic inference tool without ground-truth age data. LIWC's age-coded categories are correlational, not causal.

**Provenance.**
- Dissertation lines 226, 275: pronouns are treated as part of LIWC summary variables. "Summary variables are comprised of the composites of other LIWC scores, such as pronoun use" (line 226). "Clout is based on certain pronouns, social words, swear words, negations, and differentiation words" (line 275).
- Dissertation does not analyze pronoun differentials as a standalone demographic finding; pronouns enter the analysis via the Clout and Authentic summary variables. External validation chain referenced: Holtzman (2017) on first-person singular and depression; Kacewicz et al. (2014) on pronouns reflecting social hierarchy.

**Notes for agentic execution.**
1. **LIWC2015 pronoun categories.** Extract: `PPronoun` (personal pronouns), `i` (first-person singular), `we` (first-person plural), `you`, `shehe`, `they`, `ipron` (impersonal). For demographic inference, use first-person singular vs. plural as a rough proxy for self-focus vs. group identity.
2. **Age-coded language.** LIWC2015 does not have a single age-coded category. If age inference is needed, use external lexicons or train a classifier on age-correlated language from social media. Boyd-lab resources may provide templates.
3. **Reporting.** When comparing demographic subgroups, report pronoun percentages alongside other LIWC categories — never in isolation.

---

## 7. Behavioral pathway and sequence techniques

**When this section applies.** Phase 2 evidence suggests temporally-ordered patterns within an interaction matter (e.g., user discloses → model pathologizes → user pushes back → model escalates).

`TODO[excavate: sleep archive for Sankey flow patterns; any sequential-pattern work in TikTok content moderation; the directional_cases artifacts in sleep archive]`

### 7.1 Sankey flow construction

**Use when.** Tracing sequential paths through a multi-stage pathway. Example: search term → policy category → moderation action → outcome. Need to visualize volume flow across stages.

**Do not use when.** Temporal or sequential ordering is not present or is sparse. Cardinality is too high to render readable (too many distinct sources/targets).

**What it produces.** Sankey diagram (e.g., `plotly.graph_objects.Sankey`) showing volume flows between stages, optionally stratified by outcome.

**Single-operator feasibility.** Minutes with `plotly`. Data aggregation (computing flow volumes) adds tens of minutes on large datasets.

**Choose this over alternatives when.** Flow visualization clarifies volume distribution across a multi-stage pathway in a way that summary statistics do not.

**Anti-patterns.**
- Sankey without stratification by relevant outcome dimension. Flows are visually dominated by magnitude, masking rare paths.
- Label clutter. High cardinality makes a readable Sankey difficult; filter to top N per stage or aggregate categories.

**Provenance.**
- TikTok Elections: `Content Moderation Analysis and Reporting_txt.txt` lines 2400–2500 reference Sankey construction within HTML report generation. Pattern: group by stage variables (`search_term`, `policy_cat_0`, `moderation_action`), construct flow object with `(source, target, value)` triples per pairwise stage transition, concatenate across stages.

**Notes for agentic execution.**
1. Construct flow tables stage-by-stage: `[source=stage_1, target=stage_2, value=count]`, then `[source=stage_2, target=stage_3, value=count]`. Concatenate.
2. Filter to top N elements per stage to maintain readability.
3. Use color to distinguish meaningful outcome categories.

---

### 7.2 Sequential pattern mining

**Use when.** Within-interaction temporal sequences matter and you want to discover frequent ordered patterns (e.g., user disclosure → model reaction → user pushback → model escalation).

**Do not use when.** Sequences are very short (`< 3 events`) or the data does not preserve ordering within an interaction.

**What it produces.** Frequent sequential patterns ranked by support, with associated metrics (lift, confidence).

**Single-operator feasibility.** Trivial compute with `prefixspan` or `pymining` libraries for small-to-medium sequence corpora.

**Choose this over alternatives when.** The interactional dynamic is the question — what *follows* what — not just frequency or co-occurrence.

**Anti-patterns.**
- Treating sequence patterns as causal. Sequential patterns are descriptive only.
- Mining at too fine a grain (every token as an event). Coarsen events to meaningful units first.

**Provenance.** Not currently demonstrated in the worked-example projects. `TODO[populate when first project applies this technique]`.

**Notes for agentic execution.** Define event vocabulary first (what counts as an event in this corpus). Coarsen aggressively; sparse event vocabularies make mining tractable.

---

### 7.3 Escalation detection

**Use when.** A pattern of intensification across turns in a conversation is the question. Useful for studying model behavior under user pushback, or for detecting cascading effects.

**Do not use when.** Single-turn data with no within-thread sequence is available.

**What it produces.** Per-thread or per-interaction escalation indicator: a measure of whether intensity (affective, directive, evaluative) increased across the sequence.

**Single-operator feasibility.** Implementation-specific. Often built as a §5.x sentiment-over-turn-position regression within thread, or as a §1.x lexical-intensity-over-turn-position score.

**Choose this over alternatives when.** Within-conversation dynamics are part of the phenomenology being characterized.

**Anti-patterns.**
- Operationalizing escalation with a single feature. Escalation typically involves multiple features (affective tone, directive strength, response specificity) jointly.

**Provenance.** Patterns exist in the retired sleep pipeline's "directional analysis" outputs (`archive/contaminated_frame/.../deliverables/directional_cases_to_read.txt`). The constructs *strength-of-directive* and *response-to-pushback* originated there and are NOT to be imported into LCR or other projects per the procedural method's §C.3 prohibition on cross-project construct import. The *technique* of escalation detection is generalizable; the specific constructs are not.

**Notes for agentic execution.** Define escalation features in the corpus you are working on. Do not import the retired pipeline's directional features.

---

## 8. Hand-coding techniques

**When this section applies.** [method §C.6 dictionary validation and §C.7 construct formation]. Whenever the analyst's judgment is the rigor mechanism.

### 8.1 Sampling strategies

**Use when.** Validating hand-coded dictionaries (§1.6) or construct operationalizations (§C.6, §C.7). Sampling determines which documents are included in precision-at-N (§8.3) calculation.

**Do not use when.** Sample size is too small to stratify meaningfully (floor: 5 documents per stratum).

**Single-operator feasibility.** Depends on sample size. `N=50` per dictionary is standard.

**Sampling variants.**
- **Stratified random sampling.** Use when subgroups must be represented (sampling across themes, time periods, or categorical strata). Required when subgroup-level claims will be made.
- **Anchored sampling.** Use when validating a specific dictionary term — sample only documents where the term appears. Required for precision-at-N on a specific lexicon.
- **Random sampling from full corpus.** Use for general descriptive familiarity (§C.2 corpus read).

**Anti-patterns.**
- Sampling the "easiest" documents to code. Inflates precision estimates.
- Changing the sample after seeing the codes. p-hacks the sample.
- Anchored sampling on a term, then reporting the result as if it were random-corpus precision.

**Provenance.** Dissertation lines 267–270 describe corpus composition (publicly-available sources from three mass-violence databases plus news media; bounded by geography and language; effective census of available pre-attack communications). The dissertation does not perform random sampling for hand-coding because it uses pre-validated LIWC. For corpus-derived dictionaries under the procedural method, stratified random sampling is the default.

**Notes for agentic execution.**
- Document the sampling frame (total population `N`, sample size `n`, sampling method, inclusion/exclusion criteria) before drawing the sample.
- Save the sampled IDs to a CSV before coding begins. The pre-coding sample list is part of the audit trail.

---

### 8.2 Decision rule documentation

**Use when.** Hand-coding any material. Decision rules are the codebook — the explicit criteria for assigning a document to a category.

**Do not use when.** Using pre-validated instruments (LIWC) with published codebooks.

**What it produces.** A written decision rule (1–3 sentences operationalizing the construct), a coded sample, and a revision log documenting how the rule changed during coding.

**Single-operator feasibility.** Low time cost for initial drafting. Iteration is where time goes — budget `1–2 hours` per revision cycle.

**Choose this over alternatives when.** Transparency and defensibility are priorities. An explicit decision rule is superior to ad-hoc coding because it can be audited and forces clarity.

**Anti-patterns.**
- Implicit decision-making: coding without writing down criteria beforehand. Cannot be audited.
- Freezing the rule midway. Changes are legitimate; documenting them is what matters.
- Treating borderline cases as either/or instead of documenting them separately (see §8.4).

**Provenance.** The dissertation does not use hand-coding for its primary analysis (LIWC is pre-validated). Lines 587–630 do contain *implicit* decision-rule application: the dissertation interprets exemplar offenders' manifestos to illustrate Type 1 (Affective) vs. Type 2 (Instrumental) linguistic markers, using decision rules about what constitutes evidence of each type — but these are interpretive, not codified ahead of time.

**Notes for agentic execution.**
1. **Before coding.** Write a one-page decision rule operationalizing the construct. Example: *"A document is coded as CATEGORY=True if it contains [concrete criteria]. Documents where [edge case] are marked borderline."*
2. **During coding.** Keep a revision log: date, example case that prompted revision, old rule, new rule, rationale.
3. **Reporting.** Include the decision rule in the methods section and the revision log in supplementary materials.

Every hand-coding pass produces:
- A written decision rule (what counts as TP / FP / borderline).
- A coded sample with TP/FP/borderline labels.
- A precision-at-N value.
- A revision log documenting rule changes during coding.

This is the audit trail that substitutes for inter-rater reliability under single-coder constraints.

---

### 8.3 Precision-at-N calculation

**Use when.** Validating a dictionary or hand-coded construct. Precision-at-N is the gold standard for single-coder validity under the procedural method.

**Do not use when.** Multiple independent coders are available (use Cohen's κ or Krippendorff's α). Precision-at-N is specifically for single-operator research.

**What it produces.** `precision_at_N = TP / (TP + FP)` over a fixed sample of `N` items (default `N=50`). Acceptance threshold `≥ 0.85` per [method §C.6].

**Single-operator feasibility.** Trivial compute; time is in hand-coding. Budget `30–60 minutes` per 50-item sample.

**Choose this over alternatives when.** Not optional in the procedural method. Mandatory for any hand-coded operationalization that enters inferential analysis.

**Anti-patterns.**
- Reporting precision without showing the sample. The 50 coded items must be auditable.
- Treating precision below 0.85 as a "sensitivity-specificity tradeoff." Below 0.85, the rule needs revision or the construct needs reconsideration. This is the failure that retired the prior LCR pipeline.
- Sampling only obvious true positives to inflate precision. Random sampling within the anchor set is the only defense.

**Provenance.**
- The dissertation does not perform precision-at-N because it uses LIWC (pre-validated). Its analog rigor mechanism is leave-one-out cross-validation on K-means classifications, reporting 100% original and cross-validated correct classification (lines 308–322). For corpus-derived dictionaries that do not benefit from pre-validation, precision-at-N is the required substitute.
- Procedural method §C.6 specifies the threshold and protocol.

**Notes for agentic execution.**
1. **Calculation.** Manually code 50 random documents against the decision rule. For each: mark `TP`, `FP`, `TN`, `FN`. Compute `precision = TP / (TP + FP)`; report `recall = TP / (TP + FN)` alongside.
2. **Reporting.** Report both precision and recall, plus the sample itself as CSV (`document_id, predicted_label, hand_coded_label, match`).
3. **Iteration on failure.** If precision `< 0.85`: revise the decision rule, re-code the same 50 items to see if the revision fixes the misfires, and report both old and new precision values.

---

### 8.4 Borderline-case adjudication

**Use when.** During hand-coding, documents exist that the decision rule does not clearly resolve.

**Do not use when.** The decision rule is so clear that borderline cases do not exist. If borderlines are frequent, the rule is under-specified — return to §8.2.

**What it produces.** A list of borderline documents with documented reasoning for each adjudication, plus notation in the coded sample distinguishing borderline-resolved from clear cases.

**Single-operator feasibility.** Low compute; time is in careful reasoning about edge cases.

**Choose this over alternatives when.** Always. Borderline cases should never be silently coded as if they were clear.

**Anti-patterns.**
- Adjudicating borderlines by intuition without documentation. The absence of documentation is the failure.
- Collapsing borderlines into a third category without reconsidering whether the construct is well-defined.

**Provenance.** The dissertation does not perform hand-coding with explicit borderline handling because it uses LIWC. The pattern is articulated in procedural method §C.6. The contaminated frame's documented failures often stemmed from unresolved borderline cases coded as if clear.

**Notes for agentic execution.**
1. **During coding.** Mark any ambiguous document as `BORDERLINE_REASON=[explanation]`.
2. **Adjudication rule.** Apply a secondary criterion (domain-expert review, external lexicon cross-walk, temporal context). Document the criterion and the outcome.
3. **Reporting.** Precision-at-N calculations should note how many of the 50 items were borderline-resolved and how this affected precision. Example phrasing: *"Precision = 42/47 TP/(TP+FP) = 0.89, excluding 3 borderline-resolved cases documented in Appendix B."*

---

## 9. Voice-segmentation techniques

**When this section applies.** [method §C.4]. Separating model-attributed speech from user-attributed speech in unstructured community discourse.

### 9.0 Output schema — span-level, not row-level

**Default output structure (applies to all §9.x techniques):** voice segmentation produces **span-level** annotations, not row-level. For each input row, the segmenter produces a list of records, each:

```
(span_start_char, span_end_char, label, confidence, source)
```

- `span_start_char`, `span_end_char`: character offsets within the row's text.
- `label`: one of `Direct_Quote_Of_Model`, `Paraphrase_Of_Model`, `LCR_System_Prompt_Reproduction` (LCR-specific), `User_Original_Content`, `auto_moderator_content`, `Unclassifiable`.
- `confidence`: `high`, `medium`, `low`.
- `source`: `regex`, `llm:<model>`, `hand`.

Spans must cover the entire row text contiguously with no gaps and no overlaps. Any text not affirmatively detected by a §9.1 regex rule or §9.2 LLM rule is assigned `User_Original_Content` at low or medium confidence, depending on textual context. `User_Original_Content` is the **floor default**, not an active assertion of user attribution.

**Why span-level is mandatory.** Reddit rows routinely mix kinds of speech: author narration with embedded direct Claude quotes; user reactions interleaved with paraphrased model behavior; system-prompt-verbatim text reproduced inside surrounding commentary. Collapsing a row to a single "predominant" label erases every span that is not the majority kind. The §C.4 reliable-segmentation threshold (70%) is *measured on span coverage*, not on row-level confidence flags.

**Storage.** Save segmentation output as a long-format CSV (one row per span, joined back to the original row by `row_id`) rather than a wide CSV with a single label column. Preserve the original row text untouched so spans can be re-extracted for hand-validation.

**Hand-validation under span-level segmentation** (per §9.3): the validation sample displays the full row text with span boundaries and labels marked, and the researcher confirms or corrects the *spans*, not a single label per row. The §C.4 70% threshold is computed across spans weighted by character count (i.e., what fraction of the corpus's total character count is reliably segmented).

**Deprecation note:** any voice-segmentation outputs produced before 2026-05-18 used row-level labeling and are formally deprecated. They may be preserved for provenance but should not feed downstream analysis. See `claude-sleep-analysis/data/pass1b_canonical_voice_segmented*.csv` and `claude-lcr-analysis/data/lcr_pass1b_canonical_voice_segmented*.csv` for the deprecated row-level outputs.



### 9.1 Regex-based attribution

**Use when.** Separating model-attributed speech from user-attributed speech in unstructured community discourse. First-pass segmentation before more expensive methods.

**Do not use when.** The discourse is already structurally segmented (e.g., scraped conversational data with explicit speaker labels). Regex alone is unreliable on highly variable Reddit prose; pair with §9.2 fallback.

**What it produces.** Per-snippet labels: model-quote / user-narration / unclassified. Optionally a confidence flag.

**Single-operator feasibility.** Trivial compute. Time cost is in pattern design and hand-validation (§9.3).

**Choose this over alternatives when.** Speed and transparency are required. As a baseline before §9.2 LLM-assisted segmentation. Rules alone are fast but brittle; rules + LLM fallback balance both.

**Anti-patterns.**
- Single-method reliance without hand-validation. The contaminated LCR pipeline trusted regex segmentation without validating it; segmentation failures contributed to the 88% false-positive rate documented in `archive/contaminated_frame/`.
- Treating regex output as authoritative for downstream construct claims when it has not cleared §9.3 hand-validation.

**Provenance.** Original phenomenological notebook (`Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt`) lines 200–214 (`detect_quote` function) implements a hybrid approach:
1. spaCy NER to find PERSON/ORG entities mentioning *claude* (lines 204–206)
2. Regex patterns: `["`>]|claude said|ai responded|prompt|system prompt` (line 208)
3. Zero-shot classification fallback when ambiguous: `zero_shot_classifier(text, ["AI Quote", "User Quote", "No Quote"])` (lines 210–212)

This pattern predates the contaminated frame and is reusable.

**Notes for agentic execution.**
- Document each regex pattern with the attribution category it triggers.
- Pair every regex-segmented output with §9.3 hand-validation on a 50–100 item sample.
- Never report downstream construct prevalence on regex-segmented data without precision-at-N on the segmentation itself.

---

### 9.2 LLM-assisted segmentation

**Use when.** Regex alone misses paraphrased model attribution or fails on highly variable Reddit prose. As a fallback when §9.1 cannot disambiguate.

**Do not use when.** Budget is binding. Each LLM call costs time and (for hosted models) money; single-operator constraints limit volume.

**What it produces.** Per-snippet labels with model confidence. Categories typically: Direct_Quote, Paraphrase_Of_Model, User_Original_Content (cross-reference §9.4).

**Single-operator feasibility.** Modest compute. Local models (via `transformers` pipeline) work for hundreds-of-items batches; thousands require pacing or batched cloud calls.

**Choose this over alternatives when.** Regex coverage is below threshold and improved coverage is worth the LLM call cost.

**Anti-patterns.**
- Treating the LLM's classification as ground truth. The LLM is a tool, not a validator — per the procedural method §D.4.
- Not documenting the prompt used. Reproducibility requires prompt + model + date of run.
- Using LLM classification on the entire corpus when regex would cover 90% of it. Reserve LLM calls for the regex-residual.

**Provenance.** Original phenomenological notebook lines 216–225 (`classify_response`): zero-shot classification with candidate labels *"Direct Response to Claude", "Response to User", "Personal Anecdote"*. Confusion matrix output at lines 227–232 documents partial validation (20% holdout).

**Notes for agentic execution.**
- Document the prompt, the model name, the model version, the date, and the role of the LLM output in the analytical chain.
- Always pair LLM segmentation with §9.3 hand-validation on a sample.
- Report what the LLM was used for in the methods writeup. The LLM is transparently disclosed.

---

### 9.3 Hand-validation protocol

**Use when.** After any segmentation (§9.1 or §9.2) produces output that will be used downstream. Mandatory before construct claims depend on the segmentation.

**Do not use when.** Never skipped if segmentation feeds construct claims.

**What it produces.** A precision-at-N estimate for segmentation accuracy. A documented set of failure modes (cases the segmenter mislabels) with hypotheses about why.

**Single-operator feasibility.** Budget `1–2 hours` per 50–100 item validation pass.

**Procedure.**
1. Sample 50–100 segmented snippets stratified by predicted category.
2. Hand-code each: did the segmenter correctly identify what is model-attributed vs. user-attributed?
3. Compute precision per category and overall.
4. Document failure modes: paraphrase mislabeled as quote? speaker-label heuristic failed? embedded responses missed?
5. Compute the percent of the corpus that is *reliably segmentable*.

**Decision rule.** Per procedural method §C.4: at least 70% reliable segmentation, OR an explicit decision to treat the corpus as predominantly paraphrased narrative (which changes what downstream questions can be asked).

**Anti-patterns.**
- Sampling only documents the segmenter labeled with high confidence. Stratify by predicted category to surface failures.
- Reporting overall accuracy without per-category precision. The segmenter may be excellent at one category and unreliable at another.

**Provenance.** Original phenomenological notebook lines 227–232 implements a partial validation via confusion matrix on a 20% holdout. Pattern is reusable; the procedural method makes hand-validation mandatory rather than optional.

**Notes for agentic execution.** Save the hand-validation sample (with predicted-vs-coded labels) as `deliverables/segmentation_validation.csv`. The file is part of the audit trail.

---

### 9.4 Paraphrase handling

**Use when.** The corpus contains both direct quotes from model output and paraphrased summaries of model behavior. Distinct evidentiary weight applies to each.

**Do not use when.** All model-attributed content is direct quotes (rare in Reddit discourse).

**What it produces.** Segmented data with distinct labels: `Direct_Quote`, `Paraphrase_Of_Model`, `User_Original_Content`. Downstream analyses may stratify by category or report claims qualified by evidentiary tier.

**Single-operator feasibility.** Manual tagging on a sample (`~hours`) or automated with §9.2 zero-shot classification followed by §9.3 hand-validation.

**Choose this over alternatives when.** Claim robustness depends on distinguishing direct quotes from paraphrases. Direct quotes support specific claims about model output language; paraphrases support general claims about model behavior patterns but cannot support claims about exact wording.

**Anti-patterns.**
- Conflating direct quotes and paraphrases in the same analysis. They are different evidentiary kinds.
- Treating paraphrased model behavior as if it were a direct quote when reporting findings.
- Failing to disclose the proportion of evidence that is paraphrase vs. quote in the corpus characterization.

**Provenance.** Original phenomenological notebook identifies `is_quote` (line 214) but does not further distinguish direct quotes from paraphrases of model behavior. This is a documented gap — the original pipeline treated all model-attributed content as a single category. The active method's §C.4 makes paraphrase a first-class concern.

**Notes for agentic execution.**
- If paraphrase handling is added, create a three-category annotation: `Direct_Quote`, `Paraphrase_Of_Model`, `User_Original_Content`.
- Report corpus composition by category in the writeup.
- Qualify claims by evidentiary tier. Claims about model wording require direct quotes; claims about model behavior pattern can include paraphrase.

---

## 10. External lexical resources (validated dictionaries)

**Purpose.** Pre-validated lexicons that provide *external lexical validation* (per the procedural method's distinction between external lexical validation and coding-decision reliability). Use these in [method §C.6] as a cross-walk against corpus-derived dictionaries.

### 10.1 LIWC (Linguistic Inquiry and Word Count)
- **What.** Validated psycholinguistic dictionary with categories: pronoun classes, cognitive processes, affective processes, biological processes, etc., plus summary variables (Analytical, Clout, Authentic, Tone).
- **Access.** Commercial. The researcher used it in her dissertation.
- **Open-source approximation.** `ContentCoder-Py-LIWCish` (Boyd-lab). Schema-compatible, dictionary-loadable.
- **Best for.** Affective and cognitive process categories; pronoun-differential analysis.

### 10.2 NRC Emotion Lexicon (NRC-EmoLex)
- **What.** Words associated with eight basic emotions (anger, fear, anticipation, trust, surprise, sadness, joy, disgust) plus positive/negative valence.
- **Access.** Free for academic and personal use.
- **Best for.** Affective texture mapping across documents.

### 10.3 VADER (Valence Aware Dictionary)
- **What.** Sentiment lexicon optimized for social media.
- **Access.** Open-source (`vaderSentiment`).
- **Best for.** Polarity scoring on noisy short text.

### 10.4 mental-health-datasets
- **What.** A collection of labelled datasets and lexicons relevant to mental health discourse classification.
- **Access.** Forked locally at `C:\Users\drhea\repos\mental-health-datasets` (verify path).
- **Note.** Some entries are labelled datasets rather than dictionaries proper; they can serve as a source of validated terms for cross-walk even if not used as classifiers directly.

### 10.5 depression-datasets-nlp
- **What.** Similar to §10.4, focused on depression.
- **Access.** Forked locally.

### 10.6 Boyd-lab lexicons (via MEHv2, ContentCoder, RIOTLite)
- **What.** A family of LIWC-style and meaning-extraction dictionaries.
- **Access.** Open-source forks.
- **Caveats.** Use the lexicons; do not import the contaminated frame's mapping logic from `archetypes-boyd`. See §12.

`TODO[populate: specific path verifications for each fork; specific term-count and category-count for each resource]`

---

## 11. Feature attribution and interpretability

**When this section applies.** [method §C.8]. After validated classifiers or regressions exist, explain *which features* drive predictions.

### 11.1 SHAP (TreeExplainer, LinearExplainer)

**Use when.** A validated classifier exists and the question is *which features drive its predictions*. TreeExplainer for tree-based models (RandomForest, XGBoost). LinearExplainer for linear models with sparse feature sets (e.g., TF-IDF + LogisticRegression).

**Do not use when.** The underlying classifier has not been validated (its features have not cleared §C.6 dictionary validation, or the classifier has not been hand-validated on a holdout). SHAP on an unvalidated classifier explains noise.

**What it produces.** Per-token or per-feature attribution values. Global feature importance via aggregation. Per-instance explanations.

**Single-operator feasibility.** Trivial compute with `shap` library.

**Choose this over alternatives when.** Need interpretability of an already-validated model. Linear coefficient inspection (§11.3) is simpler and often sufficient; reach for SHAP when feature interactions matter or the classifier is non-linear.

**Anti-patterns.**
- Using SHAP to *validate* an unvalidated classifier. This was the failure mode of the contaminated LCR pipeline — SHAP top-tokens were reported as if they corroborated the four-category construct, but the underlying classifier was trained on an unvalidated lexicon.
- Reporting global SHAP importance without inspecting individual instances. Aggregation hides distributional shifts.

**Provenance.** Listed as a tool in the archived `resources_and_approaches.md`. Misuse documented in `archive/contaminated_frame/` (LCR repo).

**Notes for agentic execution.** SHAP comes *after* validation, not as a substitute for it. Per [method §C.8] the underlying constructs must have cleared Phase 6 before SHAP is meaningful.

---

### 11.2 LIME

**Use when.** Per-instance local explanations of any black-box classifier are needed and SHAP is not appropriate (e.g., the model is not a tree or linear model).

**Do not use when.** A simpler interpretation (linear coefficients) suffices. Stability of LIME explanations across runs is poor; report multiple runs.

**What it produces.** Local linear approximation of a classifier near a specific instance, with per-feature attribution.

**Single-operator feasibility.** Trivial compute with `lime` library.

**Choose this over alternatives when.** SHAP is unavailable or computationally infeasible for the model class.

**Anti-patterns.** Treating a single LIME run as authoritative; LIME has stochastic elements and explanations can shift across runs.

**Provenance.** Not currently demonstrated in worked-example projects. `TODO[populate when first project applies LIME]`.

**Notes for agentic execution.** Run LIME multiple times on each target instance and report the stability of the explanation.

---

### 11.3 Logistic regression coefficient inspection

**Use when.** Inferential analysis of binary or dichotomous outcomes. Coefficient sign, magnitude, and significance are the core interpretive units.

**Do not use when.** Outcome is continuous (use linear regression). Sample size is very small relative to predictors (use shrinkage or regularization).

**What it produces.** Fitted logistic model with per-predictor coefficients (`B`), odds ratios (`OR = exp(B)`), 95% confidence intervals, p-values, and model-fit metrics (McFadden R², AIC, BIC).

**Single-operator feasibility.** Trivial with `statsmodels.api.Logit` or `sklearn.linear_model.LogisticRegression`.

**Choose this over alternatives when.** Predictors-to-outcome relationship is the research question and outcome is genuinely binary. Coefficient interpretation is more straightforward than SHAP or LIME when the model is linear.

**Anti-patterns.**
- Reporting OR without the underlying B coefficient. Both give readers the full information.
- Reporting `p < .05` without confidence intervals. CIs are more informative than p-values alone.
- Interpreting `OR > 1` as a substantively meaningful effect on the basis of significance alone. Magnitude and significance are separate; a large OR with wide CI may be non-significant.
- Reporting coefficients without checking multicollinearity. The dissertation runs VIF checks (lines 548–550, all VIFs `< 2` reported at line 560).

**Provenance — the gold-standard worked example.**
Dissertation: `Pm_html/projects/Instrumental_and_Affective_Mass_Murder/Dissertation.txt`
- Lines 277, 294: rationale for choosing binary logistic regression over linear regression — "overcomes many of the restrictive assumptions of linear regression. For instance, normality and homoscedasticity of the residuals are not assumed, and binary logistic regression does not require the complete absence of multicollinearity."
- Lines 501–506: Hypothesis 6 model results, with full reporting convention. Example phrasing: *"The regression coefficient for the perpetrator being killed was significant (B = 2.45, OR = 11.61, p = .010), indicating that the odds of observing membership in the Instrumental Cluster increases by approximately 1,061% when a perpetrator is killed rather than commits suicide or is apprehended."* (Line 503.) Percent-change calculation: `(OR – 1) × 100 = (11.61 – 1) × 100 = 1,061%`.
- Lines 502–503: model-fit reporting. *"McFadden's R-squared was calculated to examine the model fit, where values greater than .2 are indicative of models with excellent fit (Louviere et al., 2000). The McFadden R-squared value calculated for this model was 0.22."*
- Lines 548–550, 560: VIF reporting convention.
- Lines 564–568: extended model adding demographic targeting and home location; same reporting convention.

**Notes for agentic execution.**
1. **Fit.** Use `statsmodels.api.Logit` or equivalent.
2. **Report per predictor.** Variable name, `B`, `SE(B)`, `OR`, 95% CI for `OR`, `p`, and percent-change interpretation.
3. **Percent-change convention.** Use `(OR – 1) × 100`. For `OR < 1`, report inverted: `1/OR` and the percent-increase-in-odds-of-opposite outcome.
4. **Model fit.** Report χ² of overall model, df, `p`, McFadden R² (≥ 0.2 indicates excellent fit per Louviere et al., 2000).
5. **Multicollinearity.** Compute VIF for each predictor. `VIF < 2` acceptable; `VIF > 5` problematic.

---

### 11.4 Tree-based feature importance (Gini, permutation)

**Use when.** Tree-based classifiers (RandomForest, XGBoost, LightGBM) are used and feature importance is needed without SHAP infrastructure.

**Do not use when.** A linear model is appropriate (use §11.3). The classifier has not been validated.

**What it produces.** Per-feature importance scores: Gini importance (default in sklearn) or permutation importance (more reliable but compute-heavier).

**Single-operator feasibility.** Trivial compute with sklearn's built-in `.feature_importances_` or `sklearn.inspection.permutation_importance`.

**Choose this over alternatives when.** Tree model is in use and quick global importance ranking is enough. For per-instance attribution use §11.1 SHAP.

**Anti-patterns.**
- Trusting Gini importance for correlated features. Gini favors high-cardinality features. Permutation importance is more reliable.
- Reporting feature importance without per-feature CI or stability estimate.

**Provenance.** Not currently demonstrated in worked-example projects. The dissertation does not use tree-based models. `TODO[populate when first project applies this technique]`.

**Notes for agentic execution.** Prefer permutation importance over Gini when correlated features are present. Bootstrap to estimate stability of the importance ranking.

---

## 12. External tool catalog with adaptation notes

**Purpose.** Inventory of external repositories whose techniques are referenced in this library, with notes on what is adaptable to single-operator constraints and what is not.

### 12.1 pleonasty-llm-turn-analysis (Boyd-lab fork)
- **Original purpose.** LLM batch annotation and chunking for conversational turn analysis.
- **Original scale.** vLLM and HuggingFace batch processing — assumes GPU and many API calls.
- **Single-operator adaptation.** The *prompt structure* and the *segmentation schema* (Model_Exact_Quote / Model_Paraphrase / User_Internal_Reaction / User_Task_Context) are reusable as conceptual categories for voice-segmentation work (§9.2), even when the actual segmentation is done with a small number of targeted LLM calls. The four-category schema is *not* automatically valid for any phenomenon; treat it as a candidate framework, not as the framework. The contaminated frame treated it as the framework.
- **Adaptable component.** Prompt templates for turn-level structural extraction.
- **Not adaptable.** The batch processing infrastructure under single-operator API constraints.

### 12.2 MEHv2 (Meaning Extraction Helper v2)
- **Original purpose.** Tweet-aware tokenization, lemmatization, meaning-extraction across short social media text.
- **Original language.** C# port utilizing LemmaGen v3 and NLTK.
- **Single-operator adaptation.** The tokenization rules (handling @-mentions, hashtags, URLs, contractions) and the lemmatization defaults can be re-implemented in Python with NLTK's TweetTokenizer plus a Python wrapper for LemmaGen. The *patterns* are reusable; the C# implementation is not directly.
- **Adaptable component.** Tokenization patterns, lemmatization choices.
- **Not adaptable.** Direct C# integration.

### 12.3 Contextualizer-keyword-incontext (Boyd-lab fork)
- **Original purpose.** KWIC analysis at scale.
- **Single-operator adaptation.** Fully usable — KWIC is computationally lightweight. Adopt the windowing logic and the output format directly (§1.7).

### 12.4 ContentCoder-Py-LIWCish (Boyd-lab fork)
- **Original purpose.** LIWC-style dictionary scoring without licensing constraints.
- **Single-operator adaptation.** Fully usable for dictionary-based feature extraction. Load corpus-derived dictionaries through its schema (§1.6).

### 12.5 RIOTLite-dictbased (Boyd-lab fork)
- **Original purpose.** Dictionary-based lexical scoring with wildcard support and exact-match prioritization.
- **Single-operator adaptation.** Fully usable. Useful when corpus-derived dictionaries need wildcard expansion (e.g., *spiral\** matching *spiral, spiraling, spiraled, spirals*).

### 12.6 archetypes-boyd (Boyd-lab fork)
- **Original purpose.** SentenceTransformer-based categorical embedding into pre-named archetypes.
- **Single-operator adaptation.** **The infrastructure is reusable; the workflow is contaminated.** The Boyd-lab pattern (or rather, its application in the prior LCR pipeline) pre-named the archetypes (*Pathologizing, Care Taking, Hard Directive, Soft Directive*) and mapped clusters into them. That is construct imposition. The procedural method retires this workflow. The underlying SentenceTransformer infrastructure can still be used in §2.3.

### 12.7 BUTTER_Client
`TODO[excavate: original purpose, single-operator adaptation, what is and is not reusable]`

### 12.8 mental-health-datasets, depression-datasets-nlp
*See §10.4, §10.5.*

---

## 13. Worked-example index

Pointers to prior projects that demonstrate library techniques in practice. When in doubt about how to execute a technique, read the worked example.

| Source artifact | Library sections exemplified |
|---|---|
| `Pm_html/projects/Instrumental_and_Affective_Mass_Murder/Dissertation.txt` | §2.4 (gold-standard deductive K-means with citable theoretical grounding for k=2; LOOCV at 100% accuracy; LDA secondary validation), §5.3 (LIWC summary variables Analytic/Clout/Authentic/Tone with full interpretation conventions), §6.3 (pronouns within LIWC summary variables), §8.1–§8.4 (hand-coding protocols as the single-operator analog for inter-rater reliability), §11.3 (logistic regression with full reporting convention: B, OR, 95% CI, percent-change, McFadden R², VIF) |
| `Pm_html/projects/Juggernaut/Juggernaut_Content_Moderation_Telemetry_txt.txt` (content-moderation telemetry notebook) | §1.1 (custom domain-stopword extension pattern at lines 260–280; n-gram + per-policy frequency analysis at 1000–1100), §1.5 (n-gram + co-occurrence analysis 700–1100), §5.1 (VADER usage at 3700–3750 and 3900–3950), §5.4 (custom dictionary pattern via stopword extension), §6.2 (PMI stratified by policy at 1000–1100). Not present: §4.2–§4.4 (no conversation-thread structure in data), §5.2 (no NRC), §5.3 (no LIWC), §6.1, §6.3 |
| `Pm_html/projects/TikTok_Elections/Content Moderation Analysis and Reporting_txt.txt` | §1.1 (emoji/hashtag frequency at ~680–720), §1.3 (hashtag co-occurrence networks at 1800–1950), §1.5 (n-gram analysis with per-policy stratification at 1500–1600 and 2050–2150), §7.1 (Sankey construction at 2400–2500). Not present: §2.1, §2.2, §2.3, §5.x |
| `Pm_html/projects/TikTok_Elections/Content View Source Analysis_txt.txt` | (smaller companion notebook; cross-reference for descriptive engagement patterns) |
| `Pm_html/projects/TikTok_Elections/Policy_Misinfo_Anthro_txt.txt` | §1.1 (n-gram by policy with `violation_detection_rate` pattern at 528–560, 1200–1250 — note distinct concept of policy-conditional violation rate vs. raw frequency), §1.2 (collocation analysis around policy anchors at 580–650, 900–1050), §1.3 (term-policy co-occurrence networks at 1100–1150), §1.5 (bigrams/trigrams 650–750), §1.6 (Claude-assisted thematic interpretation at 1300–1400). Not present: clustering/embedding/sentiment lexicons |
| `Pm_html/projects/Claude_Gaslighting/Claude_Sonnet_Discourse_Analysis_txt.txt` (original phenomenological notebook — **predates the contaminated frame**) | §1.1 (dual lemmatized/non-lemmatized text processing at 142–159), §1.2 (PMI bigram collocation at 638–646), §1.3 (term-term co-occurrence matrix + NetworkX visualization with pruning at 1797–1840), §1.5 (n-gram analysis at 613–655), §1.6 (**critical** corpus-derived dictionary refinement via TF-IDF + sentence-transformer cosine similarity per segment at 167–198 — predates contamination), §1.7 (implicit KWIC through narrative interpretation), §2.1 (BERTopic attempted *without* stop-word vectorizer at 1214–1234 — documented anti-pattern), §2.2 (LDA with coherence-driven k sweep + pyLDAvis at 554–611), §3.1 (daily aggregations via `.resample('D')` at 366, 496, 741), §3.4 (ARIMA used in place of ITS at 741–747 — a gap the procedural method addresses), §9.1 (hybrid regex + spaCy NER + zero-shot fallback voice segmentation at 200–214), §9.2 (zero-shot classification at 216–225), §9.3 (partial validation via confusion matrix on 20% holdout at 227–232) |
| `Pm_html/projects/TikTok_SCOPEX/` | `TODO[excavate]` |
| `Pm_html/projects/TikTok_Multi_Signal_Minor_Safety/` | `TODO[excavate]` |

### 13.1 The original phenomenological notebook — special note

The Claude_Sonnet_Discourse_Analysis notebook is **methodologically significant** because it predates the construct-imposition failure that retired the subsequent pipelines. Key features that the active method preserves:

- Seed dictionaries (`harmful_lexicon`, `user_experience_lexicon`) are pre-specified but **refined inductively per segment** via TF-IDF + sentence-transformer cosine similarity to base-lexicon embeddings. This is corpus-prior dictionary expansion, not construct-imposed dictionary freezing.
- Bias-check narrative documents potential stigma-bias in the resulting dictionary (line 196). Explicit reflexivity at the lexicon stage.
- LDA topic count is swept across `k = max(2, n_topics-2)` to `n_topics+3` and the best coherence is selected (line 568). Multi-k sensitivity is present, not absent.
- Voice segmentation uses a hybrid regex + LLM-fallback pattern with held-out validation (lines 200–232). The validation is incomplete but the pattern is correct.

Gaps in the original notebook that the active method fixes:
- KWIC reading is implicit (in narrative interpretation) rather than explicit. The active method makes it explicit and codified.
- BERTopic was attempted without stop-word vectorizer configuration — the very anti-pattern that became the smoking gun in the contaminated frame. The active method makes the stop-word vectorizer configuration mandatory.
- Cluster verification is implicit (high-coherence narratives suggest eyeballing) rather than codified. The active method's §2.5 makes verification a checkpoint with a decision rule.
- ITS is not implemented; ARIMA is used as an alternative diagnostic. The active method's §3.2 makes segmented regression with HAC standard errors the primary technique when a discrete event is part of the question.
- Paraphrase vs. direct quote is not distinguished. The active method's §9.4 makes this a first-class concern.

Read this notebook as the closest available exemplar of the disposition the active method preserves, while reading its archived successors at `claude-lcr-analysis/archive/contaminated_frame/` as the cautionary tale of what happens when that disposition is abandoned.

---

## 14. Revision log

| Date | Change | Agent / human |
|---|---|---|
| 2026-05-17 | Initial scaffold. Populated §0–§3, §10, §12 partial, schema-only for the rest. | Claude (Opus 4.7, 1M context) |
| 2026-05-17 | First excavation pass. Four parallel Explore agents populated entries from four prior sources: dissertation (§2.4 gold-standard worked example, §5.3, §6.3, §8.1–§8.4, §11.3); content-moderation telemetry notebook (§1.1, §1.5, §5.1, §5.4, §6.2); TikTok Elections trio (§1.1, §1.2, §1.3, §1.5, §1.6, §7.1); original phenomenological notebook (§1.1, §1.2, §1.3, §1.5, §1.6 pre-contamination pattern, §1.7, §2.1, §2.2, §2.5, §3.1, §9.1, §9.2, §9.3, §9.4). §13 worked-example index expanded with specific line-range citations and the "predates contamination" note. §11.1, §11.2, §11.4 populated where applicable, marked TODO otherwise. §4.x networks remain TODO (not demonstrated in worked-example sources excavated). §5.2 NRC, §6.1 group inference, §7.2, §7.3 marked as available techniques with patterns; first-project application will populate. §12.7 BUTTER_Client adaptation notes still TODO. | Claude (Opus 4.7, 1M context) integrating four Explore-agent outputs |

`TODO[ongoing: as future agent sessions populate TODO blocks above, append entries here]`
