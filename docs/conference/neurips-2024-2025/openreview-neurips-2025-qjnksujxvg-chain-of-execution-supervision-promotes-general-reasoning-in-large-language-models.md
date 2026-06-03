---
title: Chain of Execution Supervision Promotes General Reasoning in Large Language Models
title_zh: 执行链监督促进大语言模型的通用推理
authors: "Nuo Chen, Zehua Li, Keqin Bao, Junyang Lin, Dayiheng Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QjnKsujXVG"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 执行链监督用于通用推理
tldr: 构建语料库将代码执行转化为逐步思维链推理，提升通用推理能力
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-qjnksujxvg/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qjnksujxvg/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qjnksujxvg/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qjnksujxvg/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 512, \"height\": 512}]"
motivation: 代码推理隐式且与语法噪声纠缠，直接训练效果不佳。
method: 构建TraceMind语料库，将代码执行过程转化为显式的逐步思维链推理（执行链）。
result: 该语料库覆盖数学、分类等领域的推理任务，有效提升模型推理能力。
conclusion: 显式代码执行推理链可促进大语言模型的通用推理能力。
---

## Abstract
Building robust and general reasoning ability is a central goal in the development of large language models (LLMs). Recent efforts increasingly turn to code as a rich training source, given its inherent logical structure and diverse reasoning paradigms—such as divide-and-conquer, topological ordering, and enumeration. However, reasoning in code is often expressed implicitly and entangled with syntactic or implementation noise, making direct training on raw code suboptimal. To address this, we introduce TraceMind, a large-scale corpus of 2.6 million samples that transforms code execution into explicit, step-by-step chain-of-thought style rationales, which we call Chain of Execution (CoE). 
The corpus spans domains including mathematics, classical algorithms and algorithmic competition, and is enriched with variable-tracing questions and code rewritings to enhance logical granularity and code diversity.
 We evaluate Tracepile using three training setups—continue-pretraining, instruction tuning after pretraining, and two-stage finetuning. Experiments across four base models (LLaMA 3, LLaMA 3.1, Qwen-2.5, and Qwen-2.5 Coder) and 20 benchmarks covering math, code, logic, and algorithms demonstrate consistent improvements. Notably, Tracepile boosts LLaMA3-8B by 9.2\% on average across nine math datasets and delivers clear gains on LiveCodeBench, CRUX, and Zebra Logic under two-stage finetuning.

---

## 论文详细总结（自动生成）

# 中文总结：Chain of Execution Supervision Promotes General Reasoning in Large Language Models

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：大语言模型（LLM）的推理能力是迈向通用人工智能的关键，但当前数据驱动的方法主要针对特定领域（如数学、代码）提升性能，缺乏通用的、可迁移的推理能力。
- **背景**：代码天然具有逻辑结构和多种推理模式（如分治、拓扑排序、枚举），但代码中的推理通常是隐式的，与语法细节和实现噪声纠缠，直接训练原始代码效果不佳。
- **目标**：构建一个包含多样化推理模式的语料库，通过显式的、逐步的推理过程监督，提升 LLM 的通用推理能力。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- **Chain of Execution (CoE)**：将代码执行过程中的每一步转化为自然语言的链式思维（Chain-of-Thought）风格解释，使推理过程显式化、结构化、可解释。
- **TracePile 数据集**：包含 260 万样本，涵盖数学、经典算法和算法竞赛三大来源，通过查询多样化与代码多样化策略增强数据多样性。

### 关键技术细节

1. **数据来源**：
   - **算法竞赛代码**：从 Codeforces 收集提交，利用 Qwen-2.5-72B-Instruct 提取干净函数并验证一致性。
   - **经典算法代码**：从 CLRS 基准和 GraphInstruct 获取 30 种经典算法实现，并生成输入输出和中间执行迹。
   - **数学代码**：使用 OpenMath 数据集，并通过模型过滤（LLaMA3-8B 三次正确即丢弃简单问题）提高难度。

2. **多样化增强**：
   - **查询多样化**：对给定代码生成更细粒度的查询，如追踪特定变量在执行过程中的变化，迫使模型理解中间逻辑。
   - **代码多样化**：改写代码结构（如递归转迭代、更改循环顺序），保留语义但增加语法多样性。

3. **CoE 生成**：
   - 使用 Qwen-2.5-72B-Instruct，通过少样本提示生成逐步执行解释。
   - 中间结果验证：对于数学问题，输出 JSON 格式的最终结果；对于算法问题，输出包含中间变量状态的 JSON，并与真实执行迹对比，丢弃不一致样本。
   - 最终将 JSON 转换回自然语言格式以避免下游训练中的格式问题。
   - 对每个样本独立采样 5 次以提高覆盖率；对超过 8k 令牌的长样本进行截断。

4. **训练范式**：
   - **继续预训练（Continue-pretraining）**：在 TracePile 上继续预训练，增强基础模型的逐步推理能力。
   - **预训练后指令微调**：先继续预训练再指令微调，保持指令遵循能力的同时利用推理提升。
   - **两阶段微调**：先在 TracePile 上微调，再在通用指令数据集（Tulu3-SFT）上微调，作为结构化适应路径。

