# A Procedural Method for the Phenomenological Investigation of Community-Reported LLM Behavior

**Heather Leffew, PhD**
**HAIIQU**

This document specifies a topic-agnostic procedural method for investigating any community-reported behavior of a deployed large language model using observational social-discourse data. It is the load-bearing methodological artifact for any project that adopts this approach. Each instantiation (a single phenomenon studied in a single corpus) executes this method end to end, in order, with the checkpoint discipline specified at each phase.

This method generalizes the structure of the author's doctoral dissertation methodology (Leffew, *Instrumental and Affective Mass Murder*) — corpus identification → linguistic analysis → clustering → cluster verification → regression on cluster membership — to a domain where prior theoretical literature on the phenomenon is absent or thin. Where the dissertation could pre-specify cluster structure on the basis of established offender-typology literature, this method explicitly cannot. The procedural sequence reflects that shift.

---

## A. Epistemic foundations

This method operates within the tradition of **reflexive single-analyst qualitative inquiry**: Smith's Interpretative Phenomenological Analysis, Charmaz's constructivist grounded theory, and Braun & Clarke's reflexive thematic analysis. Within that tradition:

- The analyst is the instrument. Rigor is grounded in **positionality, reflexivity, audit trail, and transparent stance**.
- Constructs **emerge from the corpus** through descriptive engagement. They are not pre-specified.
- Quantification is **supporting evidence for clinical observation**, not the primary epistemic engine.
- The deliverable is a **case characterization with concept formation and theoretical contribution** — not a population estimate or a hypothesis-test outcome.

The method is **abductive in the actual sense**: it cycles between qualitative observation, descriptive engagement, and tentative construct formation, returning to earlier phases whenever evidence does not support the move forward. Abduction here means *the researcher's commitments are revised when the data does not support them*, not that constructs are pre-named and labelled abductive.

---

## B. Constraints that shape the method

The method is designed for, not despite, the following standing constraints:

- **Single researcher.** No second human coder. No research team. No academic affiliation.
- **Observational only.** No experimental manipulation. No access to model internals. No transcripts confirmed against ground truth.
- **Unfunded and unaffiliated.** No API budget for programmatic cross-model audits. No supervised-classifier training at scale. No commercial NLP licenses.
- **Noisy data.** Reddit and similar sources mix direct quotes, paraphrase, narrative reconstruction, and emotional reaction in unstructured prose. Voice attribution is often ambiguous. Some posts contain exact model quotes; many do not.
- **LLM assistance is available as a tool, not as a validator.** LLM-assisted segmentation, KWIC review, and thematic exploration are part of the toolkit; LLM-as-second-rater is not, because a single model cannot serve as an independent rater of its own family of phenomena.

