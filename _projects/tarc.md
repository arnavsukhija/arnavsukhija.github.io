---
layout: page
title: "TARC: Time-Adaptive Robotic Control"
img: /assets/img/publication_preview/TARC_perturbation_Go1.gif
importance: 1
category: work
layout: distill
published: true
date: 2025-10-18 11:30:00
tags: [Reinforcement Learning, Robotics, Adaptive-Control, publication]
authors:
  - name: Arnav Sukhija
    url: "https://arnavsukhija.github.io/"
  - name: Lenart Treven
    url: "https://lenarttreven.github.io/" 
    affiliations:
      name: LAS<d-footnote>Learning & Adaptive Systems Group</d-footnote> & ACL<d-footnote>Automatic Control Laboratory</d-footnote>, ETH Zurich
  - name: Jin Cheng
    url: "https://jin-cheng.me/"
    affiliations:
      name: CRL<d-footnote>Computational Robotics Lab</d-footnote>, ETH Zurich
  - name: Florian Dörfler
    url: "https://people.ee.ethz.ch/~floriand/"
    affiliations:
      name: ACL, ETH Zurich
  - name: Stelian Coros
    url: "http://crl.ethz.ch/people/coros/index.html"
    affiliations:
      name: CRL, ETH Zurich
  - name: Andreas Krause
    url: "https://las.inf.ethz.ch/krausea"
    affiliations:
      name: LAS, ETH Zurich
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/w0y6uusnPYc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Abstract

Fixed-frequency control in robotics imposes a trade-off between the efficiency of low-frequency control and the robustness of high-frequency control, a limitation not seen in adaptable biological systems. We address this with a reinforcement learning approach in which policies jointly select control actions and their application durations, enabling robots toautonomously modulate their control frequency in response to situational demands. We validate our method with zero-shot sim-to-real experiments on two distinct hardware platforms: a high-speed RC car and a quadrupedal robot. Our method matches or outperforms fixed-frequency baselines in terms of rewards while significantly reducing the control frequency and exhibiting adaptive frequency control under real-world conditions.


# Overview

<figure style="text-align: center;">
  <img src="/assets/img/tarc_overview.png" alt="Overview of the TARC Architecture">
  <figcaption style="margin-top: 5px;">
    Overview of the **TARC Architecture**. TARC allows policies to learn both an action and its according application duration, allowing the agent to implicitly select its own control frequency. To encourage low-frequency control where adequate, the environment returns a penalized reward (with a cost c) to the agent such that it learns a trade-off between applying high-frequency and low-frequency control. 
  </figcaption>
</figure>

# Method

<figure style="text-align: center;">
  <img src="/assets/img/tarc_method.png" alt="Overview of our Deployment method">
  <figcaption style="margin-top: 5px;">
    Overview of our **Deployment setup**. We validate TARC through zero-shot sim-to-real deployment on two distinct robotic platforms. Policies are learned entirely offline with the help of simulators and deployed zero-shot on to the hardware without further fine-tuning.
  </figcaption>
</figure>

# Results

## Quadrupedal locomotion

<div style="display: flex; justify-content: space-between; gap: 20px;">
  <div style="flex: 1;">
    <h3>Run Then Turn Scenario</h3>
    <video width="100%" height="auto" controls>
      <source src="/assets/video/tarc_videos/Go1_RunThenTurn.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
  
  <div style="flex: 1;">
    <h3>Applying perturbations</h3>
    <video width="100%" height="auto" controls>
      <source src="/assets/video/tarc_videos/Go1_perturbation.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
</div>

<div style="text-align: center; margin-top: 30px;">
  <img src="/assets/img/tarc_go1Results.png" alt="Comparative plot of TARC vs fixed-frequency control on quadrupedal robot" style="width: 80%;">
  <figcaption style="margin-top: 10px; font-size: 0.9em; width: 80%; margin-left: auto; margin-right: auto;">
    Comparison of TARC with a 50Hz baseline on the first scenario. TARC outperforms the baseline in terms of total rewards while, on average, using a significantly lower control frequency. 
  </figcaption>
</div>

## RC Car

<div style="text-align: center;">
  <video width="80%" height="auto" controls>
    <source src="/assets/video/tarc_videos/rc-car-video.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</div>


<div style="text-align: center; margin-top: 30px;">
  <img src="/assets/img/tarc_rcCarResults.png" alt="Comparative plot of TARC vs fixed-frequency control on quadrupedal robot" style="width: 80%;">
  <figcaption style="margin-top: 10px; font-size: 0.9em; width: 80%; margin-left: auto; margin-right: auto;">
    Comparison of TARC with a 30Hz baseline on the RC Car. TARC matches the fixed-frequency baseline while reducing the control frequency by more than half. 
  </figcaption>
</div>

