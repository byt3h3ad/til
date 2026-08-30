# RAG Is Simpler Than You Think

Start retrieval-augmented generation with the cheapest retrieval method that can meet measured user needs. Embeddings, vector databases, and complex chunking are not the default; they add re-indexing, model-migration, latency, and evaluation work.

### 1. Start with full-text search

BM25 or a database full-text index is fast, cheap, and explainable. It is a strong fit for keyword-heavy questions, exact identifiers, and proprietary terms.

```text
User: "How do I reset my password?"
Search: BM25("reset password")
```

Measure this baseline before adding ML infrastructure. If users can find the documents they need, stop there.

### 2. Fix queries before changing retrieval

Conversational queries often need better keywords rather than vector similarity. Use an LLM to remove filler, preserve product names, expand useful synonyms, or split a compound question into focused searches.

```text
User: "How do I use Atlas for batch jobs?"
Rewrite: "Atlas batch jobs data processing"
```

Keep domain-specific names exact. A general embedding model may map an internal project name to an unrelated public meaning, while keyword search will match the actual documentation.

### 3. Add embeddings only for a proven semantic gap

A hybrid path uses sparse search to get candidates, then embeddings to rerank them. This improves semantic matches but reintroduces chunking choices and roughly hundreds of milliseconds of request latency.

```text
query -> BM25 top 100 -> embed/rerank -> top 10
```

Choose how embeddings are computed from corpus freshness and traffic:

- **High churn:** embed a small candidate set at query time. It stays current and makes model changes cheap, but costs latency.
- **Mixed access patterns:** pre-embed frequently used documents and embed cold documents on demand.
- **High volume with a stable corpus:** pre-embed everything and use approximate nearest-neighbour search for low latency. Re-indexing becomes an operational cost when documents or embedding models change.

For multi-intent questions, split the request into sub-queries and retrieve them independently. Route only the difficult sub-queries through expensive rewriting or embedding; run the searches in parallel and synthesize the result afterward.

The decision order is simple: build full-text search, collect feedback, try query rewriting for vocabulary mismatches, then A/B test hybrid retrieval only if results still miss the mark. Match the architecture to corpus churn, query volume, latency requirements, and team capability—not to the most elaborate stack available.

[source](https://www.lighthousenewsletter.com/p/rag-is-simpler-than-you-think)
