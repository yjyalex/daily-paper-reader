---
title: "SCOUT: Teaching Pre-trained Language Models to Enhance Reasoning via Flow Chain-of-Thought"
title_zh: SCOUT：通过流式思维链增强预训练语言模型的推理
authors: "Guanghao Li, Wenhao Jiang, Mingfeng Chen, Yan Li, Hao Yu, Shuting Dong, Tao Ren, Ming Tang, Chun Yuan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=eXckZbaYma"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 流式思维链增强推理
tldr: 提出流式思维链，将推理建模为潜在认知状态的渐进轨迹
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有思维链方法依赖中间步骤，可扩展性和泛化性受限。
method: 提出流式思维链，将递归推理建模为潜在认知状态的渐进轨迹，无需显式思维链监督。
result: 该方法在多种推理任务上提升了性能。
conclusion: 流式思维链无需显式步骤即可实现有效推理。
---

## Abstract
Chain-of-Thought (CoT) prompting improves the reasoning performance of large language models (LLMs) by encouraging step-by-step thinking. However, CoT-based methods depend on  intermediate reasoning steps, which limits scalability and generalization. Recent work explores recursive reasoning, where LLMs reuse internal layers across iterations to refine latent representations without explicit CoT supervision. While promising, these approaches often require costly pretraining and lack a principled framework for how reasoning should evolve across iterations.
We address this gap by introducing **Flow Chain-of-Thought (Flow CoT)**, a reasoning paradigm that models recursive inference as a progressive trajectory of latent cognitive states. Flow CoT frames each iteration as a distinct cognitive stage—deepening reasoning across iterations without relying on manual supervision. To realize  this, we propose **SCOUT** (*Stepwise Cognitive Optimization Using Teachers*), a lightweight fine-tuning framework that enables Flow CoT-style reasoning without the need for pretraining. SCOUT uses progressive distillation to align each iteration with a teacher of appropriate capacity, and a cross-attention-based retrospective module that integrates outputs from previous iterations while preserving the model’s original computation flow.
Experiments across eight reasoning benchmarks show that SCOUT consistently improves both accuracy and explanation quality, achieving up to 1.8\% gains under fine-tuning. Qualitative analyses further reveal that SCOUT enables progressively deeper reasoning across iterations—refining both belief formation and explanation granularity. These results not only validate the effectiveness of SCOUT, but also demonstrate the practical viability of Flow CoT as a scalable framework for enhancing reasoning in LLMs.

---

## 论文详细总结（自动生成）

# SCOUT: 通过流式思维链增强预训练语言模型的推理 —— 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **现有挑战**：
  - Chain-of-Thought (CoT) 提示通过生成中间推理步骤提升了 LLM 的推理性能，但依赖人工标注的步骤，限制了可扩展性和泛化性。
  - 近期递归推理方法虽然能在潜在空间迭代细化表征而不需显式 CoT 监督，但往往需要昂贵的预训练，且缺乏一个原则性框架来指导推理如何随迭代演化。
- **本文目标**：
  - 提出 **Flow Chain-of-Thought (Flow CoT)**，一种新的推理范式，将递归推理建模为潜在认知状态的渐进轨迹，每一迭代视为不同的认知阶段，无需手动中间步骤。
  - 为实例化该范式，提出 **SCOUT (Stepwise Cognitive Optimization Using Teachers)**，一个轻量级微调框架，通过渐进蒸馏和跨注意力回顾模块赋予预训练 LLM Flow CoT 能力，无需额外预训练。

## 2. 方法论

### 2.1 核心思想：Flow CoT

- 将模型分解为三部分：**Head 模块**（编码输入）、**递归模块**（迭代细化潜在状态）、**Tail 模块**（解码最终输出）。
- 推理过程：
  - 初始状态：\( z^{(0)} = f_{head}(x) \)
  - 第一次迭代：\( z^{(1)} = f_{\theta}(z^{(0)}) \)
  - 后续迭代（\( t \geq 2 \)）：\( z^{(t)} = f_{\theta}\left( H(z^{(0)}, z^{(t-1)}) \right) \)，其中 \( H \) 是历史集成函数，融合初始上下文和上一步状态。
