---
title: "Everything in AI is Geometry: A Deep Dive Into My GitHub Projects"
subtitle: "Exploring vectors, manifolds, and high-dimensional space through real ML and data engineering projects"
excerpt: "Everything in machine learning is geometry. From training a CNN to detect fingers, to orchestrating ML pipelines with Hydra, to warehousing terabytes of business data — it all reduces to points moving through high-dimensional space. This post walks through all my major GitHub projects through that geometric lens, plus the hottest trends in AI right now."
date: 2026-06-15
author: "Shivam Chhetry"
draft: false
tags:
  - Machine Learning
  - Deep Learning
  - Data Engineering
  - AI
  - Geometry
  - MLOps
categories:
  - AI
  - Machine Learning
  - Data Engineering
layout: single
---

I've spent the last couple of years building projects that span deep learning, MLOps, and data engineering. When I look back at all of them — the custom Keras callbacks, the finger-counting CNN, the Hydra-powered training pipeline, the SQL data warehouse — I notice something they all share.

**They are all, at their core, geometry problems.**

A neural network is just a function that bends and folds space. A data warehouse organizes information along axes you can slice through. A training loop is a walk through a hilly landscape. Once you internalize this geometric intuition, the design decisions in every project start to make obvious sense.

In this post, I'll walk through each of my major GitHub projects through the lens of vectors and high-dimensional geometry — with easy examples, visual diagrams, and a section on the hottest topics in AI right now.

---

## The Geometry of Everything: What Is a Vector?

Before we dive into the projects, we need a shared language. The fundamental object in machine learning is a **vector**.

A vector is just a list of numbers. That's it. `[3.0, 1.5, 7.2]` is a vector. If a vector has *n* numbers, it represents a point in *n*-dimensional space.

**Easy example**: Imagine describing a person with three features — height (1.75m), weight (70kg), age (25). This person becomes the point `[1.75, 70, 25]` in 3-dimensional space. Add more features (income, education, location ZIP...) and you're in 10-dimensional, 100-dimensional, 1000-dimensional space. The math is the same — you just can't draw it anymore.

<div style="text-align:center; margin: 2.5rem 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 700 220" width="100%" style="max-width:700px; display:inline-block;">
  <defs>
    <linearGradient id="cardBg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#1a1a2e"/>
      <stop offset="100%" style="stop-color:#16213e"/>
    </linearGradient>
  </defs>
  <rect width="700" height="220" fill="url(#cardBg)" rx="12"/>
  <!-- 1D -->
  <text x="105" y="35" fill="#aaa" font-family="monospace" font-size="13" text-anchor="middle">1D Vector</text>
  <line x1="30" y1="75" x2="180" y2="75" stroke="#555" stroke-width="2"/>
  <circle cx="120" cy="75" r="10" fill="#f5576c"/>
  <text x="120" y="105" fill="#f5576c" font-family="monospace" font-size="12" text-anchor="middle">[3.5]</text>
  <line x1="30" y1="73" x2="30" y2="77" stroke="#777"/><line x1="180" y1="73" x2="180" y2="77" stroke="#777"/>
  <text x="30" y="90" fill="#666" font-family="monospace" font-size="10" text-anchor="middle">0</text>
  <text x="180" y="90" fill="#666" font-family="monospace" font-size="10" text-anchor="middle">5</text>

  <!-- 2D -->
  <text x="350" y="35" fill="#aaa" font-family="monospace" font-size="13" text-anchor="middle">2D Vector</text>
  <line x1="270" y1="150" x2="430" y2="150" stroke="#555" stroke-width="2"/>
  <line x1="350" y1="50" x2="350" y2="175" stroke="#555" stroke-width="2"/>
  <polygon points="430,150 420,145 420,155" fill="#555"/>
  <polygon points="350,50 345,60 355,60" fill="#555"/>
  <circle cx="400" cy="90" r="10" fill="#4facfe"/>
  <line x1="350" y1="150" x2="400" y2="90" stroke="#4facfe" stroke-width="1.5" stroke-dasharray="5,3"/>
  <text x="410" y="85" fill="#4facfe" font-family="monospace" font-size="11">[2.0, 2.0]</text>
  <text x="435" y="155" fill="#666" font-family="monospace" font-size="10">x₁</text>
  <text x="353" y="48" fill="#666" font-family="monospace" font-size="10">x₂</text>

  <!-- N-D -->
  <text x="595" y="35" fill="#aaa" font-family="monospace" font-size="13" text-anchor="middle">N-D Vector</text>
  <rect x="520" y="55" width="150" height="130" fill="#0d0d1a" rx="6" stroke="#333"/>
  <text x="595" y="80" fill="#43e97b" font-family="monospace" font-size="12" text-anchor="middle">x₁ = 1.75</text>
  <text x="595" y="100" fill="#43e97b" font-family="monospace" font-size="12" text-anchor="middle">x₂ = 70.0</text>
  <text x="595" y="120" fill="#43e97b" font-family="monospace" font-size="12" text-anchor="middle">x₃ = 25.0</text>
  <text x="595" y="140" fill="#555" font-family="monospace" font-size="12" text-anchor="middle">x₄ = 0.92</text>
  <text x="595" y="158" fill="#555" font-family="monospace" font-size="12" text-anchor="middle">...   ...</text>
  <text x="595" y="175" fill="#555" font-family="monospace" font-size="12" text-anchor="middle">xₙ = 7.3</text>

  <!-- Arrows between diagrams -->
  <text x="215" y="115" fill="#f5a623" font-family="sans-serif" font-size="20" text-anchor="middle">→</text>
  <text x="465" y="115" fill="#f5a623" font-family="sans-serif" font-size="20" text-anchor="middle">→</text>
  <text x="215" y="135" fill="#f5a623" font-family="sans-serif" font-size="9" text-anchor="middle">add a dimension</text>
  <text x="465" y="135" fill="#f5a623" font-family="sans-serif" font-size="9" text-anchor="middle">add features</text>

  <text x="350" y="210" fill="#555" font-family="sans-serif" font-size="11" text-anchor="middle">Each data point is a location in feature space. More features = more dimensions.</text>
