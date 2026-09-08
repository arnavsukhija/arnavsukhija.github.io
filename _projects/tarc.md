---
layout: distill
title: "TARC: Time-Adaptive Robotic Control"
img: /assets/img/publication_preview/TARC_perturbation_Go1.gif
importance: 1
category: work
published: true
date: 2026-09-08 12:00:00
permalink: /projects/tarc/
tags: [Reinforcement Learning, Robotics, Adaptive-Control, VLA, publication]
authors:
  - name: Arnav Sukhija
    url: "https://arnavsukhija.github.io/"
    affiliations:
      name: ETH Zurich
  - name: Lenart Treven
    url: "https://lenarttreven.github.io/"
    affiliations:
      name: LAS<d-footnote>Learning & Adaptive Systems Group</d-footnote> & ACL<d-footnote>Automatic Control Laboratory</d-footnote>, ETH Zurich
  - name: Jin Cheng
    url: "https://jin-cheng.me/"
    affiliations:
      name: CRL<d-footnote>Computational Robotics Lab</d-footnote>, ETH Zurich
  - name: Florian Dörfler
    url: "https://dorfler.ethz.ch/"
    affiliations:
      name: ACL, ETH Zurich
  - name: Stelian Coros
    url: "https://crl.ethz.ch/people/coros/index.html"
    affiliations:
      name: CRL, ETH Zurich
  - name: Andreas Krause
    url: "https://las.inf.ethz.ch/krausea"
    affiliations:
      name: LAS, ETH Zurich
---

