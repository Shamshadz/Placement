# QLoRA and quantization

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #qlora #quantization #gpu-memory

## In one minute

QLoRA combines a frozen, low-bit quantized base model with trainable LoRA adapters. It reduces memory enough to fine-tune larger models on limited hardware while preserving the same fundamental adapter-training workflow.

## How it works

1. Load the pretrained base weights in 4-bit quantized form.
2. Keep those base weights frozen.
3. Attach LoRA adapters to selected modules.
4. Compute with a higher-precision compute dtype where appropriate and backpropagate gradients into the adapters.
5. Save the adapter/configuration and evaluate against the same baseline.

QLoRA introduced NF4, double quantization, and paged optimizers as memory-saving techniques. Quantization shrinks stored base weights, but training memory also includes activations, adapter gradients, optimizer states, temporary tensors, and sequence-length effects.

## Memory reasoning

Approximate base-weight storage is parameter-count times bytes per parameter. For example, 7 billion FP16 weights need about 14 GB before training overhead; idealized 4-bit storage is about 3.5 GB. These figures are not total training memory.

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| 4-bit base weights | Major memory reduction, but can affect numerical behaviour and compatibility. |
| Compute dtype | BF16/FP16 choice affects stability, GPU support, and memory. |
| Sequence length | Long contexts can dominate memory through activations even with a quantized base. |
| Batch size | Often requires gradient accumulation on a small GPU. |
| Quantized serving | Verify the actual inference stack, latency, and adapter-loading path. |

## Interview prompts

- Why does QLoRA need much less memory than full fine-tuning?
- Why does 4-bit model storage not mean training uses only 4-bit memory?
- How would you adapt a 7B model on a 16 GB GPU?

## References

- [QLoRA paper](https://arxiv.org/abs/2305.14314)
- [LoRA paper](https://arxiv.org/abs/2106.09685)
