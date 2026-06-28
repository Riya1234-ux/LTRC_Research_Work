# 🎙️ Enhancing Automatic Pronunciation Assessment: From Speaker Embeddings to GOP-based Multi-Level Analysis

> **A research-driven study on Automatic Pronunciation Assessment (APA), speaker representation learning, and Goodness of Pronunciation (GOP), with experiments conducted on the SpeechOcean762 corpus.**

---

# 📖 Overview

Automatic Pronunciation Assessment (APA) is an important component of **Computer Assisted Language Learning (CALL)** systems. Traditional systems rely heavily on acoustic models and **Goodness of Pronunciation (GOP)** scores, while recent research has shifted towards **Self-Supervised Learning (SSL)** based speech representations.

This repository documents my complete research journey—from studying classical speaker embedding techniques to understanding modern self-supervised speech representations and finally implementing a **GOP-based pronunciation assessment framework** on the **SpeechOcean762** dataset.

Rather than being a single implementation, this project represents an exploration of **how pronunciation assessment has evolved over the last two decades** and identifies promising directions for future research.

---

# 🚀 Research Journey

---

# **Phase I — Understanding Speaker Representation Learning**

Before beginning pronunciation assessment, I first studied the complete evolution of **speaker representation learning**, starting from classical statistical models to modern deep learning architectures.

---

## 🔹 Joint Factor Analysis (JFA)

### Topics Studied

* Session Variability
* Speaker Variability
* Factor Decomposition
* Eigenvoice and Eigenchannel Modeling

### Understanding Gained

* Why early speaker verification systems required separate modeling of speaker and channel variability.
* How JFA decomposes speech variability into independent latent factors.
* Limitations of handling large-scale speaker datasets due to separate variability modeling.

---

## 🔹 i-Vectors

### Topics Studied

* Universal Background Model (UBM)
* Baum-Welch Statistics
* Total Variability Space
* Total Variability Matrix (T-Matrix)
* PLDA Scoring
* Score Normalization

### Key Understanding

* Variable-length utterances can be represented using fixed-dimensional embeddings.
* Speaker and channel information are jointly modeled in the Total Variability Space.
* i-vectors became the foundation of modern speaker verification systems.

### Limitations

* Linear representation learning.
* Requires handcrafted acoustic features (MFCCs).
* Sensitive to channel variability.
* Multi-stage training pipeline.

---

## 🔹 x-Vectors

### Topics Studied

* Time Delay Neural Networks (TDNN)
* Context Expansion
* Statistics Pooling
* Segment-Level Embeddings
* Speaker Classification

### Key Learning

* Deep neural networks outperform statistical factor analysis.
* Speaker embeddings are learned directly from speaker identities.
* Statistics pooling enables fixed-dimensional representations from variable-length utterances.

---

## 🔹 ECAPA-TDNN

### Topics Studied

* Res2Net Blocks
* SE-Res2Blocks
* Multi-layer Feature Aggregation (MFA)
* Attentive Statistics Pooling
* Additive Angular Margin Softmax (AAM-Softmax)

### Key Findings

* Better channel attention mechanisms.
* Multi-scale temporal feature extraction.
* Improved speaker discrimination.
* Robust embeddings for noisy environments.
* Current state-of-the-art speaker verification architecture.

---

## 🔹 R-Vectors

### Topics Studied

* Room embeddings
* Acoustic environment representation
* Reverberation modeling
* Room classification using TDNN

### Understanding

* Speaker embedding architectures can also learn environmental characteristics.
* Robust feature learning beyond speaker identity.
* Improved handling of acoustic variability caused by room reverberation.

---

# **Phase II — Literature Survey on Pronunciation Assessment**

The primary research paper studied was:

> **GoP2Vec: Global and Local Context-Aware Goodness of Pronunciation Embedding for Pronunciation Assessment**

---

## 📚 Main Concepts Studied

* Traditional Goodness of Pronunciation (GOP)
* Limitations of GOP
* GoP2Vec Architecture
* Global Context Modeling
* Local Context Modeling
* Pronunciation Embeddings
* Bi-LSTM Context Modeling
* Speaker Embeddings (i-vectors)
* Data Augmentation
* Pronunciation Regression Network

