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
