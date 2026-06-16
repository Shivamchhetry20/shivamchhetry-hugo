---
title: "CI/CD with Hydra & PyTorch"
subtitle: "MLOps Pipeline for Deep Learning"
excerpt: "A production-style MLOps project demonstrating CI/CD workflows for deep learning models. Uses Facebook Hydra for configuration management and PyTorch for model training, wired together with automated testing and pipeline orchestration."
date: 2024-03-01
author: "Shivam Chhetry"
featured: true
draft: false
tags:
  - Python
  - PyTorch
  - Hydra
  - MLOps
  - CI/CD
categories:
  - Machine Learning
  - MLOps
layout: single
links:
- icon: github
  icon_pack: fab
  name: Code
  url: https://github.com/Shivamchhetry20/CICD-with-Hydra-Pytorch
---

## CI/CD with Hydra & PyTorch

A hands-on **MLOps** project that wires together modern tooling for reproducible deep learning workflows — from experiment configuration to automated continuous integration.

### Key Components

- **Hydra** (by Facebook Research): Hierarchical configuration management for training jobs — override any parameter from the command line or config files without touching code.
- **PyTorch**: Model definition, training loops, and evaluation.
- **CI/CD Pipeline**: Automated testing and validation of model training code on every push, ensuring reproducibility and catching regressions early.

### What It Demonstrates

- Separating model configuration from code for clean experiment tracking
- Structured logging of training runs with Hydra's output directories
- Writing testable ML code that can be validated in CI environments
- Composable configs for quickly sweeping hyperparameters

### Why This Matters

ML projects often suffer from "it worked on my machine" problems. This project shows how to structure a PyTorch project so that experiments are fully reproducible, configs are version-controlled, and the training pipeline is automatically validated — habits that scale from research to production.
