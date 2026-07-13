---
tags:
  - embodied-intelligence
  - VLA
  - robotics
  - research
  - ai
created: 2026-06-15
---

# VLA (Vision-Language-Action) 具身智能调研

> 调研范围：VLA 模型全景、RL 结合、Benchmark、VLA vs WAM

---

## 一、20个主流VLA模型及发展脉络

### 起源

"VLA"由 Google DeepMind 在 2023年7月发布 **RT-2** 时正式提出。核心：将互联网级 VLM 知识迁移到机器人控制，实现 **图像 + 指令 → 动作** 的端到端映射。

### 2023年：奠基之年

| # | 模型 | 机构 | 特点 | 开源 |
|---|------|------|------|------|
| 1 | **Gato** | DeepMind (2022) | 多任务通才 Transformer，单一模型玩 Atari、聊天、控制机械臂 | ✅ |
| 2 | **RT-1** | Google (2022) | 机器人 Transformer 先驱，ImageNet→机器人视觉迁移 | ✅ |
| 3 | **PaLM-E** | Google (2023.3) | 562B 具身多模态 LLM，PaLM + ViT，输出高层文本计划 | ❌ |
| 4 | **RT-2** | Google DeepMind (2023.7) | 🔥 **VLA 奠基之作**，12B/55B，256-bin 离散动作 token，涌现语义推理 | ❌ |
| 5 | **RoboFlamingo** | 字节跳动 (2023.11) | OpenFlamingo VLM→VLA 迁移，高效微调 | ❌ |
| 6 | **VoxPoser** | Columbia + Google (2023) | LLM 生成 3D 操作程序，无需端到端训练 | ✅ |

### 2024年：爆发元年

| # | 模型 | 机构 | 特点 | 开源 |
|---|------|------|------|------|
| 7 | **Octo** | UC Berkeley (2024.5) | 93M 参数，Transformer + 扩散动作头，800k 轨迹训练 | ✅ MIT |
| 8 | **OpenVLA** | Stanford/TRI/UW (2024.6) | 🔥 **首个开源 VLA 基座**，7B (Llama-2 + DINOv2 + SigLIP)，970k OXE 轨迹 | ✅ MIT |
| 9 | **RDT-1B** | 清华 (2024.10) | 1.2B 扩散 Transformer，统一动作空间支持多双臂 | ✅ |
| 10 | **π₀ (pi0)** | Physical Intelligence (2024.10) | 🔥 **首个流匹配 VLA**，PaliGemma 3B + 300M 动作专家，50Hz | ✅ openpi |
| 11 | **CogACT** | 清华+微软亚研 (2024.11) | 认知动作 VL 模型，轻量级 | ✅ |
| 12 | **Diffusion Policy** | Columbia (2024) | 动作扩散策略，Visuomotor 基准方法 | ✅ |

### 2025年：百花齐放

| # | 模型 | 机构 | 特点 | 开源 |
|---|------|------|------|------|
| 13 | **π₀-FAST** | Physical Intelligence (2025.1) | FAST 动作分词器，训练效率 5x 提升 | ✅ |
| 14 | **Helix** | Figure AI (2025.2) | 🔥 **首个双系统人形 VLA**，S2(7B 7-9Hz) + S1(80M 200Hz) | ❌ |
| 15 | **GR00T N1** | NVIDIA (2025.3) | 双系统，Eagle-2 VLM + DiT，2.2B，63.9ms 推理 | ✅ |
| 16 | **GO-1** | 智元机器人 (2025.3) | 国内首个开源 VLA 之一 | ✅ 部分 |
| 17 | **π₀.₅** | Physical Intelligence (2025.4) | π₀ 升级，知识绝缘训练，10-15min 长时操控 | ✅ |
| 18 | **SmolVLA** | Hugging Face (2025.6) | 🔥 450M 轻量 VLA，SmolVLM-2 + 流匹配，单 GPU 训练 | ✅ |
| 19 | **Dream-VLA** | 港大/华为 (2025) | 离散扩散 LLM，长时规划 | ✅ |
| 20 | **LLaDA-VLA** | (2025) | 掩码扩散 VLA，层级轨迹预测，性能超 OpenVLA | ✅ |

### 三大技术范式

| | ① 自回归离散 | ② 扩散模型 | ③ 流匹配 |
|---|---|---|---|
| **代表** | RT-2, OpenVLA, π₀-FAST | Octo, RDT-1B, Dream-VLA | π₀/π₀.₅, GR00T N1, SmolVLA |
| **优点** | 推理简单，VLM 复用方便 | 并行生成，抗长程误差 | 训练简单(单MSE)，连续动作自然 |
| **缺点** | 误差累积，高频受限 | 采样步骤多，推理延迟大 | — |
| **SOTA** | — | — | π₀.₆ (灵巧+长时+泛化) |

