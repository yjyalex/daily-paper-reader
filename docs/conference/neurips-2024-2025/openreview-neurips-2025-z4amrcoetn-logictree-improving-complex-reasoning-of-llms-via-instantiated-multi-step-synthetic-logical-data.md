---
title: "LogicTree: Improving Complex Reasoning of LLMs via Instantiated Multi-step Synthetic Logical Data"
title_zh: "LogicTree:通过实例化多步合成逻辑数据改进大模型复杂推理"
authors: "Zehao Wang, Lin Yang, Jie Wang, Kehan Wang, Hanzhu Chen, Bin Wang, Jianye HAO, Defu Lian, Bin Li, Enhong Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=z4AMrCOetn"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 通过合成数据提升逻辑推理
tldr: LogicTree生成多步逻辑数据提升大模型推理能力。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-z4amrcoetn/fig-001.webp\", \"caption\": \"\", \"page\": 26, \"index\": 1, \"width\": 1095, \"height\": 746}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-z4amrcoetn/fig-002.webp\", \"caption\": \"\", \"page\": 29, \"index\": 2, \"width\": 720, \"height\": 405}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-z4amrcoetn/fig-003.webp\", \"caption\": \"\", \"page\": 29, \"index\": 3, \"width\": 720, \"height\": 405}]"
motivation: 大模型在复杂多步逻辑推理中表现不佳。
method: 迭代搜索可应用逻辑规则，生成多样化的推理链数据。
result: 合成的数据集有效提升模型逻辑推理性能。
conclusion: 高质量合成数据是提升推理能力的有效途径。
---

## Abstract
Despite their remarkable performance on various tasks, Large Language Models (LLMs) still struggle with logical reasoning, particularly in complex and multi-step reasoning processes. 
Among various efforts to enhance LLMs' reasoning capabilities, synthesizing large-scale, high-quality logical reasoning datasets has emerged as a promising direction. 
However, existing methods often rely on predefined templates for logical reasoning data generation, limiting their adaptability to real-world scenarios. 
To address the limitation, we propose **LogicTree**, a novel framework for efficiently synthesizing multi-step logical reasoning dataset that excels in both complexity and instantiation.
By iteratively searching for applicable logic rules based on structural pattern matching to perform backward deduction, **LogicTree** constructs multi-step logic trees that capture complex reasoning patterns. 
Furthermore, we employ a two-stage LLM-based approach to instantiate various real-world scenarios for each logic tree, generating consistent real-world reasoning processes that carry contextual significance.   This helps LLMs develop generalizable logical reasoning abilities across diverse scenarios rather than merely memorizing templates.
Experiments on multiple benchmarks demonstrate that our approach achieves an average improvement of 9.4\% in accuracy on complex logical reasoning tasks.

---

## 论文详细总结（自动生成）

# 论文总结：LogicTree：通过实例化多步合成逻辑数据改进大模型复杂推理

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大语言模型（LLMs）在复杂多步逻辑推理任务中表现不佳，现有逻辑推理数据合成方法过度依赖预定义模板，导致生成的推理模式简单、缺乏现实场景适应性和上下文语义，难以帮助模型发展可泛化的逻辑推理能力。
- **目标**：提出 LogicTree 框架，生成既具有复杂推理结构又能实例化到真实世界场景的高质量多步逻辑推理数据集，使 LLM 习得可迁移的逻辑推理技能，而非仅仅记忆模板或事实间的隐含关联。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：基于一阶逻辑（FOL）规则，通过**结构化模式匹配**进行**反向演绎**，构建多步逻辑推理树；再使用 **两阶段 LLM 实例化** 将符号树转换为上下文一致的自然语言推理问题和推理过程。
- **关键技术细节**：
  1. **逻辑树生成**（Algorithm 1）：
     - 从随机生成的根公式（结论）开始，迭代选择叶节点，基于**结构化模式匹配**（公式的抽象语法树（AST）的运算符骨架同构）搜索可应用逻辑规则，反向演绎推导出前提作为子节点。
     - 支持命题逻辑和一阶逻辑规则的混合（共约190种规则），生成深度2-15、步数可变的复杂树结构。
  2. **两阶段 LLM 实例化**：
     - **阶段1**：提取所有叶节点（前提），输入 LLM（如GPT-4）为每个原子公式分配真实世界的实体/事件，并保持符号关系一致性，构建连贯的场景。
     - **阶段2**：将树中按深度排序的中间节点序列（符号推理链）输入 LLM，逐步骤翻译为自然语言推理过程，利用 LLM 的语义理解能力而非推理能力，降低错误率。
  3. **后处理**：
     - 验证 LLM 输出中逻辑表达式是否与原始符号一致（字符串比对），过滤约 8.73% 的数据。
     - 构建实例：前提（叶节点自然语言）、结论-答案对（True/False/Uncertain）、推理过程。

