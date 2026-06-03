---
title: "SCOUT: Teaching Pre-trained Language Models to Enhance Reasoning via Flow Chain-of-Thought"
title_zh: SCOUT：通过Flow思维链增强预训练语言模型的推理能力
authors: "Guanghao Li, Wenhao Jiang, Mingfeng Chen, Yan Li, Hao Yu, Shuting Dong, Tao Ren, Ming Tang, Chun Yuan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=eXckZbaYma"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 提出Flow思维链，无需显式中间步骤增强推理
tldr: 提出Flow思维链，将递归推理建模为潜状态的渐进轨迹
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有思维链方法依赖显式中间步骤，可扩展性和泛化性受限，而递归推理缺乏原则性框架。
method: 提出Flow思维链，将递归推理建模为潜认知状态的渐进轨迹，无需显式CoT监督。
result: 在多个推理基准上提升了性能，且无需昂贵的预训练。
conclusion: Flow思维链是一种有效的推理范式，平衡了可扩展性和泛化性。
---

## Abstract
Chain-of-Thought (CoT) prompting improves the reasoning performance of large language models (LLMs) by encouraging step-by-step thinking. However, CoT-based methods depend on  intermediate reasoning steps, which limits scalability and generalization. Recent work explores recursive reasoning, where LLMs reuse internal layers across iterations to refine latent representations without explicit CoT supervision. While promising, these approaches often require costly pretraining and lack a principled framework for how reasoning should evolve across iterations.
We address this gap by introducing **Flow Chain-of-Thought (Flow CoT)**, a reasoning paradigm that models recursive inference as a progressive trajectory of latent cognitive states. Flow CoT frames each iteration as a distinct cognitive stage—deepening reasoning across iterations without relying on manual supervision. To realize  this, we propose **SCOUT** (*Stepwise Cognitive Optimization Using Teachers*), a lightweight fine-tuning framework that enables Flow CoT-style reasoning without the need for pretraining. SCOUT uses progressive distillation to align each iteration with a teacher of appropriate capacity, and a cross-attention-based retrospective module that integrates outputs from previous iterations while preserving the model’s original computation flow.
Experiments across eight reasoning benchmarks show that SCOUT consistently improves both accuracy and explanation quality, achieving up to 1.8\% gains under fine-tuning. Qualitative analyses further reveal that SCOUT enables progressively deeper reasoning across iterations—refining both belief formation and explanation granularity. These results not only validate the effectiveness of SCOUT, but also demonstrate the practical viability of Flow CoT as a scalable framework for enhancing reasoning in LLMs.

---

## 论文详细总结（自动生成）

# 论文总结：SCOUT: Teaching Pre-trained Language Models to Enhance Reasoning via Flow Chain-of-Thought

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有的思维链（Chain-of-Thought, CoT）提示方法通过鼓励模型生成中间推理步骤来提升大型语言模型（LLM）的推理性能，但这类方法依赖人工标注的逐步推理轨迹，导致可扩展性和泛化性受限。近期工作探索了递归推理（recursive reasoning），通过跨迭代重用内部层来细化潜在表示，无需显式CoT监督，但这些方法通常需要昂贵的预训练，且缺乏一个原则性框架来解释推理应如何随迭代演变。
- **核心问题**：如何在不依赖显式CoT标注和额外预训练的情况下，让LLM获得渐进式、深度感知的递归推理能力？
- **整体含义**：本文提出Flow Chain-of-Thought（Flow CoT）范式，将递归推理建模为潜在认知状态的渐进轨迹，并设计轻量级微调框架SCOUT实现该范式，使预训练LLM通过微调即可获得逐步认知精炼能力。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- **Flow CoT范式**：将递归推理视为一个渐进式的认知轨迹，每一步迭代代表一个独特的认知阶段，无需人工中间监督。模型通过头块（head block）、递归块（recursive block）和尾块（tail block）分解，实现从输入编码、迭代精炼到输出解码的渐进推理。
- **SCOUT框架**（Stepwise Cognitive Optimization Using Teachers）：轻量级微调框架，包含两大核心机制：
  1. **渐进蒸馏（Progressive Distillation）**：为每一步推理分配一个能力匹配的教师模型（早期步骤使用小教师，后期步骤使用大教师），通过KL散度损失对齐学生分布与教师分布，避免早期步骤过度正则化和后期步骤训练不足。
  2. **回顾推理模块（Retrospective Reasoning Module）**：基于交叉注意力（cross-attention）的非侵入式模块，使当前步可选择性地关注上一步的潜在状态，同时保留对原始输入的自注意力，保证推理连贯性和输入一致性。

