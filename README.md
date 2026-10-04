# 1174976-COMP662A2

# COMP662 Assignment 2
## Retrieval-Augmented Generation for Natural Hazard Planning

### Student
1174976

---

# Overview

This project implements a Retrieval-Augmented Generation (RAG) system for natural hazard planning questions using the COMP662 Assignment 2 policy corpus.

The system retrieves relevant planning clauses from a collection of national, regional and district planning documents and uses a large language model to generate evidence-based responses.

The solution was developed in five stages:

1. Corpus Understanding and Indexing
2. Baseline RAG System
3. Retrieval Enhancement and Evaluation
4. Prompt Engineering
5. End-to-End Evaluation and Critical Analysis

---

# Corpus

The corpus contains four planning documents:

- National Policy Statement for Natural Hazards 2025
- National Policy Statement Implementation Guide 2026
- Canterbury Regional Policy Statement Chapter 11
- Selwyn District Natural Hazards Chapter

Documents were processed using clause-level chunking. Each clause was stored together with:

- document_id
- clause_id
- authority_level
- document_status

Clause-level chunking was selected because the provided relevance labels are already defined at the clause level.

---

# Retrieval System

## Embedding Model

python
BAAI/bge-small-en-v1.5


## Vector Database

python
FAISS


## Similarity Measure

Cosine similarity using normalized embeddings and inner-product search.

## Retrieval Depth

python
top_k = 5


---

# Generation Model

python
Qwen/Qwen2.5-1.5B-Instruct


Generation settings:

python
max_new_tokens = 512
do_sample = False


Deterministic decoding was used to ensure consistent outputs during evaluation.

---

# Baseline System

The baseline system consists of:

text
Dense Retrieval (BGE + FAISS)
+
Prompt P1


Prompt P1:

text
Context:
{context}

Question:
{question}

Answer:

---

# Retrieval Evaluation

Evaluation was performed using:

- development_questions.xlsx
- gold_relevance.csv

Metrics:

- Recall@5
- Mean Reciprocal Rank (MRR)

## Baseline Results

| Metric | Score |
|----------|----------|
| Recall@5 | 0.519 |
| MRR | 0.488 |

The baseline dense retriever performed reasonably well on direct policy questions but struggled with some multi-document and hazard-specific queries.

---

# Retrieval Enhancement

A BM25 retriever was implemented and compared with dense retrieval.

Implementation:

python
BM25Okapi


Results showed that BM25 could retrieve some hazard-specific clauses that dense retrieval missed, particularly where exact terms such as:

- flood
- flooding
- inundation

were important.

However, dense retrieval remained the primary retrieval method used in the final system.

---

# Prompt Engineering

Three prompt designs were evaluated.

## P1 — Baseline

Simple question and context prompt.

## P2 — Structured Prompt

Introduced:

- Relevant Policy Evidence
- Hazards
- Missing Information
- Preliminary Assessment
- Limitations

## P3 — Final Prompt

Introduced:

- Explicit grounding
- Missing information handling
- Authority awareness
- Limitation 