---

## 💡 Key Understanding from GoP2Vec

Traditional GOP evaluates pronunciation independently for each phoneme.

However, human pronunciation assessment depends on much richer contextual information, including:

* Neighbouring phonemes
* Word context
* Sentence context
* Speaking fluency
* Co-articulation
* Prosody

GoP2Vec addresses these limitations by learning **context-aware pronunciation embeddings** instead of relying solely on raw GOP values.

---

# ⚠️ Limitations of GoP2Vec

While GoP2Vec significantly improves traditional GOP-based pronunciation assessment by incorporating contextual pronunciation embeddings, the literature review revealed several important limitations that motivated the next phase of this research.

---

## 1. Speaker Information Leakage

GoP2Vec incorporates **i-vector speaker embeddings** to provide additional acoustic information.

However, i-vectors encode not only pronunciation characteristics but also:

* Speaker identity
* Accent
* Gender
* Recording conditions
* Speaking style

As a result, the model may partially learn **who is speaking rather than how well the pronunciation is produced**, reducing its ability to generalize across unseen speakers.

---

## 2. Dependence on GOP Accuracy

The quality of GoP2Vec embeddings is directly dependent on the quality of the underlying **Goodness of Pronunciation (GOP)** scores.

Since GOP computation relies on Automatic Speech Recognition (ASR), errors in

* phoneme alignment,
* boundary detection,
* acoustic modeling, or
* accent mismatch

can propagate into the learned pronunciation embeddings, ultimately degrading pronunciation assessment performance.

---

## 3. Artificial Data Augmentation Effects

To improve model robustness, GoP2Vec employs augmentation techniques such as

* Time stretching
* Pitch scaling
* Speed perturbation

Although beneficial for increasing training data, these transformations may unintentionally alter

* phoneme duration,
* lexical stress,
* speaking rhythm,
* prosodic patterns,

producing unrealistic pronunciation characteristics that are not representative of natural speech.

---

## 4. Limited Generalization

GoP2Vec is trained on relatively small annotated pronunciation datasets.

Consequently, the model is susceptible to overfitting, learning speaker-specific or dataset-specific characteristics instead of developing pronunciation representations that generalize well across speakers, accents, and recording environments.

---

## 5. Handcrafted Feature Dependency

GoP2Vec depends heavily on manually engineered features, primarily:

* Goodness of Pronunciation (GOP) scores
* i-vector speaker embeddings

Although effective, these handcrafted representations capture only limited acoustic information and cannot fully exploit the rich contextual information available in raw speech.

---

# 🌍 Motivation for Self-Supervised Learning (SSL)

The limitations identified in GoP2Vec naturally motivate the exploration of **Self-Supervised Learning (SSL)** models for pronunciation assessment.

Recent SSL architectures such as:

* **wav2vec 2.0**
* **HuBERT**
* **WavLM**

learn robust speech representations directly from **large-scale unlabeled audio**, eliminating the need for handcrafted pronunciation features.

### Advantages of SSL

* Learns contextual acoustic representations directly from raw speech.
* Reduces dependence on forced phoneme alignment.
* Produces more speaker-invariant representations.
* Captures long-range phonetic dependencies.
* Generalizes better across speakers, accents, and recording environments.
* Requires significantly less manually annotated pronunciation data.

---

# 🔬 Proposed Research Direction

Based on the literature survey, the long-term objective of this research is to investigate whether combining traditional pronunciation confidence measures with contextual SSL representations can improve pronunciation assessment.

## Proposed Hybrid Framework

```text
Speech Audio
      │
      ▼
HuBERT / wav2vec 2.0
      │
Contextual Embeddings
      │
      ├──────────────┐
      │              │
      ▼              ▼
   GOP Features   SSL Features
      │              │
      └──────┬───────┘
             ▼
      Feature Fusion
             ▼
Regression / Transformer
             ▼
Pronunciation Score Prediction
```

The current implementation focuses on building a **strong GOP-based baseline**, which will later be compared against SSL-based pronunciation assessment models.

---

