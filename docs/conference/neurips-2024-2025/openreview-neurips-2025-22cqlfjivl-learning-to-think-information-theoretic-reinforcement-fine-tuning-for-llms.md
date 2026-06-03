---
title: "Learning to Think: Information-Theoretic Reinforcement Fine-Tuning for LLMs"
title_zh: 学会思考：大语言模型的信息论强化微调
authors: "Jingyao Wang, Wenwen Qiang, Zeen Song, Changwen Zheng, Hui Xiong"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=22CqLfjiVl"
tags: ["query:rl-nlplr"]
score: 7.0
evidence: 基于信息论过程奖励的强化学习微调用于推理
tldr: 使用信息论强化学习微调与密集过程奖励优化推理链长度
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-22cqlfjivl/fig-001.webp\", \"caption\": \"\", \"page\": 35, \"index\": 1, \"width\": 632, \"height\": 252}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-22cqlfjivl/fig-002.webp\", \"caption\": \"\", \"page\": 35, \"index\": 2, \"width\": 627, \"height\": 252}]"
motivation: 现有方法忽略推理效率，导致过长推理链浪费token。
method: 提出L2T框架，将查询-响应交互视为分层会话，利用信息增益作为密集过程奖励进行强化学习微调。
result: 模型在保持高推理性能的同时显著减少推理链长度。
conclusion: 信息论过程奖励可有效平衡推理效果与效率。
---

## Abstract
Large language models (LLMs) excel at complex tasks thanks to advances in their reasoning abilities. However, existing methods overlook the trade-off between reasoning effectiveness and efficiency, often encouraging unnecessarily long reasoning chains and wasting tokens. To address this, we propose Learning to Think (L2T), an information-theoretic reinforcement fine-tuning framework for LLMs to make the models achieve optimal reasoning with fewer tokens. Specifically, L2T treats each query-response interaction as a hierarchical session of multiple episodes and proposes a universal dense process reward, i.e., quantifies the episode-wise information gain in parameters, requiring no extra annotations or task-specific evaluators. We propose a method to quickly estimate this reward based on PAC-Bayes bounds and the Fisher information matrix. Theoretical analyses show that it significantly reduces computational complexity with high estimation accuracy. By immediately rewarding each episode's contribution and penalizing excessive updates, L2T optimizes the model via reinforcement learning to maximize the use of each episode and achieve effective updates. Empirical results on various reasoning benchmarks and base models demonstrate the advantage of L2T across different tasks, boosting both reasoning effectiveness and efficiency.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义

*   **研究动机与背景**：大型语言模型（LLMs）通过其强大的推理能力在复杂任务上取得了显著进展。然而，现有方法（如基于结果奖励的强化学习）存在一个被忽视的关键问题：**推理效果与效率之间的权衡**。这些方法倾向于鼓励模型生成过长的推理链（CoT），以微小的精度提升换取大量额外的token消耗，导致资源浪费和推理效率低下。例如，在简单问题上，过长的推理链甚至会降低性能。论文的核心目标是在保证推理效果的同时，提升推理效率，使模型能用最少的token实现最优性能。

### 2. 方法论：Learning to Think (L2T)

*   **核心思想**：提出一个基于信息论的强化学习微调框架，名为 L2T。其核心是设计一种**通用的、密集的过程奖励**，该奖励不依赖于外部标注或特定任务评估器，而是基于模型内部参数的信息增益（Information Gain）来量化每个推理步骤（情节，Episode）的贡献，从而引导模型每步都进行高效的更新。

*   **关键技术细节**：
    1.  **问题重构图 (Episodic RL)**：将每个问答对视为一个由多个连续“情节”组成的会话。每个情节对应推理链中的一个逻辑段（例如，用‘<think>'...’</ think>‘分隔）。这允许对每个中间步骤进行奖励。
    2.  **信息论密集过程奖励 (定义了公式 (3))**：
        *   **拟合信息增益 (Fitting Information Gain)**：衡量当前情节更新后，模型预测正确答案概率的提升。这由`Jr(πθ(·|sk, zk)) - Jr(πθ(·|sk))`近似，其中 Jr 是正确性概率。
        *   **参数压缩惩罚 (Parameter Compression Penalty)**：约束模型在每个情节中吸收的冗余信息，防止过度优化。它被定义为模型参数θ与上下文sk之间互信息（Mutual Information）的变化量，即`I(θk; sk) - I(θk-1; sk-1)`。
    3.  **高效的奖励计算 (定理 4.2)**：
        *   **挑战**：直接计算参数压缩惩罚中的互信息在LLM中不可行。
        *   **解法**：利用PAC-Bayes边界和Fisher信息矩阵，推导出一个可计算的近似形式。通过**低秩近似（SVD）** 降低参数维度，并假设参数服从高斯分布，最终将惩罚项近似为`(θ̃k - θ̃k-1)^T * (Fisher矩阵) * (θ̃k - θ̃k-1)`，大幅降低了计算复杂度，并提供了理论上的误差边界。
    4.  **基于强化学习的优化**：
        *   结合**分组相对策略优化（GRPO）** 来优化LLM（策略）。优化目标（公式(5)）是最大化累积奖励，该奖励由两部分组成：最终的**稀疏结果奖励**（rout）和所有情节的**密集过程奖励**（rprg）之和。

