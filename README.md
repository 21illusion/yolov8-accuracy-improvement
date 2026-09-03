# 🚀 YOLOv8 Accuracy Improvement for Object Detection

<p align="center">

  <img src="https://img.shields.io/badge/YOLOv8-Object%20Detection-00FFFF?style=for-the-badge&logo=yolo" alt="YOLOv8">

  <img src="https://img.shields.io/badge/PyTorch-Deep%20Learning-EE4C2C?style=for-the-badge&logo=pytorch" alt="PyTorch">

  <img src="https://img.shields.io/badge/Computer-Vision-6C63FF?style=for-the-badge" alt="Computer Vision">

  <img src="https://img.shields.io/badge/TensorRT-Inference-76B900?style=for-the-badge&logo=nvidia" alt="TensorRT">

  <img src="https://img.shields.io/badge/OpenVINO-Optimization-0071C5?style=for-the-badge&logo=intel" alt="OpenVINO">

</p>

<p align="center">
  <b>
    Improving YOLOv8 performance for object detection in complex scenes
    through architecture modification, experimentation, evaluation,
    and inference optimization.
  </b>
</p>

---

# 📌 Overview

This project investigates how modifications to the **YOLOv8n** architecture can affect object-detection performance across different and complex visual environments.

The work combines:

- 🧠 Neural network architecture modification
- 🔬 Experimental evaluation
- 🖼️ Data augmentation
- 📊 Multi-dataset benchmarking
- ⚡ Inference optimization
- 🚀 TensorRT and OpenVINO exploration

The objective was not only to improve accuracy, but also to study the trade-off between:

> **🎯 Accuracy ↔ ⚙️ Computational Cost ↔ ⚡ Inference Speed**

---

# 🎯 Project Objective

The main objective was to improve the performance of **YOLOv8n** while keeping the architecture relatively lightweight.

Instead of evaluating the modified model on only one dataset, the project tested it across multiple object-detection tasks:

| Dataset | Task |
|---|---|
| 🦺 PPE Dataset | Personal Protective Equipment detection |
| ⚽ Football Dataset | Football player detection |
| 🐄 Cattle Dataset | Cattle detection |
| 😷 Face Mask Dataset | Face-mask detection |
| 🧠 Brain Tumor MRI | Brain tumor detection |

---

# 🏗️ Architecture

The project started from the default **YOLOv8 architecture** and investigated several modifications intended to improve feature extraction and information flow.

## 🔧 Main Components Investigated

- `C3TR`
- `Adown`
- Additional convolutional layers
- Feature refinement and fusion modifications
- `PSA` attention
- `FOCUS` block

## 🔄 Architecture Workflow

```text
                 ┌──────────────────────┐
                 │      YOLOv8n         │
                 │   Baseline Model     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Architecture Analysis│
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Model Experiments  │
                 └──────────┬───────────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
           C3TR          Adown        Attention
              │             │             │
              └─────────────┼─────────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Modified YOLOv8n     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Multi-Dataset Testing│
                 └──────────────────────┘
```

---

# 🧪 Experiments

Not every modification improved the model.

Several experiments were tested to understand their effect on both **accuracy** and **computational requirements**.

## ❌ Additional Convolutional Blocks

**Goal:** Improve feature extraction by increasing model depth.

**Problem:**  
Introduced output-shape mismatches and architectural instability.

**Attempted solution:**  
Using stride `= 1` convolutions to preserve spatial resolution.

**Result:**  
The blocks behaved more like refinement layers, with minimal and unstable accuracy gains.

---

## ⚠️ Convolution Before Upsampling

Adding convolutional layers before upsampling produced some stability and slight accuracy improvements.

However, when combined with other modifications, the model could experience:

- Performance degradation
- Training instability
- Training failures

---

## ⚠️ FOCUS Block

The FOCUS block was investigated for:

- Better spatial compression
- Improved channel fusion

It helped on one dataset but did not generalize consistently across different datasets.

---

## ⚠️ PSA Attention

Adding **Parallel Split Attention (PSA)** after SPPF improved accuracy in experiments.

However, the computational requirements increased significantly:

```text
GFLOPs
8.1
 │
 ▼
~30 GFLOPs

Memory
~2 GB
 │
 ▼
~30 GB / epoch
```

Because of this substantial computational cost, the approach was considered impractical on limited hardware.

These experiments were useful for identifying the trade-offs between **accuracy and computational complexity**.

---

# 📊 Results

## 🦺 PPE Detection

**Dataset:** 8,814 images  
**Instances:** 22,077

