---
title: "Learning to Think: Information-Theoretic Reinforcement Fine-Tuning for LLMs"
title_zh: 学习思考：面向LLM的信息论强化微调
authors: "Jingyao Wang, Wenwen Qiang, Zeen Song, Changwen Zheng, Hui Xiong"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=22CqLfjiVl"
tags: ["query:rl-nlplr"]
score: 7.0
evidence: 使用强化学习微调及过程奖励进行推理优化
tldr: 提出基于信息论过程奖励的强化学习微调框架，实现高效推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-22cqlfjivl/fig-001.webp\", \"caption\": \"\", \"page\": 35, \"index\": 1, \"width\": 632, \"height\": 252}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-22cqlfjivl/fig-002.webp\", \"caption\": \"\", \"page\": 35, \"index\": 2, \"width\": 627, \"height\": 252}]"
motivation: 现有推理方法忽略了效果与效率的平衡，导致令牌浪费。
method: 提出L2T框架，将交互视为多轮会话，利用信息增益作为密集过程奖励进行强化微调。
result: 在保持推理性能的同时显著减少了令牌使用。
conclusion: 信息论过程奖励能有效引导模型进行简洁且有效的推理。
---

## Abstract
Large language models (LLMs) excel at complex tasks thanks to advances in their reasoning abilities. However, existing methods overlook the trade-off between reasoning effectiveness and efficiency, often encouraging unnecessarily long reasoning chains and wasting tokens. To address this, we propose Learning to Think (L2T), an information-theoretic reinforcement fine-tuning framework for LLMs to make the models achieve optimal reasoning with fewer tokens. Specifically, L2T treats each query-response interaction as a hierarchical session of multiple episodes and proposes a universal dense process reward, i.e., quantifies the episode-wise information gain in parameters, requiring no extra annotations or task-specific evaluators. We propose a method to quickly estimate this reward based on PAC-Bayes bounds and the Fisher information matrix. Theoretical analyses show that it significantly reduces computational complexity with high estimation accuracy. By immediately rewarding each episode's contribution and penalizing excessive updates, L2T optimizes the model via reinforcement learning to maximize the use of each episode and achieve effective updates. Empirical results on various reasoning benchmarks and base models demonstrate the advantage of L2T across different tasks, boosting both reasoning effectiveness and efficiency.

---

## 论文详细总结（自动生成）

# 论文结构化中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大型语言模型（LLM）在复杂推理任务中虽表现优异，但现有方法（如基于结果奖励的强化学习）往往鼓励生成不必要的长链推理，导致令牌（token）大量浪费。论文聚焦于推理**效果（effectiveness）**与**效率（efficiency）**之间的权衡，旨在用更少的令牌实现同等或更优的推理性能。
- **整体含义**：提出一种新颖的信息论强化微调框架（Learning to Think, L2T），通过密集过程奖励自适应控制推理深度，实现推理效果与效率的统一，降低计算成本，提升部署可行性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将每个查询-回答交互建模为多轮会话（session），每一轮（episode）对应推理链中的一个片段。针对每一轮提出**信息论密集过程奖励**，由两部分组成：
  - **拟合信息增益（Fitting Information Gain）**：度量该轮参数更新后模型对正确答案预测概率的提升，近似为 `Jr(πθ(·|sk, zk)) - Jr(πθ(·|sk))`。
  - **参数压缩惩罚（Parameter Compression Penalty）**：基于互信息增量 `I(θk; sk) - I(θk-1; sk-1)`，惩罚冗余信息吸收，促进高效更新。
- **关键技术细节**：
  - 利用**PAC-Bayes界限**和**Fisher信息矩阵**对不可直接计算的互信息项进行高效近似。
  - 通过**低秩近似（SVD）**降低Fisher矩阵计算复杂度，保持理论保证（误差有界）。
  - 将奖励整合进**GRPO（Group Relative Policy Optimization）**框架，实现稳定强化学习优化，目标函数为最大化累积奖励 `rout + α Σ rprg_k`，并满足令牌预算约束。
