# Retrieval Evaluation

## Overview

This project is a **retrieval-based legal QA system**, not a generative model.

Instead of producing answers via real-time LLM generation, the system retrieves pre-validated answers from a structured QA knowledge base built from GDPR Articles 1–11.

Because of this design, traditional ML metrics such as accuracy, precision, or F1-score for classification tasks are not appropriate.

Instead, this evaluation focuses on:

- retrieval correctness
- legal alignment
- response safety
- system robustness under different query types

---

## Evaluation Setup

### Test Dataset

A total of **30 manually designed test questions** were used to evaluate system behavior under different scenarios.

The test set includes:

1. **Bilingual queries (EN / ZH)**
   - to test multilingual embedding performance

2. **Semantic / ambiguous queries**
   - different phrasings with similar meaning
   - to test semantic generalization

3. **Out-of-scope (non-GDPR) queries**
   - to test fallback behavior and hallucination prevention

This setup allows us to evaluate both retrieval quality and system safety.

---

## Evaluation Metrics

### 1. Top-1 Retrieval Accuracy (Similarity Hit Rate)

This metric evaluates whether the system retrieves the correct GDPR article at rank 1.

- based on cosine similarity between query embedding and chunk embeddings
- compares:
  - predicted article (Top-1)
  - ground truth article

If they match → **Hit**

This is equivalent to **precision@1** in information retrieval.

---

### 2. Correct Article Alignment

This metric evaluates whether the **final answer cites the correct legal article**.

Why this matters:

In legal systems, even a semantically reasonable answer is **incorrect if the legal basis is wrong**.

This ensures:

- legal correctness
- traceability of answers
- alignment with source regulations

---

### 3. Fallback Trigger Reasonableness

This evaluates whether the system correctly avoids answering when:

- the query is خارج knowledge scope
- insufficient supporting legal data exists

Instead of generating uncertain answers, the system:

- triggers a fallback response
- informs the user of scope limitations

This is critical for:

- hallucination prevention
- legal safety
- production reliability

---

### 4. LLM-based Answer Quality Evaluation (Gemini)

To further assess answer quality, all 30 QA pairs were evaluated using an external LLM (Gemini).

The model was prompted to evaluate:

- correctness of legal interpretation
- clarity of explanation
- consistency of structure (article + summary + conclusion)

This serves as a **secondary qualitative evaluation**, not the primary metric.

---

## Results

### Top-1 Retrieval Accuracy

- **Top-1 Accuracy: 83.3% (20 / 24 GDPR-related questions)**

This indicates that the system can correctly identify the relevant GDPR article in most cases.

The remaining questions are primarily:

- semantically ambiguous queries
- boundary cases near article scope limits

---

### Retrieval Visualization

The following chart shows hit/miss results for Top-1 retrieval:

![Top-1 Retrieval Results](../assets/retrieval_hit_miss.png)

---

### Key Observations

#### 1. Strong multilingual performance

The system successfully retrieves correct articles for both:

- English queries
- Chinese queries

Example:

- "Can company deal with special personal data?"
→ correctly retrieves **Article 9**

---

#### 2. Improved semantic robustness

After:

- structure-aware chunking
- QA pipeline validation

The system shows:

- reduced cross-article confusion
- better handling of paraphrased queries

---

#### 3. Effective hallucination prevention

For non-GDPR queries (e.g. trade compliance, NDA):

- the system **does NOT generate answers**
- fallback is triggered correctly

This demonstrates:

- strong safety control
- correct knowledge boundary enforcement

---

#### 4. Stable and predictable behavior

Across all 30 test cases:

- responses are consistent
- no random variation (unlike generative systems)
- answers remain traceable to source articles

---

## Gemini Evaluation Summary

Gemini evaluation results indicate:

- near-perfect legal citation accuracy
- strong structural consistency (Article + Summary + Conclusion)
- high readability and clarity

Additionally, Gemini confirms:

- correct identification of knowledge boundaries
- appropriate fallback behavior for out-of-scope queries

This supports the system’s design goal of being:

> **reliable, controllable, and auditable**

---

## Limitations

Despite strong performance, several limitations remain:

### 1. Limited knowledge scope

- only covers GDPR Articles 1–11
- cannot answer broader GDPR or related legal topics

---

### 2. Retrieval-only constraints

- cannot synthesize new knowledge
- depends entirely on pre-defined QA pairs

---

### 3. Semantic edge cases

- some ambiguous queries may retrieve adjacent but not optimal articles
- especially when legal concepts overlap

---

### 4. No ranking beyond Top-1 optimization

- system currently relies on Top-1 retrieval
- does not leverage Top-k reranking or hybrid retrieval

---

## Conclusion

The evaluation results demonstrate that:

- the system achieves strong retrieval accuracy
- maintains strict legal correctness
- effectively prevents hallucination
- behaves consistently across multilingual and ambiguous queries

Most importantly, the system validates the core design principle:

> **A controlled retrieval-based legal QA system can outperform generative systems in reliability, traceability, and safety.**
