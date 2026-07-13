---
tags:
  - ai-agent
  - research
  - multi-agent
  - embodied-intelligence
  - LLM
created: 2026-06-15
---

# AI Agent 调研报告

> 综合 GitHub 仓库 + 学术论文的全面调研

---

## 一、什么是 AI Agent？

AI Agent（人工智能智能体）是能够**自主感知环境、做出决策、采取行动**以实现目标的系统。在 LLM 时代，AI Agent 特指以**大语言模型为核心推理引擎**，具备**工具使用、记忆、规划、多步执行**能力的自主系统。

**核心公式：**
```
AI Agent = LLM (大脑) + Tools (手脚) + Memory (记忆) + Planning (计划) + Environment (环境)
```

---

## 二、AI Agent 发展脉络

```
1990s — BDI Agent (信念-愿望-意图模型), SOAR, ACT-R
  ↓
2010s — DeepRL Agent (DQN, AlphaGo, OpenAI Five)
  ↓
2023  — AutoGPT, BabyAGI (LLM Agent 爆发元年)
  ↓       LangChain Agent, AgentGPT, MetaGPT
2024  — CrewAI, AutoGen, Claude Code (多Agent协作)
  ↓       MCP (Model Context Protocol) 标准化
2025  — A2A (Agent-to-Agent), Agent Teams, VLA Agent
  ↓       具身Agent, World Model Agent, Agentic Engineering
2026  — Agentic AI 全面铺开
```

---

## 三、AI Agent 架构分类

### 3.1 按智能体数量

| 类型 | 代表 | 特点 |
|------|------|------|
| **单 Agent** | AutoGPT, Claude Code | 一个模型独立完成循环 |
| **多 Agent (MAS)** | AutoGen, CrewAI, MetaGPT | 多角色协作分工 |

### 3.2 按架构范式

| 范式 | 代表 | 核心机制 |
|------|------|---------|
| **符号/经典** | BDI, MDP, SOAR | 规则推理，状态机 |
| **神经/生成式** | LangChain Agent, Claude Agent | LLM 编排，提示驱动 |

### 3.3 按记忆类型

| 类型 | 特征 | 实现 |
|------|------|------|
| **无长期记忆** | 仅单次对话上下文 | 基础 Agent |
| **工具媒介记忆** | 通过 RAG/向量数据库存储 | LangChain + Pinecone |
| **原生长期记忆** | 模型自身记忆机制 | Claude Memory, Mem0 |

### 3.4 按自主等级 (L0-L5)

| 等级 | 名称 | 描述 |
|------|------|------|
| L0 | 手动 | 完全人类操作 |
| L1 | 辅助 | AI 给建议，人执行 |
| L2 | 部分自主 | AI 执行子任务 |
| L3 | 条件自主 | AI 独立完成，人审核 |
| L4 | 高度自主 | AI 端到端执行，人监督 |
| L5 | 完全自主 | AI 自主生成+执行+迭代 |

---

## 四、核心组件

```
┌──────────────────────────────────────────┐
│              AI Agent 架构                │
├────────────┬────────────┬────────────────┤
│  Perception│  Reasoning │   Action       │
│  (感知)    │  (推理)     │   (行动)       │
├────────────┼────────────┼────────────────┤
│ • Vision   │ • CoT      │ • Tool Calling │
│ • Audio    │ • ToT      │ • Code Exec    │
│ • Text     │ • ReAct    │ • API Call     │
│ • Sensors  │ • Reflection│ • Robot Ctrl  │
├────────────┴────────────┴────────────────┤
│         Memory (记忆)                     │
│  Short-term (上下文) + Long-term (向量库)  │
├──────────────────────────────────────────┤
│         Planning (规划)                   │
│  Task Decomposition + Scheduling + Replan │
└──────────────────────────────────────────┘
```

### 关键机制

| 机制 | 含义 | 代表工作 |
|------|------|---------|
| **CoT** (Chain-of-Thought) | 逐步推理 | GPT-4 原生 |
| **ReAct** | 推理+行动交替 | LangChain Agent |
| **Reflection** | 自我反思修正 | Reflexion |
| **ToT** (Tree-of-Thought) | 树状搜索推理 | Tree of Thoughts |
| **Tool Use** | 调用外部工具/API | Function Calling |
| **RAG** | 检索增强生成 | LangChain RAG |

---

## 五、主流框架与工具 (GitHub)

### 开发框架

