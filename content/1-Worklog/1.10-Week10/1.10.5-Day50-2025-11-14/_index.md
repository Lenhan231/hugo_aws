---
title: "Day 50 - Fine-tuning & Real-World Applications"
weight: 5
chapter: false
pre: "<b> 1.10.5. </b>"
---

**Date:** 2025-11-14 (Friday)  
**Status:** "Done"  

---

# **Week 10 Complete: From Theory to Production**

You now understand transfer learning, BERT, and T5. This final day covers **how to actually deploy these models.**

---

# **Fine-tuning: The Art and Science**

## The Big Picture

```
Pre-trained Model (e.g., BERT-base)
├─ Already knows English
├─ Already understands grammar
├─ Already has some world knowledge
└─ Can be adapted to specific tasks!

Add Classification Head
├─ Simple: Take [CLS] token representation
├─ Add: Dense layer (768 → hidden_size)
├─ Add: Classification layer (hidden_size → num_classes)
└─ Result: Task-specific model

Fine-tune on your data
├─ Movie reviews: 5,000 examples
├─ Training time: 2-3 hours on single GPU
├─ Results: 94-96% accuracy
└─ Deploy!
```

---

# **Fine-tuning Strategies**

## Strategy 1: Full Fine-tuning

**Update all parameters**

```python
# Pseudocode
pretrained_model = load_bert_base()
# No frozen layers!
for epoch in range(3):
    for batch in training_data:
        logits = pretrained_model(batch)
        loss = classification_loss(logits, batch.labels)
        loss.backward()
        optimizer.step()
        
# Results: Best performance
# Time: 3+ hours on GPU
# Memory: Requires gradients for all 110M params
```

**When to use:**
- Large dataset (10,000+ examples)
- Sufficient compute (GPU/TPU)
- Task very different from pre-training

---

## Strategy 2: Layer Freezing

**Freeze early layers, fine-tune later layers**

```python
# Freeze first 10 layers
for param in model.bert.encoder.layer[:10].parameters():
    param.requires_grad = False

# Fine-tune layers 11-12 and classification head
for param in model.bert.encoder.layer[10:].parameters():
    param.requires_grad = True

# Results: Fast, good performance
# Time: 1-2 hours on GPU
# Memory: Only gradients for 2 layers + head
```

**When to use:**
- Medium dataset (1,000-10,000 examples)
- Limited compute
- Task somewhat different from pre-training

---

## Strategy 3: Progressive Unfreezing

**Gradually unfreeze layers from top to bottom**

```
Epoch 1: Only fine-tune classification head
├─ Freeze: All 12 layers
├─ Train: Classification head
└─ Learning rate: 1e-3

Epoch 2: Unfreeze last 1 layer
├─ Freeze: Layers 0-10
├─ Train: Layer 11 + head
└─ Learning rate: 1e-4

Epoch 3: Unfreeze last 2 layers
├─ Freeze: Layers 0-9
├─ Train: Layers 10-11 + head
└─ Learning rate: 1e-4

...Continue until all unfrozen

Results: Often best performance!
Time: 5+ hours, but worth it for important tasks
```

---

# **Hyperparameter Selection**

## Learning Rate

```
General Guidelines:

For full fine-tuning:
├─ Start with: 5e-5 (very small!)
├─ Try: 2e-5, 3e-5, 5e-5, 1e-4
└─ Don't use: Learning rates > 1e-4 (catastrophic forgetting)

For layer freezing:
├─ Frozen layers: No learning rate (not updated)
├─ Fine-tuned layers: 1e-4 - 1e-3
└─ Classification head: Can use slightly higher

Why so small?
├─ Pre-trained weights are already good
├─ Don't want to destroy the knowledge
├─ Small changes are safer
```

## Batch Size

```
Batch Size Impact:

Small batches (8-16):
├─ Pros: Works with limited memory
├─ Cons: Noisier gradients, unstable
├─ Use when: Small GPU (< 8GB VRAM)

Medium batches (32):
├─ Pros: Good balance
├─ Cons: Moderate memory usage
├─ Use when: Standard GPU (8-16GB VRAM)

Large batches (64-256):
├─ Pros: Stable training, better generalization
├─ Cons: Requires lots of memory or gradient accumulation
├─ Use when: TPUs, 24GB+ VRAM, or gradient accumulation
```

## Number of Epochs

```
Classification Tasks:
├─ Typical: 3-5 epochs
├─ Why: Model converges quickly
└─ Monitor: Stop early if validation stops improving

Generation Tasks (T5):
├─ Typical: 10-20 epochs
├─ Why: More complex task, slower convergence
└─ Monitor: Validation BLEU score

Rule of thumb:
├─ More data → Fewer epochs (e.g., 2 epochs for 100K examples)
├─ Less data → More epochs (e.g., 5 epochs for 1K examples)
```

---

# **Common Fine-tuning Applications**

## 1. Sentiment Analysis

