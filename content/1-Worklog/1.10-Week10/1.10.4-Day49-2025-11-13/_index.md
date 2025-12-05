---
title: "Day 49 - T5: Text-to-Text Transfer Transformer"
weight: 4
chapter: false
pre: "<b> 1.10.4. </b>"
---

**Date:** 2025-11-13 (Thursday)  
**Status:** "Done"  

---

# **T5: The Ultimate Transfer Learning Model**

Published by Google in 2019, T5 (Text-to-Text Transfer Transformer) unified ALL NLP tasks into one framework.

The key insight: **Every NLP task can be phrased as text-to-text.**

---

# **The T5 Revolution**

## Before T5: Task-Specific Models

```
Sentiment Analysis:
Input: "I love this movie!"
Output: Positive (classification)
Model: bert-sentiment-specific

Machine Translation:
Input: "I love this movie!"
Output: "J'adore ce film!"
Model: marianmt-en-fr

Summarization:
Input: "The movie was great... [1000 words]"
Output: "Great movie."
Model: bart-summarization

Question Answering:
Input: "What is the capital?" + Wikipedia passage
Output: "Paris" (span extraction)
Model: bert-qa-specific

Problem: Different input/output types, different models!
```

## After T5: Unified Framework

```
Sentiment Analysis:
Input:  "sentiment: I love this movie!"
Output: "positive"

Machine Translation:
Input:  "translate English to French: I love this movie!"
Output: "J'adore ce film!"

Summarization:
Input:  "summarize: The movie was great... [1000 words]"
Output: "Great movie."

Question Answering:
Input:  "question: What is the capital? context: Paris is..."
Output: "Paris"

Solution: ONE model, ONE architecture, ONE training procedure!
Just change the task prefix!
```

---

# **T5 Architecture**

## Encoder-Decoder Transformer

```
T5 (Both Fully Bidirectional):

Input: "translate English to French: Hello world"
       ↓
Encoder (12 layers of bidirectional transformers)
├─ Layer 1: Self-attention on all tokens
├─ Layer 2: Self-attention on all tokens
├─ ...
├─ Layer 12: Self-attention on all tokens
└─ Output: Contextual representations
       ↓
Decoder (12 layers, can attend to encoder)
├─ Layer 1: Self-attention (masked), Encoder-attention
├─ Layer 2: Self-attention (masked), Encoder-attention
├─ ...
├─ Layer 12: Self-attention (masked), Encoder-attention
└─ Output: "Bonjour le monde"

Key Difference from BERT:
├─ BERT: Encoder only (understands)
├─ T5: Encoder + Decoder (understands and generates)
└─ GPT: Decoder only (generates)
```

## Model Sizes

```
T5-small: 60 million parameters
├─ Encoder: 6 layers, hidden size 512
├─ Decoder: 6 layers, hidden size 512
└─ Fast, small memory footprint

T5-base: 220 million parameters
├─ Encoder: 12 layers, hidden size 768
├─ Decoder: 12 layers, hidden size 768
└─ Good balance

T5-large: 770 million parameters
├─ Encoder: 24 layers, hidden size 1024
├─ Decoder: 24 layers, hidden size 1024
└─ High performance

T5-3B and T5-11B: Even larger
├─ Used for very challenging tasks
└─ Impressive performance on everything
```

---

# **The Task Prefix System**

The genius of T5: **Simple task prefixes guide the model**

```
Task Prefix Examples:

1. Classification (Sentiment):
   Input:  "sentiment: I loved this movie!"
   Output: "positive"

2. Translation:
   Input:  "translate English to French: Hello"
   Output: "Bonjour"

3. Summarization:
   Input:  "summarize: Long document... [500 words] ...end"
   Output: "Summary of key points"

4. Question Answering:
   Input:  "question: Who wrote Hamlet? context: Shakespeare..."
   Output: "Shakespeare"

5. Paraphrasing:
   Input:  "paraphrase: The cat is on the mat."
   Output: "A feline rests on the floor mat."

6. Entailment:
   Input:  "nli: A person is riding a bike. A man cycles."
   Output: "entailment" or "neutral" or "contradiction"

7. Zero-shot Classification:
   Input:  "znli: I liked it. premise: The speaker liked something."
   Output: "entailment"

8. Multiple Choice:
   Input:  "multiple_choice: Paris is the capital of?
           (A) France (B) Germany (C) Italy"
   Output: "A"

Why this works:
├─ Same model learns all these patterns
├─ Task prefix acts as instruction
├─ Scalable to any text-in/text-out task
└─ Elegant!
```

---

# **T5 Pre-training: SPAN CORRUPTION**

T5 introduced a new pre-training objective: **Span Corruption**

## How It Works

```
Original: "The quick brown fox jumps over the lazy dog."

Span Corruption Process:

Step 1: Randomly select spans to corrupt
├─ Select 15% of tokens
├─ Group into spans
└─ Example: [quick brown] and [lazy]

Step 2: Replace spans with sentinel tokens
├─ "The [X] fox jumps over the [Y] dog."
├─ [X] is a unique placeholder for first span
├─ [Y] is a unique placeholder for second span

Step 3: Predict the missing spans
Input:   "The [X] fox jumps over the [Y] dog."
Output:  "[X] quick brown [Y] lazy [Z]"
         (including start token [Z])

Task: Reconstruct corrupted spans!
```

## Why Span Corruption > MLM