### 关键技术细节
- **模型分解**：将预训练LLM分解为`f_head`（编码输入）、`f_θ`（递归块，可迭代更新潜在状态）、`f_tail`（解码输出）。初始状态`z^(0)=f_head(x)`，后续迭代：`z^(t)=f_θ(H(z^(0), z^(t-1)))`，其中`H`为历史集成函数（使用交叉注意力实现）。
- **渐进蒸馏**：对于步骤`t`，教师分布`q^(t)=Softmax(f_teacher_t(x))`，学生分布`p_θ^(t)=Softmax(f_tail(z^(t)))`，损失`L^(t)=KL(q^(t)||p_θ^(t)) + α·L_hard^(t)`，总损失`L=∑λ_t·L^(t)`。
- **回顾模块**：仅应用于`t≥2`，通过自注意力处理`z^(0)`，交叉注意力处理`z^(t-1)`，实现查询驱动的信息融合。

## 3. 实验设计：数据集、基准与对比方法

### 数据集与基准
- **训练数据**：混合指令微调数据集，包括Alpaca GPT-4、Alpaca CoT、WikiQA、CodeAlpaca、MathInstruct。
- **评估基准**（8个推理基准）：
  - 常识QA：ARC-easy、ARC-challenge、OpenBookQA、TruthfulQA
  - 多步推理：GSM8K、MMLU
  - 阅读理解与对话：CoQA、GLUE
  - 代码生成：MBPP

### 对比方法
- **SFT**：标准监督微调（仅最终输出损失，无递归）
- **DSFT**：蒸馏SFT（使用Qwen2.5-7B软目标，无递归）
- **R-SFT**：递归硬标签监督（每步使用真实标签）
- **R-Distill-EQ**：递归蒸馏等权重（固定7B教师）
- **R-Distill-WT**：递归蒸馏加权损失（增大后期步骤权重）
- **R-SCOUT**：反转教师顺序（7B→3B→1.5B）
- **SCOUT**：本文方法（1.5B→3B→7B渐进蒸馏+交叉注意力回顾）

## 4. 资源与算力

- **硬件**：单块NVIDIA H20 NVLink GPU（96 GB），双路服务器，20 CPU核心，200 GB RAM。
- **训练配置**：2个epoch，学习率2×10⁻⁵（新参数4×10⁻⁵），余弦学习率调度，预热比10%，bf16精度，全局batch size 128（梯度累积）。
- **教师模型**：Qwen2.5系列（0.5B学生，1.5B/3B/7B教师），学生词汇表与教师略有差异（151936 vs 152064），通过截断和重新归一化处理。

## 5. 实验数量与充分性

- **实验组数**：主实验对比了7种方法×3个迭代步（共21个条件），消融实验包括：
  - 回顾机制对比（6种融合策略×3步）
  - 结构分区消融（Case1 vs Case2 × 3种回顾模块）
  - 渐进蒸馏单次（无递归）基线
  - 定性分析（token概率热力图、推理轨迹可视化）
- **充分性评估**：实验覆盖常识、数学、代码、阅读理解等多领域，基线设计合理（控制变量），消融实验系统。定性分析补充了推理过程的演化证据。但未报告多次运行的标准差或置信区间（仅通过评估框架的固定指标），资源限制下尚可接受。总体较充分、客观、公平。

## 6. 主要结论与发现

- SCOUT在所有基准上持续优于基线，迭代3步时平均提升+1.81%（相对SFT），且随迭代步数单调递增（+0.23→+1.05→+1.81）。
- 渐进蒸馏优于均匀蒸馏：均匀教师（R-Distill-EQ）后期性能停滞或下降，而SCOUT每步改善。
- 教师顺序至关重要：反转顺序（R-SCOUT）初始尚可但后期崩塌，验证了能力匹配的必要性。
- 交叉注意力回顾模块是唯一在所有深度下保持稳定且排名前二的融合策略，而加法融合、门控等方法深度增加后性能骤降。
- 定性分析表明SCOUT能逐步校正错误信念（如“6-3=”从预测1转向3），并细化推理解释（从算术错误到分步正确解答）。

## 7. 优点：方法或实验设计上的亮点

- **方法论创新**：Flow CoT为递归推理提供了认知渐进的新视角，区分了不同迭代步的认知阶段，并设计了渐进蒸馏来对齐监督信号与模型容量。
- **轻量级与实用性**：SCOUT无需预训练，仅需微调，且引入的交叉注意力模块很轻量，兼容现有预训练模型。
- **实验设计严谨**：控制了递归架构、损失权重、教师顺序等变量，多组消融清晰展示了各组件贡献；定性分析增强了可解释性。
- **公平对比**：所有递归变体共享相同架构，差异仅来自监督方式，隔离了影响因素。

## 8. 不足与局限

- **固定迭代步数**：当前固定T=3，不能根据任务复杂度动态调整，可能限制适应性和效率。
- **手动选择教师**：教师模型大小和顺序需手动设定，缺乏自适应选择机制。
- **评估局限性**：仅使用Qwen2.5系列（0.5B学生，1.5B/3B/7B教师），泛化到其他架构族（如LLaMA、Mistral）尚未验证。
- **计算开销**：推理需T次递归前向传播，虽比预训练成本低，但对实时应用仍可能过大。
- **无多次运行统计**：主实验结果未报告标准差或显著性检验，略有不足。
- **应用风险**：渐进蒸馏可能传播教师模型的偏见，且增强推理能力可能被用于生成有害内容或虚假信息。

（完）