```
Task: Classify reviews as positive/negative

Data: 5,000 movie reviews with labels
├─ 80%: Training (4,000)
├─ 20%: Validation (1,000)

Fine-tuning:
├─ Model: BERT-base
├─ Epochs: 3
├─ Batch size: 32
├─ Learning rate: 2e-5
├─ Training time: 30 minutes

Results:
├─ Accuracy: 94.2%
├─ Precision: 94.5%
├─ Recall: 93.9%
└─ Much better than training from scratch (78%)!
```

## 2. Named Entity Recognition (NER)

```
Task: Identify people, places, organizations in text

Example:
"John Smith works at Google in New York."
Labels:  [B-PER, I-PER, O, O, B-ORG, O, B-LOC, I-LOC]

Challenge: Token-level classification, not sentence-level

Solution:
├─ Get BERT token embeddings
├─ Add linear layer for each token
├─ Decode with CRF (Conditional Random Field)

Fine-tuning time: 1-2 hours
Performance: 92% F1 score
```

## 3. Question Answering

```
Task: Find the answer span in a passage

Input:
Question: "What is the capital of France?"
Passage: "Paris is the capital and most populous city of France..."

Output:
Answer: "Paris"

How it works:
├─ Encode question + passage together
├─ For each token, predict: "Is this the start of answer?"
├─ For each token, predict: "Is this the end of answer?"
├─ Extract span between highest probability start and end

Fine-tuning time: 2-3 hours
Performance: 89% F1 on SQuAD
```

## 4. Text Summarization

```
Task: Condensing long documents

Using T5:

Input:
"The quick brown fox jumps over the lazy dog. 
 This sentence contains all 26 letters of English alphabet.
 It's often used as a test string in computers."

Fine-tuning with T5:
├─ Prefix: "summarize:"
├─ Full training: 10-20 epochs
├─ Batch size: 16
├─ Learning rate: 5e-5

Output:
"A pangram sentence commonly used in computing."

Performance: ROUGE score of 35-40 (compared to 20-25 baseline)
```

## 5. Semantic Textual Similarity

```
Task: Rate how similar two sentences are (0-5)

Sentence A: "The cat sat on the mat"
Sentence B: "A feline rested on the rug"
Label: 4.5 (very similar)

Fine-tuning:
├─ Take [CLS] token from both sentences
├─ Encode together
├─ Regression head: Dense layer to output score (0-5)
├─ Loss: Mean Squared Error (MSE)

Results:
├─ Correlation with human judgments: 0.88 (very good!)
├─ Spearman correlation: 87%
```

---

# **Fine-tuning Pitfalls to Avoid**

## ❌ Pitfall 1: Learning Rate Too High

```
Problem: Model forgets pre-trained knowledge!

Example:
Learning rate: 1e-3
After 1 epoch: Loss = 0.5 (good)
After 2 epochs: Loss = 2.0 (terrible!)
After 3 epochs: Loss = 5.0 (worse!)

Why: Large updates destroy useful weights

Solution:
├─ Use learning rate 1-2 orders of magnitude smaller
├─ Start with 2e-5, increase only if converges too slowly
└─ Monitor: Validation loss should decrease
```

## ❌ Pitfall 2: Too Few Epochs

```
Problem: Model doesn't adapt to new task

Example:
Data: 5,000 examples
Epochs: 1
Performance: 88% accuracy

Same model with 3 epochs:
Performance: 94% accuracy!

Why: 1 epoch = each example seen once
     Not enough to learn task-specific patterns

Solution:
├─ Use at least 3-5 epochs
├─ Monitor validation accuracy
├─ Stop early when validation stops improving
```

## ❌ Pitfall 3: Overfitting on Small Data

```
Problem: Model memorizes instead of generalizing

Example:
Training data: 100 examples
Training accuracy: 99.8%
Test accuracy: 72.0%

Model memorized!

Solutions:
├─ Add dropout: Drop 10-20% of neurons randomly
├─ Early stopping: Stop when validation accuracy plateaus
├─ Data augmentation: Create more examples from existing ones
├─ Reduce model size: Use BERT-small instead of BERT-large
```

## ❌ Pitfall 4: Not Doing Hyperparameter Tuning

```
Problem: Default hyperparameters aren't optimal

Example:
Default learning rate (1e-4): 92% accuracy
Tuned learning rate (3e-5): 95% accuracy!

Solution:
├─ Try 3-5 different learning rates
├─ Try 2-3 different batch sizes
├─ Try 3-5 epochs
├─ Run small validation set on each combination
└─ Pick best combination for full training
```

---

# **Deployment Considerations**

## Model Size vs Speed

```
For Production Deployment:

BERT-base (110M):
├─ Model size: 440 MB
├─ Inference time: 100-150 ms per example
├─ Good accuracy
└─ Can fit on most servers

BERT-large (340M):
├─ Model size: 1.3 GB
├─ Inference time: 300-500 ms per example
├─ Better accuracy
└─ Need better hardware

DistilBERT (40M):
├─ Model size: 160 MB (60% smaller!)
├─ Inference time: 30-50 ms (3x faster!)
├─ Slightly lower accuracy (97% of BERT)
└─ Perfect for mobile/edge devices!

Decision tree:
├─ Accuracy critical? → Use BERT-base or BERT-large
├─ Speed critical? → Use DistilBERT or quantization
├─ Balanced? → Use BERT-base
```

