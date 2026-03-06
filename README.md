# ARC Challenge – LLM Reasoning Experiments

This repository contains experiments conducted for the **Abstraction and Reasoning Corpus (ARC) Challenge**, a benchmark designed to evaluate general reasoning capabilities in AI systems.

The goal of this project was to investigate whether **large language models (LLMs)** can improve reasoning performance on ARC tasks through **fine-tuning, reasoning augmentation, and candidate sampling strategies**.

This project was developed as part of a **university team project** focused on exploring approaches toward artificial general intelligence (AGI).

---

## Project Motivation

The **ARC (Abstraction and Reasoning Corpus)** benchmark is widely regarded as one of the most challenging benchmarks for evaluating general intelligence in AI systems.

Unlike traditional benchmarks, ARC tests whether models can:

- generalize to **completely new tasks**
- perform **abstract reasoning**
- identify **transformation rules** from examples
- apply these rules to unseen inputs

Despite major advances in large language models, ARC has remained difficult to solve. This challenge highlights the gap between **pattern recognition** and **true reasoning ability** in current AI systems. :contentReference[oaicite:0]{index=0}

---

## Dataset

The ARC dataset consists of tasks that require identifying transformation rules between input and output grids.

Each task contains:

- **2–10 training examples** demonstrating a transformation rule
- **1–3 test inputs** where the model must apply the learned rule
- Grids represented as **2D arrays of integers (0–9)** where each digit represents a color

The objective is to infer the transformation logic from the examples and generate the correct output grid.

---

## Methodology

Our approach explored multiple strategies to improve reasoning performance:

### 1. Baseline Model

We first implemented a baseline model for solving ARC tasks.

Two variants were explored:

- **Baseline without reasoning**
- **Baseline with explicit reasoning prompts**

Adding reasoning explanations improved interpretability and helped analyze model behaviour.

---

### 2. Dataset Enhancement

To improve reasoning capability, we augmented the ARC dataset with **human reasoning annotations**.

Example reasoning structure:

- Identify transformation rules
- Determine output dimensions
- Apply systematic transformation logic
- Handle intersections and value propagation rules

This allowed the model to learn **structured reasoning patterns** instead of relying purely on pattern matching.

---

### 3. LLM Fine-Tuning

We fine-tuned **LLaMA-3.1-8B** using **LoRA (Low-Rank Adaptation)** with the **Unsloth framework**.

Key training setup:

**LoRA Configuration**
- Rank: 16
- Alpha: 16
- Rank stabilization enabled
- No dropout

**Training Hyperparameters**

- Learning rate: 3e-4
- Batch size: 8
- Epochs: 1
- Warmup steps: 10
- Optimizer: AdamW (8-bit)

Unsloth was used because it provides:

- VRAM-efficient training
- integration with modern fine-tuning frameworks
- simplified setup for parameter-efficient training.

---

### 4. Hyperparameter Search

We conducted a grid search across **54 configurations** to analyze the effect of:

- reasoning prompts
- different training parameters
- candidate sampling strategies

The goal was to understand how reasoning signals impact generalization on ARC tasks.

---

### 5. Candidate Sampling

A candidate sampling approach was implemented to improve prediction robustness.

Instead of generating a single deterministic output:

1. The correct solution was extracted.
2. Controlled noise was added to create alternative candidate outputs.
3. Candidates were ranked based on confidence scores.

A **BigBird-RoBERTa model** was used to rank candidate outputs.

Training setup:

- Epochs: 3
- Learning rate: 5e-5
- Batch size: 2
- Loss function: Mean Squared Error

This ranking approach aimed to improve model robustness when multiple plausible solutions exist.

---

## Results

Key findings from the experiments:

- **Explicit reasoning significantly improved performance.**
- Models without reasoning performed substantially worse.
- Candidate sampling showed potential but requires improved ranking mechanisms.

Common model errors included:

- minor pixel prediction mistakes
- correct reasoning but incorrect final outputs
- difficulties generalizing across unseen transformations.

---

## Key Takeaways

Important insights from the project:

- Reasoning augmentation is critical for improving ARC performance.
- LLMs alone struggle with abstract reasoning without structured guidance.
- Ranking multiple candidate solutions can improve robustness.
- Combining **reasoning annotations with ranking mechanisms** is a promising direction.

---

## Tools and Technologies

- Python
- PyTorch
- HuggingFace Transformers
- LoRA / PEFT
- Unsloth
- BigBird-RoBERTa
- ARC Dataset

---

## Challenges

During development we encountered several challenges:

- limited compute resources
- cluster usage limitations
- dependency issues during environment setup
- complexity of reasoning tasks in ARC

Despite these difficulties, the project provided valuable insights into **reasoning in modern AI systems**.

---

## Future Work

Potential improvements include:

- improved candidate ranking mechanisms
- better negative sampling strategies
- larger reasoning-annotated datasets
- exploration of advanced reasoning architectures.

---

## Acknowledgements

This project was developed as part of a university team project exploring reasoning capabilities in large language models.

The ARC benchmark is available at:

https://arcprize.org/
