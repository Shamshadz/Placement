# LLM Engineering — Revision Index

## Foundations

- [Transformer architecture](notes/transformer-architecture.md)
- [Self-attention and causal masking](notes/self-attention-and-causal-masking.md)
- [Next-token training](notes/next-token-training.md)

## Fine-tuning and alignment

- [LoRA](notes/lora.md)
- [QLoRA and quantization](notes/qlora-and-quantization.md)
- [LLM fine-tuning data](notes/llm-fine-tuning-data.md)
- [LLM fine-tuning evaluation](notes/llm-fine-tuning-evaluation.md)
- [RAG versus fine-tuning](notes/rag-vs-fine-tuning.md)

## Revision checklist

- [ ] I can trace one batch from tokens to loss and parameter update.
- [ ] I can derive the main tensor shapes for multi-head attention.
- [ ] I can explain why LoRA trains A and B while freezing the base weights.
- [ ] I can justify LoRA versus QLoRA versus full fine-tuning for a given GPU budget.
- [ ] I can design a dataset and evaluation before starting a fine-tuning run.
- [ ] I can choose RAG, fine-tuning, or a hybrid solution from product constraints.
