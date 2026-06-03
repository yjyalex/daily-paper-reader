---
title: "Thinkless: LLM Learns When to Think"
title_zh: Thinkless：LLM学会何时思考
authors: "Gongfan Fang, Xinyin Ma, Xinchao Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ariVQf0KZx"
tags: ["query:rl-nlplr"]
score: 9.0
evidence: 使用强化学习让LLM决定何时使用CoT推理
tldr: Thinkless使用强化学习训练LLM自适应选择短或长CoT推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-arivqf0kzx/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 943, \"height\": 531}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-arivqf0kzx/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 281, \"height\": 578}]"
motivation: 对简单问题应用冗长推理造成效率浪费。
method: "使用强化学习训练LLM生成控制token <short>或<think>。"
result: 在保持准确率的同时显著减少推理token数。
conclusion: LLM可以学习为不同查询选择适当推理深度。
---

## Abstract
Reasoning Language Models, capable of extended chain-of-thought reasoning, have demonstrated remarkable performance on tasks requiring complex logical inference. However, applying elaborate reasoning for all queries often results in substantial computational inefficiencies, particularly when many problems admit straightforward solutions. This motivates an open question: Can LLMs learn when to think? To answer this, we propose Thinkless, a learnable framework that empowers an LLM to adaptively select between short-form and long-form reasoning, based on both task complexity and the model's ability. Thinkless is trained under a reinforcement learning paradigm and employs two control tokens, \<short\> for concise responses and \<think\> for detailed reasoning. At the core of our method is a Decoupled Group Relative Policy Optimization (DeGRPO) algorithm, which decomposes the learning objective of hybrid reasoning into two components: (1) a control token loss that governs the selection of the reasoning mode, and (2) a response loss that improves the accuracy of the generated answers. This decoupled formulation enables fine-grained control over the contributions of each objective, stabilizing training and effectively preventing the collapse observed in vanilla GRPO. Empirically, on several benchmarks such as Minerva Algebra, MATH-500, and GSM8K, Thinkless is able to reduce the usage of long-chain thinking by 50% - 90%, significantly improving the efficiency of Reasoning Language Models. The code is available at \url{https://github.com/VainF/Thinkless}

---

## 论文详细总结（自动生成）

# 论文《Thinkless: LLM Learns When to Think》详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有推理语言模型（如DeepSeek-R1）对所有查询都采用冗长的链式思维（CoT）推理，导致大量计算浪费，尤其是对于简单问题。
- **目标**：探索LLM能否自主学习“何时需要思考”与“何时直接回答”，即根据问题复杂度和自身能力自适应选择推理模式（短回答 vs 长链推理），以在保持准确率的同时显著降低计算成本。
- **意义**：这属于混合推理（hybrid reasoning）的新方向，实现了动态、数据驱动的推理深度控制，而非依赖手工规则。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **整体框架**：Thinkless采用两阶段训练：
  1. **蒸馏预热（Distillation Warm-up）**：通过两个专家模型（推理模型π_think和指令跟随模型π_short）为每个查询生成配对的长/短回答数据集，然后通过监督微调（SFT）使目标模型学会根据控制token（<think>或<short>）生成相应风格的回答。
  2. **强化学习（Reinforcement Learning with Decoupled GRPO）**：使用奖励信号训练模型自主选择控制token和生成正确回答。奖励函数设计为：正确短回答得1.0，正确长回答得1.0-γ（γ=0.5），错误回答得-1.0。
- **核心技术：Decoupled GRPO（DeGRPO）**：
  - 将标准GRPO的损失分解为两部分：控制token损失（用于模式选择）和响应token损失（用于提高答案正确性）。
  - 分别独立归一化两个部分的梯度贡献，并引入长度无关的权重α（实验中α=0.001），解决了标准GRPO中长链样本对控制token梯度压制导致的模式崩溃（mode collapse）问题。
  - 公式示意：J_DeGRPO = 平均(α * L_control + (1/T) * Σ L_response) + KL散度惩罚。
- **算法流程**：
  1. 在推理阶段，模型首先生成控制token决定推理模式，然后生成相应内容。
  2. RL训练时，每个查询采样多个响应（G=8），计算相对优势（不使用标准差归一化以避免难度偏差），然后通过DeGRPO更新策略。

