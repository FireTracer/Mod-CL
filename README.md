# Mod-CL: Modulation Consistency-based Contrastive Learning for Self-Supervised Automatic Modulation Classification

This repository provides the official implementation of the paper:

**Modulation Consistency-based Contrastive Learning for Self-Supervised Automatic Modulation Classification**

📄 Paper: [arXiv:2605.11875](https://arxiv.org/abs/2605.11875)

## Overview

Mod-CL is a self-supervised contrastive learning framework for automatic modulation classification (AMC).

The key idea is to exploit **intra-instance modulation consistency**: different temporal segments of the same signal instance may have different local waveform patterns, but they share the same modulation type.

Based on this observation, Mod-CL constructs positive pairs from different temporal segments of the same IQ signal and learns modulation-discriminative representations without requiring labels during pretraining.

## Framework

During self-supervised pretraining, each IQ signal is:

1. Augmented into two stochastic views;
2. Split into temporal segments;
3. Encoded by a shared encoder;
4. Optimized using a modulation-consistent contrastive loss.

The total loss consists of three parts:

- Segment Consistency Loss
- Augmentation Consistency Loss
- Joint Consistency Loss

After pretraining, the encoder is frozen and evaluated using linear probing for downstream modulation classification.

## Datasets

Experiments are conducted on public RadioML datasets:

- RadioML 2016.10A
- RadioML 2016.10B

Please place the datasets under the `data/` directory.

```text
data/
├── RML2016.10a/
└── RML2016.10b/
```

## Main Results

We evaluate Mod-CL on two public automatic modulation classification benchmarks, **RadioML 2016.10A** and **RadioML 2016.10B**.

Following the standard linear probing protocol, the encoder is first pretrained without labels and then frozen. Only a linear classifier is trained using limited labeled samples. Here, `N` denotes the number of labeled samples per modulation class per SNR level.

### Linear Probing Results

Mod-CL consistently outperforms generic self-supervised methods and AMC-oriented self-supervised baselines under all label budgets.

#### RadioML 2016.10A

| Method | N=2 | N=5 | N=10 | N=20 | N=50 | N=100 |
|---|---:|---:|---:|---:|---:|---:|
| Random Init. | 13.59 | 16.08 | 16.95 | 18.23 | 20.13 | 21.52 |
| SimCLR | 45.52 | 47.95 | 49.07 | 50.37 | 51.68 | 52.86 |
| MoCo | 32.91 | 38.60 | 41.30 | 42.83 | 44.32 | 45.56 |
| SemiAMC | 36.32 | 43.82 | 47.51 | 49.91 | 53.90 | 56.03 |
| MAC | 27.92 | 35.40 | 41.65 | 47.76 | 54.53 | 58.13 |
| **Mod-CL** | **51.76** | **57.33** | **59.10** | **60.32** | **61.40** | **61.88** |

#### RadioML 2016.10B

| Method | N=2 | N=5 | N=10 | N=20 | N=50 | N=100 |
|---|---:|---:|---:|---:|---:|---:|
| Random Init. | 13.37 | 15.88 | 17.18 | 18.26 | 20.05 | 21.51 |
| SimCLR | 47.03 | 49.16 | 50.66 | 51.89 | 53.23 | 54.34 |
| MoCo | 38.07 | 42.25 | 43.44 | 44.13 | 45.44 | 46.55 |
| SemiAMC | 31.61 | 39.99 | 44.14 | 48.24 | 52.62 | 55.52 |
| MAC | 20.63 | 26.92 | 32.73 | 38.82 | 51.00 | 57.51 |
| **Mod-CL** | **53.22** | **58.09** | **60.36** | **61.80** | **62.99** | **63.65** |

These results show that Mod-CL is particularly effective in low-label regimes. For example, on RadioML 2016.10A, Mod-CL achieves **57.33%** accuracy with only `N=5` labeled samples, outperforming SimCLR by **9.38 percentage points**. On RadioML 2016.10B, Mod-CL achieves **58.09%** accuracy under the same label budget.

### Per-SNR Performance

We further evaluate the performance under different SNR levels with `N=5` labeled samples per modulation class per SNR.

The per-SNR results show that Mod-CL achieves the best overall trend on both datasets and maintains clear advantages in most SNR regions, especially under low-to-medium SNR conditions.

![Per-SNR linear probing accuracy](compare.png)

**Figure:** Per-SNR test accuracy under the linear probing protocol with `N=5` labeled samples per class per SNR on RadioML 2016.10A and RadioML 2016.10B.

### Summary

The main observations are:

- Mod-CL achieves the best linear probing accuracy across all label budgets on both RadioML 2016.10A and RadioML 2016.10B.
- The advantage is most significant when labeled data are scarce.
- Mod-CL performs consistently well across different SNR levels.
- The gains are especially clear in low-to-medium SNR regions, indicating that the learned representations are more robust and modulation-discriminative.



## Citation

If you find this repository useful, please cite our paper:

```bibtex
@article{wang2026modcl,
  title={Modulation Consistency-based Contrastive Learning for Self-Supervised Automatic Modulation Classification},
  author={Wang, Chenxu and Wang, Shuang and Han, Lirong and Hu, Xinyu and Mo, Hanlin and Xing, Hantong and Tegos, Sotiris A. and Jiao, Licheng},
  journal={arXiv preprint arXiv:2605.11875},
  year={2026}
}
