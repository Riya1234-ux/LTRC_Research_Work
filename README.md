🎙️ **Enhancing Automatic Pronunciation Assessment: From Speaker Embeddings to GOP-based Multi-Level Analysis**

A research-driven study on pronunciation assessment, speaker representation learning, and Goodness of Pronunciation (GOP), with experiments conducted on the SpeechOcean762 corpus.

**Overview**

**Automatic Pronunciation Assessment (APA)** is an important component of **Computer Assisted Language Learning (CALL)** systems. Traditional systems rely heavily on acoustic models and **Goodness of Pronunciation (GOP)** scores, while recent research has shifted towards self-supervised speech representations.

This repository documents my complete research journey—from studying classical speaker embedding techniques to understanding modern self-supervised learning methods and finally implementing a GOP-based pronunciation assessment framework on the SpeechOcean762 dataset.

Rather than being a single implementation, this project represents an exploration of how pronunciation assessment has evolved over the last two decades and where future research opportunities exist.

**Research Journey
Phase I — Understanding Speaker Representation Learning**

Before beginning pronunciation assessment, I first studied how speech representations have evolved over the years.

**Joint Factor Analysis (JFA)**

Studied:

Session variability
Speaker variability
Factor decomposition

Understanding gained:

Why early speaker verification systems required separate variability modeling.
Limitations in handling large-scale speaker datasets.
**i-Vectors**

Studied:

Total Variability Space
Baum-Welch Statistics
UBM
Total Variability Matrix

Key Understanding

Fixed-length speaker embeddings can represent variable-length utterances.
i-vectors became the foundation of modern speaker verification.

Limitations

Linear representation
Requires extensive feature engineering
Sensitive to channel variability
**x-Vectors**

Studied

TDNN architecture
Statistics pooling
Segment-level embeddings

Key Learning

Deep neural networks significantly outperform i-vectors.
Learned embeddings directly from speech instead of handcrafted statistics.
**ECAPA-TDNN**

Extensively studied

Res2Net Blocks
SE-Res2Blocks
MFA (Multi-layer Feature Aggregation)
Attentive Statistics Pooling
AAM-Softmax Loss

Key Findings

Better channel attention
Multi-scale feature extraction
More discriminative speaker embeddings
State-of-the-art speaker verification performance
**r-Vectors**

Studied the motivation behind residual-based speaker embeddings.

Understanding

Robust feature learning
Improved speaker discrimination
Better handling of difficult acoustic conditions
**Phase II — Literature Survey on Pronunciation Assessment**

The primary research paper studied was

**GOP2Vec: Global and Local Context-Aware Goodness of Pronunciation Embedding for Pronunciation Assessment**

**Main ideas studied**

Traditional GOP
GOP feature limitations
GOP2Vec architecture
Context-aware embeddings
Local vs Global pronunciation information
What I Learned from GOP2Vec

Traditional GOP only evaluates pronunciation independently for each phoneme.

However,

Human pronunciation assessment depends on

neighbouring phonemes
surrounding words
sentence context
speaking fluency

GOP2Vec addresses this by learning contextual pronunciation embeddings instead of relying solely on raw GOP scores.

**Limitations of GoP2Vec**

While GoP2Vec significantly improves traditional GOP-based pronunciation assessment by incorporating contextual pronunciation embeddings, the literature review revealed several important limitations that motivated the next phase of this research.

1. Speaker Information Leakage

GoP2Vec incorporates i-vector speaker embeddings to provide additional acoustic information. However, i-vectors capture not only pronunciation characteristics but also speaker identity, accent, gender, recording conditions, and speaking style.

As a result, the model may partially learn who is speaking rather than how well the pronunciation is produced, reducing its ability to generalize across unseen speakers.

2. Dependence on GOP Accuracy

The quality of GoP2Vec embeddings is directly dependent on the quality of the underlying Goodness of Pronunciation (GOP) scores.

Since GOP computation relies on automatic speech recognition (ASR), errors in

phoneme alignment,
boundary detection,
acoustic modeling, or
accent mismatch

can propagate into the learned pronunciation embeddings, ultimately degrading pronunciation assessment performance.

3. Artificial Data Augmentation Effects

To improve model robustness, GoP2Vec employs augmentation techniques such as

time stretching,
pitch scaling, and
speed perturbation.

Although beneficial for increasing training data, these transformations may unintentionally alter

phoneme duration,
lexical stress,
speaking rhythm, and
prosodic patterns,

creating pronunciation characteristics that are not representative of natural human speech.

