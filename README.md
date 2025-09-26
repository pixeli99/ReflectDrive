<div align="center">
<h1>ReflectDrive</h1>
<h3>Discrete Diffusion for Reflective Vision-Language-Action Models in Autonomous Driving</h3>

[Pengxiang Li](https://scholar.google.com/citations?user=rUp_4RgAAAAJ&hl=en)\*, [Yinan Zheng](https://scholar.google.com/citations?user=mHXjEbQAAAAJ&hl=en&oi=ao)\*, [Yue Wang]()\*, [Huimin Wang]()\*†, [Hang Zhao](https://hangzhaomit.github.io/), [Jingjing Liu](https://air.tsinghua.edu.cn/en/info/1046/1194.htm), [Xianyuan Zhan](https://zhanzxy5.github.io/zhanxianyuan/), Kun Zhan, Xianpeng Lang

¹LiAuto, ²Tsinghua University

[**[Arxiv]**](https://arxiv.org/abs/2509.20109)

Preprint, 2025

</div>

The official implementation of **ReflectDrive**, a novel framework that integrates a **reflection mechanism** for safe trajectory generation via **discrete diffusion**. Our method bridges the gap between imitation learning and safety-critical deployment by enabling gradient-free, iterative self-correction.


<div style="text-align: center;">
  <img src="https://github.com/user-attachments/assets/9634be89-8d40-4de2-a573-c1ffcf4f4d90" width="100%">
</div>

## To Do List

The code is being cleaned and will be released gradually.

- [ ] Release code for the NAVSIM benchmark.
- [ ] Release tutorial on the Reflective Inference mechanism (Safety-Guided Regeneration).
- [ ] Release training code.
- [x] Initial repo & paper.

## Table of Contents

- [Methods](#methods)
- [Performance on NAVSIM](#performance-on-navsim)
- [Qualitative Results](#qualitative-results)
- [Getting Started](#getting-started)
- [Bibtex](#bibtex)
- [Acknowledgement](#acknowledgement)

## Methods

**ReflectDrive** introduces a novel, learning-based approach for safe and reliable autonomous driving planning. Key components include:
*   **Trajectory Discretization**: We first discretize the 2D driving space to construct an action codebook, representing continuous trajectories as sequences of discrete tokens.
*   **Discrete Diffusion Planner**: We leverage a pre-trained Vision-Language-Action (VLA) model, fine-tuned as a discrete diffusion model, to generate trajectories via a non-autoregressive denoising process.
*   **Reflective Inference**: A two-stage, gradient-free inference process ensures safety.
    1.  **Goal-Conditioned Generation**: The model first generates a diverse set of multi-modal trajectory proposals based on high-level goals.
    2.  **Safety-Guided Regeneration**: An external safety oracle identifies unsafe points in the trajectory. We then perform an efficient local search in the discrete token space to find a "safe anchor" and use the diffusion model's powerful inpainting capability to regenerate a coherent and safe trajectory around it.

## Performance on NAVSIM

`ReflectDrive` demonstrates significant advantages in safety-critical trajectory generation, approaching human-level performance on the real-world **NAVSIM** benchmark. Our reflective inference mechanism substantially improves safety metrics like Drivable Area Compliance (DAC) and Time-to-Collision (TTC) without compromising driving progress (EP).

| Method                 | Paradigm            | Input | NC↑     | DAC↑    | TTC↑    | Comf.↑  | EP↑     | PDMS↑   |
| ---------------------- | ------------------- | ----- | ------- | ------- | ------- | ------- | ------- | ------- |
| **_Base Planners_**     |                     |       |         |         |         |         |         |         |
| UniAD                  | -                   | Cam   | 97.8    | 91.9    | 92.9    | 100.0   | 78.8    | 83.4    |
| PARA-Drive             | -                   | Cam   | 97.9    | 92.4    | 93.0    | 99.8    | 79.3    | 84.0    |
| Hydra-MDP              | -                   | C&L   | 98.3    | 96.0    | 94.6    | 100.0   | 78.7    | 86.5    |
| DiffusionDrive         | Diffusion           | C&L   | 98.2    | 96.2    | 94.7    | 100.0   | 82.2    | 88.1    |
| GoalFlow               | Diffusion           | C&L   | 98.4    | 98.3    | 94.6    | 100.0   | 85.0    | 90.3    |
| AutoVLA                | Autoregressive      | Cam   | 98.4    | 95.6    | 98.0    | 99.9    | 81.9    | 89.1    |
| **_Our Method_**       |                     |       |         |         |         |         |         |         |
| ReflectDrive (w/o R.I.)| Discrete Diffusion  | Cam   | 96.9    | 95.4    | 92.2    | 100.0   | 79.0    | 84.8    |
| **ReflectDrive (Ours)**| **Discrete Diffusion** | **Cam** | **97.7**| **99.3**| **93.5**| **100.0**| **86.9**| **91.1**|
| ReflectDrive†          | Discrete Diffusion  | Cam   | **99.7**| **99.5**| **99.1**| 99.9    | **88.9**| **94.7**|
| **Human**              | -                   | -     | 100.0   | 100.0   | 100.0   | 99.9    | 87.5    | 94.8    |

*R.I. denotes Reflective Inference. † denotes using a privileged ground-truth oracle for reflection.*


## Qualitative Results
<div style="display: flex; justify-content: center; align-items: center; gap: 2%;">
  <img src="https://github.com/user-attachments/assets/90ca53a2-14d5-4a66-9546-f4a4b35c44ec" width="49%" alt="demo1">
  <img src="https://github.com/user-attachments/assets/a05e7026-e6da-4387-b1df-0dbc6be1700d" width="49%" alt="demo2">
</div>
Safety-Guided Regeneration (S.G.R.) Visualization. The top row shows scenarios where initial plans risk violating drivable area boundaries. The bottom row shows scenarios with potential collision risks. ReflectDrive iteratively optimizes the initial light-colored trajectory towards a safer, feasible final plan (darker color).

## Getting Started

Coming Soon...

## Bibtex

If you find our work useful for your research, please consider citing our paper:
```bibtex
@article{li2025discrete,
  title={{DISCRETE DIFFUSION FOR REFLECTIVE VISION-LANGUAGE-ACTION MODELS IN AUTONOMOUS DRIVING}},
  author={Li, Pengxiang and Zheng, Yinan and Wang, Yue and Wang, Huimin and Zhao, Hang and Liu, Jingjing and Zhan, Xianyuan and Zhan, Kun and Lang, Xianpeng},
  journal={arXiv preprint arXiv:2509.20109},
  year={2025}
}
```

## Acknowledgement
Our work is inspired by and builds upon several incredible open-source projects. We would like to thank the teams behind: [NAVSIM](https://navsim.github.io/), [LLaDA-V](https://github.com/Zebin-You/LLaDA), and the broader research community working on diffusion models and autonomous driving.
