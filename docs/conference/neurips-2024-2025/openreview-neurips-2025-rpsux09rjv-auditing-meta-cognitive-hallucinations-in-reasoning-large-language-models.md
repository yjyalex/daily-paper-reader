---
title: Auditing Meta-Cognitive Hallucinations in Reasoning Large Language Models
title_zh: 审计推理大语言模型中的元认知幻觉
authors: "Haolang Lu, Yilian Liu, Jingxin Xu, Guoshun Nan, Yuanlong Yu, Zhican Chen, Kun Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rPsUx09RJV"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 审计思维链轨迹，研究幻觉的产生和演化
tldr: 通过审计思维链轨迹和认知置信度，研究推理大语言模型中幻觉的因果性
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法未能全面洞察幻觉在推理链中如何产生和演化。
method: 通过审计思维链轨迹，评估模型在潜在错误或偏见声明上的认知置信度，分析幻觉因果性。
result: 发现长思维链设置中模型会生成高置信但错误的内容，揭示了幻觉的演化模式。
conclusion: 审计认知置信度有助于理解并减轻推理大语言模型中的幻觉。
---

## Abstract
The development of Reasoning Large Language Models (RLLMs) has significantly improved multi-step reasoning capabilities, but it has also made hallucination problems more frequent and harder to eliminate. While existing approaches address hallucination through external knowledge integration, model parameter analysis, or self-verification mechanisms, they fail to provide a comprehensive insight into how hallucinations **emerge** and **evolve** throughout the reasoning chain. In this work, we investigate hallucination causality under constrained knowledge domains by auditing the Chain-of-Thought (CoT) trajectory and assessing the model's cognitive confidence in potentially erroneous or biased claims.
Analysis reveals that in long-CoT settings, RLLMs may iteratively reinforce biases and errors through flawed reflective processes, ultimately inducing hallucinated reasoning paths.
Counterintuitively, even with interventions at hallucination origins, reasoning chains display pronounced ''chain disloyalty'', resisting correction and sustaining flawed trajectories.
We further point out that existing hallucination detection methods are *less reliable and interpretable than previously assumed*, especially in complex multi-step reasoning contexts.
Unlike circuit tracing that requires access to model parameters, our auditing **enables more interpretable long-chain hallucination attribution in black-box settings**, demonstrating stronger generalizability and practical utility.
Our code is available at [this link](https://github.com/Winnie-Lian/AHa_Meta_Cognitive).

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：推理大语言模型（RLLMs）通过多步推理（如思维链 CoT）显著提升了性能，但也导致幻觉问题更加频繁且难以消除。现有方法（如外部知识集成、模型参数分析、自验证机制）未能全面揭示幻觉在推理链中如何**产生**和**演化**。
- **整体含义**：作者通过审计 CoT 轨迹并评估模型对潜在错误或偏见声明的认知置信度，探究幻觉的因果性，旨在增强对 RLLMs 幻觉机制的理解，并为可解释的幻觉检测提供新方向。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：在受限知识领域下，通过审计完整的 CoT 推理轨迹，并评估模型在每一步对自身输出声明的**认知置信度**（cognitive confidence），从而识别幻觉的起源和演化路径。
- **关键技术细节**：
  - 审计 CoT 轨迹：记录模型在长链推理中每一步的推理语句。
  - 认知置信度评估：对每个推理步骤中的主张（claim）进行置信度打分，区分高置信错误与低置信正确等不同情况。
  - 幻觉归因：分析高置信错误声明如何通过迭代强化（iterative reinforcement）导致幻觉推理路径。
  - 探讨“链不忠诚”（chain disloyalty）现象：即使在幻觉起源点进行干预，推理链依然抵抗纠正，维持错误轨迹。
- **与现有方法的区别**：该方法不依赖模型参数访问（如电路追踪），而是通过输出层审计实现**黑盒场景下的可解释长链幻觉归因**，具有更强的泛化性和实用性。

## 3. 实验设计

- **数据集/场景**：摘要未明确列举具体数据集名称，但提到“在受限知识领域下”进行实验，推测使用了需要多步推理的问答或推理类数据集（如数学、常识推理等）。代码已开源（GitHub），可进一步查看。
- **基准（Benchmark）**：未明确说明。但实验对比了现有幻觉检测方法，指出这些方法在复杂多步推理中“不如先前假设的可靠和可解释”。
- **对比方法**：未列出具体方法名称，但提及对比了外部知识集成、模型参数分析、自验证机制等现有方法。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及 GPU 型号、数量、训练时长等算力信息。作者仅提供代码仓库，未在文本中描述实验硬件配置。

## 5. 实验数量与充分性

- **实验数量**：摘要未列出具体实验组数。根据描述，至少包括：① 不同模型/设置下的 CoT 审计实验；② 幻觉干扰实验（干预起源点观察“链不忠诚”）；③ 与传统幻觉检测方法的对比实验。
- **充分性评估**：实验设计了关键现象验证（迭代强化、链不忠诚、高置信错误），并对比了现有方法，但缺乏多数据集、多模型规模、消融实验等详细数据。从抽象层面看覆盖了核心论点，但公开信息不足，需结合代码和完整论文评估充分性。

## 6. 主要结论与发现

- 在长 CoT 设置下，RLLMs 会通过**有缺陷的反思过程逐步强化偏见和错误**，最终导致幻觉推理路径。
- 即使对幻觉起源进行干预，推理链也表现出明显的“**链不忠诚**”现象，抵抗纠正，维持错误轨迹。
- **现有幻觉检测方法**在复杂多步推理中的**可靠性和可解释性低于此前假设**。
- 审计认知置信度有助于理解并**减轻** RLLMs 中的幻觉（结论部分）。

## 7. 优点

- **方法创新**：从认知置信度角度审计 CoT，提供对幻觉产生和演化的因果性分析，而不仅是检测。
- **黑盒友好**：不依赖模型参数，仅通过输出层审计即可实现，**实用性强**，适用于封闭模型或 API 调用场景。
- **可解释性强**：通过显式揭示推理链中高置信错误和迭代强化路径，比传统方法更直观。
- **公开代码**：提供开源实现，便于复现和扩展。

## 8. 不足与局限

- **实验覆盖不明确**：未列出具体数据集、模型规模、对比方法详细信息，难以判断结论的泛化性。
- **算力未说明**：缺乏实验资源细节，影响可复现性评估。
- **领域限制**：研究在“受限知识领域”下进行，是否能推广到开放域、更复杂的推理任务尚需验证。
- **干预有效性不足**：“链不忠诚”现象表明仅干预起源点难以纠正，但未提出有效的缓解策略，实际应用价值受限。
- **认知置信度评估方法未详细描述**：如何定义和计算“认知置信度”是核心，但摘要中未给出具体操作细节，可能影响理解。

（完）
