---
title: "Day 48 - BERT Pre-training Objectives"
weight: 3
chapter: false
pre: "<b> 1.10.3. </b>"
---

**Date:** 2025-11-12 (Wednesday)  
**Status:** "Done"  

---

# **How BERT Actually Learned**

BERT wasn't just a lucky guess. Google scientists designed two specific pre-training tasks that made it learn bidirectional understanding. These tasks are **brilliantly simple**.

---

# **Pre-training Task 1: Masked Language Modeling (MLM)**

## The Core Idea

**Hide random words and predict them.**

```
Original: "The quick brown fox jumps over the lazy dog"
Masked:   "The quick [MASK] fox jumps over the lazy dog"
Task:     Predict what [MASK] is!
Answer:   "brown"

Why this works:
├─ Model must understand context from BOTH sides
├─ To predict "brown", need to know:
│  ├─ Left context: "The quick"
│  └─ Right context: "fox jumps over the lazy dog"
└─ Forces bidirectional learning!
```

## Masking Strategy

Not all tokens are masked the same way:

```
When we select a token to mask:

80% of the time: Replace with [MASK] token
├─ "The quick [MASK] fox"
├─ Model must learn the word
└─ Normal case

10% of the time: Replace with random word
├─ "The quick apple fox"  (instead of "brown")
├─ Model learns to correct noisy input
└─ Robustness training

10% of the time: Keep the original word
├─ "The quick brown fox"
├─ Model learns the representation anyway
└─ Helps with fine-tuning stability
```

## The Mathematics

```
For each token position i in a 15% masked vocabulary:

P(mask token at position i) = 0.15

Loss_MLM = -log(P(correct_word | context))

For masked position with word "brown":
├─ Model outputs: [0.02, 0.05, 0.88, 0.03, ...]  (probabilities for all words)
│                                  ↑ position of "brown"
├─ Correct word: "brown" (position 3)
├─ Loss = -log(0.88) = 0.128
└─ This pushes probability higher!

Aggregate: Average loss over all 15% masked tokens
```

## Example Walkthrough

```
Sentence: "The bank was robbed by masked men"
Tokens:   [The, bank, was, robbed, by, masked, men]
Mask 15%: [The, bank, [MASK], robbed, by, masked, men]

Step 1: Tokenize and embed all tokens
├─ Input IDs: [101, 1996, 2924, 103, 2001, 10122, 2039, 2095, 2273, 102]
├─ Token embeddings: 7 x 768 dimensional vectors
└─ Add positional embeddings: position 0, 1, 2, 3, ...

Step 2: Process through 12 transformer layers
├─ Layer 1: Self-attention across all positions
├─ Layer 2: Self-attention with updated representations
├─ ...
└─ Layer 12: Final contextual representations

Step 3: Predict masked token
├─ Output for position 2 (was): [0.02, 0.05, 0.88, 0.03, ...]
├─ argmax = 0.88 → Word ID 3 → "was"
├─ Correct! ✓
└─ Loss: small

Step 4: Backprop gradient through all 12 layers
└─ Updates all parameters to make this prediction likely
```

---

# **Pre-training Task 2: Next Sentence Prediction (NSP)**

## The Core Idea

**Given two sentences, predict if they're consecutive.**

```
Sentence A: "The bank was robbed yesterday."
Sentence B: "The police are investigating."
Question:   Are these consecutive sentences?
Answer:     Yes (IsNext) ✓

Sentence A: "The cat sat on the mat."
Sentence B: "I enjoy pizza for lunch."
Question:   Are these consecutive sentences?
Answer:     No (NotNext) ✗

Why this helps:
├─ Model learns to understand relationships between sentences
├─ Important for tasks like question answering
├─ Helps with semantic understanding
└─ But: Recent research shows this is actually less important than MLM!
```

## Input Format

```
BERT's special tokens for sentence pairs:

[CLS] Sentence A [SEP] Sentence B [SEP]
 ↑                 ↑                ↑
Token for          Separator       End of
classification     between         sequence
(used for NSP)     sentences

Segment IDs:
[0     0  0  0 0     0    1  1  1  1 1   1]
 CLS   The bank was robbed SEP police are investigating

Token A = 0 (first segment)
Token B = 1 (second segment)

CLS token's representation used for NSP classification!
```

## The Task

```
NSP is a binary classification problem:

Input: [CLS] ... [SEP] ... [SEP]
       ↓
Model outputs: [0.1, 0.9]
               ↑     ↑
              NotNext  IsNext
       
If label is IsNext: Loss = -log(0.9) = 0.105
If label is NotNext: Loss = -log(0.1) = 2.303

Model learns to distinguish consecutive sentences!
```

---

# **Combined Loss Function**

BERT trains on **both** objectives simultaneously:

```
Loss_total = Loss_MLM + Loss_NSP

Example iteration:
├─ Batch contains 32 sentence pairs
├─ For each pair:
│  ├─ 15% of tokens are masked → MLM loss
│  ├─ Sentence order labeled → NSP loss
│  └─ Both losses computed
├─ Average losses: 2.15 (MLM) + 1.45 (NSP)
├─ Total loss: 3.60
└─ Backprop updates all parameters

Why both?
├─ MLM forces bidirectional understanding
├─ NSP forces sentence-level semantics
└─ Together: Rich linguistic knowledge!

Note: Modern variants (RoBERTa) drop NSP
      (Found that MLM alone is sufficient!)
```

---

# **BERT's Input Embeddings (Deep Dive)**