### 3. 实验设计

*   **数据集与Benchmark**：使用了多个数学推理和代码生成基准：
    *   **数学推理**：AIME (2024, 2025), AMC (2023), MATH500, MinervaMATH, Omni-MATH (包含4个难度等级)。
    *   **代码生成**：HumanEval。
*   **对比方法**：
    *   **基线模型**：DeepScaleR-1.5B-Preview, DeepSeek-R1-Distill-Qwen-1.5B, DeepSeek-R1-Distill-Qwen-7B。
    *   **强化学习方法**：
        *   **基于结果奖励**：GRPO (标准方法)
        *   **测试时计算优化**：长度惩罚, ReST-MCTS, MRT (过程奖励模型)。

### 4. 资源与算力

*   论文提到所有实验在 **A100 GPU 集群**上运行，但**未明确说明使用的 GPU 数量或总训练时长**。通常这类训练需要数小时至数天，但具体资源未披露。

### 5. 实验数量与充分性

*   **实验数量**：实验较为充分。
    *   **主要性能对比**：在5个数学基准和2个基模上比较了5种方法，结果全面（表1）。
    *   **效率分析**：通过“准确率- token预算”图和表格，展示了不同方法在不同token预算下的性能变化（图2, 3）。
    *   **消融研究**：分析了框架不同组件（拟合信息增益、压缩惩罚、低秩近似）的贡献（图4）。
    *   **参数敏感性**：探讨了关键超参数α和β的影响（图5）。
    *   **额外实验**：还进行了统计显著性检验（误差条）、在更大模型（7B）和代码生成任务上的验证，以及与搜索方法（MCTS）和重排序的联合研究。
*   **充分性与公平性**：实验设计比较客观，涵盖了多种任务难度、模型规模和优化方法，使用了多个独立基准来验证泛化能力。消融实验清楚地显示了每个组件的价值。

### 6. 主要结论与发现

*   L2T能有效平衡推理效果和效率。与基于结果奖励的方法（如GRPO）相比，在平均准确率上提升约3.7%，同时token效率提升约**2倍**（即所需token减半）。
*   与过程奖励方法（如MRT、ReST-MCTS）相比，L2T在提升约2%准确率的同时，token消耗节省约20%。
*   在多个难度不同的任务上，L2T都能在相同token预算下获得更高的准确率，并且能在更少的token内达到峰值性能，展现了更好的token利用能力。
*   消融实验证实，信息增益和压缩惩罚两个组件都是实现高效推理的关键。

### 7. 优点

*   **方法创新性**：提出了一种全新的、基于信息论的通用的密集过程奖励。该奖励不依赖外部标注，可跨任务应用，自动适应不同难度，有效解决了奖励设计中的“相关性、效率、通用性”三大挑战。
*   **理论支撑扎实**：为奖励的计算提供了严密的数学推导（PAC-Bayes, Fisher信息矩阵），并分析了计算复杂度降低和近似误差有界，理论严谨。
*   **实验全面性**：在多个主流基准、不同规模模型上进行了广泛实验，不仅验证了有效性（Accuracy），还对效率（Token Budget）做了深入分析，结果具有很强的说服力。
*   **实际落地价值高**：模型在更少的计算资源下取得更好的效果，这对于降低LLM部署成本、提高响应速度具有重要的现实意义。

### 8. 不足与局限

*   **实验覆盖范围**：虽然在数学和代码任务上验证有效，但可能未涵盖所有类型的复杂推理任务（如多跳QA、规划等），其通用性有待进一步在更广泛的任务上验证。
*   **模型规模限制**：实验主要集中在1.5B和7B的模型上，未在更大规模（如70B以上）的模型上进行验证。文中的低秩近似比率（r/d）在不同规模模型上的设定可能无法直接迁移，其在大模型上的表现和效果未知。
*   **资源消耗**：虽然训练效率提高了，但计算过程奖励（尤其是参数压缩惩罚项）本身需要额外的计算（SVD和Fisher矩阵计算），这可能在推理阶段带来微小开销。论文分析了复杂度降低，但未详细说明单次奖励计算的时间成本。
*   **依赖的假设**：计算压缩惩罚时，假设低秩参数服从高斯分布，这是一个常见但未必在所有情况下都成立的简化假设，可能影响奖励的精确度。

（完）
