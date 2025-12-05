---
title: "Day 47 - BERT Architecture"
weight: 2
chapter: false
pre: "<b> 1.10.2. </b>"
---

**Date:** 2025-11-11 (Tuesday)  
**Status:** "Done"  

---

# **BERT: Bidirectional Encoder Representations from Transformers**

BERT was the breakthrough that changed NLP forever. Published by Google in 2018, it showed that **bidirectional context** matters more than anyone thought.

---

# **What's Different About BERT?**

## The Evolution

```
Word2Vec (2013)
├─ Embedding: "cat" → [0.2, -0.5, 0.8, ...]
├─ Problem: Same embedding everywhere, no context
└─ Approach: Simple, static

↓

ELMo (2015)
├─ BiLSTM: Process left AND right
├─ Problem: Still sequential, slower
└─ Approach: Bidirectional but not parallel

↓

GPT (2018)
├─ Transformer: Parallel attention!
├─ Problem: Only looks LEFT (left-to-right)
│           "The bank was robbed" → Can't use "robbed" to understand "bank"
└─ Approach: Unidirectional, limited

↓

BERT (2018) ⭐
├─ Transformer: Parallel AND bidirectional!
├─ Solution: Sees BOTH sides of each word
│           "The bank was robbed" → Uses both "The" and "was robbed" to understand "bank"
└─ Approach: Full bidirectional context
```

---

# **BERT Architecture Overview**

## Model Specs

```
BERT-base:
├─ Transformer layers: 12
├─ Hidden size: 768
├─ Attention heads: 12 (768/12 = 64 per head)
├─ Feed-forward size: 3072
├─ Total parameters: 110 Million
├─ Training time: 4 days on 16 TPUs
└─ Training data: 3.3 billion word pieces from Wikipedia + BookCorpus

BERT-large:
├─ Transformer layers: 24
├─ Hidden size: 1024
├─ Total parameters: 340 Million
└─ Much slower but higher performance
```

## Encoder-Only Architecture

```
BERT vs Transformer:

Transformer (Full):
Input → Encoder (6 layers) → Decoder (6 layers) → Output
        (Can see all)       (Masked, can't see future)

BERT (Encoder Only):
Input → Encoder (12 layers) → Output
        (Full bidirectional attention!)

        
Key Difference:
├─ GPT = Encoder + Decoder but decoder is unidirectional
├─ BERT = Encoder only, fully bidirectional
└─ T5 = Encoder + Decoder, both bidirectional
```

---

# **The Self-Attention Mechanism (Review)**

Remember from Day 43:

```
For each word, create three vectors:
├─ Query (Q): "What information do I need?"
├─ Key (K): "What information do I provide?"
└─ Value (V): "What's my actual representation?"

Attention Score = softmax(Q·K^T / √d) · V

Example: Processing "bank" in "The bank was robbed"

Query for "bank": [0.1, 0.2, 0.3, ...]
Keys for all words:
├─ "The" key:  [0.2, 0.1, 0.1, ...]  → Score: 0.7
├─ "bank" key: [0.1, 0.2, 0.3, ...]  → Score: 0.95 (high! self-attention)
├─ "was" key:  [0.5, 0.2, 0.1, ...]  → Score: 0.8
└─ "robbed" key: [0.3, 0.3, 0.1, ...]  → Score: 0.85

Notice: "robbed" helps understand "bank"!
This is why BERT is bidirectional.
```

---

# **BERT vs GPT: The Key Difference**

## Attention Masking

### GPT (Left-to-Right, Decoder-style)

```
Processing: "The bank was robbed"

Position 1 ("The"):
├─ Can attend to: "The" ✓
├─ Can attend to: "bank" ✗ (masked, future)
├─ Can attend to: "was" ✗ (masked, future)
└─ Can attend to: "robbed" ✗ (masked, future)

Position 4 ("robbed"):
├─ Can attend to: "The" ✓
├─ Can attend to: "bank" ✓
├─ Can attend to: "was" ✓
└─ Can attend to: "robbed" ✓

Problem: "The" can't use "robbed" to understand its meaning!
```

### BERT (Bidirectional, Encoder-style)

```
Processing: "The bank was robbed"

Position 1 ("The"):
├─ Can attend to: "The" ✓
├─ Can attend to: "bank" ✓
├─ Can attend to: "was" ✓
└─ Can attend to: "robbed" ✓

Position 4 ("robbed"):
├─ Can attend to: "The" ✓
├─ Can attend to: "bank" ✓
├─ Can attend to: "was" ✓
└─ Can attend to: "robbed" ✓

Benefit: "The" CAN use "robbed" to understand context!
Better representations for ALL words!
```

---

# **Why Bidirectional Is Better**

## Language Understanding Example

```
Sentence: "I went to the bank to deposit money"
Word: "bank" (financial institution)

GPT Processing (Left-to-Right):
├─ Can see: "I went to the"
├─ Cannot see: "to deposit money" (future!)
├─ Context understanding: 30% (incomplete)

BERT Processing (Bidirectional):
├─ Can see: "I went to the" + "to deposit money"
├─ Full context: 100%
├─ Context understanding: 90% (much better!)
```

