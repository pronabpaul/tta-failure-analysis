# When Does Test-Time Augmentation (TTA) Hurt?

**Author:** Pronab Kumar Paul  


## Overview

This project investigates a simple but important question:

> Under what conditions does test-time augmentation (TTA) reduce prediction accuracy instead of improving robustness?

Although TTA is widely used to improve predictive stability, this work shows that it can systematically *harm* performance in medical imaging models by introducing transformation-induced prediction instability.

The study focuses on chest X-ray pneumonia classification and analyses how different augmentations (rotation, scaling, horizontal flip) affect prediction correctness, calibration, and confidence stability.



## Motivation

Test-time augmentation averages predictions across transformed versions of an image (e.g., rotations, flips, scaling). This approach assumes that transformations preserve meaningful semantic information.

However, in medical imaging, this assumption may fail because many images contain:

- orientation-dependent structures,
- spatially sensitive patterns,
- asymmetric anatomical features,
- transformation-sensitive textures.

As a result, applying TTA can shift predictions across the decision boundary and produce incorrect classifications.

This project studies these failure modes.


## Research Hypotheses

- **H0 (Null Hypothesis):**  
  TTA does not produce statistically meaningful decreases in per-image accuracy.

- **H1 (Alternative Hypothesis):**  
  TTA systematically harms performance for an identifiable subset of images.

- **H2 (Mechanism Hypothesis):**  
  TTA failures are associated with transformation-sensitive image representations and prediction instability under geometric perturbations.


## Methodology

### Dataset
- Chest X-ray Pneumonia Dataset (Kaggle)

### Model
- Moderate-capacity CNN (~450k parameters)
- Batch Normalisation + Global Average Pooling
- Binary classification (NORMAL vs PNEUMONIA)

### Test-Time Augmentations
- Rotation: ±15°
- Scaling: 0.9× and 1.1×
- Horizontal flip

### Evaluation
- Baseline vs TTA accuracy
- Per-image correctness analysis
- Hurt/help categorisation
- Calibration analysis (ECE)
- McNemar’s statistical test
- Per-augmentation sensitivity analysis



## Key Findings

- TTA significantly reduced test accuracy:
  - **Baseline:** 76.6%
  - **TTA:** 65.2%

- Hurt rate greatly exceeded the help rate:
  - **Hurt:** 11.7%
  - **Helped:** 0.3%

- McNemar’s test showed strong statistical asymmetry:
  - `p < 10^-6.`

- TTA worsened calibration:
  - **ECE:** 0.20 → 0.30

- Rotation and scaling were the most harmful augmentations.

- Prediction instability was concentrated near the decision boundary.


## Contributions

This project provides:

- A systematic analysis of TTA failure modes
- Per-augmentation sensitivity evaluation
- Calibration analysis under TTA
- Statistical validation using McNemar’s test
- Comparison of BatchNorm vs non-BatchNorm architectures
- Reproducible experimental pipeline in Google Colab


## Reproducibility

- Fixed random seed (`42`)
- Single notebook implementation
- Automatic dataset download via Kaggle API
- `requirements.txt` included
- Fully reproducible in Google Colab


## Future Directions

Potential extensions include:

- Larger architectures (EfficientNet, Vision Transformers)
- Other medical modalities (fundus, histopathology)
- Adaptive or confidence-aware TTA
- Transformation selection strategies
- Calibration-aware inference pipelines


## License

MIT License
