# Self-attention and causal masking

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #attention #causal-mask #tensor-shapes

## In one minute

Self-attention lets each token choose how strongly to combine information from other tokens. For decoder-only LLMs, a causal mask prevents a position from seeing future tokens, ensuring the model cannot use the answer it is meant to predict.

## How it works

For input X, learned projections produce queries Q, keys K, and values V. Attention weights are computed as softmax(QK-transpose divided by square-root of D), then applied to V. Scaling by square-root of D prevents large dot products from making softmax too sharp.

With B batches, T tokens, C embedding dimensions, H heads, and D=C/H:

    X:                    [B, T, C]
    Q, K, V after split:  [B, H, T, D]
    QK-transpose:         [B, H, T, T]
    weighted V:           [B, H, T, D]
    concatenated output:  [B, T, C]

The causal mask is lower triangular. Before softmax, entries that point to future positions are set to negative infinity, causing their probability to become zero.

## Trade-offs and pitfalls

| Concern | Why it matters |
| --- | --- |
| Attention cost | Standard attention has quadratic T-by-T score memory/compute, which limits long contexts. |
| Missing scale factor | Large dot products can saturate softmax and hurt gradients. |
| Wrong mask direction | Leaks future tokens during training and produces misleadingly low loss. |
| Head dimensions | C must be divisible by H in a conventional multi-head implementation. |

## Interview prompts

- Why use multiple attention heads instead of one large head?
- What changes between causal self-attention and bidirectional self-attention?
- Where does the quadratic cost arise?

## References

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)

## Detailed attention flow

~~~mermaid
flowchart LR
  X[Hidden states B,T,C] --> QKV[Linear projection to Q,K,V]
  QKV --> S[Split into H heads]
  S --> Q[Queries]
  S --> K[Keys]
  S --> V[Values]
  Q --> A[Scaled QK transpose scores]
  K --> A
  A --> M[Apply causal mask]
  M --> W[Softmax attention weights]
  W --> O[Weighted sum of V]
  V --> O
  O --> C[Concatenate heads and output projection]
~~~

## Worked shape trace

With B=2, T=32, C=64, H=4, D=16: the QKV projection first creates [2,32,192], then reshapes/splits to three tensors of [2,4,32,16]. Each head calculates a 32 by 32 relation matrix. The causal mask permits entries on and below the diagonal only. After weighting V, concatenate four 16-dimensional head outputs to return [2,32,64].

## Production considerations

- Use a numerically stable attention implementation; masking after softmax is wrong because forbidden positions still affect normalization.
- Cache K and V per layer during autoregressive generation, and bound cache memory for long sessions.
- Monitor input length, truncation, and attention-memory failures separately from model-quality metrics.
- For long context, evaluate alternative attention implementations or retrieval; do not assume a larger context window solves relevance.

## Revision checklist

- [ ] I can calculate the QK transpose shape for any B, T, H, and D.
- [ ] I can explain why mask values are applied before softmax.
- [ ] I can explain why generation benefits from a KV cache.