- 序列 \( \{z^{(1)}, ..., z^{(T)}\} \) 定义为认知轨迹，每个状态应被显式优化，而非视为黑箱重复。

### 2.2 SCOUT 框架关键技术

- **渐进蒸馏**：
  - 每一迭代 \( t \) 的潜在状态 \( z^{(t)} \) 解码后，由能力匹配的教师模型提供软目标分布 \( q^{(t)} \)。
  - 教师模型大小随迭代增加（如迭代 1 用 1.5B，迭代 2 用 3B，迭代 3 用 7B），理由：KL 散度随教师增大而增大（图 3），早期迭代应避免过强监督打乱表征，后期迭代应接受更丰富信号。
  - 每步损失：\( \mathcal{L}^{(t)} = KL(q^{(t)} \| p_{\theta}^{(t)}) + \alpha \cdot \text{CE}(p_{\theta}^{(t)}, y^*) \)，其中 \( p_{\theta}^{(t)} \) 是学生分布，\( y^* \) 是真实标签。
- **回顾推理模块**：
  - 使用 **跨注意力（Cross-Attention）** 将上一步状态 \( z^{(t-1)} \) 作为外部记忆，当前步的查询来自初始状态 \( z^{(0)} \) 的自我注意力结果。
  - 优点：不破坏预训练模型的原有计算流，实现灵活、查询驱动的信息融合，保持推理连贯性。
- **训练与推理**：
  - 训练：总损失 \( \mathcal{L} = \sum_{t=1}^{T} \lambda_t \mathcal{L}^{(t)} \)，端到端微调，无需修改预训练结构。
  - 推理：按 T 步递归更新，最终解码 \( z^{(T)} \) 输出。可设固定迭代数（如 T=3）。

## 3. 实验设计

### 3.1 数据集与基准（Benchmark）

- **训练数据**：混合五个指令微调数据集（Alpaca GPT-4、Alpaca CoT、WikiQA、CodeAlpaca、MathInstruct），提升模型指令跟随和逐步推理能力。
- **评估基准**：8 个多样化任务，覆盖四个领域：
  - 常识问答：ARC-easy、ARC-challenge、OpenBookQA、TruthfulQA
  - 多步推理：GSM8K、MMLU
  - 阅读理解与对话：CoQA、GLUE
  - 代码生成：MBPP

### 3.2 对比方法（Baselines）

- **SFT**：标准监督微调，无递归。
- **DSFT**：从 Qwen2.5-7B 蒸馏，无递归。
- **R-SFT**：递归微调，每步硬标签监督。
- **R-Distill-EQ**：递归 + 固定 7B 教师，等权重。
- **R-Distill-WT**：递归 + 固定 7B 教师，递增权重（\(\lambda_1=0.2, \lambda_2=0.3, \lambda_3=0.5\)）。
- **R-SCOUT**：教师顺序反转（7B→3B→1.5B），作为控制实验。
- 所有递归变体使用相同架构，仅监督方式不同。

## 4. 资源与算力

- **具体配置**（附录 A.1）：
  - GPU：单块 **NVIDIA H20 NVLink (96 GB)**
  - CPU：双路 Intel Xeon Platinum 8457C，20 核
  - RAM：200 GB
  - 训练：2 个 epoch，学习率 \(2 \times 10^{-5}\)，全局 batch size 128（梯度累积），bf16 精度。
- **说明**：训练时长未明确给出，但基于单 GPU 和仅 2 epoch 微调，属于轻量级计算需求。作者未报告具体训练时间。

## 5. 实验数量与充分性

