---
title: "Mini-Project #3 — Corporate & AI Strategy Copilot (RAG)"
author: "Eliezer Gonzalez"
course: "IPHS 391"
date: "November 16, 2025"
---

# Mini-Project #3 — Corporate & AI Strategy Copilot (RAG)

**Course:** IPHS 391  
**Student:** Eliezer Gonzalez  
**Date:** November 16, 2025  

---

## 1. Project Context & Use Case

### 1.1 Problem / Context

**Domain:** Corporate strategy and AI transformation.

Modern strategy teams inside mid-to-large companies (and consulting firms) are flooded with **vague, high-stakes questions** such as:

- “Growth is slowing — what should we do?”
- “Should we enter this adjacent market or customer segment?”
- “Where does AI actually make sense in our business, and how should we implement it?”

These questions are:

- **Ambiguous:** Poorly scoped, with unclear problem boundaries.  
- **Knowledge-intensive:** The “right” starting point depends on frameworks, industry patterns, and prior case experience.  
- **Time-sensitive:** Strategy teams must produce structured thinking quickly, even when they lack prior exposure to a specific industry or AI use case.

At the same time, there is a growing body of **consulting-style thought leadership** (McKinsey, BCG, Bain, HBR, vendor playbooks) describing:

- Strategy frameworks and best practices (market entry, growth, portfolio strategy, M&A, etc.).  
- AI adoption patterns (high-value use cases, operating models, governance, and risk).

However, this knowledge lives in **scattered PDFs and blogs**, is hard to search effectively, and is underused in day-to-day strategy work.

> **Problem:** Strategy teams lack a fast, systematic way to turn messy questions into **structured, framework-driven strategy memos**, especially when the question involves **AI adoption**.

---

### 1.2 Primary Use Case

**Primary Use Case:**  
A Retrieval-Augmented Generation (RAG) Copilot that helps strategy teams transform ambiguous questions into **structured, best-practice-informed strategy memos**, with a special mode for **AI strategy & implementation**.

The system supports two tightly connected modes that share the same RAG backend:

#### Mode A — General Strategy Question Mode

**Example prompt**

> “We’re a mid-size B2B SaaS company in North America; growth has slowed over the last 2 years. What strategic angles should we explore?”

**Expected Copilot output**

A short, structured “strategy memo” including:

- 2–3 plausible **problem framings**  
  - e.g., market expansion vs product expansion vs pricing vs retention.  
- Relevant **frameworks** matched to each framing  
  - e.g., Ansoff matrix, retention funnel, pricing waterfall.  
- A rough **analysis plan**  
  - What data to collect, what diagnostics to run.  
- Key **risks / limitations**  
  - E.g., data gaps, external macro trends, organizational constraints.

#### Mode B — AI Strategy & Implementation Mode

**Example prompt**

> “We are a regional retail chain with ~200 stores. Where should we prioritize AI adoption, and how should we structure our AI initiative?”

**Expected Copilot output**

A structured AI strategy note including:

- Priority **AI use cases** across the value chain  
  - e.g., demand forecasting, inventory optimization, personalization, labor scheduling.  
- A phased **AI roadmap**  
  - 0–6 month pilots, 6–18 month scale-up, 18+ month institutionalization.  
- Suggested **operating model**  
  - Central AI Center of Excellence vs federated squads vs hybrid.  
- **Risk & governance checklist**  
  - Data quality, privacy, bias, workforce impact, regulatory compliance.

---

### 1.3 Stakeholders & Success Metrics

**Primary stakeholders**

- Corporate Strategy / Corporate Development teams.  
- Business unit strategy leads and internal consultants.  
- Chief Strategy, Chief Digital, and Chief AI Officers (and their direct teams).  
- *(Secondary)* Students in strategy/AI courses using the tool to learn frameworks.

**What “success” looks like**

- Strategy teams get to a **structured first-pass memo** faster (minutes instead of hours).  
- Memos **use appropriate frameworks** for the context (not just generic lists).  
- AI strategy outputs **cover multiple value-chain areas** and explicitly address risk & governance.  
- Users feel the tool is a **useful thinking partner**, not a replacement for their judgment.

