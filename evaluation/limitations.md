# Limitations

## Overview

This project is designed as a **controlled, retrieval-based legal QA system**, with a strong emphasis on:

- correctness  
- traceability  
- safety  

While this design achieves high reliability within its intended scope, it also introduces several limitations.

Importantly, many of these limitations are **intentional design trade-offs**, rather than implementation shortcomings.

---

## 1. Limited Knowledge Scope

The system currently covers only:

- **GDPR Articles 1–11**

As a result:

- it cannot answer questions related to:
  - later GDPR provisions (e.g., Articles 12–99)
  - broader regulatory or legal domains

Instead of attempting to answer beyond its scope, the system enforces **knowledge boundary control**, explicitly indicating that the question is outside the supported range.

### Design implication

This limitation is directly related to the system’s safety goal:

> preventing unsupported legal interpretation is prioritized over expanding coverage.

---

## 2. Retrieval-Only Architecture (No Generative Reasoning)

The system does not perform real-time LLM-based answer generation.

Instead:

- answers are retrieved from a **pre-validated QA dataset**

This leads to the following limitations:

- cannot synthesize new legal reasoning  
- cannot generalize beyond existing QA pairs  
- cannot dynamically compose answers for unseen scenarios  

### Trade-off

| Design Choice | Benefit | Limitation |
|-------------|--------|-----------|
| Retrieval-only | High reliability, no hallucination | Limited flexibility |
| Generative LLM | Flexible reasoning | Risk of hallucination |

This project explicitly prioritizes:

> **correctness over flexibility**

---

## 3. Dependence on QA Coverage

System performance depends heavily on:

- the coverage of the QA dataset  
- the diversity of question formulations included during offline validation  

Limitations include:

- unseen or rare phrasings may reduce retrieval accuracy  
- incomplete QA coverage may lead to suboptimal matching  

This is reflected in the evaluation results, where some semantically ambiguous queries did not achieve Top-1 retrieval success.

---

## 4. Semantic Retrieval Edge Cases

Despite using semantic embeddings, certain challenges remain:

- overlapping legal concepts across articles  
- underspecified or context-dependent queries  
- ambiguity in user intent  

For example:

- a question like “Is consent required?” may relate to multiple GDPR articles depending on context  

This can lead to:

- near-correct retrieval  
- but not always optimal Top-1 matching  

---

## 5. Top-1 Retrieval Constraint

The system currently returns only:

- the **Top-1 retrieved result**

Limitations:

- does not consider Top-k candidates  
- does not apply reranking strategies  
- may miss better matches in lower-ranked results  

This constraint simplifies system behavior but limits retrieval optimization potential.

---

## 6. Manual QA Validation Bottleneck

The QA construction process includes:

- LLM-assisted validation  
- manual review and refinement  
- bilingual consistency checks  

While this ensures high-quality outputs, it introduces:

- scalability limitations  
- higher time cost for expanding the dataset  

This makes large-scale coverage more resource-intensive.

---

## 7. No Real-Time Adaptation

The system does not:

- learn from user interactions  
- update its knowledge dynamically  

All improvements require:

- offline pipeline updates  
- regeneration of embeddings and QA data  

This limits adaptability in rapidly evolving legal or business contexts.

---

## 8. Evaluation Constraints

The current evaluation setup has several limitations:

- only **30 manually designed test questions**  
- not a large-scale benchmark dataset  
- partially relies on LLM-based qualitative evaluation  

However, the evaluation is still meaningful because it:

- covers multilingual queries  
- includes semantic variations  
- explicitly tests fallback and knowledge boundary scenarios  

---

## 9. Language Coverage Limitations

The system currently supports:

- English  
- Chinese  

Limitations:

- does not support other EU languages  
- potential nuance loss in cross-language legal interpretation  

---

## 10. Controlled Knowledge Boundary (Intentional Constraint)

One of the most important design characteristics of this system is:

> it enforces strict knowledge boundaries.

This leads to a key limitation:

- the system will **refuse to answer** certain legally valid questions  
- even if they are within GDPR, but outside Articles 1–11  

From a user perspective, this may appear as reduced capability.

However, from a system design perspective, this is intentional.

### Why this matters

Without boundary control, the system might:

- generate incomplete legal interpretations  
- extrapolate beyond validated knowledge  
- produce misleading answers  

By enforcing strict boundaries, the system ensures:

- no hallucinated legal claims  
- full traceability  
- predictable behavior  

This aligns with the evaluation findings, where the system successfully distinguishes between:

- irrelevant queries → fallback  
- out-of-scope legal queries → boundary-aware response  

---

## Future Improvements

Potential improvements to address current limitations include:

- expanding coverage to full GDPR (Articles 1–99)  
- introducing hybrid retrieval (Top-k + reranking)  
- adding query rewriting or normalization  
- semi-automating QA generation with human-in-the-loop validation  
- building larger evaluation datasets  
- extending multilingual support  

---

## Final Reflection

These limitations highlight a central design philosophy:

> In legal AI systems, knowing when not to answer is as important as answering correctly.

This project demonstrates that:

- a **controlled retrieval-based system**
- with explicit knowledge boundaries
- and offline-validated answers  

can provide a more reliable and safer alternative to fully generative approaches in legal and compliance contexts.
