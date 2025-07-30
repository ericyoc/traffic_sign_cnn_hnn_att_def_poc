# Traffic Sign Safety: Synthetic Dataset, Adversarial Attacks & Defenses

## Overview

This repository implements a deep learning models (CNN, HNN1, HNN2) for classifying traffic signs as **safe** or **not safe** using synthetic images generated with conditional GANs (cGANs). The system evaluates model robustness under **compound adversarial attacks** and applies classical and quantum-enhanced defenses across three neural architectures.

> **For academic and research purposes only.**

---

## Significance

- **Challenge:** Field collection of retroreflectivity and contrast is limited and time-consuming.
- **Solution:** A unique method combining traffic sign templates with field metadata to generate pixel-accurate synthetic signs across safe/unsafe conditions.
- **Outcome:** Enables image-only classification (no sensors required), scalable training, and safety compliance checks.

---

## Key Concepts

- **Retroreflectivity:** Light returned in the direction of its source, vital for nighttime visibility.
- **Contrast Ratio:** Difference in brightness between the legend and background, affecting visibility.
- **Templates:** STOP, YIELD, and UPWARD ARROW signs are used to isolate legend and background regions for precise synthetic generation using real field data.

---

## Dataset Structure

Generated using combinations of real metadata and template-defined structure:

```

/content/drive/MyDrive/traffic\_sign\_samples/
├── train/           # 6300 images
├── validation/      # 1350 images
├── test/            # 1350 images
├── train/train\_metadata.csv               # 6300 rows
├── validation/validation\_metadata.csv     # 1350 rows
├── test/test\_metadata.csv                 # 1350 rows

```

**Note:** A **subset** of these samples was used for adversarial evaluation.

Evaluation results saved to:

```

/content/drive/MyDrive/traffic\_sign\_samples/enhanced\_defense\_evaluation\_results.csv

```

Execution environment: **Google Colab** using **Google Drive** for data access and storage.

---

## Model Architectures

| Model        | Classical Layers | Quantum Layers | Classical % | Quantum % | Total Params |
|--------------|------------------|----------------|-------------|-----------|---------------|
| **CNN**      | 25.8M            | 0              | 100%        | 0%        | 25.8M         |
| **HNN1 (CQ)**| 18.7K            | 16.3K          | 53.5%       | 46.5%     | 35K           |
| **HNN2 (QC)**| 5.3K             | 29.7K          | 15.0%       | 85.0%     | 35K           |


**CNN is a classical convolutional neural network**

**HNN1 is a classical-quantum neural network**

**HNN2 is a quantum-classical neural network**

---

## Adversarial Attacks

Three **compound white-box attacks** were used to evaluate model robustness:

- **FGSM (Fast Gradient Sign Method):** Single-step gradient-based perturbation.
- **PGD (Projected Gradient Descent):** Iterative FGSM for stronger attacks.
- **CW (Carlini-Wagner):** Optimization-based attack minimizing distortion while forcing misclassification.

**Why Compound Attacks?**  
Compound attacks are more sophisticated than single attacks. They combine the strengths of multiple techniques (e.g., fast initialization, iterative refinement, or distortion minimization), making them harder to defend against. By layering multiple strategies, they simulate stronger, real-world adversarial scenarios and push model resilience to the limit.

**Compound Attacks Tested:**
- **FGSM + CW**
- **FGSM + PGD**
- **CW + PGD**

---

## Defense Strategies

### Adversarial Training

**Definition:**  
Train the model on adversarial examples alongside clean data.

**Purpose:**  
Improves resilience by exposing the model to adversarial patterns during learning.

**Variants Implemented:**
- FGSM + CW adversarial training  
- FGSM + PGD adversarial training  
- CW + PGD adversarial training  

---

### Randomization Defenses

**Definition:**  
Applies random transformations to disrupt the attacker’s ability to predict input structure.

**Purpose:**  
Breaks the assumption of a static model, preventing deterministic exploitation.

**Techniques Used:**
- **Random Resizing:** Random input scaling to misalign gradients.
- **Random Cropping:** Cuts portions of the input image, masking regions under attack.
- **Random Rotation:** Random angular transformation distorts known pixel positions.
- **Combined Randomization:** Applies all three to maximize input variability.

---

### Input Transformation Defenses

**Definition:**  
Applies preprocessing that neutralizes adversarial noise before classification.

**Purpose:**  
Reduces the impact of perturbations while preserving original content.

**Techniques Used:**
- **Image Quilting:** Rebuilds image using patch-based reconstruction to erase fine-grained perturbations.
- **JPEG Compression:** Removes subtle noise via lossy encoding.
- **Gaussian Blur:** Smooths sharp, artificial edges created by adversarial noise.
- **Combined Input Transformation:** Multi-technique application for layered filtering.
- **Differential Privacy:** Adds controlled noise to increase uncertainty for attackers while maintaining overall utility.
- **Adversarial Logit Pairing (ALP):** Encourages the model to output similar logits for clean and adversarial pairs.

---

## Evaluation Metrics

- **Clean Accuracy:** Accuracy on unperturbed test data.
- **Post-Attack Accuracy:** Accuracy after adversarial perturbation.
- **Precision, Recall, F1-score:** Safety classification performance.
- **Robustness:** Post-attack accuracy divided by clean accuracy.
- **Defense Effectiveness:** Improvement of metrics under each defense strategy.

---

## Limitations

- **Synthetic Gap:** Generated images may not represent full real-world variance.
- **Quantum Simulation:** Quantum circuits were simulated, not run on actual quantum devices.
- **Single-Frame Limitation:** Only single-frame (non-temporal) classification was performed.

---

## Applications

- MUTCD Compliance Validation  
- Smart City Infrastructure Monitoring  
- UAV/Autonomous Vehicle Vision Assurance  
- Low-cost Reflectivity Auditing via Image-based Methods  

---

## License

This repository is for **academic and research use only**.  

