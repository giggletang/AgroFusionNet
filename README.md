# AgroFusionNet
# Cross-Domain Plant Disease Classification

This project investigates cross-domain plant disease classification using domain adaptation and semi-supervised learning techniques. The goal is to improve model performance when transferring knowledge from laboratory images to real-world agricultural images.

---

# Datasets

We conduct experiments on two widely used plant disease datasets with significant domain differences.

### PlantVillage (Source Domain)
PlantVillage contains high-quality laboratory images captured under controlled conditions with clean backgrounds and consistent lighting.

### PlantDoc (Target Domain)
PlantDoc contains real-world field images with complex backgrounds, varying illumination, and occlusions, making it significantly more challenging.

### Cross-Domain Setting

| Data Type | Dataset |
|-----------|--------|
| Source labeled data | PlantVillage |
| Target labeled data | PlantDoc/train_labeled |
| Target unlabeled data | PlantDoc/train_unlabeled |
| Target test set | PlantDoc/test |

This setup simulates a realistic scenario where labeled data is abundant in a source domain but limited in the target domain.

---

# Model and Training

All experiments use:

- **Backbone:** ResNet-18 pretrained on ImageNet  
- **Optimizer:** Adam  
- **Learning rate:** 1e-4  
- **Training epochs:** 10  

Training consists of two stages:

1. **Warmup stage**  
   Training on the labeled subset of the target dataset.

2. **Main training stage**  
   Different strategies are applied depending on the experiment:
   - Source-only supervised training
   - Supervised fine-tuning
   - Semi-supervised learning (FixMatch)
   - Hybrid domain adaptation (DANN + FixMatch)

---

# Evaluation Metrics

We report two metrics:

- **Accuracy**
- **Macro-F1 Score**

Macro-F1 is included to better evaluate model performance under potential class imbalance in the PlantDoc dataset.

---

# Experimental Results

The table below summarizes the performance of different training strategies on the **PlantDoc test set**.

| Model | Accuracy | Macro-F1 |
|------|------|------|
| Source-only Baseline | 30.39% | 0.1936 |
| Fine-Tune | **42.16%** | 0.3189 |
| FixMatch (baseline init) | 37.25% | 0.2880 |
| FixMatch (finetune init) | 42.16% | **0.3202** |
| DANN + FixMatch (baseline init) | 40.20% | 0.2990 |
| DANN + FixMatch (finetune init) | 38.24% | 0.2806 |

---

# Key Observations

- A **significant domain gap** exists between PlantVillage and PlantDoc.
- **Fine-tuning with a small labeled target dataset** provides the largest performance improvement.
- **FixMatch** improves performance when starting from a weak baseline model.
- **Domain adversarial training (DANN)** improves cross-domain generalization when initialized from the source model.
- However, combining **DANN with a fine-tuned model** may degrade performance due to interference with target-specific features.

---

# Confusion Matrix Visualization

Below are the confusion matrices of different models on the PlantDoc test set.

- Source-only Baseline
- Fine-Tune
- FixMatch
- DANN + FixMatch

These visualizations highlight the class-level improvements achieved by fine-tuning and domain adaptation methods.

---

# Reference

If you use this project, please cite the original AgroFusionNet repository:

https://github.com/ekeneshia/AgroFusionNet
