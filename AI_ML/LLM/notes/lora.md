# LoRA

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #lora #peft #fine-tuning

## In one minute

Low-Rank Adaptation (LoRA) is parameter-efficient fine-tuning. The pretrained weight matrix W stays frozen; training learns a small update delta-W represented by two low-rank matrices. The effective layer weight is W plus delta-W, allowing specialization without training or storing a full copy of the model weights.

## Mathematics and dimensions

For a linear layer with W shaped [d-out, d-in], a common convention is:

    delta-W = B A
    A: [r, d-in]
    B: [d-out, r]
    delta-W: [d-out, d-in]

The rank r is deliberately much smaller than d-in and d-out. The forward pass becomes base-output plus scaling times the LoRA-output. Implementations commonly use alpha divided by r as the scaling factor.

A and B are not calculated from W. They are initialized and learned through the normal loss and gradient-descent loop. A common initialization makes the initial update zero, so the model begins with base-model behaviour.

## What is trained

| Parameter | Role |
| --- | --- |
| Base W | Frozen pretrained knowledge. |
| A and B | Small trainable matrices that learn the task/domain adaptation. |
| Rank r | Controls adapter capacity and trainable parameter count. |
| Alpha | Scales the learned update. |
| Target modules | Defines which projections receive adapters, often attention and/or MLP projections. |

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| Low rank | Saves memory and training cost, but may underfit a difficult adaptation. |
| High rank | Adds capacity and cost; it is not automatically better. |
| Target modules | Too few can limit adaptation; too many cost more and complicate comparison. |
| Adapter merge | Can simplify deployment for compatible serving paths, but preserve the original adapter artifact too. |
| Evaluation | Lower loss alone does not prove useful specialized behaviour. |

## Interview prompts

- Why does LoRA reduce trainable parameters without changing the base model weights?
- What do rank and alpha control?
- Why can the initial LoRA model behave like the base model?

## References

- [LoRA paper](https://arxiv.org/abs/2106.09685)

## Detailed adapter flow

~~~mermaid
flowchart LR
  X[Layer input x] --> W[Frozen base projection W]
  X --> A[Trainable low-rank A]
  A --> B[Trainable low-rank B]
  W --> S[Add outputs]
  B --> S
  S --> Y[Layer output]
  L[Loss] --> G[Gradients update A and B only]
  G --> A
  G --> B
~~~

## Parameter-count example

For a 4096 by 4096 projection, full fine-tuning exposes 16,777,216 parameters in that matrix. With rank r=8, A has 8 by 4096 parameters and B has 4096 by 8, totaling 65,536 trainable parameters before any optional bias. This is why LoRA is attractive when adapters are attached selectively to large model projections.

## Selecting a LoRA configuration

Choose target modules based on the model architecture and task; document them instead of copying a configuration blindly. Start with a modest rank and controlled dataset/evaluation, then compare rank, alpha, dropout, and target-module ablations. A larger rank can overfit small data or consume unnecessary memory.

## Production considerations

- Store adapter weights, base-model revision, tokenizer, target modules, rank, alpha, and prompt template together.
- Verify merge/unmerge parity on representative prompts if adapters are merged into base weights.
- Support adapter versioning and rollback; different tenants/tasks may require isolated adapters.
- Benchmark adapter load time and concurrent-adapter memory when serving multiple specializations.

## Revision checklist

- [ ] I can derive A, B, and delta-W shapes from a base linear layer.
- [ ] I can calculate the trainable-parameter reduction for a chosen rank.
- [ ] I can explain how adapter configuration becomes a reproducible deployment artifact.
