<div align="center">

# 📊 Corporate & AI Strategy Copilot (RAG)

### *A RAG-powered assistant for structuring ambiguous corporate strategy questions and designing AI adoption roadmaps*

[![RAG](https://img.shields.io/badge/RAG-Retrieval%20Augmented%20Generation-orange.svg)]()
[![Domain](https://img.shields.io/badge/Domain-Corporate%20Strategy%20%26%20AI-blue.svg)]()
[![Course](https://img.shields.io/badge/Course-IPHS%20391-lightgrey.svg)]()

[Overview](#-overview) • [Use-Cases](#-core-use-cases) • [Knowledge-Base](#-data--knowledge-base) • [Components](#-component-selection) • [Evaluation](#-evaluation-plan) • [Future-Work](#-future-work) • [Author](#-author--course)

---

</div>

## 📋 Overview

**Corporate & AI Strategy Copilot** is a Retrieval-Augmented Generation (RAG) system concept designed for **Mini-Project #3** in **IPHS 391**.

The goal is to help **corporate strategy teams** and **AI leaders** turn messy, under-specified questions into **clear, structured, framework-driven strategy memos**, especially when the question involves **AI adoption**.

The Copilot acts as a **thinking partner** that:

- Retrieves **best practices** from consulting thought leadership (McKinsey, BCG, Bain, HBR, vendor reports).
- Applies **classic strategy and AI frameworks**.
- Outputs **structured memos** that frame problems, suggest options, and highlight risks.

---

## 🧩 Core Use Cases

The Copilot supports two tightly connected modes, backed by a shared RAG pipeline.

### 1️⃣ General Strategy Question Mode

**Example prompt**

> “We’re a mid-size B2B SaaS company in North America; growth has slowed over the last 2 years. What strategic angles should we explore?”

**What the Copilot returns**

A **short strategy memo** including:

- **Problem framings**  
  - e.g., *market expansion vs product expansion vs pricing vs retention*.
- **Relevant frameworks**  
  - Ansoff Matrix, retention funnel, pricing waterfall, customer segmentation.
- **Analysis plan**  
  - What data to collect, what diagnostics to run, which segments to examine.
- **Key risks / limitations**  
  - Data gaps, macroeconomic uncertainty, organizational constraints.

---

### 2️⃣ AI Strategy & Implementation Mode

**Example prompt**

> “We are a regional retail chain with ~200 stores. Where should we prioritize AI adoption, and how should we structure our AI initiative?”

**What the Copilot returns**

A **structured AI strategy note** including:

- **Priority AI use cases** across the value chain  
  - E.g., demand forecasting, inventory optimization, dynamic pricing, personalization, workforce scheduling.
- **Phased AI roadmap**  
  - 0–6 month pilots → 6–18 month scale-up → 18+ month institutionalization.
- **Operating model recommendations**  
  - Central AI Center of Excellence vs embedded squads vs hybrid.
- **Risk & governance checklist**  
  - Data quality, privacy, bias, workforce impact, compliance, vendor lock-in.

---

---

## 📚 Data & Knowledge Base

### 🧠 Strategy & AI Corpus

The RAG system is powered by a curated knowledge base of **public or permissibly accessible** content:

1. **Corporate Strategy Content**
   - McKinsey Quarterly, BCG Insights, Bain / LEK insights, HBR
   - Topics:
     - Growth strategy (organic & inorganic)  
     - Market entry & geographic expansion  
     - Portfolio strategy & M&A  
     - Pricing, product strategy, customer segmentation  
     - Corporate governance & organizational design  

2. **AI Strategy & Transformation Content**
   - AI strategy whitepapers and blog series from major consultancies
   - Cloud / AI vendor reports on:
     - AI use-case “maps” by sector  
     - AI operating models (CoE, hub-and-spoke, embedded)  
     - AI risk, governance & responsible AI  

3. **Framework Explainability & “How-To” Content**
   - Short explainers and worked examples for:
     - Porter’s Five Forces, 3Cs, Ansoff, BCG Matrix  
     - Value Chain Analysis, CLV, retention funnels, growth diagnostics  

### 🏷️ Preprocessing & Metadata

Documents are:

- Converted from PDF/HTML → text  
- Chunked by **headings / sections** (~400–800 tokens)  
- Enriched with metadata for more precise retrieval:

```yaml
corpus_schema:
  fields:
    - name: "source"
      example: "McKinsey Quarterly"

    - name: "doc_title"
      example: "The New Rules of Growth"

    - name: "year"
      example: 2024

    - name: "topic"
      example: "growth_strategy"

    - name: "subtopic"
      example: "pricing"

    - name: "industry"
      example: "retail"   # "cross-industry" if generic

    - name: "mode_relevance"
      example: ["strategy", "ai_strategy"]
```

### ⚠️ Data Constraints & Biases

```yaml
data_constraints:
  - "Public or permissibly accessible strategy/AI reports only"
  - "Bias toward large, Global-North enterprises"
  - "AI content ages quickly; year-based filtering recommended"
  - "No ingestion of confidential company information"
  - "Some case studies over-index on success stories (survivor bias)"
```

---

## 🏗️ System Architecture

### 🔄 RAG Pipeline Overview

> **User Question → Mode Router → Query Expansion → Hybrid Retrieval → (Optional Reranker) → LLM Synthesis → Structured Strategy Memo**

---

## 🧱 Component Selection

| Component     | Choice                         | Alternative              | Why Selected                                               |
|--------------|---------------------------------|--------------------------|------------------------------------------------------------|
| Framework    | **LangChain**                  | LlamaIndex               | Mature RAG abstractions, simple for small projects        |
| Vector Store | **FAISS**                      | Chroma                   | Fast local similarity search, no external dependency      |
| Sparse Search| **BM25 (`rank_bm25`)**         | ElasticSearch            | Lightweight, good keyword baseline                        |
| Embeddings   | **OpenAI `text-embedding-3-large`** | Instructor-XL (local) | Strong semantic coverage, minimal tuning needed           |
| Reranker     | **Cohere ReRank**              | BGE-reranker (local)     | High precision@k with simple API integration              |
| LLM          | **GPT-4.1 / GPT-4o**           | Local 7B model           | Better for multi-step reasoning & structured memos        |
| UI           | CLI / Notebook                 | Streamlit web UI         | Enough for Mini-Project demo; UI can be added later       |

```yaml
architecture:
  retrieval:
    type: "hybrid"
    dense_index: "FAISS"
    sparse_index: "BM25"
    top_k_initial: 20
    top_k_reranked: 8

  embeddings:
    model: "text-embedding-3-large"
    dim: 3072

  llm:
    model: "gpt-4.1"
    temperature: 0.2

  modes:
    - "strategy"
    - "ai_strategy"
```

---

## 📊 Evaluation Plan

### 📐 Metrics

- **Relevance (1–5)** – Does the memo address the actual question and context?  
- **Framework Fit (1–5)** – Are suggested frameworks sensible and non-trivial?  
- **Actionability (1–5)** – Are next steps concrete and realistic?  
- **AI Risk Awareness (1–5, AI mode)** – Are governance, bias, and workforce impact addressed?  
- **Citation Support Rate** – % of key claims grounded in retrieved chunks  
- **Mode Routing Accuracy** – % of AI-related questions correctly routed  

---


## 🔮 Future Work

Key extensions beyond the MVP design:

- **Company-Specific Knowledge, interactive drill-down, governance guardrails**  

---

## 👤 Author & Course

**Student:** Eliezer Gonzalez  
**Instructor:** Prof. Jon Chun  
**Course:** IPHS 391 — AI Mini-Project Series (Composable AI Project Blueprint)  
**Mini-Project:** #3 — Real-World RAG Implementation  

---

## 🙏 Acknowledgments

- **Prof. Jon Chun & IPHS 391** for the Composable AI Project Blueprint (CAPB) framework.  
- **Consulting and AI research communities** for public strategy & AI reports.  
- **LLM tooling ecosystem** (LangChain, FAISS, OpenAI, Cohere, etc.) that makes practical RAG systems possible.

---