- **算法流程**：问题重定义（多轮会话）→ 奖励计算（信息论密集过程奖励）→ 模型优化（强化微调）。

## 3. 实验设计
- **使用的数据集/场景**：
  - **数学推理**：AIME 2024-2025、AMC 2023、MATH500、MinervaMATH、Omni-MATH。
  - **代码生成**：HumanEval。
- **基准模型**：DeepScaleR-1.5B-Preview、DeepSeek-R1-Distill-Qwen-1.5B、DeepSeek-R1-Distill-Qwen-7B、Qwen2-7B-Instruct。
- **对比方法**：
  - 基于结果奖励的RL：GRPO。
  - 基于过程奖励的方法：ReST-MCTS、MRT（Meta Reinforcement fine-Tuning）。
  - 长度惩罚方法。
- **评估指标**：Pass@1准确率、令牌预算（效率）。

## 4. 资源与算力
- 文中说明所有实验在**A100 GPU集群**上运行，未明确给出具体GPU数量、训练时长或总计算量。
- 训练配置：学习率1e-6，批大小256，最大生成长度16384 tokens，使用BF16混合精度和vLLM加速。出于资源限制，未测试72B以上规模模型。

## 5. 实验数量与充分性
- **实验数量**：
  - 主实验覆盖5个数学基准 × 2～3种基础模型，共约15＋组对比。
  - 额外包括代码生成任务（HumanEval）及不同模型规模（1.5B、7B）。
  - 针对Omni-MATH按难度分层（Tier 1-4）分析推理深度与性能关系。
  - 消融实验：替换拟合信息增益、移除压缩惩罚、替换低秩近似为随机层采样，共3组配置。
  - 参数敏感性分析（α, β网格搜索）。
  - 与MCTS结合及推理时重排应用补充实验。
- **充分性评价**：实验设计较为全面，涵盖多任务、多模型、多基线，并进行了深入的消融和敏感性分析。但未包含所有主流开源大模型（如Llama-3系列），且对更大规模模型的泛化性需进一步验证。总体上客观、公平，但存在一定选择性偏差（仅用DeepSeek系列）。

## 6. 论文的主要结论与发现
- L2T在几乎所有基准上实现**最优Pass@1准确率**，同时显著降低令牌消耗。
- 与结果奖励RL相比，准确率提升约**3.7%**，令牌效率提升约**2倍**；与过程奖励基线相比，准确率提升约**2%**，令牌节省约**20%**。
- 过程奖励和压缩惩罚两个组件**缺一不可**：移除任一都会导致性能下降或效率降低。
- 低秩Fisher近似在保证精度的前提下大幅降低计算开销。
- L2T能自适应不同难度任务：简单问题用短链，复杂问题适当延长，避免过度推理。

## 7. 优点
- **通用性**：奖励函数完全基于模型内部信号（参数信息增益），无需外部标注或任务特定评估器，可跨任务零成本复用。
- **高效性**：通过密集过程奖励抑制冗余推理，显著提升令牌利用率。
- **理论支撑**：使用PAC-Bayes和Fisher信息矩阵给出可计算近似，并提供误差界证明（Theorem 4.2, D.2, D.3）。
- **实用集成**：基于GRPO实现，训练稳定，易于扩展到现有流水线。

## 8. 不足与局限
- **实验覆盖有限**：未在更大规模模型（如72B、100B+）上验证，也未包括所有主流开源模型族（如Llama-3）。
- **基准范围**：主要涵盖数学和代码推理，未涉及更深层次的多轮对话、工具使用或长文档理解等场景。
- **计算成本**：虽然推理时节省令牌，但训练时仍需额外计算Fisher矩阵近似和奖励评估，实施成本未充分讨论。
- **局限性声明**：论文自身在Limitations部分指出，未测试web设计等新兴基准；受限于开源资源和算力，未探索超大模型。
- **潜在偏差**：超参数α、β虽经网格搜索，但固定值可能对某些任务非最优；过程奖励的近似误差在极端参数更新下可能增大。

（完）
