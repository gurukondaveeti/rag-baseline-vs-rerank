# RAG: Baseline Retrieval vs. Cross-Encoder Re-Ranking

Compares two retrieval pipelines over the same chunked document (the EU GDPR, Regulation 2016/679):

- **Pipeline 1 (Baseline):** `all-MiniLM-L6-v2` bi-encoder → FAISS inner-product index → Top-10 → Top-3 → LLM
- **Pipeline 2 (Re-rank):** same Top-10 → `cross-encoder/ms-marco-MiniLM-L-6-v2` → re-sorted Top-3 → LLM (identical prompt)

Everything is in one notebook: `rag_baseline_vs_rerank.ipynb`.

## How to run

1. Open the notebook in Google Colab (GPU runtime optional; CPU works).
2. Add a Colab secret `GEMINI_API_KEY` for the LLM. Without it, a local `Qwen2.5-1.5B-Instruct` model is used.
3. *Runtime → Run all.* The GDPR PDF is downloaded from EUR-Lex automatically (or you are asked to upload it).

## What the notebook produces

| Output | File |
|---|---|
| Rank of the "gold" chunk before/after re-ranking for 10 stress queries | `results/rank_shifts.csv` |
| LLM Answer 1 vs Answer 2 for each query | `results/answers.csv` |
| Latency per stage | `results/latency.csv` |
| Cross-encoder over the full corpus vs. 10 candidates | `results/scaling.csv` |

## Stress-query categories

Keyword trap / distractor, exceptions & corner cases, negation & constraints, and role limits.
Each query has evidence phrases taken from the clause that truly answers it, so the notebook can find the
correct chunk and report its rank movement automatically (e.g. `Chunk 212 moved from Rank #7 → Rank #1`).
