# Transformer architecture

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #transformer #architecture #llm

## In one minute

A Transformer processes a sequence using attention instead of recurrence. A decoder-only LLM converts token IDs into token and position embeddings, applies repeated Transformer blocks, and projects the final representations to vocabulary logits for next-token prediction.

## Flow

    Token IDs [B, T]
      → token embeddings + position embeddings [B, T, C]
      → N Transformer blocks [B, T, C]
      → final LayerNorm
      → LM head [B, T, V]
      → logits for every vocabulary token at every position

Each Transformer block typically contains pre-normalization, multi-head self-attention with a residual connection, then a feed-forward network with another residual connection. Attention mixes information across tokens; the feed-forward network transforms each token representation independently.

## Important components

| Component | Purpose |
| --- | --- |
| Token embedding | Maps each discrete token ID to a learned C-dimensional vector. |
| Position embedding / encoding | Supplies order because attention alone is permutation-invariant. |
| Multi-head attention | Lets each position combine information from relevant positions through several representation subspaces. |
| Feed-forward network | Applies a non-linear per-token transformation, often expanding C to about 4C and projecting back. |
| LayerNorm + residual | Stabilize optimization and preserve a gradient path through deep stacks. |
| LM head | Maps hidden state to V vocabulary logits. |

## Decoder-only versus encoder-only

Decoder-only models use causal masking and predict the next token, so they are suited to generation. Encoder-only models such as BERT use bidirectional attention and are commonly used for representations, classification, or masked-language modeling. Do not use a base encoder model when you need autoregressive text generation.

## Interview prompts

- Why are position embeddings needed in a Transformer?
- What is the different responsibility of attention and the feed-forward network?
- Why do residual connections help train deep networks?

## My example

For a tiny decoder-only implementation, use B=batch size, T=context length, C=embedding dimension, H=heads, D=C/H, and V=vocabulary size. Track every tensor shape rather than treating the block as a black box.

## References

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)

## Detailed architecture diagram

~~~mermaid
flowchart LR
  T[Token IDs] --> TE[Token embedding]
  P[Position IDs] --> PE[Position embedding]
  TE --> S[Add embeddings]
  PE --> S
  S --> B1[Transformer block 1]
  B1 --> BN[Transformer block N]
  BN --> LN[Final LayerNorm]
  LN --> H[LM head]
  H --> L[Logits over vocabulary]
~~~

## Block-level lifecycle

In a pre-normalized decoder block, apply LayerNorm before attention, add the attention output to the residual stream, then apply LayerNorm before the feed-forward network and add that output too. The residual stream lets the layer make an incremental change instead of replacing the representation; this improves gradient flow and lets each block specialize.

For C=64 and H=4 heads, each head uses D=16 dimensions. A feed-forward network might project 64 to 256, apply GELU, then project 256 back to 64. The sequence length stays unchanged through the block; only the vocabulary projection changes the last dimension from C to V.

## Production considerations

- Context length, vocabulary, width, layers, and heads jointly determine memory, latency, and capability.
- Cache key/value tensors during generation; otherwise every next token recomputes attention over the entire prefix.
- Verify tokenizer and model vocabulary compatibility. Token-ID mismatch invalidates every downstream result.
- Record parameter count, maximum context, precision, batch size, and tokens-per-second for serving comparisons.

## Revision checklist

- [ ] I can trace shapes from token IDs through logits.
- [ ] I can explain the residual stream and pre-normalization order.
- [ ] I can identify why decoder-only and encoder-only architectures serve different tasks.