### 5.1 实验数量
- **主要结果**（表 1）：在 8 个任务上比较 6 种方法（含 SCOUT 在不同迭代步 t=1,2,3 的表现），共约 8×6×3=144 个数据点。
- **回顾机制消融**（表 2）：比较 5 种替代模块（Init, Add, CatProj, Gate, ModInj）与本文的 XAttn，在 3 个迭代步上报告平均准确率。
- **结构划分消融**（表 5）：比较两种划分策略（Case 1 vs Case 2）在三种回顾机制下的效果，8 个任务，3 个迭代步。
- **附加实验**：单遍渐进蒸馏（表 4）、数据集级详细结果（附录 B.3 表 6）。
- **定性分析**：两个案例（图 4、图 5）展示概率演变和推理轨迹。

### 5.2 充分性评估
- **优点**：覆盖多个领域任务，消融实验丰富，控制变量（架构相同、仅改变监督或集成模块），对比基线全面。
- **不足**：
  - 仅报告单次运行结果（除图 3 外无误差条或置信区间），统计显著性未说明。
  - 学生模型固定为 Qwen2.5-0.5B，未推广到更大模型。
  - 教师模型均来自同一系列（Qwen2.5），可能引入同源偏差。
- 总体而言，实验设计较严谨，但缺乏重复性统计度量，结论的鲁棒性需进一步验证。

## 6. 主要结论与发现

1. **SCOUT 显著提升推理性能**：在所有 8 个基准上，SCOUT 相比 SFT 平均提升 **1.81%**（T=3 步），且准确率和解释质量均持续改善。
2. **渐进蒸馏优于均匀监督**：固定教师（R-Distill-EQ）或反转顺序（R-SCOUT）均导致后期迭代性能下降，唯有 SCOUT 实现单调递增。
3. **跨注意力回顾模块最稳定**：相比 Add、Gate 等简单融合，XAttn 随深度增加不退化，在多次迭代中保持或提升性能。
4. **推理信念逐步修正**：定性分析（图 4）显示，SCOUT 迭代中 token 概率从错误答案逐渐转向正确答案，体现渐进认知搜索。
5. **解释质量提升**：图 5 实例表明，SCOUT 的推理链在迭代中从错误数学运算过渡到正确分步解释，逻辑更清晰。

## 7. 优点

- **方法创新**：Flow CoT 首次将递归推理形式化为认知状态的渐进轨迹，为递归 LLM 提供了原则性框架。
- **轻量高效**：SCOUT 仅需微调，无需预训练或架构大幅修改，单 GPU 即可运行。
- **渐进蒸馏设计巧妙**：用能力递增的教师模型匹配迭代深度，避免早期过正则和后期欠拟合，理论动机充分（KL 散度分析）。
- **回顾模块兼容性强**：跨注意力模块非侵入式地集成历史信息，保持预训练计算流，比前期融合方法更鲁棒。
- **实验全面**：八任务评估 + 多种消融 + 定性分析，验证了方法论各环节的有效性。
- **可解释性**：通过迭代概率分布变化和推理文本演化，展示了模型的认知细化过程。

## 8. 不足与局限

- **固定迭代步数**：T=3 为人工设定，未探索动态提前停止或自适应步数，可能不适应不同难度任务。
- **教师选择依赖人工**：教师模型大小手动分配，未自动化，且教师来自同一家族（Qwen），可能缺少跨架构多样性。
- **学生模型规模小**：仅测试 0.5B 模型，未验证在更大基础模型（如 7B、14B）上的效果，扩展性未知。
- **统计可靠性不足**：未报告多次运行的标准差或置信区间，仅凭单次结果可能受随机性影响。
- **潜在偏见传播**：教师模型可能继承有害或偏见知识，学生通过蒸馏可能放大这些偏差。
- **计算开销**：推理需 T 次递归，虽然参数少但延迟增加，未与 CoT 方法比较效率（如 token 生成数）。
- **应用限制**：方法依赖可获取的、大小分层的教师模型，在一些资源受限场景可能难以部署。

（完）
