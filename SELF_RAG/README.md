# 🧠 Self-RAG: Production-Ready RAG with Quality Gates

> **Self-Reflective Retrieval-Augmented Generation** — A RAG pipeline that *thinks before it answers* and *verifies before it returns*.

---

## 📋 The 4 Quality Checks

| # | Check | What it does | Why it matters |
|---|-------|--------------|----------------|
| **1** | **Retrieval Check** | Decides whether to fetch docs or answer from general knowledge | Avoids unnecessary retrieval; faster & cheaper for simple questions |
| **2** | **Relevance Check** | Filters retrieved chunks — keeps only what's relevant to the question | Reduces noise; improves answer quality |
| **3** | **Hallucination Check** | Verifies the answer is grounded in the retrieved context | Catches unsupported claims; triggers revision if needed |
| **4** | **Usefulness Check** | Judges if the answer actually addresses the user's question | Detects off-topic or generic answers; triggers query rewrite |

---

## ✨ Key Features

### 🛑 Infinite Loop Breaking
- **Hallucination loop**: Max 10 revision attempts → accept best effort
- **Usefulness loop**: Max 3 query rewrites → graceful fallback
- **Recursion limit**: Configurable step cap (e.g. 80) to prevent runaway graphs

### 🔍 Hallucination Check
- LLM verifies each claim against retrieved context
- Labels: *Fully Supportive* | *Partially Supportive* | *Non-Supportive*
- Auto-revise answer when not fully supported; cite evidence

### 🔄 Query Rewriting
- When answer is *not useful*, rewrite the retrieval query and retry
- Optimized for vector search over company PDFs

### 📊 Stateful Graph (LangGraph)
- Single `State` object flows through all nodes
- Conditional routing at each check
- Clear audit trail: retrieval → relevance → generation → hallucination → usefulness

---

## 📁 How This Repo Is Structured

Incremental build-up — each notebook adds one more check:

| Notebook | Checks included | Purpose |
|----------|-----------------|---------|
| `Retrieval_Check.ipynb` | 1 | Decide: retrieve or not? |
| `Relevance_Check.ipynb` | 1 + 2 | + Filter irrelevant chunks |
| `Hellucination_Check.ipynb` | 1 + 2 + 3 | + Verify grounding, revise if needed |
| **`Usefullness_check.ipynb`** | **1 + 2 + 3 + 4** | **Full Self-RAG** — all checks + usefulness + query rewrite |

---

## 🛠 Tech Stack

- **LangGraph** — StateGraph for orchestration
- **LangChain** — LLM, prompts, retrievers
- **FAISS** — Vector store (OpenAI embeddings)
- **OpenAI** — GPT-4o-mini for generation & structured outputs

---

## 🚀 Quick Start

1. Add `.env` with `OPENAI_API_KEY`
2. Place PDFs in this folder: `Company_Profile.pdf`, `Company_Policies.pdf`, `Product_and_Pricing.pdf`
3. Run `Usefullness_check.ipynb` top to bottom for the full pipeline

---

## 📌 Summary for Recruiters

- **Self-RAG** = RAG + 4 quality gates (retrieval, relevance, hallucination, usefulness)
- **Production-minded**: Loop limits, graceful fallbacks, evidence extraction
- **Modular**: Step-by-step notebooks show how each check was added
- **LangGraph**: Real graph-based orchestration, not ad-hoc scripts
