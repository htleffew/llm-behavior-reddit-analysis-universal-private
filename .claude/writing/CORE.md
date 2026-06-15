# CORE — universal prose rules

These rules apply to **every** surface: portfolio pages, Medium essays, and formal
research writing. A profile may add rules; it may never relax a CORE rule. The
mechanical rules below are the ones `tools/style_gate.py` and `tools/writing-lint.sh`
enforce from `tools/banned_terms.txt`.

## 1. Punctuation

- **No em-dashes (—) or en-dashes (–) in prose.** Use commas, colons, or
  semicolons. Hyphens are allowed only in compound modifiers and numeric ranges.
- No exclamation points in professional copy.
- Trust standard sentence punctuation; do not stack parentheticals to dodge the
  em-dash rule.

## 2. Banned vocabulary

Do not use (enforced, case-insensitive, see `tools/banned_terms.txt`):

delve, delves, delving, tapestry, resonate, resonates, pivotal, multifaceted,
underscore, underscores, commendably, paradigm, testament, intricate,
transformative, symphony, mosaic, cornerstone, bedrock, navigate (as a metaphor
for "deal with"), seamless, seamlessly, leverage (as a verb), gamify, synergy,
next-gen, unlock (figurative), revolutionary, disruptive.

## 3. Sentence construction

- **No negative parallelism.** Banned: "not only X but also Y", "isn't just …",
  "is not just about …", "it's not X, it's Y".
- **Do not open sentences with** And, But, This, However, Therefore, Moreover,
  Furthermore, Indeed.
- No canned significance markers: "this demonstrates", "this proves", "this shows",
  "this is significant because", "clearly", "it is worth noting that".
- Avoid "rather than" as filler contrast.

## 4. Posture

- **Trust the reader.** Subtle delivery, no rhetorical hammers, no manufactured
  drama, no puffery ("comprehensive analysis demonstrates").
- **Concrete over abstract.** A claim earns its place with a specific number,
  mechanism, or named instance, not with intensity. If a sentence could sit in any
  document on the topic, it is too abstract.
- **No metaphor reaches.** Name the mechanism precisely instead.

## 5. The one principle

Authenticity tracks concrete, first-person evidence. Prose reads as Heather when it
reports what she found and decided; it reads as a model when it describes the idea
of the work in the abstract. Every profile rule below CORE is downstream of this.

## Provenance

CORE consolidates the rules previously duplicated in: portfolio `VOICE.md` §6–§7,
`_voice_audit/voice-spec.md`, LaTeX `outline_lcr_preprint.md` §2, and the
`dead-signal` `style_gate.py` banned-token regex. Those files are being converted to
pointers to this one (see `ROLLOUT.md`).
