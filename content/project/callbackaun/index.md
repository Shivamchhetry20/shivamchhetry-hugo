---
title: "callbackaun"
subtitle: "Custom Keras/TensorFlow Callbacks Package"
excerpt: "A Python package of custom Keras callbacks for deep learning training. Includes dynamic learning rate control, verbose per-epoch logging, and an interactive mid-training prompt to continue for N more epochs or halt — putting the researcher back in control."
date: 2023-10-01
author: "Shivam Chhetry"
featured: false
draft: false
tags:
  - Python
  - Deep Learning
  - Keras
  - Package
categories:
  - Machine Learning
  - Open Source
layout: single
links:
- icon: github
  icon_pack: fab
  name: Code
  url: https://github.com/Shivamchhetry20/callbackaun
---

## callbackaun

A Python package of custom **Keras/TensorFlow callbacks** built to give researchers finer control over the training loop.

### Key Callbacks

- **Dynamic Learning Rate Control**: Automatically adjusts the learning rate based on training signals, reducing the need to manually tune schedules.
- **Verbose Training Logger**: Prints detailed per-epoch training statistics — loss, metrics, and timing — in a readable, informative format.
- **Interactive Mid-Training Prompt**: Periodically pauses training to ask whether to continue for N more epochs or halt — useful for long runs where you want to inspect progress before committing compute.

### Use Case

Standard Keras training loops offer useful built-in callbacks but lack the kind of interactive control that's helpful during active experimentation. `callbackaun` fills that gap by making it easy to monitor and steer training without modifying the core model code.

### Installation

```bash
pip install callbackaun
```

Or clone directly from the [GitHub repository](https://github.com/Shivamchhetry20/callbackaun) and install in editable mode.
