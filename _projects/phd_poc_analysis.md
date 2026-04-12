---
layout: single
title: "Personalizing Motor Learning with Machine Learning"
excerpt: ""
header:
  overlay_color: "#dde3ea"
  # toc: true
  # toc_label: "Contents"

classes: wide project-page

---
<div class="project-content" markdown="1">
*An overview of my first PhD study, supervised by [Baptiste Caramiaux](https://baptistecaramiaux.com/). I explored how machine learning can personalize motor skill training to improve outcomes, in particular, how to tailor practice schedules to individual learners.*

---
## The Challenge

The question I set out to answer is whether we can give every learner their own adaptive coach; one that monitors movements and adjusts training instantly using Machine Learning.

---

## Approach: Machine Learning Meets Motor Learning

Random practice has been shown to be effective for learning motor skills, but does not consider the skill level of the learner. During my PhD, I set out to answer this question: **can machine learning personalize motor skill training more effectively than a random practice schedule?**

### The Experiment

My team and I designed a controlled study where participants learned to trace curved paths using an infrared marker — a task that mirrors real-world motor challenges in rehabilitation (regaining hand control) and professional training (surgical precision).

By varying the channel width, we controlled difficulty: *narrow channels = harder, wide channels = easier*. Instead of following a fixed training sequence, a Machine Learning algorithm decided which difficulty to present next, based on each learner's real-time performance.

<figure style="width: 50%; margin: 0 auto;">
 <img src="/assets/images/experiment1.png" alt="Experiment setup">
  <figcaption>Illustration of the experiment setup</figcaption>
</figure>


___

### The Machine Learning Strategy

We used a **multi-armed bandit algorithm (MAB)** — a technique that balances trying new challenges (exploration) with reinforcing what already works (exploitation). This technique allows to bypass a learner model, especially interesting in the motor learning domain, where this is complex. A learner model for motor skills needs to capture not just a point estimate of ability but the shape and rate of the learning curve, which varies across individuals and across tasks.

The system continuously analyzed two key metrics:

- **Accuracy** — can the learner stay within the path?
- **Movement smoothness (jerk)** — how expert-like are their movements?

Based on these signals, the MAB selected the next exercise to maximize learning progress.

---

## What We Discovered

Compared to traditional random practice and semi-adaptive methods, the MAB based adaptation produced two key results:

**Superior movement quality**
Learners achieved smoother movements, which may capture better internalization of a motor plan. In rehabilitation, this could mean safer, more natural motion. 

**Better skill transfer**
Skills learned with ML guidance transferred better to new situations. This captures adaptation skills required in daily life.

### Why accuracy didn't improve with the adaptive strategy

Accuracy was not shown to  be different across learning strategies. Not relying on a learner model means that "learning progress" computed by the MAB is a poor measurement instrument across most of the difficulty range. the learning progress signal is noisy and unreliable, not because there's no learning happening, but because 4 trials isn't enough to distinguish real improvement from random fluctuation at a difficulty level where the learner is mostly failing.
The informative zone — probably intermediate width channels for most learners, is where accuracy measure is sensitive enough to register genuine improvement. And indeed, the data showed that the MAB adaptation ended up scheduling more trials in the middle of the range than the semi-adaptive strategy, which frequently scheduled the hardest task. So the MAB algorithm did partially discover the informative zone, but through trial and error rather than through understanding. A learner model (e.g. IRT) would have found it.

---

## Real-World Applications

- **Rehabilitation technology** — adaptive therapy apps that adjust exercises for stroke recovery, motor impairment, or injury rehabilitation
- **EdTech platforms** — intelligent tutoring systems for music, sports, craftsmanship, or any skill requiring physical mastery

---

## The Takeaway

With traditional motor learning, human experts may adapt exercises, but only for a small number of people at a time. With AI, this kind of personalized support becomes accessible to many more learners.

One promising direction for more complex movements e.g. sports: combining the expertise of a coach with the reliability of technology by allowing the expert to guide the AI's adaptations — for example, using ontologies to encode expert knowledge about skill progression, which AI can leverage to make more informed decisions.

---

## References


[Interactive curriculum learning increases and homogenizes motor smoothness](https://www.nature.com/articles/s41598-024-53253-3)

*Published in Scientific Reports 2024. Co-authored with Baptiste Caramiaux, Antoine Loriette and Olivier Sigaud.*
</div>