| Metric | YOLOv8n | Modified YOLOv8 |
|---|---:|---:|
| Precision | 0.689 | 0.686 |
| Recall | 0.753 | **0.793** |
| mAP@0.5 | 0.738 | **0.752** |
| mAP@0.5:0.95 | 0.464 | **0.474** |

### ⚙️ Model Complexity

| Feature | YOLOv8n | Modified YOLOv8 |
|---|---:|---:|
| Layers | 72 | 104 |
| Parameters | 3,008,378 | **2,618,074** |
| GFLOPs | 8.1 | 8.3 |
| Inference Speed | 1.9 ms | 7.1 ms |

> 📌 The modified model improved recall and mAP while introducing an inference-speed trade-off.

---

# ⚽ Football Player Detection

**Dataset:** 11.8K images

| Metric | YOLOv8n | Modified YOLOv8 | Change |
|---|---:|---:|---:|
| Precision | 0.861 | **0.867** | ⬆️ +0.006 |
| Recall | 0.753 | **0.770** | ⬆️ +0.017 |
| mAP@0.5 | 0.812 | **0.819** | ⬆️ +0.007 |
| mAP@0.5:0.95 | 0.503 | **0.524** | ⬆️ +0.021 |

✅ All reported evaluation metrics improved on this dataset.

---

# 🐄 Cattle Detection

**Dataset:** 6,570 images

| Metric | YOLOv8n | Modified YOLOv8 | Change |
|---|---:|---:|---:|
| Precision | 0.942 | **0.947** | ⬆️ +0.005 |
| Recall | 0.703 | **0.710** | ⬆️ +0.007 |
| mAP@0.5 | 0.747 | 0.748 | ≈ |
| mAP@0.5:0.95 | 0.636 | **0.643** | ⬆️ +0.007 |

---

# 🧪 Data Augmentation Experiment

The project also evaluated image augmentation using:

```text
Brightness:
-19% → +19%

Gaussian Blur:
0 → 2.5 pixels
```

### Reported Results

| Metric | Before | After |
|---|---:|---:|
| Precision | 0.753 | **0.959** |
| Recall | 0.726 | 0.724 |
| mAP@0.5 | 0.757 | **0.762** |
| mAP@0.5:0.95 | 0.670 | **0.677** |

> 🔬 This experiment highlights the strong influence that data augmentation can have on individual evaluation metrics.

---

# 😷 Face Mask Detection

**Dataset:** 800 images

| Model | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 |
|---|---:|---:|---:|---:|
| YOLOv8n | 0.743 | 0.738 | 0.738 | 0.486 |
| YOLOv10n | 0.698 | 0.573 | 0.656 | 0.436 |
| YOLOv11n | 0.836 | 0.648 | 0.716 | 0.472 |
| **Modified YOLOv8n** | **0.855** | 0.704 | **0.779** | **0.497** |

🏆 The modified YOLOv8n achieved the highest reported precision and mAP@0.5 in this comparison.

---

# 🧠 Brain Tumor Detection

**Dataset:** 9,192 images

| Model | Parameters | GFLOPs | mAP@0.5 | mAP@0.5:0.95 | Inference |
|---|---:|---:|---:|---:|---:|
| YOLOv8n | 3.0M | 8.1 | 0.917 | 0.694 | 2.1 ms |
| YOLOv10n | 2.26M | 6.5 | 0.869 | 0.638 | 2.5 ms |
| YOLOv11n | 2.58M | 6.3 | 0.920 | 0.690 | 2.1 ms |
| **Modified YOLOv8n** | **2.61M** | 8.3 | **0.933** | **0.706** | 6.8 ms |

## 🏆 Best Reported Result

```text
mAP@0.5

YOLOv8n            0.917
Modified YOLOv8n   0.933
                   ▲
                 +0.016
```

The modified model achieved the highest reported mAP@0.5 and mAP@0.5:0.95 in this comparison, while requiring more inference time.

---

# 📈 Key Findings

## ✅ Accuracy

The modified architecture produced measurable improvements across several datasets and evaluation metrics.

## ✅ Model Size

The reported final comparison reduced the parameter count from approximately:

```text
3.0M → 2.61M parameters
```

## ✅ Computational Cost

The final model remained close to the baseline in GFLOPs:

```text
8.1 → 8.3 GFLOPs
```

## ⚠️ Inference Speed

Accuracy improvements came with an inference-speed trade-off on some experiments.

## 🔬 Generalization

