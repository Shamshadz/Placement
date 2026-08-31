# Next-token training

> **Area:** LLM Engineering  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-31  
> **Tags:** #training #cross-entropy #causal-language-modeling

## In one minute

A causal language model learns to predict the next token. Given a token sequence, inputs are all but the last token and labels are all but the first token. The model returns logits over the vocabulary at each position; cross-entropy compares those logits to the shifted labels and backpropagation updates the trainable parameters.

## One training step

    tokens:  [The, cat, sat, on]
    input:   [The, cat, sat]
    target:  [cat, sat, on]
    logits:  [B, T, V]
    loss:    cross-entropy(flatten(logits), flatten(target))
    loss backward → optimizer step → zero gradients

Logits are unnormalized scores. Softmax converts logits to probabilities conceptually; cross-entropy implementations normally combine the stable softmax and negative-log-likelihood calculation.

## Hyperparameters that interact

| Parameter | Effect |
| --- | --- |
| Learning rate | Too high diverges; too low learns slowly. |
| Batch size / gradient accumulation | Larger effective batch changes memory use and optimization behaviour. |
| Context length | Captures longer dependencies but raises attention cost. |
| Epochs | More passes can improve fit or overfit noisy/small data. |
| Padding and labels | Padding tokens must be excluded from loss when variable-length sequences are batched. |

## Interview prompts

- Why are input and target sequences shifted by one token?
- What are logits, and why are they used instead of probabilities in the loss?
- How does gradient accumulation help when GPU memory is limited?

## My example

Be ready to trace one mini-batch through tokenization, embeddings, logits, loss, gradients, and optimizer update. State which parameters are frozen when the training method is LoRA or QLoRA.

## Detailed training flow

~~~mermaid
flowchart LR
  D[Tokenized sequence] --> X[Inputs: tokens 0 to T-2]
  D --> Y[Labels: tokens 1 to T-1]
  X --> M[Language model]
  M --> L[Logits B,T,V]
  L --> CE[Cross-entropy with labels]
  Y --> CE
  CE --> G[Backpropagate gradients]
  G --> O[Optimizer updates trainable parameters]
  O --> M
~~~

## Worked loss intuition

At one position, the target is one vocabulary ID. Cross-entropy penalizes the negative log probability assigned to that ID. If the target probability is 0.8, loss is low; if it is 0.001, loss is high. Across a batch, sum or average only valid, non-padding target positions. In causal training, a token can attend to itself and earlier context while predicting the next-token label aligned at that position.

## Production considerations

- Save model, tokenizer, training configuration, random seeds, dataset version, and evaluation results as one reproducible run.
- Use gradient clipping, loss monitoring, and checkpointing to recover from instability.
- Separate training throughput from serving latency; optimizing one does not guarantee the other.
- Confirm that labels mask user/system prompts appropriately when training assistant responses only.

## Revision checklist

- [ ] I can describe each operation in one training step in order.
- [ ] I can explain why padding must be excluded from loss.
- [ ] I can explain how SFT labels differ from generic next-token labels.
