---
title: "Buffer of Thoughts: Thought-Augmented Reasoning with Large Language Models"
title_zh: 思维缓冲区：与大语言模型的思维增强推理
authors: "Ling Yang, Zhaochen Yu, Tianjun Zhang, Shiyi Cao, Minkai Xu, Wentao Zhang, Joseph E. Gonzalez, Bin CUI"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=ANO1i9JPtb"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 使用元缓冲区的思维增强推理方法
tldr: 提出思维缓冲区方法，通过存储和检索思维模板增强LLM推理。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-ano1i9jptb/fig-001.webp\", \"caption\": \"\", \"page\": 9, \"index\": 1, \"width\": 1462, \"height\": 1104}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-ano1i9jptb/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1462, \"height\": 1104}]"
motivation: 现有推理方法缺乏对高层面推理知识的复用。
method: 构建元缓冲区存储从问题解决过程中提取的思维模板，并在新问题上检索适配。
result: 在10个推理任务上取得显著性能提升。
conclusion: 通过复用思维模板可以有效提升推理的准确性和效率。
---

## Abstract
We introduce Buffer of Thoughts (BoT), a novel and versatile thought-augmented reasoning approach for enhancing accuracy, efficiency and robustness of large language models (LLMs). Specifically, we propose meta-buffer to store a series of informative high-level thoughts, namely thought-template, distilled from the problem-solving processes across various tasks. Then for each problem, we retrieve a relevant thought-template and adaptively instantiate it with specific reasoning structures to conduct efficient reasoning. To guarantee the scalability and stability, we further propose buffer-manager to dynamically update the meta-buffer, thus enhancing the capacity of meta-buffer as more tasks are solved. We conduct extensive experiments on 10 challenging reasoning-intensive tasks, and achieve significant performance improvements over previous SOTA methods: 11\% on Game of 24, 20\% on Geometric Shapes and 51\% on Checkmate-in-One. Further analysis demonstrate the superior generalization ability and model robustness of our BoT, while requiring only 12\% of the cost of multi-query prompting methods (e.g., tree/graph of thoughts) on average. Code is available at: https://github.com/YangLing0818/buffer-of-thought-llm

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有大语言模型（LLM）推理方法分为单查询推理（如 Chain-of-Thought）和多查询推理（如 Tree-of-Thought、Graph-of-Thought）。前者需针对特定任务手动设计提示，缺乏通用性和泛化能力；后者递归扩展推理路径，计算成本高且未能利用历史高级指导。两类方法均忽略了从已解决问题中提取通用高层思维（high-level thoughts）以提升后续推理准确率与效率的可能性。
- **整体含义**：本文提出 **Buffer of Thoughts (BoT)**，一种新颖、通用的思维增强推理框架，通过构建可复用的高级思维模板（thought-template）库，实现跨任务的准确、高效、鲁棒推理，显著提升了 LLM 在多种复杂推理任务上的表现。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：模拟人类从以往问题解决中归纳高层次指导，并在新问题中自适应实例化的过程。构建一个轻量级库（meta-buffer），存储从不同问题解决过程中蒸馏出的高级思维模板，供后续任务检索和实例化。
- **关键组件与技术细节**：
  - **Problem Distiller（问题蒸馏器）**：利用元提示（meta prompt）从输入任务中提取关键信息、约束和目标，并将具体问题转化为高层概念表示，以利于后续模板检索。
  - **Meta Buffer（元缓冲区）**：存储一系列分类的高级思维模板（thought-template），分为六类：文本理解、创意语言生成、常识推理、数学推理、代码编程、应用调度。每个模板包括描述和类别标签。
  - **Template Retrieval（模板检索）**：基于蒸馏后的问题表示与各模板描述的嵌入相似度，检索最匹配的模板，并设定阈值δ（0.5～0.7）判断是否为新任务。若为新任务，则使用三种预定义的通用粗粒度模板。
  - **Instantiated Reasoning（实例化推理）**：将检索到的模板结合蒸馏信息，通过实例化提示（instantiation prompt）自适应生成具体推理结构，从而完成推理。
  - **Buffer Manager（缓冲区管理器）**：负责模板蒸馏和动态更新。
    - **模板蒸馏**：三步流程—核心任务总结、解决步骤描述、通用回答模板。通过设计任务内示例和跨任务示例提升泛化能力。
    - **动态更新**：根据新模板与已有模板描述的相似度阈值决定是否存入元缓冲区；低于阈值则更新，否则跳过，避免冗余并保持轻量化。