These constraints are stated up front so that future agents do not propose moves (inter-rater Cohen's Kappa, API-based behavioral audits, supervised classifiers, second-coder consensus, population inference at scale) that the design cannot support. See §D.5 for the substitution table.

---

## C. The procedural phases

Each phase has: an **input**, an **action**, a **checkpoint**, a **decision rule for advancing**, and a stated **failure mode**. Phases are executed in order. A failed checkpoint returns the work to the failed phase. No phase may be skipped.

---

### Phase 0 — Triggering Observation & Warrant

**Input.** A first-hand or reported observation of an LLM behavior that appears anomalous, role-violating, or otherwise of concern.

**Action.**
1. Document the seed encounter in first person. Include the model, the version, the date, the conversational context, the trigger, the model's exact wording where retained, and the researcher's experiential reaction. The seed encounter is data, not background.
2. Conduct an informal scan of two to four relevant community forums (subreddits, Discord servers, X/Twitter, etc.) for similar reports. Read, do not yet quantify.
3. Estimate the volume and emotional weight of reports across at least a two-week window.
4. State the question being asked, in a single sentence. Distinguish what is being asked from what is being assumed.

**Checkpoint.** Is there sufficient community volume to warrant formal investigation? Is the question phenomenologically coherent (not a fishing expedition framed as a question)? Is the seed encounter documented in enough detail to be cited as evidence?

**Decision rule for advancing.** Documented seed encounter; informal observation of at least ten independent community reports across at least two distinct sources; a stated research question.

**Failure mode.** Skipping the seed encounter on the assumption that corpus volume will replace it. The researcher's own observation is irreducibly first-person evidence and is part of the case being made. A method that excludes it imports a lab-frame requirement (researcher-independence) that this design does not satisfy and does not need to.

---

### Phase 1 — Corpus Definition & Scrape

**Input.** A warranted question from Phase 0 and an informal sense of where the community discusses the phenomenon.

**Action.**
1. Specify communities (subreddits, etc.) with rationale for each inclusion and each exclusion.
2. Specify the time window. Where the phenomenon is plausibly associated with a discrete event (a model release, a policy change, a system-prompt update), the window must span both pre-event and post-event time. Where no such event exists, the window must be wide enough to reveal whether the phenomenon is steady-state or episodic.
3. Compile **seed terms**. These come from two and only two sources: (a) the seed encounter from Phase 0, and (b) the surface language of the community reports observed in Phase 0. Seed terms are not drawn from external theory, sibling projects, or general clinical lexicons. They are descriptive anchors for the corpus, nothing more.
4. **Choose retrieval mode explicitly.** Two modes are valid; the choice is documented and justified.
   - **Untargeted retrieval (wholesale).** All posts to the specified communities in the time window, no keyword filter at the retrieval layer. Appropriate when the phenomenon is plausibly common enough in untargeted discourse to support analysis (clinically marked vocabulary, community events that generated dense discussion, well-defined sub-communities). Documents what people in those communities posted during the window; does not document a population base rate, because the retrieval frame is itself a sample.
   - **Targeted retrieval (search-filtered).** Posts that match the documented seed terms. Appropriate when the phenomenon is rare enough in untargeted retrieval that wholesale would produce a corpus dominated by general discussion. Subject to a documented retrieval-frame bias: the corpus contains only posts whose authors used the seed-term vocabulary; users who experienced the phenomenon but described it in language not covered by the seed terms are absent. This bias is named in the methods section and managed via [method §1.9 iterative seed-term refinement] in the methods library.
   - When in doubt, run an untargeted pilot retrieval first (small scale, e.g., one subreddit), inspect frequency of seed-term occurrence, decide retrieval mode for the full scrape from that evidence.
5. Execute the scrape under the documented retrieval mode. Deduplicate. Document the methodology in detail sufficient for replication: source archive, API or scraper used, retrieval mode, parameters, date of pull, raw size, post-dedupe size.
6. Preserve raw output untouched at the repository's `data/` root.

**Checkpoint.** Are the seed terms documented with their provenance from the seed encounter and community reports? Is the retrieval mode (wholesale or targeted) chosen and justified explicitly? Is the scrape replicable from the documented methodology alone? Is the resulting corpus large enough to support descriptive engagement and small enough to be read in part by hand? *Note on size:* wholesale corpora typically need thousands of posts to surface phenomenon density; targeted corpora may need only hundreds (the qualitative-research traditions named in §A operate at the low hundreds comfortably). Size requirements are downstream of retrieval mode.

**Decision rule for advancing.** Yes to all four.

**Failure mode.** Seed terms drawn from sibling projects or from a theoretical taxonomy not yet derived from this corpus. This is the contamination vector that has retired prior pipelines under this method. Or: retrieval mode chosen without justification — particularly, wholesale assumed by default in a phenomenon known to be rare. Or: claiming a population-rate finding from any retrieval mode, when the retrieval frame is itself a sample of a sample.

---

### Phase 2 — Descriptive Engagement

This is the long phase. Most of the analytical insight in the final case characterization comes from doing this phase honestly.

**Input.** Raw corpus from Phase 1.

**Action.**
1. Raw token frequency distributions on the full corpus.
2. Bigram, trigram, and skipgram frequency tables.
3. Lemmatization and stop-word removal **as ablations, not defaults**. Compute frequencies with and without each. Compare. Document each decision and its consequence for the frequency table.
4. Collocations around each seed term: co-occurring words within window sizes of 5, 10, and 20 tokens. Save as tables.
5. Keyword-in-context (KWIC) reads on every seed term. At each window size, sample at least ten random hits and read them as text. Embodied familiarity with the corpus is a Phase 2 deliverable.
6. Co-occurrence networks at the post and at the comment-thread level.
7. **Read the corpus.** Spend hours, not minutes, reading randomly sampled posts and comment threads in their raw form. The numbers and the tables are not a substitute. Take reflexive notes (see §D.1).

**Checkpoint.** Do the high-frequency terms make sense, or are they noise dominated by stop words? Has lemmatization collapsed meaningful variation (e.g., merging *help* into *helping* into *helped* when the contexts differ)? Has stop-word removal stripped phrases the phenomenon hinges on? Have the seed terms been examined in context, or only counted? Has the corpus been read?

**Decision rule for advancing.** The top fifty most-frequent content-bearing terms have each been examined in KWIC reads at at least ten random hits. Stop-word ablation has been performed and documented. Lemmatization decision has been documented with rationale. The researcher can describe in plain language what the corpus contains, beyond what the frequency tables show.

**Failure mode.** Skipping the corpus read in favor of the frequency tables. Treating lemmatization and stop-word removal as defaults. Trusting any term's apparent prominence without inspecting it in context.

---

### Phase 3 — Unit-of-Analysis Determination

**Input.** Descriptive engagement output from Phase 2.

**Action.**
1. Re-run a representative subset of Phase 2 analyses at each candidate unit of analysis:
   - The whole post in isolation
   - The post combined with its full comment tree
   - The post separated from its comments
   - Individual comments as independent units
   - Comment-and-reply pairs as units
   - All posts aggregated to the subreddit/domain level
2. Compare across units: at which unit do meaningful patterns stabilize? At which unit does the phenomenon concentrate? At which unit does signal dilute into noise?

**Checkpoint.** Has more than one unit been examined? Is there a defensible empirical reason for the chosen primary unit? Is there a defensible secondary unit for triangulation?

**Decision rule for advancing.** An empirically chosen primary unit and a documented secondary unit, with the rationale for each tied to specific Phase 2 evidence.

**Failure mode.** Defaulting to "the post" as the unit of analysis because that is what the scrape returns. The unit is itself an empirical question and must be answered before downstream analyses are meaningful.

---

### Phase 4 — Voice Segmentation and Tier Assignment

**Input.** Chosen unit of analysis and the raw corpus.

**Action.**
1. Develop heuristics for separating model-attributed speech from user speech. Span-level output per [methods library §9.0]: each row produces a contiguous list of `(span_start, span_end, label, confidence, source)` records. Labels: `Direct_Quote_Of_Model`, `Paraphrase_Of_Model`, project-specific verbatim categories (e.g., `LCR_System_Prompt_Reproduction`), `User_Original_Content` (floor default), `auto_moderator_content`, `Unclassifiable`. Candidate detection signals: blockquote markers, attribution phrases (*it said*, *Claude told me*, *the model replied*), speaker-label conventions, inline quotation marks, paraphrase markers, project-specific verbatim phrasals from any prior iterative-refinement seed list.
2. Validate the heuristics on a hand-sampled subset of fifty to one hundred items, **at the span level** per [methods library §9.0]. The hand-validation reviews per-row span breakdowns, not a single predominant label per row.
3. Document failure modes explicitly: paraphrase ambiguity, missing quote markers, embedded responses, narrative reconstruction, span-boundary errors.
4. **Per-row tier assignment.** For each row in the corpus, compute the character-count-weighted fraction of the row's text that is reliably segmented (high or medium confidence on regex-or-LLM source, not the floor default). Apply the tier rule below.
5. Where direct quotes are scarce — typical for community-reported LLM behavior — also document paraphrased model behavior as a distinct category, with appropriate epistemic caution about its reliability.

**Tier rule.** Each row is assigned to one of two tiers:

- **Tier 1 — segmentable.** The row contains at least one model-attributed span (Direct_Quote_Of_Model, Paraphrase_Of_Model, or a project-specific verbatim category) at high or medium confidence covering at least 20% of the row's character count. Tier-1 rows support discrete model-vs-user analyses downstream: PMI between user-disclosure features and model-output features, conditional probabilities, regression-of-attribution-category, segmented analyses by voice label.
- **Tier 2 — unitary.** The row does not meet the Tier-1 criterion. Tier-2 rows feed downstream analyses as whole-unit qualitative evidence: theme discovery, sentiment characterization, narrative content, frequency / collocation / temporal patterns, base-rate-of-discussion measures. Tier-2 rows do *not* feed discrete model-vs-user attribution claims.

The tier assignment is recorded as an additional column on the corpus (`evidence_tier`: `1` or `2`). The voice-segmentation output continues to carry the full span breakdown for all rows; the tier column expresses the row-level epistemic capacity.

**Checkpoint.** Is segmentation valid on the hand-coded sample? What fraction of the corpus, weighted by character count, falls in Tier 1? Are the failure modes documented?

**Decision rule for advancing.** Segmentation is valid on the sample. Tier 1 and Tier 2 are populated; the corpus continues to Phase 5 with the tier assignment in place. There is no pass-fail threshold; the size of Tier 1 determines what kinds of claims Phase 8 inferential work can make, and that is documented honestly in the writeup.

**Replaces the prior 70% pass-fail threshold (2026-05-19).** Earlier versions of this method specified a 70% reliable-segmentation threshold as the §C.4 advance criterion. Empirical work on the sleep-nudge and LCR phenomena demonstrated that reliable segmentation in the 5–30% range is the realistic ceiling for community-reported LLM-behavior corpora, because community discourse is predominantly paraphrased narrative. The two-tier structure replaces the threshold-pass-fail framing with an honest epistemic stratification: claims that require discrete attribution rest on Tier 1; claims about theme, sentiment, and discourse structure rest on the whole corpus. See `claude-sleep-analysis` and `claude-lcr-analysis` audit trails for the empirical basis.

**Failure mode.** Trusting regex-based or LLM-based segmentation without hand validation. Treating paraphrase as if it were quote. Reporting downstream model-vs-user analyses on Tier 2 evidence. Reporting Tier 1 as if it represented a population sample of the phenomenon rather than the segmentable subset of the corpus.

---

### Phase 5 — Inductive Theme Discovery

**Input.** Segmented (or knowingly unsegmented) corpus at the chosen unit.

**Action.**
1. Topic modeling at multiple values of `k`: at minimum 3, 5, 8, 12, and 20. Use BERTopic, LDA, or a clustering method on sentence-transformer embeddings; record which is used and why.
2. **Stop-word handling is explicit.** Before any topic-model output is read, configure the representation step (e.g., the `CountVectorizer` inside BERTopic, or equivalent) to strip standard English stop words and any domain-specific stop words observed in Phase 2 to dominate without semantic content (*claude*, *gpt*, *prompt*, etc., as warranted).
3. **Sensitivity sweep.** Re-run topic modeling with: (a) alternative stop-word lists, (b) lemmatization on versus off, (c) at least two embedding choices where computationally feasible.
4. **Stability matrix.** For each `k`, list the top terms of each cluster. Across `k` values and across the sensitivity sweep, identify which themes appear repeatedly with content-bearing top terms. Construct a table showing which themes survive each ablation.
5. Inspect every top-term list as text. If the top terms of a cluster are stop words (*the, to, and, you, it*), the cluster is a noise cluster, not a theme. Mark it as such.

**Checkpoint.** Are the discovered themes content-bearing or are they stop-word artifacts? Do themes survive across `k` values? Do themes survive stop-word ablation? Do themes survive lemmatization ablation? Are the cluster contents semantically coherent when re-examined in context (KWIC on the cluster's most distinctive terms)?

**Decision rule for advancing.** Only themes that (a) have content-bearing top terms, (b) survive multi-`k` stability, (c) survive the sensitivity sweep, and (d) cohere semantically on KWIC re-inspection are eligible for Phase 6. Anything that fails any of (a)–(d) returns to Phase 5 with revised preprocessing, or is discarded as not a theme.

**Failure mode.** Reporting every cluster as if it were a theme. Failing to notice that one or more "themes" are stop-word smears. Treating BERTopic output as construct-validating rather than as descriptive.

---

### Phase 6 — Lexicon Construction & Refinement

**Input.** Surviving themes from Phase 5.

**Action.** For each surviving theme:
1. Derive a candidate dictionary of terms and phrases **from the corpus**: from the cluster's top terms, from KWIC reads, from collocations, from skipgrams.
2. **External lexical cross-walk.** Compare the candidate dictionary against pre-validated external dictionaries in the relevant domain — `mental-health-datasets`, `depression-datasets-nlp`, LIWC clinical categories, NRC emotion lexicons, or other community-validated lexical resources. The cross-walk serves *external lexical validation*: it confirms that the terms used have been adjudicated as belonging to their claimed semantic category by people outside this study. This is a separate concern from coding-decision reliability (which is handled by step 4). Terms in the candidate dictionary that have no analog in any external resource are not disqualified, but they are flagged for closer KWIC scrutiny. Terms in well-established external lexicons that the corpus has surfaced as relevant are weighted toward inclusion. Document the cross-walk explicitly: which external resources, which overlapping terms, which corpus-unique terms, which external-unique terms intentionally excluded.
3. Hand-sample fifty items where each dictionary term appears in the chosen unit of analysis.
4. Code each instance by hand: does the term function the way the theme claims it does in this context, or is it something else (slang, sarcasm, casual usage, distinct meaning)? This step is what stands in for inter-rater reliability under single-researcher constraints — it produces precision-at-N, which is a documentable rigor metric.
5. Compute precision-at-N for the dictionary (proportion of sampled hits that are true positives for the theme).
6. **Iterate.** Add terms that the sampling reveals were missed. Remove or split terms that the sampling reveals are firing on the wrong meaning. Where a single term covers two distinct meanings, split the dictionary or add a contextual disambiguator. Document every revision and its rationale.

**Checkpoint.** Is each dictionary performing at acceptable precision in context? Has the external lexical cross-walk been documented? Are there terms that should be added based on what the sampling surfaced? Are there terms that should be removed or split?

**Decision rule for advancing.** Dictionary stabilizes at precision-at-N of at least 0.85, with documented sensitivity analysis: precision at alternative thresholds, alternative dictionary variants tested, decisions transparent. **A precision below 0.85 returns to lexicon refinement or to Phase 5 theme reconsideration. It is not a "limitation."**

**Failure mode.** Locking a dictionary based on bare-word matching without context coding. Treating sub-0.85 precision as a sensitivity-specificity tradeoff to be footnoted rather than as construct invalidation to be addressed. This is the failure mode that retired the prior LCR pipeline.

---

### Phase 7 — Construct Formation

**Input.** Stable themes from Phase 5 with validated dictionaries from Phase 6.

**Action.**
1. Name the constructs. Each construct must be tied to: (a) specific Phase 5 stability evidence, (b) a Phase 6 validated dictionary, (c) representative KWIC examples, and (d) coherence with the seed encounter from Phase 0 and with any foundational phenomenological prior.
2. Where the constructs match a prior phenomenological taxonomy (e.g., a researcher's own prior qualitative work on the phenomenon), document the match and any divergence. The prior taxonomy is a prior, not a target.
3. Where the constructs differ from a sibling project's constructs, do not import. Leave them distinct. Document the difference. Treat any cross-project comparison as a Phase 10 question, not a Phase 7 question.

**Checkpoint.** Is each named construct defensible from the evidence in (a)–(d)? Could the researcher, asked cold, justify the construct from the corpus itself rather than from prior commitment to it?

**Decision rule for advancing.** Yes to both.

**Failure mode.** Pre-naming the constructs in Phase 0 or Phase 1 and then Phase 7 becoming a labelling exercise. Importing constructs from a sibling project on the theory that "the phenomena are related."

---

### Phase 8 — Inferential & Quantitative Analysis

**Input.** Validated constructs with validated dictionaries.

**Action.**
1. Compute base rates, frequencies, and conditional probabilities at the chosen unit.
2. Compute pointwise mutual information (PMI) between constructs only where each side is independently validated.
3. Where a discrete event (model release, policy change) is part of the question, run segmented-regression interrupted time series with Newey-West HAC standard errors.
4. Run regression models where motivated, with simple specifications preferred over complex ones. Report effect sizes, not only `p` values.
5. Where useful, compute SHAP or other feature-attribution diagnostics, but interpret them as descriptive rather than explanatory.
6. Each statistical claim is a claim about the relationship between two independently validated instruments. Statistical relationships between an unvalidated instrument and anything else are not findings.

**Checkpoint.** Does every quantitative claim correspond to a relationship between Phase-6-validated constructs? Have effect sizes and uncertainty been reported?

**Decision rule for advancing.** Yes to both.

**Failure mode.** Scaling quantitative analysis on unvalidated lexicons. Reporting prevalence and slope-change findings as the headline before construct work has stabilized.

---

### Phase 9 — Reflexive Synthesis & Writeup

**Input.** All prior phases.

**Action.**
1. Write the case characterization. **Concept formation is the central deliverable**, not the regression table.
2. The seed encounter from Phase 0 is part of the case being made, not omitted.
3. Document the audit trail: every Phase-3 unit decision, every Phase-5 `k` and sensitivity outcome, every Phase-6 dictionary revision, every Phase-7 construct decision, every Phase-8 statistical specification.
4. State positionality and the analyst's stance explicitly. The disposition described in §A is part of the methodological warrant for the claims.
5. Quantitative findings serve as supporting evidence for the phenomenological characterization. They do not lead.
6. Limitations are stated truthfully: single researcher, observational, paraphrased-prominent data, no second coder, no programmatic validation. These are the design's epistemic shape, not its failures.

**Checkpoint.** Does the writeup match the foundational phenomenological disposition in §A? Are limitations stated as the design's epistemic shape rather than as apologies? Is concept formation the central deliverable?

**Failure mode.** Lab-frame writeup that elides the analyst's role and treats quantification as primary. Limitations section that promises a future second-coder paper as if that is the missing piece.

---

### Phase 10 — Cross-Corpus Comparison (Downstream Synthesis Only)

**Input.** Two or more completed Phase 9 case characterizations from independent phenomena.

**Action.** Examine whether constructs derived independently in each corpus exhibit family resemblance, divergence, or possible shared underlying disposition. Where shared underlying disposition is hypothesized, frame it as a *finding* of the comparison, not as the *frame* that produced either of the underlying case characterizations.

**Failure mode.** Doing this before both Phase 9 outputs exist. Using the cross-corpus hypothesis as the prior for either underlying study, which contaminates both.

---

## D. Cross-cutting practices

### D.1. Reflexive memo trail

A continuous research journal is kept across all phases. It is not a code commit log. It is a record of the analyst's thinking: hypotheses entertained and abandoned, decisions made and the felt sense behind them, surprises, discomforts, and reversals. The reflexive memo trail is what substitutes for an external coder's challenge: by externalizing the analyst's thinking in writing, the analyst's positionality is available for inspection by future readers (and by future agents).

### D.2. Decision-rule transparency

Every phase's checkpoint has a decision rule. Every decision rule's outcome is documented with its rationale. An auditor reading the project should be able to identify, for any claim, the chain of decision-rule outcomes that produced it.

### D.3. Sensitivity ablations as default

Wherever a preprocessing or modelling choice has plausible alternatives (stop-word list, lemmatization, window size, `k` value, embedding model, dictionary threshold), the analysis is run with at least two alternatives and the comparison is reported. Findings that do not survive sensitivity are not findings.

### D.4. LLM-as-tool, not LLM-as-validator

LLM assistance is part of the method. It is used for: rapid KWIC summarization, candidate paraphrase identification, initial segmentation drafts, dictionary expansion suggestions, and reading partner functions during reflexive memo writing. It is **not** used as: a second rater whose agreement is reported as Cohen's Kappa, an independent source of ground truth, a substitute for hand validation. Where LLM output is incorporated into the analysis, the prompt, the model, the date, and the LLM's role in the decision are documented. The LLM is an instrument, like a tokenizer or a regex library, and is reported with the same transparency.

### D.5. Substitution table

| What an academic lab frame demands | What this method substitutes |
|---|---|
| Inter-rater Cohen's Kappa | Reflexive audit trail; transparent positionality; sensitivity/stability ablations |
| Second human coder | LLM-as-tool transparently reported as tool, never as validator |
| Programmatic cross-model API audits | Triangulation across kinds of evidence: lived experience + community discourse + clinical or domain theory + ethics literature |
| Supervised classifier at scale | Hand-coded case characterization with explicit decision rules and documented dictionary precision |
| Population inference | Case characterization with epistemic claims sized to observational paraphrase-prominent data |
| Inter-coder consensus on construct definition | Phase 7 construct defensibility from corpus evidence alone, independently of researcher's prior commitments |

These are not concessions. They are the appropriate rigor criteria for this design.

---

## E. When and how to do cross-corpus comparison

A cross-corpus comparison (Phase 10) is appropriate only after at least two phenomena have been carried through Phases 0–9 in their own right, each with its own constructs, its own validated dictionaries, and its own case characterization. The comparison then examines:

- Where do constructs derived independently exhibit semantic or structural family resemblance?
- Where do they diverge, and what does the divergence indicate?
- Where independence is suggestive of a shared underlying disposition (e.g., a model-family attribute that persists across releases via distillation training weight inheritance), state the hypothesis as a *contribution of the comparison*, not as the frame that produced either input study.

A cross-corpus paper cannot retroactively legitimize unvalidated constructs in its input studies. If either input study has been carried out under the contaminated frame this document retires, the comparison is also contaminated.

---

## F. Glossary

- **Seed encounter.** The researcher's own first-hand observation of the phenomenon. Treated as data, not as background.
- **Seed terms.** A small a priori list of words and phrases drawn from the seed encounter and from informal community-report scanning. Used to anchor the descriptive engagement, not to operationalize a construct.
- **Construct imposition.** Pre-naming the analytical categories before descriptive engagement has surfaced them from the corpus. The failure mode this method exists to prevent.
- **Sensitivity ablation.** Re-running an analysis with at least one alternative preprocessing or modelling choice and reporting the comparison. Default practice, not a robustness add-on.
- **Precision-at-N.** The proportion of hand-sampled hits of a dictionary term that are true positives for the construct it is meant to indicate. Phase 6 acceptance threshold is 0.85.
- **Stability matrix.** A table showing which Phase 5 themes survive across `k` values, stop-word lists, and lemmatization choices. The eligibility filter for Phase 6 entry.
- **Validated instrument.** A construct that has cleared Phases 5 through 7 with documented evidence. A precondition for any inferential statistic in Phase 8.

---

## G. Versioning and revision

This method is itself revisable. Where an instantiation surfaces a failure mode this document does not anticipate, the document is updated and the version is noted. Project case characterizations cite the version of the method under which they were produced.

**Version 1.0** — Initial codification, retiring the prior "abductive pipeline" architecture archived under `archive/contaminated_frame/`.
