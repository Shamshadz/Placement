# LLM fine-tuning evaluation

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #evaluation #llm #fine-tuning

## In one minute

Evaluation answers whether fine-tuning improved the intended product behaviour. Evaluate the base model and fine-tuned model on the same held-out, representative test set; combine task-specific metrics with qualitative error analysis and human review where needed.

## Evaluation layers

| Layer | Examples |
| --- | --- |
| Training diagnostics | Train/validation loss, gradient behaviour, throughput, memory. |
| Task success | Exact match, structured-output validity, classification F1, retrieval metrics, tool-call success. |
| Response quality | Helpfulness, correctness, format adherence, tone, completeness. |
| Risk | Hallucination, unsafe outputs, privacy leakage, bias, prompt-injection behaviour. |
| Operations | Latency, token cost, failure rate, and serving reliability. |

Use a fixed evaluation protocol and preserve prompts, model version, decoding parameters, and rubric. LLM-as-a-judge can scale comparisons but needs calibration with human review and should not be the sole source of truth for high-stakes claims.

## Preference data

Supervised fine-tuning uses desired responses. Preference optimization uses pairs of chosen and rejected responses. DPO optimizes preference alignment directly with a classification-style objective, avoiding the separate reward-model-plus-RL loop of typical RLHF workflows.

## Interview prompts

- Why is validation loss insufficient for an instruction-following assistant?
- How would you compare a base model, SFT model, and RAG system fairly?
- What would make an LLM-as-a-judge evaluation unreliable?

## References

- [Direct Preference Optimization paper](https://arxiv.org/abs/2305.18290)
