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