## 3. 实验设计

### 使用的数据与基准
- **模型**：LLaMA 3-8B、LLaMA 3.1-8B、Qwen-2.5-7B、Qwen-2.5 Coder-7B（四个基础模型）。
- **基准数据集（共 20 个）**：
  - **数学推理**：GSM8K、MATH、GSM8K-Hard、SVAMP、ASDIV、MAWPS、MinMath、MMLU-STEM、TABMWP、MATHQA、SAT（共 11 个）。
  - **代码推理**：LiveCodeBench、CRUX。
  - **逻辑推理**：Zebra Logic、RuleTaker、KORBench。
  - **算法推理**：GraphWiz、GraphInstruct、CLRS、BBH 子集（Tracking Shuffled Objects、Multi-Step Arithmetic、Logical Deduction、Web of Lies）。

### 对比方法
- 在表 4 中，对比了 OpenMathInstruct-1（纯代码解决方案）、OpenMathInstruct-2（纯数学思维链）、WebInstruct（通用指令）、CodeI/O、CodeI/O++ 等替代监督策略。

## 4. 资源与算力

- **硬件**：16 × H800-80GB GPU。
- **训练超参数**：batch size 512，最大序列长度 8192 tokens，训练 3 个 epoch，学习率 1e-5。
- **框架**：使用 LLaMA-Factory。
- **备注**：论文未明确报告总训练时长或具体能耗，但给出了上述配置。

## 5. 实验数量与充分性

- **实验数量**：包含多组对比实验：
  - 主实验：继续预训练（表 1）、两阶段微调（表 3）、继续预训练+指令微调（附录表 7），共覆盖 4 个模型 × 20 个基准。
  - 消融实验（表 5）：移除各数据源（数学、算法、竞赛）和多样化策略（查询、代码）的影响。
  - 替代监督策略对比（表 4）：5 种基线方法。
  - 规模效应（图 3）：数据量从 50K 到 4.3M 的扩展。
  - BBH 子集分析（表 6）：4 个推理维度评估。
- **充分性评价**：
  - 实验设计较为全面，涵盖多种训练范式、多个模型族、多个推理领域，对比基准丰富。
  - 消融实验和扩展实验验证了各组件贡献，结论具有说服力。
  - 但未报告多次运行的方差或统计显著性检验，客观性稍弱；模型评估基于标准工具（OpenCompass、Qwen2.5-Math、ZeroEval），保证了一致性。

## 6. 主要结论与发现

- **TracePile 显著提升通用推理能力**：在继续预训练下，LLaMA3-8B 数学平均提升 8.4%（从 54.8% 到 63.4%）；两阶段微调下，LLaMA3-8B 平均提升 6.4%。
- **跨域泛化能力强**：在 LiveCodeBench、CRUX、Zebra Logic 等无直接数据覆盖的数据集上也观察到增益。
- **CoE 格式优于传统 I/O 或解决方案监督**：与 OpenMathInstruct、CodeI/O 等相比，TracePile 在数学和代码任务上均更优，尤其在 OOD 任务上优势明显。
- **多源数据和多样化增强至关重要**：消融实验表明，任一数据源或增强策略的缺失都会导致性能下降。
- **数据规模正向影响性能**：随数据量增加，性能持续提升，但拒绝采样引入偏差后提升有限（TracePile++ 部分领域饱和）。
- **细粒度推理改善**：在 BBH 子集中，状态跟踪（+19.2%）和多步算术（+11.6%）提升显著，说明 CoE 增强了过程化推理能力。

## 7. 优点

- **方法创新**：首次将代码执行过程显式转化为自然语言思维链，提供比最终答案更丰富的监督信号。
- **数据多样性**：融合竞赛、经典算法、数学三类代码，并通过查询和代码改写增强，覆盖多种推理范式。
- **训练范式灵活**：设计了三种训练设置，证明了 CoE 数据作为通用中间训练阶段的潜力。
- **广泛验证**：使用 4 个不同基座模型、20 个基准，跨越数学、代码、逻辑、算法，实验规模大。
- **可迁移性**：OOD 泛化实验表明，CoE 监督可转移到未见的任务格式，提升鲁棒性。

## 8. 不足与局限

- **领域偏重**：数据集主要基于代码推理，对需要常识或世界知识的任务（如法律、规划、因果推理）覆盖不足。
- **生成成本与偏差**：CoE 数据依赖大型指令模型（Qwen-2.5-72B-Instruct）生成，引入计算成本且可能带有提示模型的固有偏差。
- **长截断问题**：8k 令牌截断可能丢弃复杂但信息丰富的长推理迹，限制对长程依赖的学习。
- **可复现性隐忧**：未提供代码或数据开源链接（论文中仅描述方法），且未报告多次实验的统计误差。
- **拒绝采样引入偏差**：在数据扩展实验中，拒绝采样虽然提高了局部一致性，却可能导致分布偏移，限制泛化。

（完）