**High-level success metrics**

```yaml
success_metrics:
  - name: "Time-to-structured-draft"
    description: "Average time to get a structured strategy memo from a raw question"
    target: "≤ 5 minutes per query"

  - name: "Framework-fit-score"
    description: "Human rating (1–5) of how appropriate the suggested frameworks are"
    target: "≥ 4.0 on average"

  - name: "Actionability-score"
    description: "Human rating (1–5) of how actionable the recommended next steps are"
    target: "≥ 4.0 on average"

  - name: "AI-risk-awareness-score"
    description: "For AI prompts, inclusion of risk & governance considerations"
    target: "Mentioned in ≥ 80% of AI strategy responses"
```

---

## 2. Data & Constraints

### 2.1 Corpus / Data Sources

The RAG knowledge base focuses on **consulting-grade strategy and AI content** that is public or permissibly accessible.

**Planned corpus structure**

1. **General Corporate Strategy Content**

   - Articles and reports from:
     - McKinsey Quarterly  
     - BCG Insights  
     - Bain / LEK insights  
     - Harvard Business Review (HBR)
   - Topics:
     - Growth strategy (organic vs inorganic).  
     - Market entry and geographic expansion.  
     - Portfolio strategy and M&A.  
     - Pricing, product strategy, and customer segmentation.  
     - Corporate governance and organizational design.

2. **AI Strategy & Implementation Content**

   - AI strategy whitepapers and blog series from:
     - Major management consultancies.  
     - Cloud / AI vendors (e.g., responsible AI, AI operating model).  
   - Topics:
     - AI use-case “maps” by sector (retail, banking, manufacturing, etc.).  
     - AI operating models (CoE vs hub-and-spoke vs embedded).  
     - AI risk, governance, and responsible AI guidelines.

3. **Framework Explainability & “How-To” Guides**

   - Short explainers of:
     - Porter’s Five Forces  
     - 3Cs  
     - Ansoff Matrix  
     - BCG Growth-Share Matrix  
     - Value Chain Analysis  
     - Customer Lifetime Value (CLV)  
   - Examples of how these frameworks were applied in real cases.

---

### 2.2 Preprocessing & Metadata

To support high-quality retrieval, documents are normalized and enriched with metadata.

**Preprocessing steps**

1. **Ingestion**
   - Download PDFs and HTML pages.  
   - Convert to UTF-8 text.  
   - Remove boilerplate (navigation, cookie banners, footers, unrelated sidebars).

2. **Chunking**
   - Chunk by **structural boundaries** (headings, subheadings, bullet sections).  
   - Target chunk size: ~400–800 tokens each.  
   - Preserve headings inside each chunk to retain semantic context.

3. **Metadata**

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
      example: "pricing"   # or "market_entry", "ai_use_cases", etc.

    - name: "industry"
      example: "retail"    # optional; "cross-industry" if generic

    - name: "mode_relevance"
      example: ["strategy", "ai_strategy"]  # used to route to appropriate mode
```

4. **Indexing Preparation**
   - Normalize text (e.g., lowercasing where appropriate, but preserve proper nouns for display).  
   - Remove duplicates and near-duplicates.  
   - Store per-chunk metadata alongside embeddings in the vector index.

---

### 2.3 Constraints & Risks in the Data

**Key constraints**

- **Bias toward large, developed-market corporations**
  - Most consulting content is written for large enterprises in North America/Europe.  
  - SMEs and emerging-market contexts may be underrepresented.

- **Marketing and “halo effect”**
  - Some documents have a marketing tone.  
  - Case studies may overemphasize success stories and underreport failures.

- **Recency and fast-moving AI landscape**
  - AI recommendations can become outdated quickly.  
  - Need to consider document **year** in retrieval and synthesis.

- **Licensing & usage**
  - Only use content that can be legally scraped or accessed for educational purposes.  
  - Avoid proprietary internal decks.

```yaml
data_constraints:
  - "Public or permissibly accessible strategy/AI reports only"
  - "Likely bias toward large enterprises and Global North"
  - "AI content may age quickly; year-based filtering recommended"
  - "No ingestion of confidential company information"
