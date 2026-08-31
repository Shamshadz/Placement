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

## Detailed QLoRA flow

~~~mermaid
flowchart LR
  W[Pretrained weights] --> Q[Quantize and freeze base model]
  Q --> F[Forward computation]
  X[Training batch] --> F
  F --> L[Loss]
  L --> G[Backpropagate through base]
  G --> A[Update LoRA adapters]
  A --> F
~~~

## Memory budget checklist

Estimate all of these before a run:

    base weights + LoRA weights + gradients + optimizer states
    + activations + attention/KV-related temporaries + framework overhead

Activation memory grows with batch size, sequence length, layers, and hidden width. Gradient checkpointing reduces activation memory by recomputing portions of the forward pass, trading memory for extra compute. Gradient accumulation raises effective batch size without simultaneously storing a large batch.

## Practical experiment

Run the same small validation set with a full-precision LoRA baseline and a QLoRA configuration. Compare task quality, output stability, peak memory, training throughput, and inference latency. If QLoRA quality regresses, first verify quantization settings, compute dtype, sequence length, and data pipeline before increasing rank.

## Production considerations

- Check GPU architecture support for the selected compute dtype and quantization kernel.
- Quantization changes the base artifact; pin library versions and test loading from a clean environment.
- Use memory telemetry and stop conditions to avoid out-of-memory retries masking a configuration problem.
- Benchmark cold start, adapter attachment, and batch inference rather than quoting model-only memory.

## Revision checklist

- [ ] I can enumerate why total training memory exceeds quantized-weight storage.
- [ ] I can explain NF4, double quantization, and paged optimizers at a high level.
- [ ] I can design a memory-safe experiment on a constrained GPU.
