# RAG versus fine-tuning

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #rag #fine-tuning #system-design

## In one minute

Use retrieval-augmented generation (RAG) to supply changing, attributable external knowledge at request time. Use fine-tuning to change repeatable behaviour, task performance, format, style, or domain adaptation. Many production systems combine both: a fine-tuned model consumes retrieved context.

## Decision guide

| Requirement | Prefer | Why |
| --- | --- | --- |
| Frequently changing knowledge | RAG | Update documents/index instead of retraining weights. |
| Citations / source visibility | RAG | Retrieved chunks can be returned or audited. |
| Consistent output format or behaviour | Fine-tuning | Learns a repeatable response policy from examples. |
| Specialized task with labelled examples | Fine-tuning | Can improve task performance or reduce prompt burden. |
| Confidential tenant knowledge | RAG with access controls | Supports per-request authorization and removal. |
| Both behaviour and fresh knowledge | Hybrid | Fine-tune the behaviour; retrieve current evidence. |

## Compare fairly

Test the same dataset and prompts across base model, base plus RAG, fine-tuned model, and fine-tuned plus RAG. Measure task quality, factuality, freshness, latency, token/inference cost, training cost, retrieval quality, and failure modes.

## Trade-offs and pitfalls

| Concern | Why it matters |
| --- | --- |
| Retrieval quality | Bad chunking, embedding, filters, or ranking gives the LLM bad context. |
| Fine-tuning freshness | Updating facts requires another training lifecycle and can be expensive. |
| Context overload | Too much irrelevant retrieved text raises cost and can degrade answers. |
| False attribution | A citation is not evidence unless the retrieved content actually supports the answer. |

## Interview prompts

- Why not fine-tune a model whenever company documentation changes?
- How would you evaluate whether RAG is helping rather than harming?
- What components would you add to a production RAG system for tenant isolation?
