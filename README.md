# 🧠 AI Customer Intelligence for Hotel Services

![Python](https://img.shields.io/badge/Python-3.10-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red)
![Transformers](https://img.shields.io/badge/HuggingFace-Transformers-yellow)
![LangChain](https://img.shields.io/badge/LangChain-RAG-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

# 📌 Project Overview

This project explores how **Artificial Intelligence and Natural Language Processing (NLP)** can improve customer experience in the hospitality industry.

The system combines several AI techniques to build an intelligent assistant capable of:

- analyzing customer reviews
- identifying key service themes
- answering operational questions
- retrieving information from hotel documentation

The project is built around the fictional **Hôtel De la Promenade**.

---

# 🎯 Project Objectives

The project focuses on three main objectives:

1. **Understand customer feedback**
2. **Create a specialized hotel assistant**
3. **Improve factual accuracy using RAG**

To achieve this, the project integrates:

- Thematic analysis
- Large Language Models
- Fine-tuning
- Retrieval-Augmented Generation
- Prompt engineering
  
---


# 📊 1. Thematic Analysis of Customer Reviews

Customer reviews were analyzed to identify the **main themes influencing customer satisfaction**.

## Methodology

The analysis included:

- text preprocessing
- stopword removal
- keyword extraction
- grouping similar terms into themes

## Main Themes Identified

| Theme | Description |
|------|-------------|
| Cleanliness | Room and facility cleanliness |
| Service Quality | Staff behavior and responsiveness |
| Location | Accessibility and nearby attractions |
| Comfort | Room quality and bed comfort |
| Facilities | Pool, parking, and hotel services |

## Impact

This analysis helps hotels:

- identify strengths
- detect improvement areas
- better understand customer expectations

---

# 🤖 2. Fine-Tuning a Large Language Model

A **Meta-Llama-3.1-8B-Instruct model** was fine-tuned using **LoRA (Low-Rank Adaptation)**.

## Training Dataset

The dataset contains a **structured FAQ about hotel operations** including:

- check-in policies
- parking rules
- guest services
- operational procedures

## Training Configuration

| Parameter | Value |
|-----------|------|
| Base Model | Meta-Llama-3.1-8B-Instruct |
| Quantization | 4-bit |
| LoRA Rank | 16 |
| Learning Rate | 1e-4 |
| Epochs | 3 |
| Optimizer | AdamW 8-bit |

## Goal

Fine-tuning allows the assistant to:

- adopt the hotel's communication style
- specialize in hotel-related questions
- generate more relevant responses

---

# 🔎 3. Retrieval-Augmented Generation (RAG)

To improve factual accuracy, a **RAG pipeline** was implemented.

Instead of relying solely on the model’s internal knowledge, the system retrieves relevant information from hotel documents before generating responses.

## RAG Pipeline

User Question
↓
Text Embedding
↓
Vector Search (Chroma)
↓
Relevant Document Chunks
↓
LLM Generation
↓
Final Answer


## Benefits

- reduces hallucinations
- ensures factual grounding
- allows easy knowledge updates

---

# 🧠 4. Prompt Engineering

Different prompt strategies were tested to evaluate their impact on model performance.

## Prompt Strategies

| Prompt Type | Description |
|-------------|-------------|
| Zero-Shot | Instructions only |
| Few-Shot | Includes examples |
| Chain-of-Thought | Encourages reasoning |

## Observations

| Prompt | Result |
|------|--------|
| Zero-Shot | concise responses |
| Few-Shot | better structure |
| Chain-of-Thought | more detailed explanations |

---

# 📈 5. Model Evaluation

The **base model** and the **fine-tuned model** were compared using several test scenarios.

## Evaluation Criteria

- factual accuracy
- style alignment
- hallucination rate
- completeness of answers

## Comparison

| Aspect | Base Model | Fine-Tuned Model |
|------|-------------|----------------|
| Style | Generic | Hotel-specific |
| Accuracy | Moderate | Slight improvement |
| Hallucinations | Present | Still present |
| Detail | Basic | More structured |

Fine-tuning improved **tone and domain alignment**, but hallucinations still occur.

---

# ⚖️ RAG vs Fine-Tuning

| Approach | Advantages | Limitations |
|--------|-------------|-------------|
| Fine-Tuning | Style adaptation | Possible hallucinations |
| RAG | Factual accuracy | Retrieval dependency |
| Combined | Best performance | More complex pipeline |

The best solution combines:

**Fine-Tuning for style**  
+  
**RAG for factual grounding**

---

# 🛠 Technologies Used

- Python
- PyTorch
- Hugging Face Transformers
- Unsloth
- LangChain
- ChromaDB
- Sentence Transformers
- Jupyter Notebook

---

# 🚀 Key AI Concepts Demonstrated

This project demonstrates several important AI concepts:

- Large Language Models (LLMs)
- Parameter-efficient fine-tuning (LoRA)
- Retrieval-Augmented Generation
- Prompt engineering
- Text analytics
- Thematic analysis

---

# 🔮 Future Improvements

Potential improvements include:

- larger training datasets
- improved document chunking
- better retrieval strategies
- integration into a real hotel chatbot

---

# 👤 Author

**Ibrahima Sory Traore**

ML Engineer | AI Enthusiast | BI Developer  
📍 Canada

---
