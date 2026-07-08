# Design Decisions

A running log of design choices, the alternatives considered, and why.

## Infrastructure

### Vector store: Postgres + pgvector
- **Chosen:** `pgvector/pgvector:pg16` via Docker Compose.
- **Alternatives:** Chroma / FAISS (local-only), managed vector DBs (Pinecone).
- **Why:** Keeps vectors next to relational metadata (source file, section, doc type) in one
  queryable store, and reflects a common production setup better than an in-process index.

## Corpus

### Source: PyTorch Tutorials
- **Chosen:** [`pytorch/tutorials`](https://github.com/pytorch/tutorials), pinned to commit
  `cb473bc` for reproducibility.
- **Why:** Public, permissively licensed, technically dense, and mixes prose with code, which
  makes chunking and retrieval non-trivial.

### Scope: 120 documents
- 89 `.py` tutorials (54 from `beginner_source/`, 35 from `intermediate_source/`)
- 31 `.rst` tutorials, selected with a two-pass filter:
  1. **Stub removal:** dropped 20 files whose content is only a redirect or deprecation notice
     (matching `"Redirecting in"` or `"is deprecated"`; the repo uses two phrasings).
  2. **Length check:** the remaining files split cleanly by line count (stubs ≤ 11 lines,
     real content ≥ 48 lines). Ambiguous index/overview files (`nlp_from_scratch_index.rst`,
     `dist_overview.rst`) were checked by hand and kept, since both have real content.

### Deliberate inclusion: deprecated FSDP1 tutorial
- `FSDP1_tutorial.rst` (deprecated API) is kept alongside `FSDP_tutorial.rst` (current FSDP2).
- **Why:** It is a realistic retrieval hazard. A query like "how do I use FSDP?" should surface
  the current API, not the deprecated one. This becomes an explicit evaluation case.