## 3. 实验设计：数据集、Benchmark 与对比方法
- **数据集与场景**（共10个推理密集型任务）：
  - Game of 24（数学游戏）
  - BIG-Bench Hard（BBH）：Geometric Shapes、Multi-Step Arithmetic Two、Word Sorting
  - BIG-Bench：Checkmate-in-One、Penguins、Date Understanding
  - Python Programming Puzzles (P3)
  - Multilingual Grade School Math (MGSM，10种语言平均)
  - Shakespearean Sonnet Writing（严格韵律和词汇嵌入）
- **对比方法**：
  - 标准提示（Standard）
  - 单查询方法：GPT-4, GPT-4+CoT, Expert Prompting, PAL
  - 多查询方法：ToT, GoT, Meta Prompting
- **评估指标**：准确率、推理时间对数、成功率（10次重复采样1000个示例的平均准确率）

## 4. 资源与算力
- **文中提及**：实验基于 NVIDIA A100-PCIE-40GB GPU，使用 GPT-4 作为主要基础模型，同时也使用了 Llama3-8B 和 Llama3-70B。但**未明确说明**模型训练时长、GPU 数量、具体推理成本等详细信息。仅提及 BoT 平均仅需多查询方法 12% 的成本，但未给出绝对数值。

## 5. 实验数量与充分性
- **实验数量丰富**：
  - 主表对比（10个任务 × 9种方法）
  - 推理时间对比（4个任务的对数时间图）
  - 鲁棒性成功率分析（4个任务×10次重复）
  - 模板分布分析（6个任务各100样本）
  - 时间成本分布分析
  - 模型大小-性能权衡（Llama3-8B vs 70B，3个任务）
  - 自动模板 vs 手动模板质量对比（MATH-500）
  - 消融实验：
    - Buffer Manager 影响（4轮、4个任务×50样本）
    - Problem Distiller 影响（4个任务×2个基础模型）
    - Meta Buffer 影响（4个任务×2个基础模型）
- **充分性与客观性**：实验覆盖了数学、逻辑、语言生成、编程等多种推理类型，对比了主流的单查询和多查询方法，并进行了重复采样和消融验证，结果具有统计意义。实验设计较为系统、公平。

## 6. 主要结论与发现
- **准确率显著提升**：在 10 个任务上均超过所有对比方法，尤其在复杂推理任务上提升明显（Game of 24 提升 8.4% 对比 ToT，Checkmate-in-One 提升 51% 对比 Meta Prompting）。
- **效率优势**：BoT 仅需多查询方法平均 12% 的成本（推理时间）。
- **鲁棒性强**：10 次重复平均成功率比第二名高约 10%。
- **小模型潜力**：Llama3-8B + BoT 可超越单独的 Llama3-70B，缩小了模型规模差异。
- **缓冲区管理器持续改进**：随着解决任务增多，准确率和效率持续上升；缺少缓冲区管理器则无此趋势。
- **自动模板高质量**：自动生成的模板在 MATH 数据集上优于手动模板（73.4% vs 52.8%），且经过积累后进一步提升（78.4%）。

## 7. 优点（方法或实验设计亮点）
- **创新性**：提出“思维缓冲区”概念，将高级思维模板作为一种可复用的推理知识单元，区别于传统相似案例检索。
- **通用性强**：仅需 6 类模板即可覆盖多种推理任务，且自动蒸馏适应新任务。
- **高效低耗**：单次实例化推理即可完成复杂任务，大幅降低多查询方法的递归成本。
- **动态扩展**：缓冲区管理器增量更新，避免重新训练，实现了知识积累。
- **消融实验全面**：分别验证了问题蒸馏器、元缓冲区、缓冲区管理器、模板质量、模型规模等关键组件的作用。

## 8. 不足与局限
- **实验资源细节缺失**：未提供 GPU 数量、训练/推理总时长等算力信息，不利于复现和成本评估。
- **模板分类可能不全面**：仅 6 类模板可能无法覆盖所有领域（如生物医学、法律推理等），通用性有待验证。
- **依赖检索阈值设定**：阈值 δ（0.5～0.7）凭经验给出，不同任务可能需要微调，缺乏自动确定阈值的方法。
- **未讨论失败情况**：未分析哪些任务类型或条件下 BoT 效果不佳或模板检索失败的情况。
- **应用限制**：方法基于 LLM 生成和蒸馏，对基础模型能力敏感；可能不适用于需要严格逻辑证明或实时性极高的场景。
- **模型选择局限**：主要实验基于 GPT-4，仅部分消融使用 Llama3，未在其他架构（如 Mixtral、Claude）上验证泛化性。

（完）
