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

## Detailed decision and request flow

~~~mermaid
flowchart LR
  U[User request] --> A[Authorize tenant and sources]
  A --> R[Retrieve and rerank relevant context]
  R --> C[Compose grounded prompt]
  C --> M[Base or fine-tuned model]
  M --> V[Validate answer, citations, and safety]
  V --> O[Response with evidence]
~~~

## Architecture comparison

RAG changes the model input at request time; fine-tuning changes trainable model behaviour over a training lifecycle. RAG requires ingestion, chunking, metadata, embeddings, indexing, retrieval, reranking, authorization, and citation handling. Fine-tuning requires an example dataset, training infrastructure, evaluation, model registry, and deployment. A hybrid system has both operational surfaces, so only use it when the product need justifies both.

## Worked decision example

For internal engineering documentation that changes weekly, use RAG: index approved documents, filter by team access, retrieve relevant chunks, and cite them. For a support assistant that must always return a strict JSON action schema and follow a consistent triage procedure, fine-tuning may reduce prompt complexity. If it needs both current policy documents and reliable output format, use fine-tuning plus RAG and evaluate the combined system.

## Production considerations

- Enforce authorization before retrieval, not only in the final prompt; do not leak unauthorized chunks into context.
- Version documents, embeddings, chunking strategy, index, reranker, prompt template, and model independently.
- Measure retrieval recall and context precision separately from final-answer quality.
- Provide a no-answer/needs-review path when evidence is missing, contradictory, stale, or insufficient.

## Revision checklist

- [ ] I can choose RAG, fine-tuning, or hybrid from freshness, behaviour, cost, and evidence requirements.
- [ ] I can list the security boundaries in a multi-tenant RAG request.
- [ ] I can define separate metrics for retrieval quality and answer quality.