```

---

## 3. RAG Architecture (MVP)

### 3.1 High-Level Pipeline

**Overview**

> User Question → Mode Router → Query Expansion → Hybrid Retrieval →  Reranker → LLM Synthesis → Structured Strategy Memo

**Detailed steps**

1. **User Input**
   - User provides:
     - Short free-text question.  
     - Optional structured context: industry, company size, geography.

2. **Mode Routing**
   - Simple heuristic or lightweight classifier:
     - If the query mentions “AI”, “machine learning”, “LLM”, “data science”, etc. → **AI Strategy Mode**.  
     - Otherwise → **General Strategy Mode**.
   - The selected mode influences:
     - Which documents are prioritized (via `mode_relevance`).  
     - Which output template is used.

3. **Query Construction**
   - Expand the query with inferred keywords based on mode and industry:
     - General strategy: “growth strategy”, “market entry”, “pricing”, “retention”.  
     - AI strategy: “AI use cases”, “operating model”, “AI governance”, “AI roadmap”.  
   - Build a retrieval query that combines:
     - Free-text query.  
     - Metadata filters (topic, industry, year range, mode).

4. **Hybrid Retrieval**
   - Use both:
     - **Sparse (BM25)** keyword search.  
     - **Dense (vector) search** via embeddings.
   - Combine top results from each:
     - Union, then score-normalized and sorted.  
     - Or weighted combination of sparse and dense scores.

5. **Reranking**
   - Rerank up to top-`k` retrieved chunks using a cross-encoder or reranker model.  
   - Prioritize:
     - Direct semantic relevance to question and context.  
     - Matching industry and recency when applicable.

6. **LLM Synthesis**
   - Pass to the LLM:
     - User question + context.  
     - Top retrieved chunks (with metadata).  
     - A **mode-specific prompt template**:
       - Strategy Mode: “Write a strategy memo with problem framings, frameworks, next analyses, risks.”  
       - AI Mode: “Write an AI roadmap with use cases, operating model, and risk/governance.”

7. **Structured Output Formatting**
   - Ensure consistent structure:
     - **Strategy Mode:** Overview, Framing Options, Recommended Frameworks, Next Steps, Risks & Limitations.  
     - **AI Mode:** Overview, Priority Use Cases, Roadmap, Operating Model, Risks & Governance.

```yaml
pipeline:
  steps:
    - "mode_routing"
    - "query_expansion"
    - "hybrid_retrieval"
    - "reranking"
    - "llm_synthesis"
    - "structured_output_formatting"
