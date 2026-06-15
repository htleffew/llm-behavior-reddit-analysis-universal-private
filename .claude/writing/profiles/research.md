# Profile: RESEARCH (arXiv-style LaTeX papers and preregistrations)

Applies to formal research writing in LaTeX: arXiv/journal articles, preprints, and
preregistrations (`claude-lcr-analysis/paper/`, `claude-sleep-analysis/`, and
siblings). Obey **CORE** first, then everything here.

## Voice

- **Single-author voice. No plural "we / our."** The subject of methodological
  actions is the data, the methods, the paper, or the analysis. Never "we."
- **Anti-LLM prose fingerprint, strictly.** No puffery ("comprehensive analysis
  demonstrates"), no sentence-start Moreover / Furthermore / Indeed (already CORE),
  no drama, no over-sanded uniformity.
- **Trust the reader.** Subtle delivery, parenthetical precision, no rhetorical
  hammers, no metaphor reaches.

## Structure

- **IMRaD abstract:** one dense paragraph covering background, methods, key results,
  and implications. Target 280–320 words for articles.
- **Discoverability keyword list:** ~25–35 keywords spanning the method, the
  phenomenon, the model, the corpus type, and the framing.
- **Preregistrations** additionally state hypotheses, design, sampling/stopping
  rules, and the analysis plan before data are examined, per the procedural method.

## Citations

- **APA, alphabetical**, matching the archived exemplar draft. The `.bst` is the
  venue's; APA is the prose-citation style throughout.

## Inherited authorities (do not duplicate, point to them)

- Procedural method and methods library:
  `llm-behavior-reddit-analysis-universal/community_reported_llm_behavior_method.md`
  and `methods_library.md`.
- Prose/citation exemplar inherited by current papers:
  `claude-lcr-analysis/archive/contaminated_frame/paper/leffew_2026_guardrail-paradox.md`.

## Exit checklist

- [ ] CORE clean (no dashes, no banned tokens, no canned openers)
- [ ] No "we/our" anywhere in methodological prose
- [ ] IMRaD abstract within target length
- [ ] 25–35 keyword list present
- [ ] APA citations, alphabetical
- [ ] Preregistration: hypotheses + design + analysis plan fixed before data

## Provenance

Consolidates `claude-lcr-analysis/.project/outline_lcr_preprint.md` §2 and the
"inherited from the auto-memory" prose rules. That section becomes a pointer here.