Different architectural changes behaved differently across datasets, showing that an improvement on one dataset does not automatically generalize to another.

---

# 🖼️ Visual Results

## Before vs After

Add your visual comparison images here:

```markdown
![PPE Detection](results/comparisons/ppe.png)

![Football Detection](results/comparisons/football.png)

![Cattle Detection](results/comparisons/cattle.png)

![Face Mask Detection](results/comparisons/face-mask.png)

![Brain Tumor Detection](results/comparisons/brain-tumor.png)
```

---

# ⚡ Inference Optimization

Improving accuracy was only part of the project.

The project also explored deployment-oriented optimization.

## 🚀 TensorRT

TensorRT was investigated for optimizing inference on NVIDIA hardware.

The studied optimization techniques included:

- 🎯 Precision Calibration
- 🔗 Layer & Tensor Fusion
- 🧠 Dynamic Tensor Memory
- 🔄 Multi-Stream Execution
- ⚙️ Kernel Auto-Tuning

### PyTorch → TensorRT

```text
PyTorch Model
      │
      ▼
TensorRT Conversion
      │
      ▼
Optimized Engine
      │
      ▼
Faster / More Efficient Inference
```

---

# 💻 OpenVINO

OpenVINO was also explored as an inference optimization and deployment toolkit, particularly for Intel CPUs and integrated GPUs.

The objective was to investigate more efficient deployment of YOLO-based detection models on systems that may not have a high-end GPU.

---

# 🔬 Experimental Workflow

```text
                    ┌─────────────────┐
                    │  YOLOv8n Model  │
                    └────────┬────────┘
                             │
                             ▼
                   ┌──────────────────┐
                   │ Architecture     │
                   │ Analysis         │
                   └────────┬─────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
          C3TR           Adown           PSA
             │              │              │
             └──────────────┼──────────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │ Training &       │
                   │ Experimentation  │
                   └────────┬─────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │ Multi-Dataset    │
                   │ Evaluation       │
                   └────────┬─────────┘
                            │
                            ▼
              ┌──────────────────────────┐
              │ Accuracy / Complexity    │
              │ / Speed Analysis         │
              └────────────┬─────────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Deployment &        │
                │ Inference           │
                │ Optimization        │
                └─────────────────────┘
```

---

# 🛠️ Technologies

## 🧠 Deep Learning

- Python
- PyTorch
- YOLOv8
- Object Detection

## 🏗️ Model Development

- Neural Network Architecture Modification
- Data Augmentation
- Model Training
- Model Evaluation

## ⚡ Optimization

- NVIDIA TensorRT
- Intel OpenVINO

---

# 📚 Documentation

The complete academic presentation contains:

- YOLO evolution and foundations
- YOLOv8 architecture
- Modified architecture
- Experimental methodology
- Dataset evaluations
- Visual comparisons
- TensorRT optimization
- OpenVINO optimization
- Future work

📄 **[View the Complete Presentation](YOUR_PDF_LINK_HERE)**

---

# 🔮 Future Work

The project identifies several directions for further research.

## ⚡ Speed Efficiency

Investigate architecture blocks that focus more strongly on inference efficiency, including lightweight alternatives such as **Ghost Convolution** and **C2f-CiB**.

## 🧪 Advanced Augmentation

Search for more universal augmentation and hyperparameter configurations that perform consistently across different datasets instead of relying only on default Ultralytics settings.

## 🪶 Lightweight Variants

Investigate effective pruning methods to reduce model size and inference time while preserving detection performance.

---

# 📌 Conclusion

This project demonstrates that improving YOLOv8 is not simply a matter of adding more layers or increasing model complexity.

Different architectural modifications produced different outcomes:

```text
        Accuracy
           ▲
           │
           │       ●
           │     ●
           │   ●
           │ ●
           └──────────────────►
            Computational Cost
```

Some modifications improved accuracy but introduced excessive computational requirements, while others provided more practical improvements.

The final experiments demonstrate measurable improvements across several datasets while maintaining a relatively small parameter count and documenting the associated speed/complexity trade-offs.

---

# 👨‍💻 Authors

 
**HAMDANI MOHAMED AMINE**

🎓 Hassiba Benbouali University of Chlef

🏫 Faculty of Exact Sciences & Computer Science

📚 Department of Computer Science

📅 Academic Year: 2024–2025

---

<p align="center">

  <b>YOLOv8 Accuracy Improvement</b>

  <br>

  Computer Vision • Deep Learning • Object Detection

  <br><br>

  ⭐ If this project was useful or interesting, consider starring the repository.

</p>
