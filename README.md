# Clearer Images, Controlled Detail

> An English public archive of a 2026 CSIG Camera Academic Star Track 1 project on controllable generative image enhancement.

## Project Overview

This project studies how diffusion-based enhancement can improve degraded 4K images while preserving scene structure, text, identity, and natural color.

The final pipeline combines two complementary stages:

1. **FreqOrtho-SR** restores the main pixel structure at quarter resolution.
2. **TADSR** performs a time-aware one-step refinement back to native 4K.

An interpretable utility rule then selects the strongest candidate for each test image.

## Competition Result

| Item | Result |
| --- | --- |
| Competition | 2026 CSIG Camera Academic Star, Track 1 |
| Official preliminary score | **4.0003** |
| Preliminary ranking | **15 / 513** |
| Submitted cases | 100 / 100 4K outputs |
| Hardware | Single NVIDIA A100 40GB |
| Candidate shortlist | case 44, 60, 97, 79, 51 |

The repository reports the official competition result and the associated engineering measurements from the private project archive. The public release does not include the original competition package.

## One-Sentence Method

FreqOrtho-SR protects low-level structure through frequency-aware routing and orthogonal gradient projection; TADSR restores high-resolution detail with a time-aware diffusion refinement step; an explicit utility rule selects the final output.

## Pipeline

```mermaid
flowchart LR
    A[Low-quality 4K input] --> B[Quarter-resolution preparation]
    B --> C[FreqOrtho-SR]
    C --> D[Structure-preserving candidate]
    D --> E[TADSR one-step refinement]
    E --> F[Native 4K candidates]
    F --> G[Metric comparison]
    G --> H[Utility-based selection]
    H --> I[Final submission package]
```

## Core Modules

### FreqOrtho-SR

FreqOrtho-SR separates pixel fidelity from semantic adaptation:

- **FreqMoE** routes degradation features in the frequency domain to pixel-level LoRA experts.
- **Orthogonal Gradient Projection** uses a pixel subspace to reduce interference between structure recovery and semantic adaptation.
- A one-step inference path restores a compact candidate before high-resolution refinement.

### TADSR

TADSR refines the coarse recovery into a natural 4K result:

- a time-aware encoder conditions the diffusion step;
- a student UNet with LoRA learns a compact refinement path;
- teacher-guided distillation constrains high-frequency detail;
- the time step changes for ordinary and difficult 4K scenes.

## Engineering Results

| Measurement | Result |
| --- | ---: |
| Official competition score | **4.0003** |
| DRealSR PSNR | 29.08 |
| DRealSR LPIPS | 0.2799 |
| Complete 4K submission set | 100 / 100 images |
| Candidate replacements | 5 selected cases |
| Inference hardware | 1 × A100 40GB |

The private archive also records a controlled candidate comparison across all 100 cases. The final package replaced only the selected candidates and kept the remaining outputs aligned with the baseline package.

## Challenge Cases

The project focused on difficult scenes where hallucinated detail is especially risky:

| Scene type | Control objective |
| --- | --- |
| Small faces | Preserve identity structure |
| Text | Preserve stroke topology before sharpening edges |
| Dense foliage | Avoid repeated texture and oversharpening |
| Clocks | Preserve circular geometry, ticks, and hands |
| Birds | Recover feather boundaries without inventing patterns |

## Reproduction Scope

This public repository is a documentation and results archive. It does not distribute the private competition package.

Excluded from this public release:

- trained checkpoints and LoRA weights;
- competition test images and official result packages;
- private validation data;
- raw server logs and internal paths;
- original defense slides and private work directories;
- third-party base models.

The reported score is a historical competition result. A full reproduction requires compatible base models, legally shareable data, an NVIDIA GPU environment, and the original competition evaluation protocol.

## Public Repository Structure

```text
.
├── README.md
├── docs/
│   ├── results.md
│   ├── method.md
│   └── reproduction-scope.md
└── LICENSE
```

## Attribution

Maintained by [qiuy-ue](https://github.com/qiuy-ue). The competition data and official evaluation artifacts remain subject to the organizer's rules.
