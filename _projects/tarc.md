---
layout: page
title: "TARC: Time-Adaptive Robotic Control"
img: /assets/img/publication_preview/racecar_simfsvgd.gif
importance: 1
category: work
layout: distill
published: true
date: 2025-09-25 18:15:00
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

<iframe width="560" height="315" src="https://youtu.be/w0y6uusnPYc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Abstract
Fixed-frequency control in robotics imposes a trade-off between the efficiency of low-frequency control and the robustness of high-frequency control, a limitation not seen in adaptable biological systems. We address this with a reinforcement learning approach in which policies jointly select control actions and their application durations, enabling robots toautonomously modulate their control frequency in response to situational demands. We validate our method with zero-shot sim-to-real experiments on two distinct hardware platforms: a high-speed RC car and a quadrupedal robot. Our method matches or outperforms fixed-frequency baselines in terms of rewards while significantly reducing the control frequency and exhibiting adaptive frequency control under real-world conditions.


# Overview