## 3. 实验设计
- **数据集**：
  - **训练数据**：生成 5,000 符号逻辑树，每个实例化 3 种不同场景，共 15,000 条，过滤后约 13.8k 条高质量推理实例。
  - **评估基准**：6个主要逻辑推理任务——LogicBench、LogiQA2.0、FOLIO、BIG-Bench Hard（BBH）逻辑子集、AGIEval（LSAT-LR 和 LSAT-AR）、Multi-LogiEval（按推理深度 d1-d5 分类）；外加泛化任务：ProofWriter、MathQA、GPQA、Humaneval、CommonsenseQA、MNLI。
- **对比方法**：
  - Vanilla（原始模型）
  - PARARULE（模板+重复规则）
  - LogicAsker（原子规则评估和样本）
  - FLD×2（多规则随机组合）
- **模型与训练设置**：Llama-3.1 (8B,70B)、Mistral-7B-v0.3、Qwen2.5 (1.5B,3B,7B)、DeepSeek-R1-Distill (7B,8B)；小模型全参数微调，70B 使用 LoRA；学习率 1e-6到2e-5，批次128，3 epoch，BF16，DeepSpeed。
- **评估指标**：准确率（Accuracy），报告标准差。

## 4. 资源与算力
- 论文**未明确说明**具体使用的 GPU 型号、数量及训练总时长。仅提及使用 DeepSpeed 梯度检查点和 BF16 精度以优化内存；训练采用 3 epoch、全局批次 128、最大上下文长度 4096 tokens，但未量化计算成本。
- 在数据合成部分，估计每实例约需 LLM 调用 3 次（实例化+翻译+验证），但未公布实际 API 调用量或 GPU 小时数。

## 5. 实验数量与充分性
- **实验数量**：共进行 ≥8 组主实验（4个骨干网络 × 5种方法）、8组多步推理深度分析（Multi-LogiEval 上 3个模型 × 5深度）、6组泛化任务、多组消融（4变体）、额外模型扩展（DeepSeek、小模型）。总计约 20+ 组实验。
- **充分性与公平性**：
  - 覆盖多种架构（Llama、Mistral、Qwen、DeepSeek）和尺度（1.5B-70B），结果具有统计误差（±标准差）。
  - 对比基线均为现有代表性方法，且在同一设置下重新训练。
  - 消融实验设计合理，分别验证了实例化场景、自然语言推理过程、场景多样性三个组件的贡献。
  - 泛化实验覆盖逻辑、数学、代码、NLI 等多个领域，论证了推理能力的可迁移性。
  - 总体实验设计较全面、客观，结论可信。

## 6. 主要结论与发现
- LogicTree 在 6 个逻辑推理基准上平均提升 9.4%（Llama-3.1-8B），最高提升达 13.9%（BBH-Logic）。
- 在多步推理（Multi-LogiEval）中，尤其在 d3-d5 深度任务上显著优于基线，表明复杂推理树有助于处理深层推理。
- 消融证实：**实例化真实场景**、**自然语言推理过程**、**场景多样性** 三者缺一不可；仅使用单一场景甚至不如不实例化，暗示多样性能防止过拟合特定事实关系。
- 泛化实验显示：在需大量知识记忆的任务（如常识、科学）上提升较小，但在推理密集型任务（逻辑、数学、代码）上增益显著，说明模型学到了真正的推理泛化能力。

## 7. 优点
- **方法创新**：首次将结构化模式匹配反向演绎与 LLM 实例化结合，突破模板限制，生成真正复杂且上下文丰富的推理数据。
- **数据质量高**：后处理过滤确保逻辑一致性，且 LLM 仅做翻译而非推理，降低错误引入。
- **实验全面**：多模型、多基准、消融、泛化一应俱全，结论稳健。
- **可解释性贡献**：消融实验清晰揭示了实例化、推理过程、多样性各自的作用，为后续工作提供指导。
- **实用性强**：合成数据能有效提升多种 LLM（包括先进推理模型如 DeepSeek-R1-Distill）的推理能力。

## 8. 不足与局限
- **无法融合其他推理范式**：当前仅关注形式逻辑推理，未集成常识推理、因果推理等，限制了综合推理能力的提升。
- **知识密集型任务增益有限**：对需要丰富预训练知识的任务（如科学问答）提升较小，表明 LogicTree 主要强化推理技能而非知识记忆。
- **算力成本未量化**：未提供合成数据的具体 GPU 耗时或 API 费用，不利于复现和评估可扩展性。
- **依赖 LLM 质量**：实例化过程依赖 GPT-4 等高端模型，若使用较弱 LLM 可能产生更多错误，尽管后处理可部分过滤。
- **数据规模有限**：仅 13.8k 训练实例，对于更大模型（如 70B+）可能不足，且未探索更大规模合成数据的效果。
- **评估偏重准确性**：未分析推理过程忠实性（faithfulness）或模型输出中是否真正使用了逻辑规则。

（完）
