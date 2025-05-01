---
layout: post
title: "Fake News Detection Using EmoLLM + LLaMA"
date: 2025-05-01
---

# 🧠 Fake News Detection Using EmoLLM + LLaMA 3.1

> How emotion-aware analysis and large language models helped us identify misinformation better.

---

## 🔍 Introduction

Misinformation is everywhere — social media, websites, news portals. But fake news detection is still hard. Many claims *look* believable. Traditional fake news detection methods rely heavily on surface-level textual cues. The existing models trained specifically for this problem involve either complex fine-tuning or do not take context into account when classifying news as fake or real. That’s why we combined emotional and sentiment analysis with large language models to to deepen semantic understanding and improve classification accuracy.

---
## 🔍 Related Works
- RaemoLLM: Uses emotion-aware LLMs to construct a retrieval-based affective embedding database for misinformation detection.
- LEMMA: A multimodal framework that enhances large vision-language models (LVLMs) by incorporating external knowledge and affective reasoning for misinformation classification.
These studies highlight the importance of incorporating emotional cues alongside raw textual information.

---

## 🛠️ Our Approach

We built a two-stage pipeline:

### 1️⃣ EmoLLM for Affective Analysis
- We input each news piece into **EmoLLM** (LLaMA2/OPT/BLOOM based)
- EmoLLM outputs:
  - **Sentiment** (positive/negative/neutral)
  - **Emotion intensity** (0–1 score)

### 2️⃣ LLaMA 3.1 for Classification
- We feed the news + emotion data into **LLaMA 3.1 8B Instruct**
- Use either:
  - Zero-shot or few-shot prompts
  - **Ranking inference** to reduce bias

> Instead of just letting LLaMA generate an answer, we **score both “TRUE” and “FALSE” completions** using log-likelihood and pick the better one.

---

## 🧪 Datasets & Experiments

We tested on two datasets:

| Dataset | Description |
|--------|-------------|
| **LIAR-15** | Short, fact-checked political claims |
| **PHEME** | Twitter rumor threads, emotionally rich |

We ran:
- Zero-shot and few-shot prompts
- With and without sentiment/emotion
- And used ranking vs generation inference

---

## 📊 Results (Highlights)

| Method-LLama-3.1-8B model| Dataset | Accuracy | F1 Score |
|--------------------------|---------|----------|----------|
| Zero-shot (baseline)     | LIAR    | 0.35     | 0.18     |
| + EmoLLM (zero-shot)     | LIAR    | 0.35     | 0.18     |
| + EmoLLM (few-shot)      | LIAR    | 0.35     | 0.18     |
| + Ranking (zero-shot)    | LIAR    | 0.40     | 0.28     |
| + Ranking (few-shot)     | LIAR    | 0.65     | 0.66 ✅  |
| Zero-shot (baseline)     | PHEME   | 0.55     | 0.40     |
| + EmoLLM (zero-shot)     | PHEME   | 0.62     | 0.48     |
| + EmoLLM (few-shot)      | PHEME   | 0.62     | 0.48     |
| + Ranking (zero-shot)    | PHEME   | 0.57     | 0.52     |
| + Ranking (few-shot)     | PHEME   | 0.57     | 0.54 ✅  |

✅ **Takeaways:**
- **PHEME** benefits from emotion-aware prompts
- **LIAR** gets biggest gain from ranking (not emotion)
- Generation bias (e.g., defaulting to “TRUE”) is reduced with ranking

---

## 💡 What We Learned

- EmoLLM helps when text is **emotion-rich** (PHEME)
- Ranking-based classification outperforms naive generation
- LIAR doesn’t benefit from sentiment — it's too short/formal

---

## 🔮 What’s Next?

- Fine-tune LLaMA with **LoRA** or **RAG**
- Apply to other datasets (COVID-19, CoAID, etc.)
- Add **explainability**: Why does the model say it's fake?

---

## 🚀 Try It Out or Read More

📂 GitHub: [link to your repo]  
🧠 Blog powered by GitHub Pages & Jekyll  
✉️ Contact: [your email or LinkedIn]