## Three Embeddings Combined

```
BERT's embedding = Token + Segment + Position

Example: "The quick brown fox"
Position: 0    1     2      3

Token Embedding:
├─ "The" (ID=1996):    [0.2, -0.5, 0.1, ...]
├─ "quick" (ID=2522):  [0.1,  0.3, -0.2, ...]
├─ "brown" (ID=2829):  [-0.1, 0.2, 0.4, ...]
└─ "fox" (ID=4397):    [0.3,  0.1, -0.3, ...]

Segment Embedding (same sentence):
├─ Position 0: [0.1, 0.2, 0.3, ...]  (Segment A)
├─ Position 1: [0.1, 0.2, 0.3, ...]  (Segment A)
├─ Position 2: [0.1, 0.2, 0.3, ...]  (Segment A)
└─ Position 3: [0.1, 0.2, 0.3, ...]  (Segment A)

Positional Embedding:
├─ Position 0: [0.0, 0.5, -0.1, ...]
├─ Position 1: [1.0, 0.3,  0.2, ...]
├─ Position 2: [0.5, -0.1, 0.5, ...]
└─ Position 3: [-0.3, 0.6, 0.1, ...]

Final Embedding (sum all three):
├─ Position 0: [0.3,  0.7,  0.3, ...]
├─ Position 1: [1.1,  0.8,  0.0, ...]
├─ Position 2: [0.5,  0.3,  0.9, ...]
└─ Position 3: [0.0,  0.7, -0.2, ...]
```

## Positional Encoding Details

```
BERT uses sinusoidal positional encoding:

PE(pos, 2i)     = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1)   = cos(pos / 10000^(2i/d_model))

Where:
├─ pos = position in sequence (0, 1, 2, ...)
├─ i = dimension index (0, 1, 2, ...)
├─ d_model = 768 (hidden size)

For position 0:
├─ PE(0, 0) = sin(0) = 0.0
├─ PE(0, 1) = cos(0) = 1.0

For position 1:
├─ PE(1, 0) = sin(1 / 10000^0) = sin(1) = 0.841
├─ PE(1, 1) = cos(1 / 10000^0) = cos(1) = 0.540

Pattern: Different position → Different encoding!
Can't change two positions and keep encoding same!
```

---

# **Pre-training Data and Scale**

## Corpus

```
BERT Pre-training Corpus:

Wikipedia: 13 GB
├─ 2,500M words
├─ High quality, diverse topics
└─ But relatively small!

BookCorpus: Not publicly released but ~12 GB
├─ 800M words
├─ Books written by people → Good grammar
└─ Long documents (good for learning long-range dependencies)

Total: ~25 GB (3.3 billion word pieces)
├─ This is the secret sauce!
├─ Modern models use 100-1000x more data
└─ More data = Better performance (scaling laws)
```

## Training Details

```
BERT Pre-training:

Duration: 4 days (BERT-base)
Hardware: 16 TPUv2 devices
Batch size: 256 sequences (512 tokens each) = 131,072 tokens per batch
Learning rate: 1e-4 with warmup
Optimizer: Adam
Steps: ~1,000,000 steps

Computational cost:
├─ 16 TPUs × 4 days = 64 TPU-days
├─ ~$100,000+ in compute
└─ That's why we use pre-trained models now!

After training:
├─ Model can be used for fine-tuning
├─ Just add classification head!
└─ No need to train from scratch ever again
```

---

# **Why These Tasks Work So Well**

## MLM Advantages

✅ **Bidirectional learning:** Must use both sides
✅ **Natural task:** Similar to human reading
✅ **Flexible:** Can mask any proportion
✅ **Hard enough:** Non-trivial prediction

## NSP Advantages

✅ **Discourse understanding:** Learns sentence relationships
✅ **Pair tasks:** Helps with QA and NLI
✅ **Classification format:** Easy to extract feature for classification

## Combined

✅ **Multi-task learning:** Rich supervision signal
✅ **Complementary:** MLM + NSP teach different things
✅ **Efficient:** One training run, two learning signals

---

# **Comparison: Different Pre-training Objectives**

```
Word2Vec:
├─ Task: Predict neighboring words
├─ Result: Static embeddings
└─ Problem: Same embedding everywhere

ELMo:
├─ Task: Bidirectional language modeling
├─ Result: Context-dependent embeddings
└─ Problem: Still sequential processing

GPT:
├─ Task: Next-word prediction (left-to-right)
├─ Result: Good for generation
└─ Problem: Can't see future context

BERT (MLM + NSP):
├─ Task: Masked prediction + sentence order
├─ Result: Deep bidirectional understanding
└─ Advantage: Best of both worlds!

T5:
├─ Task: Text-to-text with multiple objectives
├─ Result: Unified framework for all tasks
└─ Advantage: Simpler than two separate losses
```

---

# **Key Takeaways**

✅ **MLM is the star:** Most important for performance
✅ **15% masking rate:** Empirically determined to be optimal
✅ **Masking strategy:** 80-10-10 makes model robust
✅ **NSP helps but is secondary:** Mainly for paired tasks
✅ **Pre-training is expensive:** But reusable!
✅ **Simple ideas, powerful results:** BERT's genius

---

# **What's Next**

- **Day 49:** How T5 scaled this idea (and dropped MLM+NSP)
- **Day 50:** Fine-tuning these pre-trained models

The pre-training objectives are the **foundation** of modern NLP. Everything downstream depends on these two simple tasks!
