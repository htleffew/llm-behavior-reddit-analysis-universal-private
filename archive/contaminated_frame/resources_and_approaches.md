# Researcher Profile & Methodological Toolkit
**Principal Investigator:** Dr. Heather Leffew, Ph.D.
**Focus:** Computational AI Anthropology & LLM Behavioral Anomaly Research
**Domain:** Topic-Agnostic Reddit/Social Discourse Analysis

This document serves as the definitive guide to the scientific philosophy, methodological preferences, and analytical toolkit of Dr. Leffew. Any agent, researcher, or system operating in this repository must internalize this document to understand *why* certain tools are used, *how* they fit into the broader research paradigm, and the exact value they provide to computational social science.

---

## 1. Scientific Philosophy: The Abductive (Bayesian-Updating) Mixed-Methods Paradigm

Dr. Leffew’s research bridges a critical gap between two traditionally opposed fields: the **Bottom-Up Inductive Exploration** of modern Data Science, and the **Top-Down Deductive Rigor** of clinical psychology and traditional PhD research.

Historically, research forces a false binary:
- **Exploratory (Fishing Expeditions):** Throwing raw data into unsupervised models without theoretical grounding.
- **Confirmatory (Rigid Deductive):** Forcing data into pre-conceived theoretical buckets using brittle, hardcoded keyword lists.

**The Synthesis:** Dr. Leffew utilizes an **Abductive (Bayesian-Updating)** approach. 
When investigating novel, undocumented LLM anomalies (e.g., spontaneous pathologizing or system prompt exhaustion), a rigid top-down theory is impossible because the phenomenon hasn't been mapped yet. Therefore, the pipeline must:
1. **Establish a Prior**: Start with qualitative observations (e.g., reading user complaints).
2. **Inductive Exploration**: Use heavy data science techniques (unsupervised clustering, NLP) to organically surface the shape of the data. Let the clusters speak before naming them.
3. **Contextual Validation**: Ensure the organic clusters actually mean what they appear to mean in context.
4. **Deductive Testing**: Use mathematically validated psychometrics to formally test those organic clusters against the qualitative prior. 

This philosophy ensures that all final statistical claims are organically grounded in the data rather than artificially forced.

---

## 2. The Toolkit: Past Approaches & Internal Methods

Dr. Leffew’s analytical toolkit is derived from her formal dissertation work and her extensive portfolio of applied Data Science notebooks (e.g., *Juggernaut Content Moderation*, *TikTok Elections*, *Claude Sonnet Discourse Analysis*).

### From the Dissertation (Deductive Psychometrics & Hypothesis Testing)
In her PhD dissertation researching the linguistic markers of Instrumental vs. Affective perpetrators, Dr. Leffew established a foundation in rigorous psycholinguistics:
*   **LIWC (Linguistic Inquiry and Word Count):** Moving beyond basic sentiment analysis by utilizing validated summary variables (`Analytical`, `Clout`, `Authentic`, `Tone`) and pronoun differentials (e.g., first-person singular vs. plural) to define deep cognitive and affective states.
*   **Deductive K-Means Triangulation:** While Data Scientists often use K-Means *inductively* to guess how many clusters exist, Dr. Leffew uses it *deductively*. By defining $k$ a priori based on theoretical literature (e.g., $k=2$ for Affective vs. Instrumental), the unsupervised algorithm acts as a mathematical proof that the theoretical constructs naturally exist in the dataset.
*   **Frequentist Statistics:** Utilizing rigorous non-parametric tests (Mann-Whitney U), Chi-Square, and Regression models to prove statistically significant relationships.

### From the Data Science Portfolio (Inductive Exploration)
When operating on massive, uncharted social media corpora, Dr. Leffew employs robust unsupervised methods to map the terrain:
*   **EFA (Exploratory Factor Analysis) & PCA:** To discover underlying latent factors driving dataset variance before theory is applied.
*   **LDA (Latent Dirichlet Allocation) & BERTopic:** For organic, unsupervised thematic discovery.
*   **UMAP & HDBSCAN:** For non-linear dimensionality reduction and density-based spatial clustering to visualize massive text embeddings.
*   **NetworkX (Emotion Propagation):** Building conversational loop graphs (Parent-Child comment chains) to calculate how emotional states mathematically transfer across nodes.
*   **Time Series (ARIMA) & Anomaly Detection:** Isolating when and why discourse spikes relative to external events (like model updates).
*   **SHAP Explainers:** Opening the "black box" of regression/classification models to definitively prove *which* linguistic features drive specific user reactions.

---

## 3. External Repositories & Modern Tool Integrations

To operationalize the Abductive Pipeline on messy Reddit discourse without proprietary software constraints, this repository integrates a suite of advanced, open-source computational linguistics tools. 

*(Attribution: Many of these tools are heavily adapted from, or directly utilize, the open-source frameworks and methodologies developed by Dr. Ryan L. Boyd and associated computational linguistics communities).*

### A. The Structural Segmenter
*   **Tool:** `pleonasty` / `pleonasty-llm-turn-analysis`
*   **Purpose:** Resolving the ambiguity of Reddit storytelling.
*   **Usage:** Uses LLM batch processing to structurally isolate text into a **Dynamic Unit of Measurement** (`Model_Exact_Quote`, `Model_Paraphrase`, `User_Internal_Reaction`, `User_Task_Context`). This ensures that downstream analytics compare apples to apples, extracting precise model behavior distinct from user perception.

### B. The Inductive Explorer
*   **Tool:** `MEHv2` (Meaning Extraction Helper)
*   **Purpose:** Tweet-aware tokenization and root-word lemmatization.
*   **Usage:** Rather than manually hardcoding word variations (e.g., "spiral", "spiraling"), MEHv2 organically surfaces n-grams, lemmas, and linguistic clusters present in the corpus without pre-defining them.

### C. The Contextual Validator
*   **Tool:** `Contextualizer-keyword-incontext` (KWIC)
*   **Purpose:** Eliminating semantic blind spots.
*   **Usage:** Validates the organic indicators surfaced by MEHv2. For example, ensuring that the word "stop" is actually functioning as a *Hard Directive* from the AI, rather than a user saying "I can't stop crying."

### D. The Psychometric Anchors
*   **Tools:** `ContentCoder-Py-LIWCish`, `RIOTLite-dictbased`, `mental-health-datasets`
*   **Purpose:** Solving the **Single-Coder Methodological Problem**.
*   **Usage:** Replaces subjective, ad-hoc, hardcoded dictionary lists (which introduce extreme researcher bias) with established, empirically validated clinical dictionaries. This guarantees objective psychometric validity and peer-reviewable reproducibility.

### E. The Categorical Embeddings
*   **Tool:** `archetypes-boyd`
*   **Purpose:** Theory-driven categorical triangulation.
*   **Usage:** Uses `SentenceTransformers` to map the validated, Contextualized, and Dictionary-approved indicators into rigid categorical variables (e.g., *Pathologizing*, *Care Taking*). These Archetypes can then be regressively tested against user context to mathematically map the LLM's interaction dynamics.

---

## Conclusion
By treating Reddit comments, posts, and conversational threads as dynamic linguistic units, and by flowing data through this precise Abductive toolkit (Pleonasty $\rightarrow$ MEHv2 $\rightarrow$ Contextualizer $\rightarrow$ ContentCoder $\rightarrow$ Archetypes $\rightarrow$ K-Means/Regression), Dr. Leffew’s pipeline transforms anecdotal social media complaints into rigorous, statistically undeniable proofs of LLM behavioral anomalies.
