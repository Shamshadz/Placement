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