4. Limited Generalization

GoP2Vec is trained on relatively small annotated pronunciation datasets.

Consequently, the model is susceptible to overfitting, learning speaker-specific or dataset-specific patterns rather than developing pronunciation representations that generalize well across different speakers, accents, and recording environments.

5. Handcrafted Feature Dependency

The model relies heavily on manually engineered features, primarily

Goodness of Pronunciation (GOP) scores
i-vector speaker embeddings

Although effective, these handcrafted representations capture only limited acoustic information and cannot fully exploit the rich contextual information contained in raw speech signals.

**Motivation for Self-Supervised Learning**

These limitations naturally motivate the exploration of Self-Supervised Learning (SSL) models for pronunciation assessment.

Recent SSL architectures such as

wav2vec 2.0
HuBERT
WavLM

learn powerful speech representations directly from large-scale unlabeled audio, eliminating the need for manually engineered pronunciation features.

Compared to traditional GOP-based methods, SSL embeddings offer several advantages:

Learn contextual acoustic representations from raw speech.
Reduce dependence on forced phoneme alignment.
Produce more speaker-invariant representations.
Capture long-range phonetic and contextual information.
Generalize better across speakers, accents, and recording conditions.
Require significantly less manually annotated pronunciation data.

**Possible future direction**

Instead of computing pronunciation quality using only GOP,

combine

GOP
HuBERT embeddings

to jointly model

pronunciation quality
acoustic context
phonetic similarity
Proposed Research Direction

The study proposes investigating whether combining

traditional GOP scores

with

self-supervised speech embeddings

can improve automatic pronunciation assessment.

**Potential pipeline**

Audio

↓

HuBERT

↓

Embedding

GOP Features

↓

Regression / Transformer

↓

Pronunciation Score Prediction

**Phase IV — Experimental Analysis on SpeechOcean762**

**Dataset**

SpeechOcean762

Contains

learner speech
phoneme annotations
expert pronunciation scores
word scores
sentence scores
**Methodology**
Step 1

Extracted learner pronunciation files.

Each text file contains

Phoneme + GOP score.

Step 2

Parsed more than 2500 learner pronunciation files into structured Pandas DataFrames.

Step 3

Performed phoneme-level statistical analysis

Computed

average GOP
pronunciation variability

Visualized

pronunciation difficulty
difficult phonemes
Step 4

Applied K-Means clustering

Features

Mean GOP
Standard deviation

Grouped phonemes into clusters representing different pronunciation characteristics.

Step 5

Separated vowels and consonants

Studied

pronunciation behaviour
cluster distribution
vowel vs consonant variability
Step 6

Reconstructed Words

The learner files contain

_B
_I
_E

markers.

Using these markers,

individual phonemes were grouped into complete words.

Word GOP

=

Average phoneme GOP

Step 7

Constructed Sentence-Level GOP

Sentence GOP

=

Average Word GOP

This enabled pronunciation assessment at three different linguistic levels

phoneme
word
sentence
Human Annotation Comparison

Extracted expert annotations from

scores.json

**Compared machine-derived GOP scores against human scores at**

Phoneme Level
Word Level
Sentence Level

using

**Pearson Correlation
Spearman Correlation
Experimental Results**

Observed trend

Level	Correlation with Human Scores
Sentence	Highest
Word	Moderate
Phoneme	Lowest

This indicates that aggregated GOP representations align better with human perception than isolated phoneme scores.

**Vowel vs Consonant Analysis**

Separated all phonemes into

vowels
consonants

Measured

independent Pearson correlations
pronunciation variability
pronunciation clusters

This provides insight into whether pronunciation assessment behaves differently across major phonetic categories.

**Current Status**

✔ Literature review completed

✔ Speaker embedding evolution studied

✔ GOP2Vec analysed

✔ SSL-based pronunciation assessment reviewed

✔ SpeechOcean762 preprocessing completed

✔ Phoneme-level GOP extraction

✔ Word reconstruction

✔ Sentence-level GOP generation

✔ Multi-level correlation analysis

✔ Vowel vs consonant analysis

**Future Work**
Integrate HuBERT embeddings with GOP features.
Explore transformer-based pronunciation scoring models.
Compare SSL embeddings (HuBERT, wav2vec 2.0, WavLM) for pronunciation assessment.
Develop an interpretable pronunciation assessment framework that combines acoustic representations with phonetic confidence scores.
Investigate multilingual pronunciation assessment using self-supervised speech representations.