## Optimization Techniques

```
Before Deployment:

1. Quantization (8-bit instead of 32-bit):
   ├─ Model size: 1/4 of original
   ├─ Inference speed: 2-4x faster
   └─ Accuracy: 95-99% of full precision

2. Knowledge Distillation:
   ├─ Train small model on outputs of large model
   ├─ Size: 10x smaller
   ├─ Speed: 10x faster
   └─ Accuracy: 98% of teacher model

3. Pruning:
   ├─ Remove unimportant weights
   ├─ Size: 30-50% smaller
   ├─ Speed: 2-3x faster
   └─ Accuracy: 98% of full model

4. TorchScript/ONNX:
   ├─ Compile model for production
   ├─ Speed: 1.5-2x faster
   └─ Framework-agnostic (TensorFlow, PyTorch, etc.)
```

---

# **Real-World Example: Building a Sentiment Classifier**

## Complete Pipeline

```
Step 1: Prepare Data
├─ Load: 5,000 movie reviews with labels
├─ Split: 80% train, 20% test
├─ Tokenize: Convert to BERT tokens
└─ Dataloader: Create batches of 32

Step 2: Load Pre-trained Model
├─ Download BERT-base from HuggingFace
├─ Add classification head: 768 → 2 (binary)
└─ Move to GPU

Step 3: Fine-tune
├─ Optimizer: AdamW (best for transformers)
├─ Learning rate: 2e-5
├─ Epochs: 3
├─ Train loop: Forward pass → Loss → Backward → Update

Step 4: Evaluate
├─ Validation accuracy: 94.2%
├─ Test accuracy: 93.8%
└─ Per-class metrics: Precision 94%, Recall 94%

Step 5: Save & Deploy
├─ Save: model.pt, tokenizer, config.json
├─ Test on new review: "Best movie ever!" → Positive ✓
└─ Deploy to production!

Total time: 2-3 hours
Total cost: ~$2-5 on cloud GPU
Performance: State-of-the-art!
```

---

# **The Transfer Learning Advantage**

## Comparison

```
Baseline (Training from Scratch):
├─ Training time: 2-4 weeks
├─ Required data: 100,000+ examples
├─ Accuracy: 82-85%
├─ Cost: $1000-10000 in compute
└─ Difficulty: Requires ML expertise

Transfer Learning (BERT Fine-tuning):
├─ Training time: 2-3 hours
├─ Required data: 100-1000 examples
├─ Accuracy: 92-95%
├─ Cost: $1-10 in compute
└─ Difficulty: Can use HuggingFace library!

Speedup: 200-300x faster!
Data reduction: 100x less data needed!
Better results: 10-15% higher accuracy!
```

---

# **Key Takeaways**

✅ **Transfer learning is practical:** Works great for real problems
✅ **Fine-tuning is simple:** Add head + train 3-5 epochs
✅ **Learning rate matters:** Use 1-2 orders of magnitude smaller
✅ **Avoid overfitting:** Monitor validation, use early stopping
✅ **Consider deployment:** Optimize for your constraints
✅ **HuggingFace is your friend:** Use pre-trained models + library

---

# **Your NLP Journey**

You've learned:

**Week 1-7 (Foundation):** Basics, text processing, embeddings
**Week 8 (NLP Foundations):** Linguistic fundamentals, voice search, seq2seq, attention
**Week 9 (Transformers):** Self-attention, scaled dot-product, attention mechanisms, implementation
**Week 10 (Transfer Learning):** Transfer learning, BERT, MLM, T5, fine-tuning

From understanding language to building production systems!

---

# **Next Steps**

To become an NLP expert:

1. **Build projects:** Fine-tune models on real data
2. **Try different models:** RoBERTa, ELECTRA, XLNet, GPT-2
3. **Explore advanced techniques:** Prompt tuning, in-context learning, RAG
4. **Stay updated:** Read papers, follow research (Papers with Code, Twitter)
5. **Contribute:** Open source NLP projects

---

# **Resources**

- **HuggingFace:** https://huggingface.co/ (Models & library)
- **BERT Paper:** "BERT: Pre-training of Deep Bidirectional Transformers" (Devlin et al., 2018)
- **T5 Paper:** "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer" (Raffel et al., 2019)
- **Attention Paper:** "Attention Is All You Need" (Vaswani et al., 2017)

---

# **Congratulations!**

You've completed 10 weeks of comprehensive NLP training. You now understand:
- How transformers work (math & implementation)
- How transfer learning enables rapid development
- How to fine-tune models for specific tasks
- How to deploy models in production

**Now go build something amazing!** 🚀

The future of NLP is not about building models from scratch—it's about creatively applying pre-trained models to solve real problems. You have the foundation to do exactly that.
