# AgroFusionNet
# Cross-Domain Plant Disease Classification

This project studies **cross-domain plant disease classification** under the domain shift from **PlantVillage** to **PlantDoc**.  
The goal is to evaluate how well different adaptation strategies transfer disease recognition models from clean laboratory images to real-world field images.

## Task Overview

- **Source domain:** PlantVillage  
- **Target domain:** PlantDoc  

PlantVillage contains controlled laboratory images, while PlantDoc contains field images with complex backgrounds, illumination changes, and larger visual variation.  
This creates a challenging domain gap for plant disease classification.

We compare the following methods:

- **Source-Only**
- **Fine-Tuning**
- **FixMatch**
- **DANN + FixMatch**

## Experimental Settings

### Data Splits

For PlantDoc, the following target-domain splits are used:

- `train_labeled`
- `train_unlabeled_weak`
- `train_unlabeled_strong`
- `test`

### Methods

- **Source-Only:** train on PlantVillage only, test directly on PlantDoc.
- **Fine-Tuning:** initialize from the source-trained model and fine-tune on labeled PlantDoc samples.
- **FixMatch:** semi-supervised learning with pseudo-labeling and consistency regularization using labeled and unlabeled target data.
- **DANN + FixMatch:** FixMatch combined with adversarial domain alignment through a gradient reversal layer and a domain discriminator.

### Evaluation Metrics

We report:

- **Accuracy**
- **Macro-F1**

Macro-F1 is important in this task because it better reflects class-balanced performance on the target domain.

---

## Main Results (ResNet-50)

| Method | Accuracy (%) | Macro-F1 |
|---|---:|---:|
| Source-Only | 34.31 | 0.2361 |
| Fine-Tuning | 41.18 | 0.3113 |
| FixMatch (Source Init.) | 41.18 | 0.3076 |
| FixMatch (Fine-Tune Init.) | 40.20 | 0.3103 |
| DANN + FixMatch (Source Init.) | 38.24 | 0.2847 |
| DANN + FixMatch (Fine-Tune Init.) | **41.18** | **0.3149** |

### Observations

- The **Source-Only** baseline performs poorly on PlantDoc, confirming a clear domain gap.
- **Fine-Tuning** gives the most stable improvement over the baseline.
- **FixMatch** provides only limited gains and does not consistently outperform fine-tuning.
- **DANN + FixMatch** is highly dependent on initialization.
- **DANN + FixMatch (Fine-Tune Init.)** achieves the best **Macro-F1**, but the gain over standard fine-tuning is small.

---

## Backbone Comparison: ResNet-18 vs ResNet-50

| Method | ResNet-18 Acc. | ResNet-18 Macro-F1 | ResNet-50 Acc. | ResNet-50 Macro-F1 |
|---|---:|---:|---:|---:|
| Source-Only | 30.39 | 0.1936 | 34.31 | 0.2361 |
| Fine-Tuning | 42.16 | 0.3189 | 41.18 | 0.3113 |
| FixMatch (Source Init.) | 37.25 | 0.2880 | 41.18 | 0.3076 |
| FixMatch (Fine-Tune Init.) | 42.16 | 0.3202 | 40.20 | 0.3103 |
| DANN + FixMatch (Source Init.) | 40.20 | 0.2990 | 38.24 | 0.2847 |
| DANN + FixMatch (Fine-Tune Init.) | 38.24 | 0.2806 | **41.18** | **0.3149** |

### Backbone Takeaways

- ResNet-50 improves the **Source-Only** baseline over ResNet-18.
- However, a stronger backbone does **not consistently improve final adaptation performance**.
- This suggests that the main bottleneck is not backbone capacity alone, but:
  - domain shift,
  - pseudo-label quality,
  - and the trade-off between domain alignment and class discrimination.

---

## Qualitative Results

Additional qualitative analysis was performed using:

- **Confusion Matrices**
- **Grad-CAM visualizations**
- **t-SNE feature embeddings**

### Summary of Qualitative Findings

- The **Source-Only** model shows strong cross-class confusion and poor feature alignment between source and target domains.
- **Fine-Tuning** significantly improves class discrimination and target-domain feature organization.
- **FixMatch** and **DANN + FixMatch** produce only modest visual improvements beyond fine-tuning.
- **DANN + FixMatch** shows stronger source-target feature overlap in t-SNE, but this does not translate into a large performance gain.

---

## Conclusions

This project shows that cross-domain plant disease classification from **PlantVillage** to **PlantDoc** remains challenging due to substantial domain shift.

### Main Conclusions

1. **Target-domain supervision is the most effective source of improvement.**  
   Fine-tuning with labeled PlantDoc data provides the most stable and reliable gain.

2. **More complex adaptation methods do not automatically outperform simple fine-tuning.**  
   FixMatch and DANN + FixMatch offer only limited and initialization-dependent benefits.

3. **A stronger backbone does not solve the adaptation problem by itself.**  
   ResNet-50 improves source-only performance, but does not consistently improve final target-domain results.

Overall, the experiments suggest that **robust target adaptation matters more than backbone strength alone** in this task.

---

## Future Work

Possible next directions include:

- improving pseudo-label reliability,
- designing more stable semi-supervised adaptation strategies,
- reducing the conflict between domain alignment and class discrimination,
- and evaluating on larger and more diverse real-world agricultural datasets.

---

# Reference

If you use this project, please cite the original AgroFusionNet repository:

https://github.com/ekeneshia/AgroFusionNet
