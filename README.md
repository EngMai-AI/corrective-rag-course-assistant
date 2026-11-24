# Corrective RAG: Course Knowledge Assistant

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1B0u8IXGbR5Vq_zbJ87Qixk0eaOUzoZDc?usp=sharing) **Corrective RAG notebook**
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1o4KO_M9vEIA5HhpwZoZMU_4sOWzLJRGR?usp=sharing) **Agentic RAG notebook (extension)**

A Retrieval-Augmented Generation system that **checks its own retrieval** before answering. If the retrieved context is weak, it rewrites the query and searches again. If nothing relevant exists, it says so instead of guessing.

## Pipeline

```
Documents → extract → clean → split → embed → Chroma
Question → retrieve top-k → GRADE each chunk (relevant / ambiguous / irrelevant)
        ├─ relevant chunks found → answer from those chunks only, with sources
        └─ none relevant → rewrite query → re-retrieve (max 2 rewrites)
                         └─ still nothing → "I don't have any background on this question"
```

## Features
- Ingestion of PDF, DOCX, PPTX, TXT and CSV (upload new files from the UI)
- Multilingual retrieval (English and Arabic) with `intfloat/multilingual-e5-small`
- LLM chunk grader (via Groq) that judges meaning, not keyword overlap
- Query rewriting and re-retrieval when context is weak
- Answers generated **only** from verified chunks, cited as `[1] [2]` with file and page/slide
- Unanswerable questions are declined, in the language of the question
- Gradio interface; LLM response cache and automatic model fallback on rate limits

## Notebooks
| Notebook | What it is |
|---|---|
| `notebooks/corrective_rag_final.ipynb` | The Corrective RAG project (main deliverable) |
| `notebooks/agentic_rag_v2_fixed.ipynb` | Extension: adds a planner and tools (calculator, comparison, data analysis) |

## Results
On a small sanity test of 8 questions (5 answerable, 3 out of scope), every answerable question was answered with sources and every out-of-scope question was declined. This is a small test, not a benchmark.

## Run it (Google Colab)
1. Add a Colab secret named `GROQ_API_KEY` (Notebook access on).
2. Upload your own course files as `data.zip` (folders per course, e.g. `M269/`, `TM112/`).
3. Run the notebook top to bottom.

The course material and the API key are **not** included in this repository.
## Screenshot App 
<img width="859" height="899" alt="image" src="https://github.com/user-attachments/assets/9d09653e-db9e-4ab3-ae3b-428f016b5203" />
<img width="827" height="903" alt="image" src="https://github.com/user-attachments/assets/05924141-aa76-47e1-a84c-6c4f03ad4a51" />

## Tech
Python · ChromaDB · sentence-transformers · Groq (gpt-oss) · LangChain text splitters · Gradio
