---
title: "Day 40 - Search Intent & Bag of Words"
weight: 5
chapter: false
pre: "<b> 1.8.5. </b>"
---

**Date:** 2025-10-31 (Friday)  
**Status:** "Done"  

---

# **Search Intent Recognition Engine**

## Example: Coffee Table Search

When someone searches for **"glass coffee tables"**, the intent engine performs the following steps:

### Step 1: Parse the Query
The system identifies key terms:
- **"glass"** → Material attribute
- **"coffee tables"** → Product category
- **"tables"** → Object type

### Step 2: Semantic Mapping
The term **"glass"** is mapped to the attribute **'Top Material'** in the coffee tables category through semantic understanding and historical search patterns.

### Step 3: Filter & Rank Results
The search engine directs queries to the coffee tables category with:
- **Top Material:** glass (filtered)
- Results ranked by relevance, rating, and popularity

### Step 4: Return Results
Users see coffee tables specifically with glass tops, improving search accuracy and user satisfaction.

---

# **Bag of Words (BoW) Model Fundamentals**

## What is Bag of Words?

The **Bag of Words** model is a foundational technique in NLP that:
- Converts text into a **vector representation**
- Counts the **frequency** of each word
- **Ignores word order** and grammar
- Focuses on **word presence and frequency**

## BoW Process:

### 1. Tokenization
Break text into individual words:
- "I love machine learning" → ["I", "love", "machine", "learning"]

### 2. Vocabulary Building
Create a unique list of all words across documents

### 3. Vector Creation
Represent each document as a vector where:
- Each position = a unique word
- Each value = frequency or presence (0/1)

### 4. Example Vector:
```
Document: "machine learning is great"
Vocabulary: [machine, learning, is, great, I, love]
Vector: [1, 1, 1, 1, 0, 0]
```

## Advantages:
- Simple and interpretable
- Fast to compute
- Works well for text classification
- Effective baseline for many NLP tasks

## Limitations:
- Loses word order information
- Doesn't capture semantics or context
- Treats rare and common words equally
- Struggles with synonyms

---

# **Summary**

This week covered the linguistic foundations of NLP and practical applications. Key takeaways:
- NLP enables computers to understand human language
- Core linguistic components (phonetics to pragmatics) form the foundation
- Real-world applications include search, translation, and dialogue systems
- Bag of Words is a simple but effective starting point for text processing
