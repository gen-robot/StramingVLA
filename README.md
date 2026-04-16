# StreamingVLA

<a href="https://arxiv.org/abs/2603.28565">
  <img alt="arxiv" src="https://img.shields.io/badge/arXiv-%3C2603.28565%3E-%23a72f20.svg">
</a>
<a href="https://ghahahahag.github.io/StreamingVLA_Website/">
    <img alt="Project Page" src="https://img.shields.io/badge/Project_Page-blue?style=flat&logo=googlechrome&logoColor=white">
</a>

Official project page and resources for:

**StreamingVLA: Streaming Vision-Language-Action Model with Action Flow Matching and Adaptive Early Observation**

## Overview

Vision-Language-Action (VLA) models are powerful for language-conditioned robot control, but real-world deployment is often limited by high latency and frequent execution halts.

StreamingVLA addresses this issue by enabling asynchronous, streaming-style coordination across observation, action generation, and action execution. Instead of waiting for each stage to fully finish before the next stage begins, StreamingVLA overlaps critical stages to reduce idle time while maintaining task success.

<p align="center">
  <img src="assets/final_tea_01.png" alt="Overall Framework" width="900">
</p>

## Key Ideas

### 1) Action Flow Matching (AFM)

Traditional VLA pipelines generate action chunks first and execute later, which creates unavoidable waiting gaps.

StreamingVLA replaces chunk-wise denoising with a state-based action flow matching formulation:
- The model maintains an internal action-space state.
- It predicts a velocity field to evolve that state over time.
- Each action can be produced and executed immediately, while the next action is being generated.

This enables effective overlap between action generation and execution.

### 2) Adaptive Early Observation (AEO)

After reducing generation-execution gaps, observation-execution serialization becomes the next bottleneck.

StreamingVLA introduces adaptive early observation:
- A Transformer predictor estimates action saliency (how much pending actions will change future observations).
- If predicted change is small, the next observation starts early.
- If predicted change is large, observation will not start early to maintain accuracy.

This adaptively overlaps observation and execution while controlling performance risk.

## Main Results

### LIBERO Simulation

- Success rate: **94.9%** (close to baseline **95.1%**)
- Single-action latency: **49.9 ms -> 31.6 ms**
- End-to-end latency speedup: up to **2.4x**
- Execution halting reduction: **230.8 ms -> 36.0 ms** (about **6.5x**)

<p align="center">
  <img src="assets/Result.png" alt="Simulation Results" width="820">
</p>

### Real-World Robot (Franka Panda)

- Task: grasp-and-place on a desktop setup
- Average action latency: **271.49 ms -> 170.88 ms**
- Real-world speedup: about **1.58x**

## Why StreamingVLA Matters

StreamingVLA shows that improving embodied AI efficiency is not only about model compression. System-level scheduling and stage parallelism can provide major gains in fluency and responsiveness without sacrificing capability.

The streaming design principle can also inspire other multi-stage, multi-modal real-time interactive systems.




## Citation

```bibtex
@misc{shi2026streamingvlastreamingvisionlanguageactionmodel,
  title={StreamingVLA: Streaming Vision-Language-Action Model with Action Flow Matching and Adaptive Early Observation},
  author={Yiran Shi and Dongqi Guo and Tianchen Zhao and Feng Gao and Liangzhi Shi and Chao Yu and ZhiJian Mo and Qihua Xiao and XiaoShuai Peng and Qingmin Liao and Yu Wang},
  year={2026},
  eprint={2603.28565},
  archivePrefix={arXiv},
  primaryClass={cs.RO},
  url={https://arxiv.org/abs/2603.28565}
}
```
