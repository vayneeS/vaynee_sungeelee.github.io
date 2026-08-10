---
layout: single
title: "Adaptive Training for Prosthetic Control"
excerpt: ""
header:
  teaser: /assets/images/prosthesis_hand.JPG
  overlay_color: "#dde3ea"
# toc: true
# toc_label: "Contents"
classes: wide project-page


---
<div class="project-content" markdown="1">
*My second PhD study focused on optimizing prosthetic control to explore how an adaptive algorithm affects performance and human agency in human-machine interaction.*

[View Code](https://github.com/vayneeS/prosthesis-strategies-with-ml)

---

## The Problem

Prosthetic arm users struggle to maintain reliable control of their devices. Training strategies directly impact how quickly users learn to translate muscle signals (EMG) into accurate prosthetic movements — but most systems use random, one-size-fits-all approaches that ignore individual learning needs.

**Question:** Can an adaptive training strategy that personalizes gesture selection based on real-time performance help users learn faster and better understand how to train the Machine Learning-based prosthetic arm?

---

## Approach

To test the adaptive training strategy, I ran a user study with more than 50 participants, comparing it to two other strategies.

- **Random** — gestures presented in random order (baseline)
- **User-Choice** — users select gestures based on perceived difficulty
- **Adaptive** — selects gestures based on classifier performance, focusing practice where it matters most

<figure style="width: 50%; margin: 0 auto;">
 <img src="/assets/images/experiment.png" alt="Experiment setup">
  <figcaption>Participants wore a Myo armband and performed gestures following one of three training strategies</figcaption>
</figure>

---

## Data Pipeline Design

Raw EMG signals from wearable sensors are produced from muscle contractions.

I designed and implemented a data pipeline to turn raw, noisy sensor signals into features for the machine learning algorithm:

1. **Signal cleaning** — removing electrical interference while preserving meaningful muscle activity
2. **Normalization** — standardizing the 8 sensor channels so each contributed equally, similar to balancing audio inputs so no single signal dominates
3. **Aggregation** — consolidating high-frequency predictions (200Hz) into one performance score per trial, enabling consistent tracking across 120 training rounds per user

<!-- **Why it matters:** Without proper preprocessing, models risk learning noise instead of real movement patterns. This pipeline improved data quality, ensured fair comparisons across 50+ users with different physiologies and sensor placements, and made performance metrics robust enough for large-scale analysis. -->

---

## Machine Learning to classify gestures

**The Model:** I implemented Scikit Learn's Linear Discriminant Analysis (LDA) to classify gestures from sensor data. LDA identifies the clearest boundaries between gesture categories by maximizing the separation between them.

The dataset was relatively small (120 training rounds) and the signals were inherently noisy. LDA performs efficiently with limited data and low-dimensional inputs (8 sensor values). Its simplicity reduces the risk of overfitting and keeps the model stable and interpretable.

**The Adaptive Layer:** After each training round, the system analyzes classification errors to identify which gesture pairs are most frequently confused. These gestures are then prioritized in the next round — instead of random repetition, the training loop focuses on the user's specific weak points.

---

## Statistical Analysis: Validating Impact

**The Objective:** Observing different learning curves was not enough — we needed to validate that the performance gaps between training strategies were statistically meaningful, not random variation.

**Learning Speed:** I quantified each participant's improvement rate over time and compared groups using ANOVA. The adaptive strategy produced significantly faster improvement than the other groups — the likelihood that this difference occurred by chance was less than 0.1%, indicating a strong and reliable effect.

**Skill Transfer:** I evaluated whether participants could apply what they learned to reproduce gestures likely to be recognised by the trained classifier — an indicator of true skill acquisition rather than memorization. Because performance scores were not normally distributed, I used a non-parametric test (Kruskal-Wallis). The adaptive group outperformed the user-choice group, with only a 3% probability that this result was due to chance.

**Effect Size:** Beyond statistical significance, effect size quantifies the magnitude of impact.

---

## Key Results

### 1. Adaptive training reduces time-to-competency
<!-- 
![Learning rates by strategy](/assets/images/Learning_rates_by_strategy.png) -->
*Users exposed to adaptive gesture selection improved significantly faster than those using random or self-directed practice. In practical terms: the adaptive system increases learning speed.*

### 2. Faster learning without sacrificing final performance

<!-- ![Accuracy across trials](/assets/images/accuracy_across_trials_by_strategy.png) -->
*All groups reached similar peak accuracy by the end of training. The difference lies in speed: users trained with the adaptive strategy achieved proficiency earlier — an efficiency gain with no trade-off in quality.*

### 3. Stronger skill transfer and system understanding

<!-- ![Test accuracy](/assets/images/TPR.png) -->
*When tested on new movement sequences, users trained using the adaptive strategy performed significantly better than users of the user-choice strategy — indicating deeper skill acquisition and better generalization.*

---

## Key Insights

**Faster onboarding**  
Adaptive training reduces time-to-competency by focusing practice on gestures the classifier struggles with. This creates steeper learning curves without requiring more total practice time.  
*Impact: shorter training sessions, lower user frustration, faster clinical deployment.*

**Better mental representation of the system**  
Users trained adaptively developed stronger mental models — they could predict what the system would do given particular gesture examples.  
*Impact: improved long-term adherence, reduced support load, higher user confidence.*

**Recommendation**  
While it doesn't show significant long term benefits in terms of accuracy compared to other techniques, the adaptive strategy shows promising benefits in terms of user adoption.
Further experiments should collect users' preferences to understand whether one strategy is preferred over another and implications for long-term use. For example, the data shows that  adaptive training would be useful for quick onboarding and clearer understanding. The user could be given more agency after familiarizing with the tool or for those who prefer more control.


## Reference

[Comparing Teaching Strategies of a Machine Learning-based Prosthetic Arm — HAL Archive](https://amu.hal.science/ISIR_ACIDE/hal-04527854v1)

*Published at IUI 2024. Co-authored with Baptiste Caramiaux, [Nathanaël Jarrassé](https://www.n-jarrasse.fr/), [Téo Sanchez](https://teo-sanchez.github.io/).*

</div>