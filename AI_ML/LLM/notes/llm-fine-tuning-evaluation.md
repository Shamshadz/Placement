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

## Detailed evaluation flow

~~~mermaid
flowchart LR
  A[Define product rubric] --> B[Build held-out test set]
  B --> C[Run base model]
  B --> D[Run fine-tuned model]
  C --> E[Compare quality, risk, cost, latency]
  D --> E
  E --> F[Slice and error analysis]
  F --> G[Release decision and regression suite]
~~~

## Evaluation design

Freeze the test dataset before model selection where possible. Define pass/fail criteria per category: factual correctness, JSON/schema validity, policy adherence, refusal quality, tool-call success, latency, and cost. Compare outputs blinded to model identity for human ratings. Sample difficult slices deliberately: long context, rare intents, adversarial requests, multilingual prompts, and domain-specific edge cases.

For preference data, each example contains the same prompt plus a chosen and rejected response. DPO raises the model's relative preference for chosen responses without separately fitting a reward model. It simplifies training, but its result still depends on preference-data coverage and quality.

## Production considerations

- Log model ID, prompt template, decoding parameters, tool versions, retrieval state, and evaluator version with every benchmark result.
- Build a regression suite from past production failures, then rerun it before every model or prompt release.
- Use human escalation for safety-critical, legal, medical, or high-impact evaluation; automated judge scores alone are insufficient.
- Monitor live quality through sampled review, explicit user feedback, abstention rate, safety events, and distribution shift.

## Revision checklist

- [ ] I can design a fair base-versus-fine-tuned comparison.
- [ ] I can separate task success, response quality, operational metrics, and safety metrics.
- [ ] I can explain the input structure and purpose of DPO.
