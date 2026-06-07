# Universal LLM Behavioral Anomaly Research Engine

This document outlines the architecture for a universal, dataset-driven pipeline designed to investigate any emergent LLM behavioral anomaly (e.g., Gaslighting, Sleep Nudges, etc.) in public discourse. 

The engine is built strictly on an **Abductive (Bayesian-Updating) Mixed-Methods Methodology**, designed to continuously loop from qualitative observation $\rightarrow$ unsupervised data mining $\rightarrow$ contextual validation $\rightarrow$ deductive psychometric testing.

## The 5-Phase Abductive Pipeline Architecture

### Phase 1: The Sensor (Descriptive & Qualitative Prior Generation)
Before formal analysis begins, the pipeline ingests raw social discourse (Reddit/Twitter) based on qualitative complaints.
*   **Action**: Rapid, ad-hoc regex/keyword sweeping based on initial human observation (e.g., searching for "go to sleep" or "seek professional help").
*   **Goal**: Establish the "Prior." Determine if there is enough data mass to warrant formal investigation.

### Phase 2: The Tri-Modal Segmenter & Hierarchical Unit of Measurement
This is the most critical phase for resolving the ambiguity of social media storytelling. The pipeline uses vLLM/HuggingFace (`Pleonasty` architecture) to structuralize unstructured paragraphs. 

Crucially, this phase establishes a **Dynamic Unit of Measurement**. Rather than forcing all analysis to happen at the "post" level, the engine structurally tags every piece of text so it can be dynamically aggregated or isolated later:
- **Macro (Domain-Level)**: Aggregating all data to look for macro-trends.
- **Meso (Thread-Level)**: Analyzing a singular post + all its comments + all their replies as a coherent conversation loop.
- **Micro (Unit-Level)**: Treating posts, comments, and replies as completely distinct linguistic units.

*   **Action**: An LLM agent processes the selected unit of measurement and segments the text into four distinct vectors:
    1.  **`Model_Exact_Quote`**: Text explicitly attributed to the LLM (e.g., *“Claude said: '...'”*).
    2.  **`Model_Paraphrase`**: User's anecdotal description of model behavior (e.g., *"It started diagnosing me."*).
    3.  **`User_Internal_Reaction`**: Emotional or cognitive fallout (e.g., *"I felt totally gaslit and confused."*).
    4.  **`User_Task_Context`**: What the user was actually trying to do (e.g., *"I was just asking it to format code."*).
*   **Goal**: Create a structurally pristine database where we can extract both model and user behavior at *any* unit of measurement, allowing us to compare apples to apples.

### Phase 3: The Inductive Explorer (Unsupervised Feature Discovery)
The pipeline throws the segmented vectors into the "kitchen sink" to see what organic patterns emerge.
*   **Action**:
    *   `MEHv2` tokenizes and lemmatizes the chunks.
    *   NetworkX builds co-occurrence graphs.
    *   Semantic embeddings (`all-MiniLM-L6-v2`) are fed into `HDBSCAN` and `K-Means` (without pre-set labels) to discover organic clusters within the *Model* chunks and the *User* chunks separately.
*   **Goal**: Discover the raw shape of the phenomenon without theoretical bias.

### Phase 4: The Contextual Anchor (Psychometric Mapping)
We take the organic clusters from Phase 3 and translate them into academically rigorous constructs.
*   **Action**:
    *   Use `Contextualizer-keyword-incontext` (KWIC) to ensure the organic clusters make semantic sense.
    *   Feed the validated words into `ContentCoder` and `RIOTLite` to evaluate them against established external dictionaries (`mental-health-datasets`).
    *   Map the findings into categorical constructs using `archetypes-boyd`.
*   **Goal**: Evolve the raw data into peer-reviewable categorical variables (e.g., turning a raw cluster of "stop/help/doctor" into the formal variable **Hard Directive**).

### Phase 5: The Deductive Synthesizer (Hypothesis Testing & Telemetry)
The pipeline completes the Bayesian update by formally testing the theoretical constructs against the telemetry.
*   **Action**:
    *   Run *a priori* K-Means clustering to statistically prove the constructs exist organically.
    *   Run regressions (e.g., Linear, Logistic) correlating the *User Task Context* and *User Internal Reaction* against the *Model Behavior* constructs.
    *   Generate SHAP values to explain the drivers of the correlation.
*   **Goal**: Produce definitive, statistically significant proof answering *Why* and *When* the LLM behavioral anomaly occurs, ready for publication.