## 3. 实验设计
- **基础模型**：DeepSeek-R1-Distill-Qwen-1.5B。
- **数据集**：
  - 蒸馏阶段：使用OpenR1（97K）、OpenThoughts-114K、OpenThoughts-1M等数据集生成配对长/短回答。
  - 强化学习阶段：使用DeepScaleR（约40K标注的数学问题）。
- **评估基准**：AIME 2024（困难）、Minerva Algebra、MATH-500、GSM8K（简单）。
- **对比方法**：
  - 基础模型：DeepSeek-R1-1.5B（纯长链）、Qwen2.5-Math-1.5B-Instruct（纯短回答）、Qwen2.5-1.5B（基础LLM）。
  - 短CoT方法：模型合并（Merging，不同系数）、CoT-Valve（不同α）、L1（控制长度）。
  - 路由方法：Random Router（随机）、基于Qwen-7B的路由器。
- **评估指标**：Pass@1（准确率）、输出token数、长链使用比例（Think%）。

## 4. 资源与算力
- **硬件配置**：单个节点4张H100 GPU。
- **训练细节**：
  - 预热阶段：训练1个epoch，最大上下文长度16K，优化器AdamW（lr=1e-5，weight decay=0.1），使用余弦退火调度。
  - RL阶段：训练600步，上下文长度24K，batch size 128（每查询采样8个响应，共1024样本），温度0.6，学习率1e-6，优化器AdamW（β=(0.9,0.999)）。
- **框架**：预热使用Megatron，RL使用VeRL。具体训练耗时文中未明确给出。

## 5. 实验数量与充分性
- **主要实验**：在4个数学基准上对比了多种基线方法（共约10+种对比设置），包含准确率和token数双重指标。
- **消融实验**：
  - 蒸馏数据集规模对比（97K vs 114K vs 1M）。
  - DeGRPO与标准GRPO的对比（显示模式崩溃问题）。
  - 控制token权重α的影响（α=0.5 vs 0.001）。
  - 学习曲线分析（U型曲线、准确率变化）。
- **案例研究**：展示模型在简单和复杂问题上的决策行为。
- **充分性评价**：实验设计较为全面，覆盖了不同难度等级的数据集，并与多种相关方法进行了公平比较。但未报告误差棒或统计显著性检验，且仅在数学领域验证（未扩展到编程、逻辑等其他领域），存在一定局限性。

## 6. 主要结论与发现
- **核心发现1**：通过DeGRPO训练的模型能将长链推理使用率减少50%–90%（例如在Minerva Algebra上Think比例降至25.88%，在GSM8K上降至13.31%），同时准确率与全时思考模型相当。
- **核心发现2**：标准GRPO会导致控制token更新不平衡，引发模式崩溃（模型倾向于短链），而DeGRPO通过解耦归一化实现了稳定训练，并呈现U形学习曲线（先上升后下降）。
- **核心发现3**：模型能够根据问题难度自适应选择推理深度：简单问题（如算术）倾向于短模式，复杂问题（多步推理）倾向于长模式。

## 7. 优点
- **方法创新**：提出了DeGRPO算法，针对混合推理的不平衡问题给出了有效解决方案，技术简单但实用。
- **效率显著**：在几乎不损失准确率的前提下大幅降低推理成本，具有实际部署价值。
- **实验设计合理**：同时报告准确率和token数，直观展示效率与性能的权衡；对比了多种相关基线（模型合并、长度控制、路由等）。
- **分析深入**：通过训练动态可视化揭示了模式崩溃和U形曲线现象，提供了对强化学习训练行为的洞察。

## 8. 不足与局限
- **实验覆盖有限**：仅验证了数学推理任务（AIME、MATH等），未扩展到编程、科学推理、逻辑等领域，通用性存疑。
- **蒸馏阶段性能下降**：简单的SFT蒸馏导致初始模型相比原推理模型有轻微准确率下降，可能影响RL最终性能。
- **缺乏参数调优**：未对蒸馏阶段进行充分的训练技巧优化（如模型合并、LoRA等），可能不是蒸馏的最佳实践。
- **统计显著性缺失**：未报告误差棒或多次运行的结果，难以判断结果稳定性。
- **模型规模有限**：仅测试1.5B参数模型，未探讨更大规模（如7B、70B）上的表现。
- **奖励设计简单**：仅基于正确性和模式偏好，未考虑更细粒度的反馈（如部分正确、推理质量等）。

（完）
