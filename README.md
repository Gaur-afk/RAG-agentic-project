# Agentic RAG

A tool-using agent where RAG retrieval (pgvector + embeddings) is one tool among several
(web search, calculator/code exec, a custom domain tool). Built without a framework first,
to understand the agent loop mechanically. Design decisions and their rationale are
logged in [`docs/DECISIONS.md`](docs/DECISIONS.md).

## Setup

```bash
# Postgres + pgvector
docker compose up -d

# Python env
conda create -n rag-agentic python=3.11
conda activate rag-agentic
python -m pip install anthropic psycopg2-binary pgvector python-dotenv

# Corpus: PyTorch tutorials, pinned to the commit used for this project
git clone https://github.com/pytorch/tutorials.git data/raw/pytorch-tutorials
git -C data/raw/pytorch-tutorials checkout cb473bcf67d48a3eeb6355be66f3e73814861fb1
```

Create a `.env` with `ANTHROPIC_API_KEY`, `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`.
