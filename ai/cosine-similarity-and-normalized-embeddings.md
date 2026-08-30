# Cosine Similarity and Normalized Embeddings

Cosine similarity compares the direction of two vectors while ignoring their magnitude. For unnormalized embeddings, it prevents a larger vector from winning purely because of its length.

```text
cosine(a, b) = (a · b) / (||a|| × ||b||)
```

The dot product is only the numerator:

```text
dot(a, b) = Σ aᵢbᵢ
```

For L2-normalized vectors, `||a|| = ||b|| = 1`, so the formulas have identical rankings:

```text
cosine(a, b) = (a · b) / (1 × 1) = a · b
```

Many current embedding APIs return normalized vectors, but this is a model contract—not an assumption. Check the model documentation or measure a sample before selecting a vector-store metric.

```python
from math import sqrt


def l2_normalize(vector):
    length = sqrt(sum(value * value for value in vector))
    return [value / length for value in vector]


def dot(left, right):
    return sum(a * b for a, b in zip(left, right))


query = l2_normalize([3, 4])
document = l2_normalize([6, 8])
assert dot(query, document) == 1.0
```

Normalize both indexed documents and incoming queries exactly once when the embedding model does not already do so. After that, use dot product for retrieval: it is the same score as cosine similarity without repeatedly computing vector lengths.

Use cosine similarity directly when vectors are not normalized and magnitude should not affect relevance, such as with some custom embeddings or TF-IDF vectors. Use dot product with normalized vectors, and keep the indexing and query-time normalization policy consistent.

[source](https://www.lighthousenewsletter.com/p/cosine-similarity-is-dead-long-live)
