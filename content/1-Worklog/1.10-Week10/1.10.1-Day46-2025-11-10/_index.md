---
title: "Day 46 - Transfer Learning Fundamentals"
weight: 1
chapter: false
pre: "<b> 1.10.1. </b>"
---

**Date:** 2025-11-10 (Monday)  
**Status:** "Done"  

---

# **Transfer Learning: Why It Matters**

Transfer learning is **THE most important technique** in modern NLP. It's why a model trained on Wikipedia can be fine-tuned in days instead of months.

---

# **The Classical Approach vs Transfer Learning**

## Classical Machine Learning

```
Task: Predict movie rating from review

Step 1: Collect labeled data (movies + ratings)
Step 2: Initialize model randomly
Step 3: Train from scratch for weeks/months
Step 4: Deploy

Problem: Slow and requires lots of labeled data
```

## Transfer Learning Approach

```
Task: Predict movie rating from review

Step 1: Use pre-trained model (trained on 800GB of text)
Step 2: Fine-tune on your small dataset (100 examples)
Step 3: Train for hours/days
Step 4: Deploy with better performance!

Benefit: Fast and works with little data
```

---

# **Two Forms of Transfer Learning**

## 1. Feature-Based Transfer Learning

**Concept:** Use pre-trained embeddings as fixed features

**Example:**
```
Pre-training Task: Learn word2vec embeddings from Wikipedia

Step 1: Train Word2Vec on Wikipedia
  "The cat sat on the mat"
  Learns: embedding["cat"] = [0.2, -0.5, 0.8, ...]

Step 2: Use these embeddings for a different task
  Task: Sentiment analysis on product reviews
  
  Review: "Great product!"
  Embed: "Great" → [0.1, 0.3, 0.9, ...]
         "product" → [0.5, -0.2, 0.1, ...]
  
  Feed embeddings to simple classifier
```

**Pros:**
- Simple to implement
- Fast inference (pre-computed embeddings)
- Works with small downstream models

**Cons:**
- Fixed embeddings don't adapt to new domain
- Context not captured well (same embedding for "bank" everywhere)

---

## 2. Fine-tuning Transfer Learning

**Concept:** Use pre-trained model weights and update them for new task

**Example:**
```
Pre-training Task: Predict next word on Wikipedia (Language Modeling)

Step 1: Train transformer on 800GB Wikipedia data
  Model learns: linguistic patterns, world knowledge, grammar

Step 2: Fine-tune on downstream task
  Movie Review → Rating Prediction
  
  Option A: Update all layers
    ├─ Last layer: Fine-tune (most task-specific)
    ├─ Middle layers: Fine-tune (some task-specific)
    └─ First layers: Fine-tune (general language)
    
  Option B: Freeze some layers, train new ones
    ├─ Layer 1-10: FROZEN (keep pre-trained weights)
    ├─ Layer 11-12: Fine-tune
    └─ New classification head: Train from scratch
```

**Pros:**
- Weights adapt to new task
- Better performance than feature-based
- Fast convergence

**Cons:**
- Computational cost (need to update all parameters)
- Risk of catastrophic forgetting

---

# **Pre-training Data vs Downstream Task**

## The Scale Difference

```
Pre-training: 800 GB of text
├─ Wikipedia: 13 GB
├─ Books: 200 GB
├─ Web crawl: 500+ GB
└─ Total knowledge learned: Massive

Downstream: 100 - 10,000 examples
├─ Movie reviews: 5,000 examples
├─ Medical texts: 1,000 examples
└─ Customer feedback: 500 examples
```

**Key Insight:** Pre-training has 80,000x more data than typical downstream tasks! This explains why transfer learning works so well.

---

# **Pre-training Objectives**

### Objective 1: Language Modeling

**Task:** Predict next word given previous words

```
Input: "The quick brown"
Output: "fox"

Loss: Cross-entropy between predicted and actual next word

Example:
"Learning from deeplearning.ai is like watching the _____"
Model predicts: "sunset"
Target: "sunset"
Loss = 0 (perfect prediction!)
```

**Why it works:**
- Model learns grammar, syntax, semantics
- Model learns common patterns
- Model learns world knowledge

### Objective 2: Masked Language Modeling

**Task:** Predict masked (hidden) words

```
Input: "The quick [MASK] fox"
Output: "brown"

This is what BERT uses!

More complex:
"The [MASK] brown [MASK] fox [MASK]"
Predict all three: "quick", "jumps", "here"
```

---