<style>
  /* Figures with baked-in white backgrounds (TikZ line art, matplotlib plots)
     are illegible on a dark page. Invert in dark mode; hue-rotate keeps the
     coloured curves roughly true. */
  html[data-theme="dark"] .tarc-invert {
    filter: invert(1) hue-rotate(180deg);
  }

  /* Neutral plate behind the platform photos, so the white-background Go1
     shot does not dissolve into the page. Theme-aware. */
  .tarc-plate { background: #f4f4f4; }
  html[data-theme="dark"] .tarc-plate { background: #2b2b2b; }

  /* Captions and callout inherit the theme's text colour instead of
     hard-coding black. */
  .tarc-caption {
    font-size: 0.9em;
    text-align: center;
    margin-top: 12px;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
    opacity: 0.85;
  }
  .tarc-callout {
    max-width: 760px;
    margin: 32px auto;
    padding: 20px 24px;
    border-left: 3px solid currentColor;
  }
  .tarc-row {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 24px;
    margin-top: 20px;
  }
</style>

<div style="text-align: center; margin-bottom: 24px;">
  <p style="font-size: 1.05em; font-weight: 600; letter-spacing: 0.04em; margin-bottom: 12px;">
    Conference on Robot Learning (CoRL) 2026 &middot; Austin, TX
  </p>
  <p>
    <a href="https://arxiv.org/abs/2510.23176">Paper</a> &nbsp;&middot;&nbsp;
    <a href="https://youtu.be/lTcANSTLAYU">Video</a> &nbsp;&middot;&nbsp;
    <a href="https://github.com/arnavsukhija/tarc">Code</a>
  </p>
</div>

<div style="text-align: center;">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/lTcANSTLAYU" title="TARC: Time-Adaptive Robotic Control" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

<div class="tarc-callout">
  <p style="margin-top: 0;"><strong>A robot does not need the same control rate when standing still as when recovering from a push.</strong> TARC lets the policy decide, choosing both <em>what</em> action to take and <em>how long</em> to hold it, subject to a budget the user specifies directly.</p>
  <ul style="margin-bottom: 0;">
    <li><strong>64% fewer policy queries</strong> on the Unitree Go1, at equal or better task reward</li>
    <li><strong>21% less commanded throttle travel</strong> on a drifting RC car</li>
    <li><strong>38% fewer transformer forward passes</strong> on a &pi;<sub>0</sub> vision-language-action model</li>
    <li>Runs on top of <strong>any RL algorithm</strong> &mdash; the constraint enters as a single scalar in the reward</li>
  </ul>
</div>

# Adaptation, not just a lower rate

<div style="text-align: center; margin-top: 20px;">
  <video width="85%" height="auto" autoplay loop muted playsinline controls style="border-radius: 4px;">
    <source src="/assets/video/tarc_videos/Go1_perturbation.mp4" type="video/mp4">
  </video>
</div>

<p class="tarc-caption">
TARC's control frequency during a perturbation experiment on the Go1. The policy holds <strong>16.7 Hz</strong> during stable standstill, spikes to <strong>50 Hz</strong> the instant it is pushed, and drops straight back once the robot stabilises. No fixed-rate controller can do this.
</p>

# Abstract

Most robotic systems rely on fixed-frequency discrete-time controllers, creating a trade-off between the efficiency of low-frequency control and the responsiveness of high-frequency feedback. As a result, systems typically default to high control rates for robustness, at the cost of wasted inference bandwidth and unnecessary actuation. Addressing this, we introduce **Time-Adaptive Robotic Control (TARC)**, a reinforcement learning framework in which the policy jointly predicts a control action and its duration of application. TARC learns temporally extended actions by optimizing task performance under soft or hard constraints on the number of control switches, enabling adaptive modulation of control rates. We evaluate TARC on two robotic hardware platforms — a high-speed RC car and the Unitree Go1 quadruped — and on a vision-language action model in simulation, where each query incurs a costly transformer forward pass which we aim to minimize. Across all settings, TARC matches the performance of high-frequency discrete-time controllers while operating at less than half their control frequency. Unlike fixed-rate controllers, TARC adapts its control frequency online, allocating high-frequency feedback only when required.

# Overview

<div style="text-align: center;">
  <img class="tarc-invert" src="/assets/img/tarc_query_schedule.png" alt="TARC's adaptive query schedule" style="width: 85%;">
</div>

<p class="tarc-caption">
The policy is queried only when a new action is needed. Between queries the previous action is held for &Delta;<i>t</i> steps: short holds during demanding phases, long holds when the situation is stable, cutting inference cost proportionally.
</p>

A time-adaptive policy maps the state to a pair: an action <i>u</i><sub>t</sub> and a duration &Delta;<i>t</i> for which that action is held. One query therefore covers &Delta;<i>t</i> control steps, and the effective control frequency <i>f</i> = <i>f</i><sub>max</sub> / &Delta;<i>t</i> becomes something the policy chooses online rather than a number fixed at design time.

# Method

We pose this as a **constrained MDP**: maximize task reward subject to a budget on the expected number of policy queries. With <i>q</i><sub>t</sub> &isin; {0, 1} indicating whether the policy is queried at step <i>t</i>,

$$
\max_{\pi} \; \mathbb{E}_{\pi}\!\left[\sum_{t} \gamma^{t} r(x_t, u_t)\right]
\quad \text{s.t.} \quad
\mathbb{E}_{\pi}\!\left[\sum_{t} \gamma^{t} q_t\right] \leq \frac{K}{1-\gamma},
$$

where <i>K</i> &isin; (0, 1] is the query budget — a fixed-frequency controller queries at every step, saturating it at <i>K</i> = 1. Constraining the rate itself has two practical consequences:

- **The Lagrangian relaxation adds a single scalar to the existing reward.** Grouping terms by decision epoch gives the per-decision reward \\( R(s_t, a_t) = \big(\sum_{k=0}^{\Delta t - 1} \gamma^{k} r(x_{t+k}, u_t)\big) - c \\). No task-specific reward redesign, and any standard RL algorithm applies — we use PPO throughout.
- **Under the hard-constraint variant, dual ascent tunes the multiplier automatically.** The user specifies a target query rate <i>K</i> directly, rather than specifying a cost which affects the query rate implicitly.

<div style="display: flex; flex-wrap: wrap; gap: 16px; margin-top: 20px; max-width: 900px; margin-left: auto; margin-right: auto;">
  <figure style="flex: 1 1 220px; margin: 0; text-align: center;">
    <img class="tarc-plate" src="/assets/img/rccar.png"
         alt="High-speed radio-controlled car"
         style="width: 100%; aspect-ratio: 4/3; object-fit: cover; object-position: center; border-radius: 6px;">
    <figcaption style="font-size: 0.85em; margin-top: 8px;">(a) Radio-Controlled car</figcaption>
  </figure>
  <figure style="flex: 1 1 220px; margin: 0; text-align: center;">
    <img class="tarc-plate" src="/assets/img/Unitree_go1.jpg"
         alt="Unitree Go1 quadruped"
         style="width: 100%; aspect-ratio: 4/3; object-fit: contain; object-position: center; border-radius: 6px;">
    <figcaption style="font-size: 0.85em; margin-top: 8px;">(b) Unitree Go1 quadruped</figcaption>
  </figure>
  <figure style="flex: 1 1 220px; margin: 0; text-align: center;">
    <img class="tarc-plate" src="/assets/img/libero_benchmark.png"
         alt="LIBERO benchmark pick-and-place task"
         style="width: 100%; aspect-ratio: 4/3; object-fit: cover; object-position: center 75%; border-radius: 6px;">
    <figcaption style="font-size: 0.85em; margin-top: 8px;">(c) LIBERO benchmark</figcaption>
  </figure>
</div>

<p class="tarc-caption" style="max-width: 900px;">
Three platforms spanning very different demands: a drifting RC car, a quadruped, and a vision-language-action model. Policies for the two hardware platforms are trained entirely in simulation and deployed <strong>zero-shot</strong>, with no fine-tuning — the average frequencies selected in simulation transfer almost exactly to the real robots.
</p>

# Results

## Vision-language-action models

Each query to a VLA is a full transformer forward pass, which makes inference frequency a direct deployment cost — and makes this the setting where adaptive querying pays off most. We apply TARC at the post-training stage of a frozen &pi;<sub>0</sub> checkpoint on the LIBERO benchmark, using diffusion steering to predict how much of an action chunk to execute before re-querying.

<div class="tarc-row">
  <div style="flex: 0 1 320px;">
    <img src="/assets/img/libero_enhanced2.gif" alt="TARC controlling a pi-0 policy on a LIBERO pick-and-place task" style="width: 100%; border-radius: 4px;">
  </div>
  <div style="flex: 1 1 380px;">
    <img class="tarc-invert" src="/assets/img/tarc_vlaResults.png" alt="TARC on the LIBERO benchmark against fixed query-frequency baselines" style="width: 100%;">
  </div>
</div>

<p class="tarc-caption">
TARC matches the success rate of the fixed chunk-size-20 baseline while using an average chunk length of 32.1 ± 7.8, roughly <strong>38% fewer transformer forward passes</strong>. Against a fixed baseline matched to TARC's own average chunk length (32), TARC achieves a <em>higher</em> success rate.
</p>

That last comparison is the informative one. A fixed schedule running at TARC's average rate does worse than TARC — so the gain comes from *where* the queries are spent, not simply from querying less. The learned chunk length also varies per task: longer where sustained open-loop execution suffices, shorter where the task demands frequent replanning.

## Quadrupedal locomotion

<div class="tarc-row">
  <div style="flex: 1 1 300px;">
    <video width="100%" height="auto" autoplay loop muted playsinline controls style="border-radius: 4px;">
      <source src="/assets/video/tarc_videos/Go1_RunThenTurn.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
  <div style="flex: 1 1 300px;">
    <img class="tarc-invert" src="/assets/img/tarc_go1Results.png" alt="TARC vs fixed-frequency control on the Go1" style="width: 100%;">
  </div>
</div>

<p class="tarc-caption">
<em>Run Then Turn</em>, one of three scenarios unseen during training. Across all three, TARC exceeds the 50 Hz baseline in task reward at less than half its control frequency — <strong>365 policy queries per 1000 environment steps versus 1000</strong>, a 64% reduction in onboard inference load.
</p>

Control frequency responds to task difficulty on its own: lower on smooth low-speed turning, higher during velocity changes and abrupt transitions.

## RC car

<div class="tarc-row">
  <div style="flex: 1 1 300px;">
    <video width="100%" height="auto" autoplay loop muted playsinline controls style="border-radius: 4px;">
      <source src="/assets/video/tarc_videos/rc-car-video.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
  <div style="flex: 1 1 300px;">
    <img class="tarc-invert" src="/assets/img/tarc_rcCarResults.png" alt="TARC vs fixed-frequency control on the RC car" style="width: 100%;">
  </div>
</div>

<p class="tarc-caption">
A reverse-parking maneuver requiring a drift. TARC is equivalent to the 30 Hz baseline in task reward at less than half the control frequency, with substantially smaller variance across rollouts and <strong>21% less total commanded throttle travel</strong> — a direct reduction in mechanical wear.
</p>

# BibTeX

```bibtex
@misc{sukhija2025tarctimeadaptiveroboticcontrol,
      title={TARC: Time-Adaptive Robotic Control}, 
      author={Arnav Sukhija and Lenart Treven and Jin Cheng and Florian Dörfler and Stelian Coros and Andreas Krause},
      year={2025},
      eprint={2510.23176},
      archivePrefix={arXiv},
      primaryClass={cs.RO},
      url={https://arxiv.org/abs/2510.23176}, 
}
```

# Acknowledgments

This project has received funding from the Swiss National Science Foundation under NCCR Automation, grant agreement 51NF40 180545.