# **Phase III — Experimental Analysis on SpeechOcean762**

## 📂 Dataset

Experiments were conducted on the **SpeechOcean762** pronunciation assessment corpus.

The dataset contains:

* Learner speech recordings
* Phoneme-level annotations
* Word-level pronunciation scores
* Sentence-level pronunciation scores
* Expert human evaluation scores

---

# 🧪 Methodology

## Step 1 — Parsing Learner Files

Extracted pronunciation files from the SpeechOcean762 corpus.

Each learner file contains:

```text
Phoneme    GOP Score
```

---

## Step 2 — Data Preprocessing

Parsed more than **2,500 learner pronunciation files** into structured Pandas DataFrames.

---

## Step 3 — Phoneme-Level Analysis

Computed:

* Average GOP
* Pronunciation variability

Visualized:

* Difficult phonemes
* Easy phonemes
* Pronunciation distributions

---

## Step 4 — Phoneme Clustering

Applied **K-Means Clustering** using:

* Mean GOP
* Standard Deviation

to group phonemes into clusters representing similar pronunciation characteristics.

---

## Step 5 — Vowel vs Consonant Analysis

Separated phonemes into:

* Vowels
* Consonants

Studied:

* Pronunciation variability
* Cluster distributions
* Relative pronunciation difficulty

---

## Step 6 — Word-Level GOP Reconstruction

Using the `_B`, `_I`, and `_E` markers, phonemes belonging to the same word were grouped together.

Word GOP was computed as:

> **Average of the constituent phoneme GOP scores**

---

## Step 7 — Sentence-Level GOP Reconstruction

Sentence GOP was computed as:

> **Average of all reconstructed Word GOP scores**

This enabled pronunciation assessment at three linguistic levels:

* Phoneme
* Word
* Sentence

---

# 📊 Human Annotation Comparison

Expert annotations were extracted from **scores.json**.

Machine-derived GOP scores were compared against expert human scores at:

* Phoneme Level
* Word Level
* Sentence Level

using:

* Pearson Correlation
* Spearman Correlation

---

# 📈 Experimental Findings

### Observed Trend

| Level    | Correlation with Human Scores |
| -------- | ----------------------------- |
| Sentence | Highest                       |
| Word     | Moderate                      |
| Phoneme  | Lowest                        |

This indicates that **aggregated GOP representations align more closely with human perception than isolated phoneme-level scores.**

---

# 🔤 Vowel vs Consonant Analysis

Separated all phonemes into:

* Vowels
* Consonants

Measured:

* Independent Pearson Correlations
* Pronunciation Variability
* Cluster Distributions

This analysis provides insight into whether pronunciation assessment behaves differently across major phonetic categories.

---

# ✅ Current Status

* ✔ Literature Survey Completed
* ✔ Speaker Embedding Evolution Studied
* ✔ GoP2Vec Analysis Completed
* ✔ SSL-based Pronunciation Assessment Reviewed
* ✔ SpeechOcean762 Preprocessing Completed
* ✔ Phoneme-Level GOP Extraction
* ✔ Word-Level GOP Reconstruction
* ✔ Sentence-Level GOP Reconstruction
* ✔ Multi-Level Correlation Analysis
* ✔ Vowel vs Consonant Analysis

---

# 🚀 Future Work

* Integrate **HuBERT embeddings** with GOP features.
* Explore Transformer-based pronunciation scoring models.
* Compare SSL embeddings (**HuBERT**, **wav2vec 2.0**, **WavLM**) for pronunciation assessment.
* Develop an interpretable pronunciation assessment framework combining contextual speech representations with phonetic confidence scores.
* Investigate multilingual pronunciation assessment using self-supervised speech representations.
* Explore end-to-end pronunciation assessment without explicit forced alignment.

---

## 👩‍💻 Author

**Riya Paul**

**B.Tech Student | Machine Learning | Speech Processing | Automatic Speech Recognition | Speaker Verification | Pronunciation Assessment**

*"This repository documents my research journey toward building more robust, interpretable, and context-aware Automatic Pronunciation Assessment systems by bridging classical GOP-based techniques with modern Self-Supervised Learning representations."*