# **Advantages of Transfer Learning**

## 1. Reduce Training Time

```
Classical Training:        Transfer Learning:
┌─────────────────────┐   ┌──────────┐
│ Pre-training: 3 mo  │   │ Pre-train│ (already done!)
│ (From scratch!)     │   │ (reuse)  │
│                     │   └──────────┘
│ Fine-tuning: 1 mo   │   ┌──────────┐
│ (On task data)      │   │Fine-tune │ 1-7 days!
│                     │   │ (on task)│
│ Total: 4 MONTHS     │   └──────────┘
└─────────────────────┘   
                          Total: ~1 WEEK
```

Speedup: **15-30x faster!**

## 2. Improve Predictions

```
Small dataset (100 examples) with classical approach:
├─ Model overfits (memorizes the 100 examples)
├─ 60% accuracy on test set
└─ Essentially useless

Small dataset (100 examples) with transfer learning:
├─ Start from pre-trained model (knows language!)
├─ Fine-tune carefully
├─ 85% accuracy on test set
└─ Much better!

Improvement: 25% better!
```

## 3. Use Smaller Datasets

```
Classical: "You need 10,000 labeled examples"
Transfer:  "100-1000 labeled examples is enough!"

Why?
  Pre-trained model already knows language
  You just need to teach it your specific task
  Less data needed
```

---

# **Pre-training vs Fine-tuning Data**

## Labeled vs Unlabeled Data

```
Unlabeled Data (used in pre-training):
"The quick brown fox jumps over..."
(No labels needed! Just raw text)

Labeled Data (used in fine-tuning):
"The quick brown fox jumps over..." → Positive (label)
(Requires manual annotation)

Insight: 
Unlabeled data >> Labeled data in quantity
Pre-training can use trillions of tokens!
```

---

# **Transfer Learning Strategy**

### Strategy 1: Light Fine-tuning
```
Freeze most layers, train only last layer
├─ Use case: Large pre-trained model + small dataset
├─ Layers frozen: 1-11
├─ Layers trained: 12 + classification head
├─ Training time: 1-2 days
└─ Risk of underfitting: Low
```

### Strategy 2: Full Fine-tuning
```
Update all layers
├─ Use case: Medium pre-trained model + medium dataset
├─ Layers frozen: None
├─ Layers trained: All 12 + classification head
├─ Training time: 3-7 days
└─ Risk of overfitting: Medium
```

### Strategy 3: Progressive Unfreezing
```
Unfreeze layers gradually
├─ Day 1: Unfreeze last layer, train
├─ Day 2: Unfreeze last 2 layers, train
├─ Day 3: Unfreeze last 3 layers, train
├─ ...
├─ Training time: 7+ days
└─ Best performance: Often!
```

---

# **When Transfer Learning Fails**

❌ **Domain mismatch:** Pre-training on English, fine-tune on Chinese
❌ **Catastrophic forgetting:** Update all weights too aggressively
❌ **Too much fine-tuning:** Use too high learning rate
❌ **Poor feature extraction:** Pre-train task too different from downstream

---

# **Historical Timeline**

```
2013: Word2Vec
└─ First successful transfer learning
└─ Simple word embeddings

2015: ELMo
└─ Bidirectional LSTM
└─ Context-aware embeddings

2018: GPT
└─ Transformer-based language model
└─ Unidirectional (left-to-right)

2018: BERT
└─ Bidirectional transformers!
└─ Masked language modeling
└─ Huge performance improvements

2019: T5
└─ Text-to-text transfer transformer
└─ Multi-task learning
└─ State-of-the-art on everything!

2020+: GPT-2, GPT-3, RoBERTa, ELECTRA, XLNet...
└─ Scaling laws discovered
└─ Bigger models → Better performance
```

---

# **Key Takeaways**

✅ **Transfer learning is the standard:** Most NLP done today uses it
✅ **Pre-training is crucial:** 800GB of pre-training >> 10KB of fine-tuning
✅ **Speed advantage is massive:** Days instead of months
✅ **Data efficiency:** Works with far less labeled data
✅ **Domain adaptation:** Can transfer across languages, domains, tasks

---

# **What We'll Learn Next**

- **BERT:** How bidirectional context improves understanding
- **MLM:** Masked language modeling pre-training
- **T5:** Text-to-text framework for all NLP tasks
- **Fine-tuning:** Practical tricks for downstream tasks

This foundation (transfer learning) is why one person can now build state-of-the-art NLP systems in weeks!