```
BERT's MLM:
├─ Mask individual tokens: "The [MASK] brown [MASK] jumps"
├─ Predict each independently
├─ Problem: Easier than real text corruption
└─ Tokens are not correlated

T5's Span Corruption:
├─ Corrupt word-level spans: "The [X] [Y] fox jumps"
├─ Generate entire spans
├─ Advantage: Harder, more realistic
└─ Spans are correlated (important for generation!)

Result: T5 is better at generation tasks!
```

## Mathematical Formulation

```
Span corruption loss:

For each corrupted span s with length L:

Loss = -∑(log P(y_i | y_<i, x))

Where:
├─ y_i = token i of the corrupted span
├─ y_<i = previously generated tokens
├─ x = input with corrupted spans
└─ Sum over all tokens in span

This is essentially language modeling loss
But on spans instead of individual tokens!
```

---

# **T5 Pre-training Data**

## The C4 Corpus

T5 was trained on **C4: Colossal Clean Crawled Corpus**

```
C4 Statistics:
├─ Source: 750 billion web documents
├─ Size: 800 GB of text
├─ 200 billion tokens
├─ Processed: Deduplicated, filtered, cleaned
├─ Languages: Primarily English (but multilingual versions exist)

How it was created:
├─ Take Common Crawl (web snapshots)
├─ Filter: Remove bad content
├─ Clean: Fix HTML, remove boilerplate
├─ Deduplicate: Remove near-duplicates
├─ Result: High-quality 800GB corpus!

Comparison:
├─ BERT pre-training: 3.3 billion word pieces (~13GB)
├─ T5 pre-training: 200 billion tokens (~800GB)
└─ T5 trained on 60x more data!
```

---

# **Multi-task Pre-training**

T5 introduced pre-training on **multiple tasks simultaneously**

```
During pre-training, T5 sees diverse objectives:

50% Span Corruption (main task):
├─ "The [X] fox jumps over [Y] dog"
├─ Predict: "[X] quick brown [Y] lazy"

50% Task-specific objectives:
├─ Translate (15%): "translate en to fr: Hello"
├─ Summarize (15%): "summarize: Article... "
├─ QA (15%): "question: What... context: ..."
├─ Other tasks (5%): Various NLP tasks

Why?
├─ Exposes model to task diversity early
├─ Better transfer to downstream tasks
├─ Model learns that same weights work for multiple things
└─ Results in more generalizable representations
```

---

# **T5 Comparison with BERT**

```
                T5              BERT
                ──              ────
Architecture    Enc-Dec         Enc only
Mask Type       Span            Token
Pre-train Data  800GB (C4)       13GB (Wiki+Books)
Scale           220M-11B         110M-340M
Generation      Excellent       Can't generate
Understanding   Good            Excellent
Training Time   ~31 days (TPU)  ~4 days (TPU)
Best For        Generation      Understanding
Flexibility     Text-to-text    Task-specific fine-tuning

Which to use?
├─ Translation: T5 (handles sequences better)
├─ Classification: BERT (faster, simpler)
├─ Summarization: T5 (generation task)
├─ Sentiment: BERT (classification task)
├─ QA: Both work, but T5 is more flexible
```

---

# **T5 Performance**

## GLUE Benchmark (Understanding)

```
Task                          T5-base    Best Previous
Sentiment (SST-2)             94.9%      92.1%
Similarity (STS-B)            89.2%      87.1%
Inference (RTE)               93.5%      91.0%
Question Answering (QNLI)     95.1%      93.2%
Paraphrase (MRPC)             89.2%      87.1%

Average improvement: +2-3%
```

## BLEU Score (Generation)

```
Task                          T5-base    Best Previous
English to German             28.4       23.1
English to French             41.0       35.2
English to Romanian           34.2       29.1

T5 is much better at generation!
```

---

# **T5 Variants**

```
T5
├─ T5-small (60M)
├─ T5-base (220M)
├─ T5-large (770M)
├─ T5-3B
├─ T5-11B
└─ mT5 (multilingual, 13 variants)

Each scales to different trade-offs:
├─ Smaller = Faster, less memory
└─ Larger = Better performance, more parameters
```

---

# **Why T5 Matters**

✅ **Unification:** One model, one architecture, many tasks
✅ **Text-to-text simplicity:** Don't design task-specific architectures
✅ **Span corruption:** Better pre-training objective
✅ **Large data:** 800GB corpus shows scaling benefits
✅ **Flexible:** Can tackle any generation task
✅ **Strong baselines:** Best performance on many benchmarks

---

# **Key Takeaways**

1. **Task Prefixes:** Simple but powerful way to guide models
2. **Span Corruption:** Better than token masking for generation
3. **Large Pre-training Data:** More data = better transfer
4. **Encoder-Decoder:** Perfect for seq2seq tasks
5. **Unified Framework:** Simplifies NLP systems

---

# **Evolution of Transfer Learning**

```
Word2Vec (2013) → ELMo (2015) → BERT (2018) → T5 (2019) → GPT-3 (2020) → Present

Scale:        Simple → Medium → Large → Very Large → Huge → Massive
Architecture: Embeddings → BiLSTM → BiTransformer → Enc-Dec → Dec-only
Data:         Millions → Billions → 13GB → 800GB → 570GB → Trillions?
Performance:  Basic → Good → Excellent → Better → Amazing → Superhuman?
```

---

# **Next: Fine-tuning**

- **Day 50:** How to take T5 (or BERT) and fine-tune for specific tasks

T5 is the **ultimate generalist** model. With just a task prefix and 100-1000 examples, you can build systems that work better than specialist models from 2 years ago!
