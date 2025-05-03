---
layout: post
title: "Fake News Detection Using Sentiment and Emotion Analysis"
date: 2025-05-01
---

<link rel="stylesheet" href="/Fake-News-Detection/assets/css/custom.css">
#  Fake News Detection Using EmoLLM + LLaMA 3.1

> How emotion-aware analysis and large language models helped us identify misinformation better.



##  Introduction

Misinformation is everywhere — social media, websites, news portals. But fake news detection is still hard. Many claims *look* believable. Traditional fake news detection methods rely heavily on surface-level textual cues. The existing models trained specifically for this problem involve either complex fine-tuning or do not take context into account when classifying news as fake or real. That’s why we combined emotional and sentiment analysis with large language models to to deepen semantic understanding and improve classification accuracy.


##  Related Works
- RaemoLLM: Uses emotion-aware LLMs to construct a retrieval-based affective embedding database for misinformation detection.
- LEMMA: A multimodal framework that enhances large vision-language models (LVLMs) by incorporating external knowledge and affective reasoning for misinformation classification.
These studies highlight the importance of incorporating emotional cues alongside raw textual information.



##  Our Approach

We built a two-stage pipeline:  
The flow diagram we used to combine sentiment and emotion analysis with LLaMA 3.1:.  
![Fake News Pipeline](/Fake-News-Detection/assets/images/model_image.png)
### 1. EmoLLM for Affective Analysis

 One of the models we incorporated in this project is EmoLLM (Emotion-aware Large Language Model). While not the centerpiece of our work, EmoLLM is a valuable tool that goes beyond standard text analysis by detecting both sentiment and specific emotions within a piece of text. This means it can pick up on whether an article is positive, negative, or neutral, identify underlying emotions like anger, joy, or fear and also extract emotional and sentimental score. Using EmoLLM in our project helped add another layer of understanding to the data, making it possible to analyze not just what is being said, but also the emotional undertones behind it. This is particularly useful in tasks like fake news detection, where emotional manipulation can play a significant role.
 
-> We input each news piece into **EmoLLM** (LLaMA2/OPT/BLOOM based) 

-> EmoLLM outputs:

**Sentiment** (positive/negative/neutral)  
![Fake News Pipeline](/Fake-News-Detection/assets/images/senti_claim_image.png)

**Emotion intensity** (0–1 score)  
![Fake News Pipeline](/Fake-News-Detection/assets/images/emo_score_image.png)

### 2. LLaMA 3.1 for Classification
 We have here used LLaMA 3.1 8B Instruct as our base classifier and also the pre-trained model for the emotional analysed data to be fed into. Once the emotional and sentimental analysis is done the EmoLLM for a given new piece/tweet, we append the sentiment analysis and emotional score as data features to the dataset. This modified dataset now has the emotional and sentimental context. This embedded dataset is now passed through LLaMA pre-trained model with zero-shot prompting/few-shot prompting.
<pre>
Title: {row['title']}
Text: {example_text}
Sentiment: {row['sentiment']}
Emotion Score: {row['emotion_score']}
This article has a sentiment of {row['sentiment']} and an emotion score of {row['emotion_score']}.
Label: {row['label'].upper()}
</pre>

We adopted another method to improve upon the few-shot, zero shot classification which aimed at reducing the generation bias that large-language models geenrally have towards a safe/default label('TRUE' in our case). Instead of just letting LLaMA generate an answer, we **score both “TRUE” and “FALSE” completions** using log-likelihood and pick the better one as the prediction.


##  Demo
Here is a demonstration of the model in action:

<div style="text-align:center;">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/_lv-eTEePM4"
    title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
  </iframe>
</div>


##  Datasets & Experiments

We tested on two datasets:

| Dataset | Description |
|--------|-------------|
| **LIAR-15** | Short, fact-checked political claims |
| **PHEME** | Twitter rumor threads, emotionally rich |

We ran:
- LLaMA-3.1-8b for zero-shot classifications the baseline method, against which we would compare our approach. 
- The output we got from the EmoLLM was appended to the dataset and this new embedded daatset was run on the Llama classifier with zero-shot and few-shot prompting.
- And used ranking experiments were run combined with zero-shot and few-shot promopting to mitigate generation bias.


##  Results (Highlights)

| Method-LLama-3.1-8B model| Dataset | Accuracy | F1 Score |
|--------------------------|---------|----------|----------|
| Zero-shot (baseline)     | LIAR    | 0.35     | 0.18     |
| + EmoLLM (zero-shot)     | LIAR    | 0.35     | 0.18     |
| + EmoLLM (few-shot)      | LIAR    | 0.35     | 0.18     |
| + Ranking (zero-shot)    | LIAR    | 0.40     | 0.28     |
| + Ranking (few-shot)     | LIAR    | 0.65     | 0.66     |
| Zero-shot (baseline)     | PHEME   | 0.55     | 0.40     |
| + EmoLLM (zero-shot)     | PHEME   | 0.62     | 0.48     |
| + EmoLLM (few-shot)      | PHEME   | 0.62     | 0.48     |
| + Ranking (zero-shot)    | PHEME   | 0.57     | 0.52     |
| + Ranking (few-shot)     | PHEME   | 0.57     | 0.54     |

 **Takeaways:**
- **PHEME** benefits from emotion-aware prompts
- **LIAR** gets biggest gain from ranking (not emotion)
- Generation bias (e.g., defaulting to “TRUE”) is reduced with ranking



##  Conclusion

 Incorporating EmoLLM to the classification model proves that emotional/senntimental-context aids in improvement in the classification. PHEME being rich in emotional/sentimental tone results in improvement in accuracy but that is not the case with LIAR which consists short, concise facts lacking much emotional/sentimental tone.  We also see the generation bias of LLMs towards default/safe labels comes into play when we are doing generation based classification. Ranking-based classification helped in mitigating this shortcoming outperforms naive generation.



##  What’s Next?

- Fine-tune LLaMA with **LoRA** or **RAG**
- Apply to other datasets (COVID-19, CoAID, etc.)
- Add **explainability**: Why does the model say it's fake?

---

###  Contributors

- **Ankit Kumar Sahoo**  
  📧 [ankit_5107@tamu.edu](mailto:ankit_5107@tamu.edu)

- **Sushmitha Bangarwa**  
  📧 [sushimtha_bangarwa@tamu.edu](mailto:sushimtha_bangarwa@tamu.edu)


##  Try It Out or Read More

📂 GitHub: https://github.com/ankit5107tamu/Fake-News-Detection/  
🧠 Blog powered by GitHub Pages & Jekyll  
✉️ Contact: ankit_5107@tamu.edu
