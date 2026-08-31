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