## Word Sense Disambiguation

```
"I went to the bank to deposit money"      → Financial institution
"The river bank was very scenic"           → Land along river
"The bank was robbed yesterday"             → Company/institution

GPT: Sees left context only
├─ "I went to the" (incomplete)
├─ Risk: Wrong sense!

BERT: Sees full context
├─ "I went to the _____ to deposit money"
├─ Automatically disambiguates! ✓
```

---

# **BERT's Input Representation**

## Token Embeddings

```
BERT uses WordPiece tokenization:

Input: "The bank was robbed"

Tokenization:
├─ "The" → [101] (CLS token adds to beginning)
├─ "bank" → [1998]
├─ "was" → [2003]
├─ "robbed" → [6861]
├─ "[SEP]" → [102] (separator token adds to end)

Raw tokens: [101, 1998, 2003, 6861, 102]
```

## Three Types of Embeddings

```
BERT combines THREE embedding types:

Token Embedding: "bank" → [0.2, -0.5, 0.8, ...]
├─ Specific word representation
├─ Learned during pre-training

Segment Embedding: Sentence A vs Sentence B → [0.1, 0.2, ...]
├─ Marks which sentence each token belongs to
├─ For sentence pair tasks (Next Sentence Prediction)

Positional Embedding: Position 2 → [0.5, 0.1, ...]
├─ Encodes position in sequence
├─ 0, 1, 2, 3, 4...

Final Embedding = Token + Segment + Positional
                = [0.2, -0.5, 0.8, ...] + [0.1, 0.2, ...] + [0.5, 0.1, ...]
                = [0.8, -0.2, 1.3, ...]
```

---

# **Comparison: BERT vs GPT vs Original Transformer**

```
                BERT          GPT              Transformer
                ────          ───              ─────────────
Encoder-Decoder Encoder only  Encoder+Decoder  Encoder+Decoder
Attention Type  Bidirectional Unidirectional   Mixed
Pre-train Task  MLM + NSP     Language Model   None (N/A)
Direction       Both ways     Left to right    Decoder masked
Best For        Understanding Generation      Translation
Performance     Best (90%)    Good (85%)       Varies (varies%)
Speed           Medium        Fast             Slowest
Parameters      110M/340M     117M/345M        ~100M
```

---

# **BERT's Performance on NLP Tasks**

## GLUE Benchmark Results

```
Before BERT (State-of-the-art 2017):
├─ Sentiment: 92.1% accuracy
├─ Textual Similarity: 80.5% Spearman
├─ Named Entity: 91.6% F1
└─ Question Answering: 78.2% F1

BERT-base:
├─ Sentiment: 94.9% accuracy (+2.8%)
├─ Textual Similarity: 85.8% Spearman (+5.3%)
├─ Named Entity: 96.4% F1 (+4.8%)
└─ Question Answering: 84.2% F1 (+6.0%)

Improvement: 4-6% across the board!
For a deployed system, that's huge!
```

---

# **Why BERT Became So Popular**

1. **Simple Fine-tuning:** Add one classification head, fine-tune all layers
2. **Strong Performance:** Beats all previous methods by large margins
3. **Pretrained Weights:** Can download weights, use immediately
4. **Multilingual:** BERT trained on 104 languages (mBERT)
5. **Transferable:** Works for almost any NLP task

---

# **BERT's Limitations**

⚠️ **Bidirectional = Can't generate text directly**
```
Problem: You can't do "given 'The bank', generate the rest"
Reason: Each token can see the future!
Would collapse the generation task.
Solution: Fine-tune for classification, not generation
```

⚠️ **Fixed context window: 512 tokens maximum**
```
Long documents don't fit!
Solution: Hierarchical BERT or RoBERTa (4096 tokens)
```

⚠️ **Computationally expensive**
```
110M parameters = 440MB just for weights
Inference slower than smaller models
Solution: Distillation (DistilBERT, 40% parameters, 60% speed)
```

---

# **Variants of BERT**

```
BERT
├─ BERT-base: Original, 110M params
├─ BERT-large: Bigger, 340M params
├─ mBERT: 104 languages, multilingual
├─ DistilBERT: Distilled, 40M params, faster
└─ RoBERTa: Improved training, 125M params

All based on same encoder-only architecture
```

---

# **Key Takeaways**

✅ **Bidirectional context matters:** BERT's breakthrough insight
✅ **Encoder-only architecture:** Perfect for understanding
✅ **Pre-trained weights are gold:** Don't train from scratch!
✅ **Self-attention with full context:** Better representations
✅ **Simple fine-tuning:** Add head + train 1-3 epochs

---

# **What's Coming Next**

- **Day 48:** How BERT learned this understanding (MLM + NSP)
- **Day 49:** T5 - Scaling up to encoder-decoder
- **Day 50:** Fine-tuning BERT for real tasks

The beauty of BERT: Bidirectional transformers showed that **seeing everything at once** is the key to understanding language.
