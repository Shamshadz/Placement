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