### 发展脉络图

```
2022: Gato, RT-1 ──── 多任务先驱
       │
2023: PaLM-E → ★RT-2★(VLA诞生) → RoboFlamingo, VoxPoser
       │
2024: Octo → ★OpenVLA★(首个开源基座) → ★π₀★(流匹配开创) → RDT-1B
       │                                              (Physical Intelligence)
2025: π₀-FAST → ★Helix★(双系统人形) → ★GR00T N1★(NVIDIA开源)
       → π₀.₅ → ★SmolVLA★(450M轻量) → 各大模型持续迭代
```

### 关键 GitHub 仓库

| ⭐ | 仓库 | 简介 |
|---|------|------|
| 12,338 | [[Physical-Intelligence/openpi]](https://github.com/Physical-Intelligence/openpi) | π₀ 系列官方实现 (π₀/π₀-FAST/π₀.₅) |
| 6,423 | [[openvla/openvla]](https://github.com/openvla/openvla) | 首个开源 VLA 基座，7B 参数 |
| 1,729 | [[google-research/robotics_transformer]](https://github.com/google-research/robotics_transformer) | Google RT-1 官方实现 |
| 1,669 | [[octo-models/octo]](https://github.com/octo-models/octo) | Octo 开源通用策略 |
| 291 | [[sou350121/VLA-Handbook]](https://github.com/sou350121/VLA-Handbook) | VLA 中文学习/面试手册 |
| — | [[yueen-ma/Awesome-VLA]](https://github.com/yueen-ma/Awesome-VLA) | VLA 综述论文 + 精选列表 |

---

## 二、VLA 与 RL 的结合

### 当前趋势：SFT → RL Post-Training 成为标准范式

纯粹的模仿学习（SFT）存在分布偏移问题。**RL 精调正在成为 VLA 训练的标准流程**，标志性转折点：2025 年 NeurIPS 接收多篇 VLA+RL 论文。

### 结合方式分类

#### 1. RLVR + GRPO：推理增强

**代表：[[VLA-R1]](https://arxiv.org/abs/2510.01623)**
- 用 **Reinforcement Learning from Verifiable Rewards (RLVR)** + **Group Relative Policy Optimization (GRPO)**
- 引入 VLA-CoT-13K 思维链数据集
- 系统性优化 VLA 的推理和执行能力

#### 2. PPO 在线精调：泛化增强

**代表：NeurIPS 2025 "What Can RL Bring to VLA Generalization?"**
- 对比 PPO / DPO / GRPO 三种 RL 方法
- **结论：PPO > DPO/GRPO**，显著增强语义理解和执行鲁棒性
- SFT+RL 混合链路效果最优

#### 3. World Model + RL：仿真精调

**代表：[[VLA-RFT]](https://huggingface.co/papers/2510.00406)**
- 用数据驱动 World Model 作为 RL 仿真器
- 预测未来视觉观测 → 高效 RL 精调
- **<400 步精调**即可获得强抗扰动能力

#### 4. Continual RL：持续学习

**代表：[[Simple Recipe Works]](https://arxiv.org/abs/2603.11653)**
- **核心发现**：LoRA + 顺序精调天然抗灾难遗忘
- 大预训练模型 + 参数高效微调 + On-policy RL (GRPO) 的协同效应
- 简单的 Sequential FT + LoRA 竟优于 EWC/Expert Replay 等复杂方法

#### 5. 统一框架

**代表：[[RLinf-VLA]](https://arxiv.org/abs/2510.06710)**
- 支持多架构 (OpenVLA/OpenVLA-OFT)、多 RL 算法 (PPO/GRPO)、多仿真器 (ManiSkill/LIBERO)
- 1.61x-1.88x 训练加速
- 130 个 LIBERO 任务达 98.11%，真机部署优于 SFT

### 关键 RL 算法对比

| 算法 | 优势 | 适用场景 |
|------|------|---------|
| **PPO** | 泛化能力最强，鲁棒性最优 | VLA 后训练主流 |
| **GRPO** | 推理增强，群体相对优化 | Chain-of-Thought VLA |
| **DPO** | 无需奖励模型，离线学习 | 偏好对齐场景 |
| **RLVR** | 可验证奖励，自动标注 | 机器人任务天然适配 |

---

## 三、主流 Benchmark

### 仿真环境类

| Benchmark | 特点 | 任务数 | 适用 VLA |
|-----------|------|--------|---------|
| **CALVIN** | 长时规划基准，连续操作 | 34 任务，5 序列 | OpenVLA, RT-1 等 |
| **LIBERO** | 最新大规模基准 (2024) | 130 任务，4 场景 | RLinf-VLA, Octo 等 |
| **RLBench** | 经典基准，数百任务 | 100+ 任务 | PerAct, RVT 等 |
| **ManiSkill** | SAPIEN 物理引擎 | 25 任务 | RLinf-VLA 等 |
| **MetaWorld** | 元学习基准 | 50 任务 | RL 方法 |

### 评估协议类

| 协议 | 特点 | 代表工作 |
|------|------|---------|
| **SIMPLER / SimplerEnv** | VLA 评估标准协议，桥接 sim-to-real | OpenVLA, Octo 都用 |
| **DROID** | 大规模真实机器人数据集 | π₀-FAST-DROID |
| **OXE (Open X-Embodiment)** | 最大开源机器人数据集 | OpenVLA (970k 轨迹) |

### 关键指标

| 指标 | 含义 | 代表值 (2025 SOTA) |
|------|------|---------------------|
| **Success Rate** | 任务成功率 | 98.11% (RLinf-VLA @ LIBERO) |
| **Generalization** | 跨场景泛化 | 97.66% (RLinf-VLA @ ManiSkill) |
| **Inference Hz** | 推理频率 | 50Hz (π₀), 200Hz (Helix S1) |
| **Latency** | 推理延迟 | 63.9ms (GR00T N1) |
| **Model Size** | 模型参数 | 450M (SmolVLA) ~ 7B (OpenVLA) |

### GitHub 资源

- [[CALVIN]](https://github.com/mees/calvin) — 官方长时规划基准
- [[LIBERO]](https://github.com/Lifelong-Robot-Learning/LIBERO) — 最新大规模基准
- [[SimplerEnv]](https://github.com/simpler-env/SimplerEnv) — VLA 评估标准环境
- [[DROID]](https://github.com/droid-dataset/droid_policy_learning) — 大规模真实数据

---

## 四、VLA 与 WAM 的区别

### 核心定义

| 维度 | **VLA (Vision-Language-Action)** | **WAM (World Action Model)** |
|------|------|------|
| **本质** | 端到端"条件反射"策略 | 先预测物理后果，再决策 |
| **问题** | "我现在该做什么？" | "我这样做后会怎样？" |
| **类比** | 模仿人类遥控操作 | 在内部模型里预演动作后果 |
| **核心能力** | 语言理解 + 动作映射 | 物理理解 + 因果推理 |

### 技术架构差异

```
VLA:   [Image + Text] → VLM Backbone → Action Head → [Robot Action]
WAM:   [State + Action候选] → World Model(预测未来) → [未来State] → 选择最优Action
```

WAM 分两类：
- **级联式**：世界模型先生成未来状态 → 动作模型解码动作（如 DreamZero）
- **联合式**：单一系统同时预测未来和动作（如 Cosmos-Policy）

### 关键对比

| | **VLA** | **WAM** |
|---|---|---|
| **泛化能力** | 有限（光照/物体变化敏感） | 显著更强（零样本 +33% vs SOTA VLA） |
| **长程任务** | 2-3 动作 | 10+ 原子动作 |
| **推理频率** | ~50Hz | ~7Hz（延迟高） |
| **精细操控** | ⭐ 毫米级力控 | 较弱 |
| **数据依赖** | 昂贵遥操作数据 | 🔥 互联网人类视频即可 |
| **算力门槛** | 相对低 | 极高（8×H100×25天） |
| **幻觉风险** | 低（直接映射） | 高（预测错误→危险动作） |

### 业界阵营

| 路线 | 代表 |
|------|------|
| **纯 VLA** | Physical Intelligence π₀.₅, Figure Helix, 小鹏 VLA |
| **纯 WAM** | 英伟达 DreamZero/Cosmos-Policy, 银河通用 LDA-1B, 生数 Motubrain |
| **融合路线** | 理想 MindVLA-o1 (隐世界模型), 智平方 GOVLA 1.0 (快慢系统) |

### 结论：不是替代，是融合

> VLA 负责任务理解（语义入口、精细控制），WAM 负责物理预判（因果推理、泛化场景）。未来具身智能在二者交汇处生长。

---

## 相关笔记

- [[ai agent]]
- [[科研/vibe coding]]
