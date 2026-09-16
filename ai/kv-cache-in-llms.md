# KV Caches in LLMs

Autoregressive LLMs generate one token at a time. Without a cache, every decoding step sends the entire growing sequence through each transformer layer again, recomputing the keys and values for tokens that have not changed.

A KV cache keeps those attention keys and values after the prompt prefill. Each later step projects only the new token, appends its key and value, and attends the new query over all cached keys and values.

```text
prefill: prompt tokens -> keys, values -> cache

decode:  next token -> new key, new value
         cache      -> old keys, old values
         attention(new query, all cached keys, all cached values)
```

```python
# Shape: (batch, heads, sequence, head_dim)
def append_kv(cache_k, cache_v, keys_new, values_new):
    if cache_k is None:
        return keys_new, values_new
    return (
        torch.cat((cache_k, keys_new), dim=2),
        torch.cat((cache_v, values_new), dim=2),
    )
```

The cache must belong to one generation sequence. Clear it before processing another prompt, otherwise the new request attends to stale context. Positional encodings must also advance from the cached length; assigning every incremental token position zero changes the model input.

A cache does not make attention free: the query for token `t` still scores against `t` cached keys. It eliminates repeated key/value projections and repeated processing of prior tokens. In practice this substantially improves decode latency, especially as generated context grows.

The trade-off is memory. A cache grows linearly with sequence length, layer count, batch size, number of KV heads, and head dimension. Production implementations therefore pre-allocate cache memory when a maximum context is known, or use a bounded/sliding-window cache when the model does not need to attend to the full history.

Avoid repeatedly concatenating tensors in a hot generation loop. It reallocates and copies a growing cache each step. Reserve storage and write the new key/value tensors into the next sequence slice instead.

```python
# Shape: (batch, heads, max_sequence, head_dim)
cache_k[:, :, position:position + step_tokens, :] = keys_new
cache_v[:, :, position:position + step_tokens, :] = values_new
position += step_tokens
```

KV caching is inference-only state. Do not retain it across independent requests or use it during training, where each sequence is processed as a complete batch and gradients require a different memory model.

[source](https://magazine.sebastianraschka.com/p/coding-the-kv-cache-in-llms)