| ⭐ | 仓库 | 简介 |
|---|------|------|
| ~100k | [[LangChain]](https://github.com/langchain-ai/langchain) | Agent 开发框架鼻祖 |
| ~50k | [[AutoGPT]](https://github.com/Significant-Gravitas/AutoGPT) | 自主 Agent 先驱 |
| ~45k | [[MetaGPT]](https://github.com/geekan/MetaGPT) | 多 Agent 协作，软件公司模拟 |
| ~40k | [[AutoGen]](https://github.com/microsoft/autogen) | 微软多 Agent 对话框架 |
| ~25k | [[CrewAI]](https://github.com/crewAIInc/crewAI) | 角色化多 Agent 编排 |
| ~15k | [[Dify]](https://github.com/langgenius/dify) | 可视化 Agent 构建平台 |
| ~10k | [[Semantic Kernel]](https://github.com/microsoft/semantic-kernel) | 微软 Agent 编排 SDK |
| — | [[Agno]](https://github.com/agno-agi/agno) | 轻量高性能 Agent 框架 |

### 协议与基础设施

| 协议 | 提出方 | 功能 |
|------|--------|------|
| **MCP** (Model Context Protocol) | Anthropic | AI Agent 与外部工具/数据标准化连接 |
| **A2A** (Agent-to-Agent) | Google | Agent 间标准化通信协议 |
| **ACP** (Agent Communication Protocol) | 社区 | 跨框架 Agent 协作 |

### 具身 Agent 专项

| ⭐ | 仓库 | 简介 |
|---|------|------|
| — | [[openpi]](https://github.com/Physical-Intelligence/openpi) | π₀ VLA → 具身 Agent 物理执行 |
| — | [[openvla]](https://github.com/openvla/openvla) | 开源 VLA 具身 Agent 基座 |
| — | [[GR00T]](https://github.com/NVIDIA/GR00T) | NVIDIA 人形机器人 Agent |

---

## 六、Agent 与具身智能的交叉

### 融合架构

```
        ┌──────────────────────────────┐
        │     AI Agent (数字世界)       │
        │  LLM → 推理/规划/工具调用     │
        └──────────┬───────────────────┘
                   │  MCP / A2A / API
        ┌──────────▼───────────────────┐
        │    VLA/WAM (物理世界桥梁)     │
        │  视觉+指令 → 物理动作映射    │
        └──────────┬───────────────────┘
                   │  电机控制 / 力控
        ┌──────────▼───────────────────┐
        │     Robot (物理执行)          │
        │  机械臂 / 人形 / 移动底盘     │
        └──────────────────────────────┘
```

### 关键融合点

| 层次 | Agent 角色 | VLA/WAM 角色 |
|------|-----------|-------------|
| **任务理解** | LLM 解析用户意图，任务分解 | 接收结构化指令 |
| **规划** | 多步任务规划 (CoT/ReAct) | 动作序列生成 |
| **执行** | 工具调用、异常处理 | 实时物理控制 (50Hz+) |
| **反馈** | 任务级反思与重规划 | 力控/视觉反馈闭环 |
| **学习** | 经验积累、知识更新 | RL 精调、持续学习 |

### 代表项目

| 项目 | 融合方式 |
|------|---------|
| **GR00T N1** | VLA 双系统：VLM (慢思考) + DiT (快执行) |
| **Helix** | 人形 Agent：S2(7B 理解) + S1(200Hz 控制) |
| **π₀.₅** | 流匹配 VLA = Agent 大脑 + 灵巧手 |
| **Dream-VLA** | 离散扩散 LLM = 推理 + 物理动作 |

---

## 七、关键趋势 (2025-2026)

| 趋势 | 说明 |
|------|------|
| **Agentic Engineering** | Vibe Coding 进阶版：多 Agent 协作开发 |
| **MCP 标准化** | 工具调用协议统一，生态爆发 |
| **Agent Teams** | 多 Agent 角色分工 (Claude Code 原生支持) |
| **World Model 融合** | Agent 拥有物理世界内部模型 |
| **Continual RL Agent** | LoRA+RL 持续学习，无遗忘 |
| **Edge Agent** | 端侧部署 (SmolVLA 450M 可 CPU 推理) |
| **Vibe Coding Agent** | 自然语言驱动开发全流程 |
| **Humanoid Agent** | 人形机器人 + Agent 大脑 |

---

## 八、经典论文速查

| 论文 | 年份 | 贡献 |
|------|------|------|
| [[ReAct]](https://arxiv.org/abs/2210.03629) | 2022 | 推理+行动交替范式 |
| [[AutoGPT]](https://github.com/Significant-Gravitas/AutoGPT) | 2023 | 自主 Agent 先驱 |
| [[MetaGPT]](https://arxiv.org/abs/2308.00352) | 2023 | 多 Agent 软件公司 |
| [[RT-2]](https://arxiv.org/abs/2307.15818) | 2023 | VLA 诞生 |
| [[OpenVLA]](https://arxiv.org/abs/2406.09246) | 2024 | 首个开源 VLA 基座 |
| [[π₀]](https://www.physicalintelligence.company/blog/pi0) | 2024 | 流匹配 VLA |
| [[GR00T N1]](https://developer.nvidia.com/gr00t) | 2025 | 开源人形 VLA |
| [[VLA-R1]](https://arxiv.org/abs/2510.01623) | 2025 | RLVR+GRPO 推理增强 |
| [[RLinf-VLA]](https://arxiv.org/abs/2510.06710) | 2025 | VLA+RL 统一框架 |

---

## 九、与 Vibe Coding 的关系

Vibe Coding 本质上是一种**人-Agent 协作模式**：

```
Vibe Coding = Human (意图) + Coding Agent (执行) + 自然语言接口
Agentic Engineering = Vibe Coding + 多 Agent 协作 + 工程规范
```

关键工具：Claude Code 的 Sub-agents、Skills、Hooks、MCP Servers 等，构成了从单 Agent 到多 Agent 协作的完整技术栈。

---

## 相关笔记

- [[VLA具身智能调研]]
- [[科研/vibe coding]]
- [[Claude Code]]
