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

The question I set out to answer is whether we can give every learner their own adaptive coach, that can monitor movements and adjust training in real time using Machine Learning.

---

## Approach: Machine Learning Meets Motor Learning

Random practice has been shown to be effective for learning motor skills, but does not consider the skill level of the learner. During my PhD, I set out to answer this question: **can Machine Learning personalize motor skill training more effectively than a random sequence of exercises?**

### The Experiment

My team and I designed a controlled study where 36 participants learned to trace curved paths using an infrared marker.

By varying the channel width, we controlled difficulty: *narrow channels = harder, wide channels = easier*. Instead of following a fixed training sequence, a Machine Learning algorithm decided which difficulty to present next, based on each learner's real-time performance.

<figure style="width: 50%; margin: 0 auto;">
 <img src="/assets/images/experiment1.png" alt="Experiment setup">
  <figcaption>Illustration of the experiment setup</figcaption>
</figure>


___

### The Machine Learning algorithm to personalize learning

I used a **multi-armed bandit algorithm (MAB)**, a technique that alternates between exploration of new challenges and exploitation of known outcomes. To get a clearer picture, imagine a foodie who can explore new restautants in town or go to the ones he knows are good. The problem with always exploiting is that he can miss out on new restaurants with better food than he's tasted so far. On the other hand, exploring too much can lead him to restaurants he does not like at all.  
In learning, this technique allows to bypass what is called a learner model, which supposes that learners' cognitive processes can be modelled mathematically. 
Not having to rely on a learner model is especially interesting in the motor learning domain, since modelling motor learning is a complex endeavour - the shape and rate of the motor learning curve  varies across individuals and across tasks. 
To account for changes in learning over time with the MAB, the notion of learning progress is introduced. It allows for changing exercise when the learner has stoppped progressing on a task. 
Imagine an exercise rated out of 10, no progress means either the learner is stuck after 2 attempts at the exercise (5/10 then 5/10) or has mastered the exercise (10/10 then 10/10). 
On a pedagogical level, this way of proposing exercises also presents an advantage. Learners are more motivated to complete exercises which are suitable for their current level and has been shown to be linked to [long-term engagement] (https://flowers.inria.fr/wp/wp-content/uploads/2014/09/14icdl_kidlearn-smaller.pdf). 

The system continuously analyzed two key metrics:

- **Accuracy** — can the learner stay within the curved path?
- **Movement smoothness (jerk)** — how smooth (not jagged) are their movements?

The algorithm selected the next exercise to maximize learning progress. Participants were asked to focus on both accuracy and smoothness while performing the movements.

---

## Findings

Compared to traditional random practice and semi-adaptive methods, the MAB based adaptation produced two key results: 

**Intermediate range of exercise difficulty**
The data showed that the MAB adaptation ended up scheduling more trials in the middle of the range than the semi-adaptive strategy, which frequently scheduled the hardest task.

**Superior movement quality**
Learners achieved smoother movements at retention and transfer. The level of difficulty of the tasks proposed enabled them to refine their motion. In rehabilitation, this could mean safer, more natural motion. 

**Better skill transfer**
Skills learned with adaptive guidance were transferred to new situations. In real life, learning should not only bring about lasting changes (retention), but also improve adaptation to previously unseen situations. 

### Accuracy didn't differ with the adaptive strategy

With the MAB adaptive approach, we didn't rely on a learner model. This means that the learning progress and the exploratory nature of the algorithm determined the next exercise. The MAB algorithm is sensitive to hyperparameters chosen, such as the window size (distance between the attempts) or the amount of exploration. This means that a learner who is anxious can make more mistakes at the start, resulting in a slower progress. 

In addition,  
---


## What next

With the demoncratization of LLMS, creating personalized learning tools has become much easier. Being equipped with an AI like Claude or ChatGPT, an educator can design a course curriculum for an audience or propose an adaptive quiz on a given topic in a matter of minutes. 
For motor learning, this could mean coupling sensors and wearables with adaptive instructions using LLMs for example.   
Many products are being developed using gamification and data-driven techniques for further personalization. 
In robotics, deep learning techniques are making humain-machine co-learning more concrete.Humans learn to control exoskeletons (e.g. a prosthetic arm or limb) through electromyography (EMG) and receive feedback through movement of the machine (human-in-the-loop).
The algorithms continuously learn changes in human movement (e.g. due to fatigue or movement patterns) and can predict changes in movement to adapt the movement of exoskeletons. This reduces effort on muscles and allows better [coordination](https://www.mdpi.com/2224-2708/14/6/113).
---

## References


[Interactive curriculum learning increases and homogenizes motor smoothness](https://www.nature.com/articles/s41598-024-53253-3)

*Published in Scientific Reports 2024. Co-authored with Baptiste Caramiaux, Antoine Loriette and Olivier Sigaud.*
</div>