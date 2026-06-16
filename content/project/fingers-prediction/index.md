---
title: "Fingers Prediction"
subtitle: "Custom Callbacks for Deep Learning Training"
excerpt: "A deep learning model that predicts the number of fingers shown in an image. Built with Keras, this project showcases the callbackaun package in action — using custom callbacks to manage learning rate, log training details, and interactively control training epochs."
date: 2023-11-01
author: "Shivam Chhetry"
featured: false
draft: false
tags:
  - Python
  - Deep Learning
  - Computer Vision
  - Keras
categories:
  - Machine Learning
  - Computer Vision
layout: single
links:
- icon: github
  icon_pack: fab
  name: Code
  url: https://github.com/Shivamchhetry20/Fingers-prediction-using-custom-callbackaun
---

## Fingers Prediction Using Custom Callbacks

A **computer vision** project that trains a deep learning model to predict the number of fingers shown in an image (0–5). The primary goal is demonstrating practical use of the `callbackaun` custom callbacks package in a real training scenario.

### Model

- Image classification network built with **Keras/TensorFlow**
- Trained on a labeled dataset of hand images with finger counts as targets
- Achieves multi-class classification across 6 classes (0 to 5 fingers)

### Custom Callbacks in Action

This project serves as a showcase for the [`callbackaun`](https://github.com/Shivamchhetry20/callbackaun) package:

- **Dynamic learning rate callback** adjusts the rate during training based on loss plateaus
- **Verbose logger** prints detailed training statistics after each epoch
- **Interactive epoch prompt** pauses every N epochs to ask whether to continue training — useful when experimenting with how many epochs actually improve performance

### Key Takeaway

By packaging the callback logic separately in `callbackaun`, the model training code stays clean and focused. The callbacks are plug-and-play and reusable across other Keras projects.