```

---

### 3.2 Tools & Components

| Component    | Proposed Choice                   | Rationale                                           |
|-------------|------------------------------------|-----------------------------------------------------|
| Framework   | **LangChain**                      | Popular RAG abstractions; quick to prototype        |
| Vector Store| **FAISS (in-memory index)**        | Fast local similarity search                        |
| Sparse Search| **BM25** (e.g., `rank_bm25`)      | Strong keyword baseline for jargon/framework names  |
| Embeddings  | **OpenAI `text-embedding-3-large`**| High-quality semantic embeddings across domains     |
| Reranker    | **Cohere ReRank** (or similar)     | Boosts precision at top-k with minimal setup        |
| LLM         | **GPT-4.1 / GPT-4o**               | Strong reasoning and formatting capabilities        |
| Orchestration| Python script / notebook          | Sufficient for Mini-Project 3 demo                  |

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

## 4. Component Alternatives (Mini-Bakeoff)

### 4.1 Candidate Components & Trade-offs

| Component  | Option A                        | Option B                   | Criteria                                   | Selected (MVP) | Reason                                            |
|-----------|----------------------------------|----------------------------|--------------------------------------------|----------------|---------------------------------------------------|
| Framework | LangChain                        | LlamaIndex                 | Learning curve, docs, ecosystem            | LangChain      | Familiar, good docs, widely used for RAG          |
| Vector DB | FAISS                            | Chroma                     | Local performance, simplicity              | FAISS          | Fast, lightweight, no external service            |
| Embeds    | OpenAI `text-embedding-3-large`  | Instructor-XL (HF)         | Cost, quality, domain adaptability         | OpenAI         | Strong semantic performance out-of-the-box        |
| Reranker  | Cohere ReRank                    | BGE-reranker-large (local) | Quality vs latency, API vs local loading   | Cohere ReRank  | High quality with minimal engineering             |
| LLM       | GPT-4.1 / GPT-4o                | Local 7B model             | Reasoning, reliability, GPU requirements   | GPT-4.1        | Better for complex strategy reasoning             |
| UI        | CLI / Notebook                   | Streamlit web app          | Time-to-demo vs UI polish                  | CLI/Notebook   | Sufficient for mini-project; can extend later     |

```yaml
component_selection:
  framework:
    options: ["LangChain", "LlamaIndex"]
    selected: "LangChain"
    reason: "Simplest path to hybrid retrieval and chains for a small project"

  vector_store:
    options: ["FAISS", "Chroma"]
    selected: "FAISS"
    reason: "Fast local search; no external dependencies"

  embeddings:
    options: ["OpenAI text-embedding-3-large", "Instructor-XL (local)"]
    selected: "OpenAI text-embedding-3-large"
    reason: "Higher semantic coverage across diverse strategy topics"

  reranker:
    options: ["Cohere ReRank", "BGE-reranker-large (local)"]
    selected: "Cohere ReRank"
    reason: "Balanced precision and simplicity for an academic project"

  llm:
    options: ["GPT-4.1", "Local 7B model"]
    selected: "GPT-4.1"
    reason: "Better reasoning for multi-step strategy and AI roadmaps"
```

---

## 5. Evaluation Plan & Expected Results

### 5.1 Test Set Design

Create a small test suite of ~15–20 questions, covering:

1. **General Strategy (≈10 questions)**

   Examples:
   - Growth slowdown in B2B SaaS.  
   - Market entry in an emerging market.  
   - Portfolio rationalization (divest vs double-down).  
   - Pricing strategy for a new premium product.  
   - Customer retention improvement in a mature market.

2. **AI Strategy & Implementation (≈5–10 questions)**

   Examples:
   - AI priorities for a regional retail chain.  
   - AI roadmap for a mid-size manufacturer.  
   - Implementing AI in customer service for a bank.  
   - Structuring an AI Center of Excellence.  
   - Responsible AI practices for an HR tech platform.

Each question will have:

- A short **context description** (industry, size, constraints).  
- A “reference” answer (manually created or adapted from real reports) for qualitative comparison.

---

### 5.2 Metrics

**Qualitative (human or LLM-assisted grading)**

- **Relevance (1–5)**  
  Does the memo address the actual question and context?

- **Framework Fit (1–5)**  
  Are the suggested frameworks appropriate and non-trivial for the situation?

- **Actionability (1–5)**  
  Are the recommended next steps concrete and realistic?

- **AI Risk Awareness (1–5, AI mode only)**  
  Does the answer explicitly mention governance, bias, workforce impact, and other risks?

**Quantitative**

- **Citation Support Rate**  
  Percentage of key claims in the memo that can be traced back to retrieved sources.

- **Mode Routing Accuracy**  
  Percentage of AI-related questions correctly routed to the AI Strategy Mode.

- **Retrieval Precision@k**  
  For a subset, fraction of top-k chunks judged “highly relevant.”

```yaml
evaluation_plan:
  test_questions: 18
  metrics:
    - "relevance_score_1_5"
    - "framework_fit_score_1_5"
    - "actionability_score_1_5"
    - "ai_risk_awareness_score_1_5"
    - "citation_support_rate"
    - "mode_routing_accuracy"