</svg>
<p style="color:#888; font-size:0.85rem; margin-top:0.5rem;">From a single number on a line, to a point in a plane, to a location in high-dimensional space — all the same idea.</p>
</div>

Here's the key insight: **machine learning is about finding patterns in how these points are arranged in space.** Classification = finding a surface that separates different colored points. Regression = finding a surface that passes through the points. Clustering = finding which points are close to each other.

Everything else is detail.

---

## Project 1: callbackaun — Dancing on the Loss Landscape

**GitHub**: [callbackaun](https://github.com/Shivamchhetry20/callbackaun)

When you train a neural network, you're playing a game with billions of numbers called **weights**. Each set of weights makes your model more or less accurate — that accuracy is captured by the **loss function**. A high loss means bad predictions; a low loss means good ones.

Here's the geometric picture: imagine all your weights laid out as a landscape. The height at any point is the loss value. Your goal is to find the lowest valley in this landscape.

<div style="text-align:center; margin: 2.5rem 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 700 280" width="100%" style="max-width:700px; display:inline-block;">
  <defs>
    <linearGradient id="bg2" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" style="stop-color:#0d1b2a"/>
      <stop offset="100%" style="stop-color:#1b263b"/>
    </linearGradient>
  </defs>
  <rect width="700" height="280" fill="url(#bg2)" rx="12"/>

  <!-- Loss curve (a bowl / parabola shape with noise) -->
  <path d="M 60 40 Q 120 220 200 245 Q 260 255 310 240 Q 350 228 380 205 Q 420 175 460 130 Q 500 90 550 55 Q 600 30 640 25"
        stroke="#f5576c" stroke-width="2.5" fill="none" opacity="0.4"/>

  <!-- Main smooth bowl curve -->
  <path d="M 60 230 C 150 230, 220 240, 270 242 C 330 244, 380 230, 430 200 C 480 170, 530 120, 580 60"
        stroke="#f5576c" stroke-width="3" fill="none"/>

  <!-- Shaded area under curve -->
  <path d="M 60 230 C 150 230, 220 240, 270 242 C 330 244, 380 230, 430 200 C 480 170, 530 120, 580 60 L 580 270 L 60 270 Z"
        fill="#f5576c" opacity="0.06"/>

  <!-- Gradient descent steps -->
  <circle cx="550" cy="100" r="9" fill="#ffdd57"/>
  <line x1="550" y1="100" x2="490" y2="145" stroke="#ffdd57" stroke-width="2"/>
  <polygon points="490,145 499,137 500,148" fill="#ffdd57"/>

  <circle cx="490" cy="145" r="9" fill="#f5a623"/>
  <line x1="490" y1="145" x2="430" y2="185" stroke="#f5a623" stroke-width="2"/>
  <polygon points="430,185 440,178 440,188" fill="#f5a623"/>

  <circle cx="430" cy="185" r="9" fill="#43e97b"/>
  <line x1="430" y1="185" x2="370" y2="215" stroke="#43e97b" stroke-width="2"/>
  <polygon points="370,215 380,207 381,218" fill="#43e97b"/>

  <circle cx="370" cy="215" r="9" fill="#4facfe"/>
  <line x1="370" y1="215" x2="330" y2="232" stroke="#4facfe" stroke-width="2"/>
  <polygon points="330,232 341,226 340,237" fill="#4facfe"/>

  <!-- Minimum marker -->
  <circle cx="290" cy="242" r="12" fill="none" stroke="#ffffff" stroke-width="2.5"/>
  <circle cx="290" cy="242" r="5" fill="#ffffff"/>
  <text x="290" y="268" fill="#ffffff" font-family="sans-serif" font-size="11" text-anchor="middle">global min</text>

  <!-- Labels -->
  <text x="560" y="88" fill="#ffdd57" font-family="sans-serif" font-size="12">Start</text>
  <text x="60" y="220" fill="#aaa" font-family="sans-serif" font-size="12">Loss</text>
  <line x1="60" y1="30" x2="60" y2="255" stroke="#555" stroke-width="1.5"/>
  <line x1="50" y1="255" x2="650" y2="255" stroke="#555" stroke-width="1.5"/>
  <polygon points="650,255 640,250 640,260" fill="#555"/>
  <polygon points="60,30 55,40 65,40" fill="#555"/>
  <text x="650" y="270" fill="#aaa" font-family="sans-serif" font-size="12">Weights →</text>

  <!-- LR annotation -->
  <text x="460" y="130" fill="#ccc" font-family="sans-serif" font-size="11">← learning rate</text>
  <text x="460" y="143" fill="#ccc" font-family="sans-serif" font-size="11">controls step size</text>

  <text x="350" y="22" fill="#aaa" font-family="sans-serif" font-size="13" text-anchor="middle" font-weight="bold">Loss Landscape: Gradient Descent Walk</text>
</svg>
<p style="color:#888; font-size:0.85rem; margin-top:0.5rem;">Each colored dot is a training step. The model takes steps downhill until it reaches the minimum loss. The learning rate controls how big each step is.</p>
</div>

**The problem**: The real loss landscape isn't a neat bowl. It's a jagged, high-dimensional terrain with valleys, plateaus, ridges, and saddle points. Navigating it blindly is inefficient.

That's exactly why I built **callbackaun** — a Python package of custom Keras callbacks that make this navigation smarter:

### Dynamic Learning Rate Callback
If you take too large a step, you leap over the valley. Too small, and you crawl forever. The dynamic LR callback **watches the loss plateau** and shrinks your step size when you stop making progress — like switching from hiking boots to careful footing on a steep descent.

```python
from callbackaun import DynamicLRCallback

model.fit(X_train, y_train,
    callbacks=[DynamicLRCallback(patience=5, factor=0.5)])
```

Geometrically: when the gradient magnitude drops (the terrain is flattening), reduce the learning rate to avoid overshooting the minimum.

### Verbose Training Logger
In high-dimensional spaces, you can't "see" where you are. The verbose logger is your GPS — it prints loss, accuracy, and timing at each epoch so you can detect when you're stuck on a plateau or diverging away from the minimum.

### Interactive Mid-Training Prompt
Sometimes the loss has converged but you want to verify. The interactive prompt pauses training every N epochs and asks: *"Keep going for 10 more epochs?"* — putting you back in the driver's seat. In terms of the landscape metaphor: you can look around, assess the terrain, and decide whether to keep walking.

> **Simple analogy**: Imagine you're hiking blind in the mountains, searching for the lowest point. callbackaun gives you a GPS (logger), auto-shortens your stride when you're near flat ground (dynamic LR), and pauses to ask if you want to continue (interactive prompt). Each one is a geometric strategy for navigating the loss landscape smarter.

---

## Project 2: Fingers Prediction — Classification in High-Dimensional Space

**GitHub**: [Fingers-prediction-using-custom-callbackaun](https://github.com/Shivamchhetry20/Fingers-prediction-using-custom-callbackaun)

Let's say you want to train a model to count fingers in an image. A 64×64 grayscale image is 4,096 pixels. Each pixel is a number. So your image becomes a vector in **4,096-dimensional space**.

4,096 dimensions. That's completely unvisualizable — but completely workable with the right geometry.

### The Manifold Hypothesis

Here's a beautiful insight: even though your images live in 4,096 dimensions, *natural images of hands* occupy a tiny, low-dimensional curved surface — a **manifold** — within that vast space. A random collection of 4,096 pixel values won't look like a hand. The set of all "hand images" is a tiny island in a giant ocean.

<div style="text-align:center; margin: 2.5rem 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 700 260" width="100%" style="max-width:700px; display:inline-block;">
  <defs>
    <linearGradient id="bg3" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" style="stop-color:#0a0a1a"/>
      <stop offset="100%" style="stop-color:#1a1a2e"/>
    </linearGradient>
  </defs>
  <rect width="700" height="260" fill="url(#bg3)" rx="12"/>

  <!-- Random noise points (ambient space) -->
  <g fill="#333" opacity="0.7">
    <circle cx="80" cy="40" r="2.5"/><circle cx="120" cy="70" r="2.5"/><circle cx="55" cy="110" r="2.5"/>
    <circle cx="95" cy="140" r="2.5"/><circle cx="145" cy="30" r="2.5"/><circle cx="170" cy="100" r="2.5"/>
    <circle cx="30" cy="85" r="2.5"/><circle cx="200" cy="55" r="2.5"/><circle cx="60" cy="170" r="2.5"/>
    <circle cx="190" cy="155" r="2.5"/><circle cx="110" cy="195" r="2.5"/><circle cx="155" cy="185" r="2.5"/>
    <circle cx="45" cy="215" r="2.5"/><circle cx="185" cy="230" r="2.5"/><circle cx="135" cy="240" r="2.5"/>
    <circle cx="70" cy="250" r="2.5"/><circle cx="210" cy="200" r="2.5"/><circle cx="90" cy="230" r="2.5"/>
  </g>

  <!-- Label for ambient space -->
  <text x="115" y="258" fill="#555" font-family="sans-serif" font-size="11" text-anchor="middle">Random pixel noise</text>
  <text x="115" y="16" fill="#555" font-family="sans-serif" font-size="11" text-anchor="middle">(ambient 4096-D space)</text>

  <!-- Arrow -->
  <text x="260" y="138" fill="#f5a623" font-family="sans-serif" font-size="28">→</text>
  <text x="255" y="158" fill="#f5a623" font-family="sans-serif" font-size="10">CNN layers</text>
  <text x="252" y="170" fill="#f5a623" font-family="sans-serif" font-size="10">fold the space</text>

  <!-- Manifold region: hand images cluster -->
  <!-- Class 0 fingers cluster -->
  <ellipse cx="410" cy="115" rx="50" ry="35" fill="#f5576c" opacity="0.15" stroke="#f5576c" stroke-width="1.5" stroke-dasharray="5,3"/>
  <circle cx="395" cy="105" r="7" fill="#f5576c"/>
  <circle cx="415" cy="120" r="7" fill="#f5576c"/>
  <circle cx="430" cy="108" r="7" fill="#f5576c"/>
  <circle cx="405" cy="130" r="7" fill="#f5576c"/>
  <circle cx="420" cy="95" r="7" fill="#f5576c"/>
  <text x="410" y="165" fill="#f5576c" font-family="sans-serif" font-size="11" text-anchor="middle">✊ 0 fingers</text>

  <!-- Class 1-5 fingers clusters -->
  <ellipse cx="510" cy="85" rx="45" ry="30" fill="#4facfe" opacity="0.15" stroke="#4facfe" stroke-width="1.5" stroke-dasharray="5,3"/>
  <circle cx="495" cy="80" r="7" fill="#4facfe"/>
  <circle cx="515" cy="75" r="7" fill="#4facfe"/>
  <circle cx="530" cy="88" r="7" fill="#4facfe"/>
  <circle cx="505" cy="97" r="7" fill="#4facfe"/>
  <text x="510" y="130" fill="#4facfe" font-family="sans-serif" font-size="11" text-anchor="middle">☝️ 1 finger</text>

  <ellipse cx="600" cy="160" rx="45" ry="30" fill="#43e97b" opacity="0.15" stroke="#43e97b" stroke-width="1.5" stroke-dasharray="5,3"/>
  <circle cx="585" cy="155" r="7" fill="#43e97b"/>
  <circle cx="605" cy="148" r="7" fill="#43e97b"/>
  <circle cx="620" cy="162" r="7" fill="#43e97b"/>
  <circle cx="595" cy="172" r="7" fill="#43e97b"/>
  <text x="600" y="205" fill="#43e97b" font-family="sans-serif" font-size="11" text-anchor="middle">✌️ 2 fingers</text>

  <!-- "..." for more classes -->
  <text x="500" y="220" fill="#888" font-family="sans-serif" font-size="18" text-anchor="middle">· · · 3, 4, 5 fingers clusters · · ·</text>

  <!-- Boundary line suggestion -->
  <line x1="460" y1="60" x2="560" y2="230" stroke="#ffdd57" stroke-width="1.5" stroke-dasharray="6,4" opacity="0.5"/>

  <text x="350" y="248" fill="#555" font-family="sans-serif" font-size="11" text-anchor="middle">After CNN transforms: finger classes become separated clusters in feature space</text>
</svg>
<p style="color:#888; font-size:0.85rem; margin-top:0.5rem;">CNNs transform the chaotic high-dimensional pixel space into a structured feature space where different finger counts cluster together and can be separated by hyperplanes.</p>
</div>

### How CNNs Do It — Layer by Layer

A Convolutional Neural Network is a **manifold unfolding machine**. Each layer applies a geometric transformation to the data:

1. **Conv layers**: Detect local patterns (edges, textures) — they're learning to describe the local curvature of the manifold.
2. **Pooling layers**: Reduce dimensionality while preserving structure — they "zoom out" and make the manifold smoother.
3. **Dense layers**: Perform linear transformations in the feature space — they find the hyperplane separating classes.
4. **Softmax output**: Converts distances to probabilities — how close is this point to each class centroid?

**Easy analogy**: Imagine you have a crumpled piece of paper. On one side of the paper (before crumpling) you drew zero-finger hands; on the other side, one-finger hands. When crumpled, they're all mixed up. A CNN is like a magical paper-uncrumpler — it carefully unfolds the paper until the two classes are on opposite sides of a flat sheet. That's the hyperplane.

The `callbackaun` package was used directly in this project to control training — the dynamic LR callback prevented overshooting the minimum in the fingers classifier's loss landscape.

---

## Project 3: CI/CD with Hydra & PyTorch — Navigating Hyperparameter Space

**GitHub**: [CICD-with-Hydra-Pytorch](https://github.com/Shivamchhetry20/CICD-with-Hydra-Pytorch)

Every deep learning model has a set of choices you make before training: learning rate, batch size, number of layers, hidden size, dropout rate, optimizer type... Each choice is a **dimension**. Together, they define a **hyperparameter space**.

If you have 8 hyperparameters, each with 10 possible values, that's 10⁸ = 100 million configurations. You can't test them all. But the performance of each configuration defines a **surface** over this hyperparameter space — and you're trying to find the peak of that surface.

<div style="text-align:center; margin: 2.5rem 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 700 280" width="100%" style="max-width:700px; display:inline-block;">
  <defs>
    <linearGradient id="bg4" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#0d1b2a"/>
      <stop offset="100%" style="stop-color:#162032"/>
    </linearGradient>
  </defs>
  <rect width="700" height="280" fill="url(#bg4)" rx="12"/>

  <!-- Grid for hyperparameter space -->
  <g stroke="#1e3a5f" stroke-width="1">
    <line x1="80" y1="60" x2="620" y2="60"/>
    <line x1="80" y1="100" x2="620" y2="100"/>
    <line x1="80" y1="140" x2="620" y2="140"/>
    <line x1="80" y1="180" x2="620" y2="180"/>
    <line x1="80" y1="220" x2="620" y2="220"/>
    <line x1="80" y1="60" x2="80" y2="230"/>
    <line x1="170" y1="60" x2="170" y2="230"/>
    <line x1="260" y1="60" x2="260" y2="230"/>
    <line x1="350" y1="60" x2="350" y2="230"/>
    <line x1="440" y1="60" x2="440" y2="230"/>
    <line x1="530" y1="60" x2="530" y2="230"/>
    <line x1="620" y1="60" x2="620" y2="230"/>
  </g>

  <!-- Performance heatmap (circles showing accuracy at different LR/BS combos) -->
  <!-- Low performance (red) -->
  <circle cx="125" cy="80" r="18" fill="#f5576c" opacity="0.4"/><text x="125" y="84" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.51</text>
  <circle cx="215" cy="80" r="18" fill="#f5576c" opacity="0.5"/><text x="215" y="84" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.58</text>
  <circle cx="125" cy="160" r="18" fill="#f5a623" opacity="0.5"/><text x="125" y="164" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.67</text>
  <!-- Medium performance (orange) -->
  <circle cx="305" cy="80" r="20" fill="#f5a623" opacity="0.6"/><text x="305" y="84" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.71</text>
  <circle cx="215" cy="160" r="20" fill="#f5a623" opacity="0.65"/><text x="215" y="164" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.75</text>
  <!-- High performance (green) -->
  <circle cx="395" cy="120" r="24" fill="#43e97b" opacity="0.7"/><text x="395" y="124" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.89</text>
  <circle cx="305" cy="160" r="24" fill="#43e97b" opacity="0.75"/><text x="305" y="164" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.91</text>
  <!-- Peak (bright) -->
  <circle cx="395" cy="200" r="28" fill="#4facfe" opacity="0.85"/><text x="395" y="204" fill="white" font-family="monospace" font-size="9" text-anchor="middle" font-weight="bold">0.96 ★</text>
  <circle cx="485" cy="160" r="22" fill="#43e97b" opacity="0.7"/><text x="485" y="164" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.88</text>
  <circle cx="575" cy="120" r="18" fill="#f5a623" opacity="0.55"/><text x="575" y="124" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.72</text>
  <circle cx="485" cy="80" r="16" fill="#f5576c" opacity="0.45"/><text x="485" y="84" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.60</text>
  <circle cx="575" cy="200" r="18" fill="#f5a623" opacity="0.5"/><text x="575" y="204" fill="white" font-family="monospace" font-size="9" text-anchor="middle">0.70</text>

  <!-- Axes labels -->
  <text x="350" y="250" fill="#aaa" font-family="sans-serif" font-size="12" text-anchor="middle">Learning Rate →</text>
  <text x="30" y="145" fill="#aaa" font-family="sans-serif" font-size="12" text-anchor="middle" transform="rotate(-90,30,145)">Batch Size →</text>
  <text x="80" y="56" fill="#666" font-family="monospace" font-size="9">1e-4</text>
  <text x="170" y="56" fill="#666" font-family="monospace" font-size="9">3e-4</text>
  <text x="260" y="56" fill="#666" font-family="monospace" font-size="9">1e-3</text>
  <text x="350" y="56" fill="#666" font-family="monospace" font-size="9">3e-3</text>
  <text x="440" y="56" fill="#666" font-family="monospace" font-size="9">1e-2</text>
  <text x="530" y="56" fill="#666" font-family="monospace" font-size="9">3e-2</text>
  <text x="75" y="65" fill="#666" font-family="monospace" font-size="9">16</text>
  <text x="75" y="105" fill="#666" font-family="monospace" font-size="9">32</text>
  <text x="75" y="145" fill="#666" font-family="monospace" font-size="9">64</text>
  <text x="75" y="185" fill="#666" font-family="monospace" font-size="9">128</text>
  <text x="75" y="225" fill="#666" font-family="monospace" font-size="9">256</text>
  <text x="350" y="22" fill="#ccc" font-family="sans-serif" font-size="13" text-anchor="middle" font-weight="bold">Hyperparameter Space: Accuracy Surface (2D slice)</text>
  <text x="350" y="270" fill="#555" font-family="sans-serif" font-size="10" text-anchor="middle">Each circle = one training run. Size = accuracy. Blue star = best config found.</text>
</svg>
<p style="color:#888; font-size:0.85rem; margin-top:0.5rem;">A 2D slice of hyperparameter space, showing how accuracy (bubble size/color) varies with learning rate and batch size. The real space has many more dimensions.</p>
</div>

### Where Hydra Fits In

**Facebook Hydra** solves the configuration explosion problem. Instead of hardcoding values or maintaining a jungle of JSON files, Hydra lets you:

- Define hierarchical config files (YAML)
- Override any parameter from the command line
- Compose configs modularly (e.g., swap the optimizer config without touching the model config)

```yaml
# config/model/resnet.yaml
_target_: src.models.ResNet
hidden_size: 256
num_layers: 4
dropout: 0.3

# config/optimizer/adam.yaml
_target_: torch.optim.Adam
lr: 3e-3
weight_decay: 1e-5
```

```bash
# Override learning rate from command line
python train.py optimizer.lr=1e-4 model.dropout=0.5
```

**Geometrically**: Hydra treats each experiment as a **named point in hyperparameter space**. Every run is recorded with its coordinates (config values) and its height (performance metric). Over time, you're drawing a map of the performance landscape — and you can make increasingly informed decisions about where to explore next.

### Why CI/CD Is a Geometric Necessity

Without CI/CD, exploring hyperparameter space is chaos: configs get lost, experiments aren't reproducible, you can't track which configuration produced which result. The CI/CD pipeline enforces:

- **Coordinate tracking**: Every config is version-controlled, so you know exactly which point in hyperparameter space each experiment corresponds to.
- **Reproducibility**: The same coordinates always produce the same height (loss) — if they don't, your pipeline is broken.
- **Automated validation**: CI runs your training code on push, catching issues before they corrupt your map.

> **Simple analogy**: Hydra is your coordinate system for hyperparameter space. CI/CD is the auditor that makes sure your GPS coordinates are accurate and your map is trustworthy.

---

## Project 4: Data Science Project Template — The Pipeline as a Space Transform

**GitHub**: [My-Data-Science-template](https://github.com/Shivamchhetry20/My-Data-Science-template)

A data science pipeline is a **sequence of coordinate transformations**. Each step changes the shape, dimensionality, or distribution of your data in feature space.

Here's the geometric view of a typical data science pipeline:

```
Raw Data          Cleaned Data         Feature Matrix        Predictions
   ↓                   ↓                     ↓                   ↓
Noisy cloud      Filtered cloud        Rotated/scaled        Class labels /
in R^p  ──────▶  (outliers gone)  ──▶  R^k (k < p)    ──▶   regression
                                       (PCA, encoding)         values
```

Each arrow is a transformation in geometric space:
- **Data cleaning** removes points that are noise (outliers far from the data manifold)
- **Feature engineering** adds or removes dimensions, often rotating the axes to align with the data's natural structure
- **Normalization** rescales all dimensions so no single feature dominates by magnitude

### Why Folder Structure Is a Geometric Concept

The template enforces a strict separation:

```
data/
  raw/       ← original coordinates (never modify)
  processed/ ← transformed coordinates (reproducible derivation)
src/
  features/  ← the transformation functions
  models/    ← the learning algorithms
notebooks/   ← exploratory mappings
```

This separation mirrors a fundamental principle in geometry: **keep your original coordinate system intact, and apply transformations cleanly and reversibly**. The `raw/` folder is your origin. The `processed/` folder is your transformed space. If you mix them up, you lose the ability to trace transformations back to their source.

> **Real-world impact**: I've seen projects where raw data was overwritten by cleaning scripts. You can't go back. You can't reproduce the experiment. It's like erasing the original map and only keeping a blurry photocopy. The template makes this mistake structurally impossible.

---

## Project 5: SQL Data Warehouse — Business Data as a Multidimensional Cube

**GitHub**: [SQL-Data-Warehouse-project](https://github.com/Shivamchhetry20/SQL-Data-Warehouse-project)

The term "data cube" isn't metaphorical — it's literally how OLAP (Online Analytical Processing) systems were designed. Business data lives in a multidimensional space where each dimension is an axis you can slice along.

**Easy example**: Sales data has at least three natural axes:
- **Time** (when did the sale happen?)
- **Geography** (where did it happen?)  
- **Product** (what was sold?)

Each sale is a point in this 3D cube. Analytical queries are geometric operations: slices, projections, and aggregations across dimensions.

### The Medallion Architecture as a Noise Reduction Pipeline

The Bronze → Silver → Gold architecture maps perfectly to a geometric pipeline:

<div style="text-align:center; margin: 2.5rem 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 700 200" width="100%" style="max-width:700px; display:inline-block;">
  <defs>
    <linearGradient id="bg5" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" style="stop-color:#0d1b2a"/>
      <stop offset="100%" style="stop-color:#1b263b"/>
    </linearGradient>
    <linearGradient id="bronzeGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#cd7f32"/>
      <stop offset="100%" style="stop-color:#a0522d"/>
    </linearGradient>
    <linearGradient id="silverGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#c0c0c0"/>
      <stop offset="100%" style="stop-color:#909090"/>
    </linearGradient>
    <linearGradient id="goldGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#ffd700"/>
      <stop offset="100%" style="stop-color:#ffa500"/>
    </linearGradient>
  </defs>
  <rect width="700" height="200" fill="url(#bg5)" rx="12"/>

  <!-- Bronze box -->
  <rect x="40" y="50" width="160" height="100" fill="url(#bronzeGrad)" rx="10" opacity="0.85"/>
  <text x="120" y="95" fill="white" font-family="sans-serif" font-size="15" font-weight="bold" text-anchor="middle">BRONZE</text>
  <text x="120" y="112" fill="white" font-family="sans-serif" font-size="10" text-anchor="middle">Raw Ingestion</text>
  <text x="120" y="127" fill="rgba(255,255,255,0.8)" font-family="sans-serif" font-size="9" text-anchor="middle">CRM + ERP data</text>
  <text x="120" y="140" fill="rgba(255,255,255,0.8)" font-family="sans-serif" font-size="9" text-anchor="middle">No transforms</text>

  <!-- Noise dots around bronze box -->
  <g fill="#cd7f32" opacity="0.35">
    <circle cx="55" cy="40" r="4"/><circle cx="170" cy="35" r="3"/><circle cx="190" cy="60" r="5"/>
    <circle cx="48" cy="165" r="3"/><circle cx="185" cy="170" r="4"/><circle cx="200" cy="140" r="3"/>
  </g>

  <!-- Arrow 1 -->
  <line x1="200" y1="100" x2="260" y2="100" stroke="#aaa" stroke-width="2"/>
  <polygon points="260,100 250,95 250,105" fill="#aaa"/>
  <text x="230" y="90" fill="#aaa" font-family="sans-serif" font-size="9" text-anchor="middle">cleanse</text>
  <text x="230" y="118" fill="#aaa" font-family="sans-serif" font-size="9" text-anchor="middle">dedup</text>

  <!-- Silver box -->
  <rect x="260" y="50" width="160" height="100" fill="url(#silverGrad)" rx="10" opacity="0.85"/>
  <text x="340" y="95" fill="#1a1a1a" font-family="sans-serif" font-size="15" font-weight="bold" text-anchor="middle">SILVER</text>
  <text x="340" y="112" fill="#1a1a1a" font-family="sans-serif" font-size="10" text-anchor="middle">Cleansed Data</text>
  <text x="340" y="127" fill="#333" font-family="sans-serif" font-size="9" text-anchor="middle">Type cast + dedup</text>
  <text x="340" y="140" fill="#333" font-family="sans-serif" font-size="9" text-anchor="middle">Standardized</text>

  <!-- Arrow 2 -->
  <line x1="420" y1="100" x2="480" y2="100" stroke="#aaa" stroke-width="2"/>
  <polygon points="480,100 470,95 470,105" fill="#aaa"/>
  <text x="450" y="90" fill="#aaa" font-family="sans-serif" font-size="9" text-anchor="middle">model</text>
  <text x="450" y="118" fill="#aaa" font-family="sans-serif" font-size="9" text-anchor="middle">aggregate</text>

  <!-- Gold box -->
  <rect x="480" y="50" width="180" height="100" fill="url(#goldGrad)" rx="10" opacity="0.9"/>
  <text x="570" y="95" fill="#1a1a1a" font-family="sans-serif" font-size="15" font-weight="bold" text-anchor="middle">GOLD</text>
  <text x="570" y="112" fill="#1a1a1a" font-family="sans-serif" font-size="10" text-anchor="middle">Star Schema</text>
  <text x="570" y="127" fill="#333" font-family="sans-serif" font-size="9" text-anchor="middle">dim_customers</text>
  <text x="570" y="140" fill="#333" font-family="sans-serif" font-size="9" text-anchor="middle">fact_sales · dim_products</text>

  <text x="350" y="185" fill="#555" font-family="sans-serif" font-size="10" text-anchor="middle">Each layer reduces noise and increases analytical dimensionality (more useful axes, fewer useless ones)</text>
</svg>
<p style="color:#888; font-size:0.85rem; margin-top:0.5rem;">The Medallion Architecture: each layer reduces noise and adds structure, projecting the data onto axes that matter for business analysis.</p>
</div>

- **Bronze = raw data in ambient space** — like pixels in 4,096-D space before any CNN layer. Everything is there, including noise, duplicates, and garbage.
- **Silver = cleansed manifold** — like after the first few CNN layers. The noise is gone. The data lives on a cleaner, smaller manifold.
- **Gold = analytically structured dimensions** — like the final CNN embedding layer. The data is projected onto exactly the axes that matter: customers, products, time, sales.

### Star Schema as a Geometric Projection

<div style="text-align:center; margin: 2.5rem 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 500 400" width="100%" style="max-width:500px; display:inline-block;">
  <defs>
    <linearGradient id="bg6" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#0a0a1a"/>
      <stop offset="100%" style="stop-color:#1a1a2e"/>
    </linearGradient>
  </defs>
  <rect width="500" height="400" fill="url(#bg6)" rx="12"/>

  <!-- Connection lines -->
  <line x1="250" y1="200" x2="250" y2="80" stroke="#ffd700" stroke-width="2" opacity="0.6"/>
  <line x1="250" y1="200" x2="120" y2="155" stroke="#4facfe" stroke-width="2" opacity="0.6"/>
  <line x1="250" y1="200" x2="380" y2="155" stroke="#f5576c" stroke-width="2" opacity="0.6"/>
  <line x1="250" y1="200" x2="120" y2="300" stroke="#43e97b" stroke-width="2" opacity="0.6"/>
  <line x1="250" y1="200" x2="380" y2="300" stroke="#f5a623" stroke-width="2" opacity="0.6"/>

  <!-- Fact table (center) -->
  <rect x="185" y="165" width="130" height="70" fill="#2a2a4a" rx="8" stroke="#ffd700" stroke-width="2"/>
  <text x="250" y="192" fill="#ffd700" font-family="monospace" font-size="12" font-weight="bold" text-anchor="middle">fact_sales</text>
  <text x="250" y="208" fill="#ccc" font-family="monospace" font-size="9" text-anchor="middle">customer_id (FK)</text>
  <text x="250" y="220" fill="#ccc" font-family="monospace" font-size="9" text-anchor="middle">product_id (FK)</text>
  <text x="250" y="232" fill="#ccc" font-family="monospace" font-size="9" text-anchor="middle">amount · date_id</text>

  <!-- Dim: Customer -->
  <rect x="175" y="40" width="150" height="60" fill="#1a1a35" rx="6" stroke="#4facfe" stroke-width="1.5"/>
  <text x="250" y="62" fill="#4facfe" font-family="monospace" font-size="11" font-weight="bold" text-anchor="middle">dim_customers</text>
  <text x="250" y="77" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">customer_id (PK)</text>
  <text x="250" y="90" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">name · segment · country</text>

  <!-- Dim: Product -->
  <rect x="310" y="115" width="165" height="65" fill="#1a1a35" rx="6" stroke="#f5576c" stroke-width="1.5"/>
  <text x="392" y="137" fill="#f5576c" font-family="monospace" font-size="11" font-weight="bold" text-anchor="middle">dim_products</text>
  <text x="392" y="152" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">product_id (PK)</text>
  <text x="392" y="165" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">name · category · price</text>

  <!-- Dim: Date -->
  <rect x="25" y="115" width="160" height="65" fill="#1a1a35" rx="6" stroke="#43e97b" stroke-width="1.5"/>
  <text x="105" y="137" fill="#43e97b" font-family="monospace" font-size="11" font-weight="bold" text-anchor="middle">dim_date</text>
  <text x="105" y="152" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">date_id (PK)</text>
  <text x="105" y="165" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">year · month · quarter</text>

  <!-- Dim: Region -->
  <rect x="310" y="280" width="165" height="55" fill="#1a1a35" rx="6" stroke="#f5a623" stroke-width="1.5"/>
  <text x="392" y="302" fill="#f5a623" font-family="monospace" font-size="11" font-weight="bold" text-anchor="middle">dim_region</text>
  <text x="392" y="317" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">region_id (PK)</text>
  <text x="392" y="330" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">city · country · zone</text>

  <!-- Dim: Salesperson -->
  <rect x="25" y="280" width="165" height="55" fill="#1a1a35" rx="6" stroke="#c084fc" stroke-width="1.5"/>
  <line x1="250" y1="200" x2="107" y2="280" stroke="#c084fc" stroke-width="2" opacity="0.6"/>
  <text x="107" y="302" fill="#c084fc" font-family="monospace" font-size="11" font-weight="bold" text-anchor="middle">dim_employee</text>
  <text x="107" y="317" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">emp_id (PK)</text>
  <text x="107" y="330" fill="#aaa" font-family="monospace" font-size="9" text-anchor="middle">name · dept · region</text>

  <text x="250" y="385" fill="#555" font-family="sans-serif" font-size="10" text-anchor="middle">Star Schema: fact table at center, dimension tables as analytical axes</text>
</svg>
<p style="color:#888; font-size:0.85rem; margin-top:0.5rem;">The star schema positions the fact table at the origin, with dimension tables as the coordinate axes of the analytical space. A SQL query is a geometric projection along selected axes.</p>
</div>

**The geometric insight**: a SQL query like `SELECT customer.country, SUM(sales.amount) FROM fact_sales JOIN dim_customers...` is literally a **projection** from the full N-dimensional business space onto the single axis of "country." Aggregation functions (SUM, AVG, COUNT) are the mathematical operators that compute statistics along projected dimensions.

The **T-SQL stored procedures** in this project encode these geometric transformations: Bronze→Silver is a noise-filtering transform; Silver→Gold is a dimensionality reduction and re-projection into the analytical space.

---

## Trending Hot Topics in AI (2026)

Now that we've grounded every project in geometric intuition, let's zoom out to where the field is heading. Every one of these topics, when you look closely, is a new geometric idea.

### 1. Large Language Models and the Geometry of Language

GPT-4, Claude, Gemini — these models embed words and sentences into vectors in very high-dimensional spaces (768, 1024, or 4096 dimensions). Related words end up geometrically close to each other. Analogies become vector arithmetic:

```
king - man + woman ≈ queen
```

That equation works because the relationship "male → female" is a consistent direction (a vector) in embedding space. Language is geometry.

The "transformer" in these models is essentially a learned attention mechanism that lets each word "look at" every other word, computing similarity scores in the embedding space. Self-attention is literally **dot products** (a measure of geometric similarity) between query and key vectors.

### 2. Retrieval-Augmented Generation (RAG)

RAG is pure geometry. You embed all your documents into a vector space. You embed the user's query into the same space. Then you find the documents whose vectors are **nearest neighbors** to the query vector. The insight: semantic similarity = geometric proximity.

```
similar meaning ↔ small cosine distance in embedding space
```

The vector database (Pinecone, Weaviate, FAISS) is just an efficient data structure for finding nearest neighbors in high-dimensional space.

### 3. AI Agents and State Space Navigation

AI agents that plan through multi-step tasks are navigating a **state space** — the set of all possible world states. Each action moves the agent to a new point in state space. The goal is to find a path from the current state to the desired goal state.

This is exactly the same as gradient descent: you're trying to navigate from a starting point to a target, taking steps that improve your position. Modern reasoning models (o1, o3, DeepSeek-R1, Claude 3.7 Sonnet) learn to plan trajectories through reasoning space before committing to an answer.

### 4. Mixture of Experts (MoE)

MoE models partition the input space into regions, each handled by a specialized "expert" sub-network. The routing mechanism learns a **Voronoi tessellation** of the input space: "if the input vector falls in region R, use expert E."

This is geometric partitioning. GPT-4, Gemini Ultra, and Mistral 8x7B all use MoE architectures because they scale more efficiently than dense models — you grow the total parameter count but only activate a slice of the experts for each input.

### 5. Diffusion Models and Manifold Walking

Stable Diffusion, DALL-E 3, Flux — these are **diffusion models**. They work by learning to reverse a "diffusion" process: starting from a point on the image manifold, they gradually add Gaussian noise until the image becomes random noise. Then, at generation time, they reverse this process, starting from random noise and walking step-by-step back toward the image manifold.

It's literally a walk through high-dimensional space, guided by a learned "denoising" vector field that always points toward the manifold of real images.

### 6. Multimodal Models and Shared Embedding Spaces

Models like CLIP, GPT-4V, and Gemini Ultra can understand both images and text. The trick is to learn a **shared embedding space** where semantically related images and texts map to nearby vectors.

This is trained with **contrastive learning**: pull matching image-text pairs together in embedding space; push non-matching pairs apart. The result is a space where you can search for images using text queries (like Google Image Search) or compare different modalities geometrically.

### 7. Test-Time Compute and Reasoning as Search

The newest generation of reasoning models (o3, o4-mini, Claude's extended thinking) shows that you can trade *inference compute* for better performance. Instead of giving one answer immediately, the model searches through a **tree of possible reasoning steps**, evaluating many paths before committing.

This is a tree search in reasoning space. More compute = more nodes explored = better chance of finding a path to the right answer. It's the same geometry, applied to the space of thoughts instead of the space of images.

---

## Putting It All Together

Every project in my GitHub — from the simplest callback utility to the full data warehouse — is an instantiation of the same core idea: **structure and transform high-dimensional data to make the pattern you care about geometrically obvious.**

| Project | Geometric Idea |
|---|---|
| callbackaun | Navigate the loss landscape smarter with adaptive step sizes |
| Fingers Prediction | CNN as manifold unfolding: separate finger-count classes in feature space |
| CICD-Hydra-PyTorch | Map the hyperparameter landscape systematically and reproducibly |
| Data Science Template | Clean pipeline = invertible, traceable transformations through feature space |
| SQL Data Warehouse | Medallion = noise reduction; Star Schema = analytical projection onto business axes |

And every hot topic in AI is just a new frontier of the same geometry: LLMs find patterns in the geometry of language, diffusion models walk manifolds, agents search state spaces, and MoE models partition input space into specialized regions.

The more clearly you see these projects through a geometric lens, the more elegant and inevitable their design choices appear. The loss landscape is real. The manifold hypothesis is real. The hyperparameter space is real. You're always navigating high-dimensional space — the only question is whether you do it deliberately or by accident.

---

*Thanks for reading! These are the projects that shaped how I think about ML and data engineering. If you're curious about any of them, all the code is on GitHub — links in the project section of this site.*
