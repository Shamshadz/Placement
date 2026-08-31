# LLM fine-tuning data

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #sft #dataset-engineering #chat-template

## In one minute

Fine-tuning quality is constrained by dataset quality. A good supervised fine-tuning dataset clearly represents the behaviour desired in production, uses the correct prompt/chat format, has clean and diverse examples, and is split to prevent leakage before training begins.

## Dataset pipeline

    Raw examples
      → define schema and target behaviour
      → clean, deduplicate, filter, and redact
      → format with the model chat template
      → split into train, validation, and test
      → tokenize, pack/truncate/pad
      → train and evaluate

Common schemas are instruction/input/output triples and multi-turn messages. The exact chat template belongs to the selected base model; formatting training data differently from inference prompts can cause avoidable failures.

## Quality checklist

| Check | Why it matters |
| --- | --- |
| Clear label policy | Prevents contradictory answers and inconsistent style. |
| Deduplication | Stops repeated examples from dominating training and evaluation. |
| Sequence-level split | Avoids near-duplicates or related conversations leaking across splits. |
| Domain coverage | Includes expected inputs, edge cases, and negative/abstain cases. |
| Privacy and rights | Removes sensitive data and respects data-use permission. |
| Length distribution | Reveals truncation and context-window risks. |

## Trade-offs and pitfalls

| Concern | Why it matters |
| --- | --- |
| More data | Quantity does not compensate for wrong, noisy, or misformatted examples. |
| Packing | Improves utilization but requires correct loss masking and boundaries. |
| Class / task imbalance | The model may overproduce the dominant style or answer type. |
| Train-validation leakage | Makes offline metrics look good without real generalization. |

## Interview prompts

- How would you build and validate an instruction-tuning dataset for a support assistant?
- Why must the chat template match the base model?
- How would you detect data leakage in a conversation dataset?

## Detailed data lifecycle

~~~mermaid
flowchart LR
  R[Raw source data] --> P[Privacy, rights, and policy review]
  P --> C[Clean and normalize]
  C --> D[Deduplicate and decontaminate]
  D --> S[Split by source or conversation]
  S --> F[Apply model chat template]
  F --> T[Tokenize, pack, truncate, mask]
  T --> V[Validate samples and statistics]
  V --> E[Train and evaluate]
~~~

## Worked conversation example

For an assistant that should answer only the final user turn, render system, user, and assistant messages with the target model's chat template. Create labels for assistant-token positions and ignore labels for prompt tokens. A model trained to predict the prompt itself can waste capacity and behave differently from the intended assistant objective.

Before training, inspect: token-length percentiles, duplicate rate, empty fields, language distribution, class/task distribution, refusal examples, and source overlap between splits. Manually review a random sample plus the longest, shortest, and highest-risk records.

## Production considerations

- Treat dataset version, labeling guide, template version, and redaction policy as first-class artifacts.
- Maintain provenance so a harmful or incorrect example can be traced and removed in a future dataset revision.
- Add regression examples from real failures only after privacy review and de-identification.
- Evaluate after any tokenizer, template, preprocessing, or data-source change; these are model-behaviour changes.

## Revision checklist

- [ ] I can define labels for assistant-only supervised fine-tuning.
- [ ] I can explain why random frame-like splitting is dangerous for conversation data.
- [ ] I can propose data-quality metrics before launching training.
