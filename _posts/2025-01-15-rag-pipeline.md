---
layout: post
title: "Fine-tuning PhoBERT for Vietnamese NLP: Lessons Learned"
date: 2025-01-15
category: NLP
tags: [nlp, fine-tuning, transformer]
tech_tags: [PhoBERT, Fine-tuning, Vietnamese NLP]
excerpt: "A deep dive into fine-tuning PhoBERT for Vietnamese sentiment analysis, including dataset preparation, training strategies, and evaluation."
description: "A deep dive into fine-tuning PhoBERT for Vietnamese sentiment analysis — covering dataset preparation, training strategies, evaluation, and lessons learned."
subtitle: "A deep dive into training a state-of-the-art Vietnamese sentiment analysis model on 500K+ social media samples — covering everything from data cleaning to deployment."
read_time: 10
emoji: "🔍"
cover_image: "https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?w=1200&auto=format&fit=crop&q=80"
---

![Vietnamese NLP — Fine-tuning PhoBERT](https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?w=1200&auto=format&fit=crop&q=80)
*Transforming Vietnamese text understanding through fine-tuned BERT models*


Vietnamese NLP presents unique challenges that don't exist in English: word segmentation, diacritical marks, rich morphology, and a severe scarcity of labeled data. In this post, I'll share my experience fine-tuning PhoBERT — a BERT model pre-trained on a large Vietnamese corpus — for sentiment analysis on social media data.

## Why Vietnamese NLP is Hard

Vietnamese has several properties that make NLP challenging:

- **Tonal language**: 6 tones encoded as diacritical marks — "ma", "má", "mà", "mã", "mả", "mạ" are all different words
- **Word segmentation**: Unlike English, Vietnamese words can be multi-syllabic with no spaces: "học sinh" (student), not "họcsinh"
- **Code-switching**: Vietnamese social media mixes Vietnamese, English, and sometimes teen speak
- **Negation**: "không tốt lắm" (not very good) vs "không tệ" (not bad) — very different sentiments

## Dataset Preparation

```python
import pandas as pd
import re
from underthesea import word_tokenize

def clean_vietnamese_text(text: str) -> str:
    """Clean Vietnamese social media text."""
    # Remove URLs
    text = re.sub(r'http\S+|www\S+', '', text)
    # Remove emojis (keep text only)
    text = re.sub(r'[^\w\s\u00C0-\u024F\u1E00-\u1EFF]', ' ', text)
    # Normalize whitespace
    text = ' '.join(text.split())
    # Lowercase
    text = text.lower()
    return text.strip()

def tokenize_vn(text: str) -> str:
    """Word-segment Vietnamese text."""
    return word_tokenize(text, format="text")

# Load raw dataset (CSV with 'text' and 'label' columns)
df = pd.read_csv("raw_data.csv")
df['text_clean'] = df['text'].apply(clean_vietnamese_text)
df['text_tokenized'] = df['text_clean'].apply(tokenize_vn)

# Label distribution
print(df['label'].value_counts())
# Positive: 210,432 (42%)
# Negative: 183,891 (37%)
# Neutral:  105,677 (21%)
```

## Model Setup with Hugging Face

```python
from transformers import (
    AutoTokenizer,
    AutoModelForSequenceClassification,
    TrainingArguments,
    Trainer,
    EarlyStoppingCallback
)
from datasets import Dataset
import torch
import numpy as np
from sklearn.metrics import f1_score, accuracy_score

MODEL_NAME = "vinai/phobert-base-v2"
NUM_LABELS = 3  # Positive, Negative, Neutral
MAX_LENGTH = 256

tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME)
model = AutoModelForSequenceClassification.from_pretrained(
    MODEL_NAME,
    num_labels=NUM_LABELS,
    ignore_mismatched_sizes=True
)

def tokenize_function(examples):
    return tokenizer(
        examples["text"],
        truncation=True,
        max_length=MAX_LENGTH,
        padding="max_length"
    )

# Convert to HuggingFace Dataset
train_dataset = Dataset.from_pandas(train_df[['text_tokenized', 'label']]
    .rename(columns={'text_tokenized': 'text'}))
eval_dataset = Dataset.from_pandas(eval_df[['text_tokenized', 'label']]
    .rename(columns={'text_tokenized': 'text'}))

train_dataset = train_dataset.map(tokenize_function, batched=True)
eval_dataset = eval_dataset.map(tokenize_function, batched=True)
```

## Training with Optimal Hyperparameters

```python
def compute_metrics(eval_pred):
    logits, labels = eval_pred
    predictions = np.argmax(logits, axis=-1)
    f1 = f1_score(labels, predictions, average='macro')
    acc = accuracy_score(labels, predictions)
    return {"f1": f1, "accuracy": acc}

training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=5,
    per_device_train_batch_size=32,
    per_device_eval_batch_size=64,
    learning_rate=2e-5,
    weight_decay=0.01,
    warmup_ratio=0.1,
    evaluation_strategy="epoch",
    save_strategy="epoch",
    load_best_model_at_end=True,
    metric_for_best_model="f1",
    fp16=True,  # Mixed precision training
    logging_steps=100,
    report_to="mlflow"
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
    compute_metrics=compute_metrics,
    callbacks=[EarlyStoppingCallback(early_stopping_patience=2)]
)

trainer.train()
```

## Key Lessons Learned

### 1. Word Segmentation Matters Enormously

Using `underthesea` for Vietnamese word segmentation improved F1-score by ~4 percentage points compared to character-level or naive whitespace tokenization. Vietnamese "học sinh" (student) as one token vs. "học" + "sinh" as two tokens makes a big difference.

### 2. Learning Rate Scheduling is Critical

I tried several learning rate schedules. The warmup + linear decay (used above) worked best. A learning rate of `2e-5` consistently outperformed both `5e-5` (too aggressive) and `1e-5` (too slow convergence).

### 3. Class Imbalance Handling

The dataset was imbalanced (neutral class was under-represented). I experimented with weighted loss but found that simply oversampling the minority class worked better in practice:

```python
from sklearn.utils import resample

neutral_upsampled = resample(
    df[df.label == 'neutral'],
    replace=True,
    n_samples=len(df[df.label == 'positive']),
    random_state=42
)
df_balanced = pd.concat([
    df[df.label != 'neutral'],
    neutral_upsampled
]).sample(frac=1, random_state=42)
```

## Final Results

After 5 epochs of training on an A100 GPU (approximately 4 hours), the model achieved:

- ✅ **Macro F1-score: 92.3%** on the test set
- ✅ **Accuracy: 93.1%**
- ✅ Inference latency: ~12ms per sample (GPU)

> The model significantly outperformed the previous state-of-the-art (mBERT, 84.7% F1) on our benchmark, and even slightly beat the contemporaneous XLM-R (91.8% F1).

## Deployment

The fine-tuned model is deployed as a FastAPI service and powers a browser extension that analyzes sentiment on Vietnamese social media in real-time. The model is on Hugging Face Hub for public use.

```python
# Load and use the model
from transformers import pipeline

sentiment_analyzer = pipeline(
    "text-classification",
    model="TrinhHuuTho/phobert-sentiment-v2",
    tokenizer="TrinhHuuTho/phobert-sentiment-v2",
    device=0  # GPU
)

result = sentiment_analyzer("Sản phẩm này rất tốt, tôi rất hài lòng!")
# [{'label': 'POSITIVE', 'score': 0.9847}]
```