```

---

### 5.3 Expected Results

**Hypotheses**

- The Copilot will produce **structured, reasonably framework-aware memos** for most general strategy questions (average ≥ 4.0 on relevance and actionability).  
- AI Strategy Mode will:
  - Surface **multiple value-chain use cases** (not only chatbots and generic “automation”).  
  - Mention risk & governance in **≥ 80%** of AI-related answers.  
- Hybrid retrieval + reranking will outperform simple vector search in:
  - Perceived relevance of retrieved chunks.  
  - Quality of synthesized memos, particularly for jargon-heavy industries.

---

## 6. Future Work & Improvements

### 6.1 Company-Specific Knowledge

- Add internal reports, strategy decks, and performance data for a particular (hypothetical or real) company.  
- Allow the Copilot to combine **public best practices** with **firm-specific constraints and objectives**.

### 6.2 Interactive Drill-Down

- Support follow-up questions that:
  - Refine one specific framing.  
  - Explore a single use case in the AI roadmap.  
- Maintain conversational state so the user can iteratively refine the strategy.

### 6.3 Structured Data Integration

- Pull basic financial or market data (e.g., from APIs or CSVs) to:
  - Quantify growth, margins, and market share.  
  - Inform which strategic levers matter most.

### 6.4 More Robust Evaluation

- Introduce automated evaluation with **LLM-as-judge** for larger test sets.  
- Compare against a pure-LLM baseline:
  - “Answer without any external documents” vs “Answer with RAG.”

### 6.5 Ethics & Governance Layer

- Add lightweight guardrails that:
  - Avoid recommending strategies that are clearly unethical or illegal.  
  - Flag when data is too old or too biased to support strong AI recommendations.  
- Provide disclaimers emphasizing:
  - Human accountability.  
  - Need for local legal and cultural review.

```yaml
improvements:
  - "Add company-specific corpora for more tailored recommendations"
  - "Support interactive drill-down and follow-up questioning"
  - "Integrate simple financial KPIs for more grounded strategies"
  - "Run a larger-scale evaluation with LLM-as-judge metrics"
  - "Add governance guardrails for obviously harmful recommendations"
```

---

## 7. References

> **Note:** These are example references to illustrate the type of sources that would be used. In the actual project, each reference will be replaced with concrete URLs / DOIs and properly formatted citations.

1. **Retrieval-Augmented Generation**
   - Lewis, P. et al. *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*.
   ---https://arxiv.org/abs/2005.11401

2. **Strategy Frameworks & Thought Leadership**
   - McKinsey & Company. *The Ten Rules of Growth*.  
   ---https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/the-ten-rules-of-growth#/
   - Boston Consulting Group (BCG). *The Growth Share Matrix*.  
   ---https://www.bcg.com/about/overview/our-history/growth-share-matrix
   - Harvard Business Review. Selected articles on growth, market entry, and corporate strategy.
   ---Porter, Michael E. "The Five Competitive Forces That Shape Strategy." Special Issue on HBS Centennial. Harvard Business Review 86, no. 1 (January 2008): 78–93.

3. **AI Strategy & Transformation**
   - McKinsey & Company. *The State of AI in 2025*.  
   ---https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai
   - BCG. *How to prepare for an AI first future*.  
   ---https://www.bcg.com/publications/2025/how-companies-can-prepare-for-ai-first-future
   

4. **RAG & Tooling**
   - LangChain Documentation — Retrieval and VectorStore modules.  
   ---https://docs.langchain.com/oss/javascript/langchain/retrieval
   ---https://docs.langchain.com/oss/python/integrations/vectorstores
   - FAISS Documentation 
   ---https://docs.langchain.com/oss/python/integrations/vectorstores/faiss

```yaml
references:
  - "Lewis, P. et al. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks."
  - " McKinsey & Company. The Ten Rules of Growth. "
  - "Boston Consulting Group (BCG). The Growth Share Matrix."
  - "Porter, Michael E. "The Five Competitive Forces That Shape Strategy." Special Issue on HBS Centennial. Harvard Business Review 86, no. 1 (January 2008): 78–93."
  - "LangChain and FAISS documentation for RAG implementation details."
```
