---
title: "A Implies B: Circuit Analysis in LLMs for Propositional Logical Reasoning"
title_zh: A蕴含B：LLM中命题逻辑推理的电路分析
authors: "Guan Zhe Hong, Nishanth Dikkala, Enming Luo, Cyrus Rashtchian, Xin Wang, Rina Panigrahy"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=M0U8wUow8c"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 通过电路分析研究LLM推理能力
tldr: 电路分析揭示了LLM如何进行命题逻辑推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 1694, \"height\": 1946}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 2048, \"height\": 1887}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2048, \"height\": 1271}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-004.webp\", \"caption\": \"\", \"page\": 8, \"index\": 4, \"width\": 815, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-005.webp\", \"caption\": \"\", \"page\": 8, \"index\": 5, \"width\": 794, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 815, \"height\": 449}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 777, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 963, \"height\": 945}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-009.webp\", \"caption\": \"\", \"page\": 30, \"index\": 9, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-010.webp\", \"caption\": \"\", \"page\": 30, \"index\": 10, \"width\": 2048, \"height\": 1697}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-011.webp\", \"caption\": \"\", \"page\": 30, \"index\": 11, \"width\": 2048, \"height\": 1676}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-012.webp\", \"caption\": \"\", \"page\": 31, \"index\": 12, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-013.webp\", \"caption\": \"\", \"page\": 32, \"index\": 13, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-014.webp\", \"caption\": \"\", \"page\": 32, \"index\": 14, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-015.webp\", \"caption\": \"\", \"page\": 34, \"index\": 15, \"width\": 953, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-016.webp\", \"caption\": \"\", \"page\": 34, \"index\": 16, \"width\": 2010, \"height\": 1331}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-017.webp\", \"caption\": \"\", \"page\": 34, \"index\": 17, \"width\": 2010, \"height\": 1331}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-018.webp\", \"caption\": \"\", \"page\": 34, \"index\": 18, \"width\": 957, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-019.webp\", \"caption\": \"\", \"page\": 35, \"index\": 19, \"width\": 2048, \"height\": 1926}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-020.webp\", \"caption\": \"\", \"page\": 35, \"index\": 20, \"width\": 962, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-021.webp\", \"caption\": \"\", \"page\": 35, \"index\": 21, \"width\": 962, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-022.webp\", \"caption\": \"\", \"page\": 36, \"index\": 22, \"width\": 2048, \"height\": 1976}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-023.webp\", \"caption\": \"\", \"page\": 36, \"index\": 23, \"width\": 953, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-024.webp\", \"caption\": \"\", \"page\": 36, \"index\": 24, \"width\": 953, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-025.webp\", \"caption\": \"\", \"page\": 37, \"index\": 25, \"width\": 1243, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-026.webp\", \"caption\": \"\", \"page\": 37, \"index\": 26, \"width\": 1243, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-027.webp\", \"caption\": \"\", \"page\": 37, \"index\": 27, \"width\": 883, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-028.webp\", \"caption\": \"\", \"page\": 38, \"index\": 28, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-029.webp\", \"caption\": \"\", \"page\": 38, \"index\": 29, \"width\": 1676, \"height\": 1069}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-030.webp\", \"caption\": \"\", \"page\": 39, \"index\": 30, \"width\": 2415, \"height\": 1515}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-031.webp\", \"caption\": \"\", \"page\": 39, \"index\": 31, \"width\": 2415, \"height\": 1515}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-032.webp\", \"caption\": \"\", \"page\": 42, \"index\": 32, \"width\": 1047, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-033.webp\", \"caption\": \"\", \"page\": 42, \"index\": 33, \"width\": 1047, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-034.webp\", \"caption\": \"\", \"page\": 42, \"index\": 34, \"width\": 1612, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-035.webp\", \"caption\": \"\", \"page\": 43, \"index\": 35, \"width\": 1473, \"height\": 940}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-036.webp\", \"caption\": \"\", \"page\": 43, \"index\": 36, \"width\": 943, \"height\": 1352}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-037.webp\", \"caption\": \"\", \"page\": 44, \"index\": 37, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-038.webp\", \"caption\": \"\", \"page\": 44, \"index\": 38, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-039.webp\", \"caption\": \"\", \"page\": 44, \"index\": 39, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-040.webp\", \"caption\": \"\", \"page\": 44, \"index\": 40, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-041.webp\", \"caption\": \"\", \"page\": 44, \"index\": 41, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-042.webp\", \"caption\": \"\", \"page\": 44, \"index\": 42, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-043.webp\", \"caption\": \"\", \"page\": 44, \"index\": 43, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-044.webp\", \"caption\": \"\", \"page\": 44, \"index\": 44, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-045.webp\", \"caption\": \"\", \"page\": 44, \"index\": 45, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-046.webp\", \"caption\": \"\", \"page\": 44, \"index\": 46, \"width\": 1931, \"height\": 1759}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-047.webp\", \"caption\": \"\", \"page\": 44, \"index\": 47, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-048.webp\", \"caption\": \"\", \"page\": 44, \"index\": 48, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-049.webp\", \"caption\": \"\", \"page\": 44, \"index\": 49, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-050.webp\", \"caption\": \"\", \"page\": 45, \"index\": 50, \"width\": 2684, \"height\": 2336}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-051.webp\", \"caption\": \"\", \"page\": 46, \"index\": 51, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-052.webp\", \"caption\": \"\", \"page\": 46, \"index\": 52, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-053.webp\", \"caption\": \"\", \"page\": 46, \"index\": 53, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-054.webp\", \"caption\": \"\", \"page\": 46, \"index\": 54, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-055.webp\", \"caption\": \"\", \"page\": 46, \"index\": 55, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-056.webp\", \"caption\": \"\", \"page\": 46, \"index\": 56, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-057.webp\", \"caption\": \"\", \"page\": 46, \"index\": 57, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-058.webp\", \"caption\": \"\", \"page\": 46, \"index\": 58, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-059.webp\", \"caption\": \"\", \"page\": 46, \"index\": 59, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-060.webp\", \"caption\": \"\", \"page\": 46, \"index\": 60, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-061.webp\", \"caption\": \"\", \"page\": 46, \"index\": 61, \"width\": 2048, \"height\": 1782}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-062.webp\", \"caption\": \"\", \"page\": 46, \"index\": 62, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-063.webp\", \"caption\": \"\", \"page\": 46, \"index\": 63, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-064.webp\", \"caption\": \"\", \"page\": 46, \"index\": 64, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-065.webp\", \"caption\": \"\", \"page\": 46, \"index\": 65, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-066.webp\", \"caption\": \"\", \"page\": 46, \"index\": 66, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-067.webp\", \"caption\": \"\", \"page\": 46, \"index\": 67, \"width\": 2048, \"height\": 1773}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-068.webp\", \"caption\": \"\", \"page\": 46, \"index\": 68, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-069.webp\", \"caption\": \"\", \"page\": 47, \"index\": 69, \"width\": 1536, \"height\": 1091}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-070.webp\", \"caption\": \"\", \"page\": 47, \"index\": 70, \"width\": 537, \"height\": 892}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-071.webp\", \"caption\": \"\", \"page\": 48, \"index\": 71, \"width\": 2967, \"height\": 2516}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-072.webp\", \"caption\": \"\", \"page\": 55, \"index\": 72, \"width\": 2948, \"height\": 2888}]"
motivation: 揭示大语言模型在命题逻辑推理中的内部机制。
method: 通过电路分析方法研究Mistral和Gemma模型在命题逻辑问题上的推理路径。
result: 发现了模型解决逻辑问题的核心组件和顺序执行步骤。
conclusion: 电路分析有助于理解LLM的推理能力。
---

## Abstract
Due to the size and complexity of modern large language models (LLMs), it has proven challenging to uncover the underlying mechanisms that models use to solve reasoning problems. For instance, is their reasoning for a specific problem localized to certain parts of the network? Do they break down the reasoning problem into modular components that are then executed as sequential steps as we go deeper in the model? To better understand the reasoning capability of LLMs, we study a minimal propositional logic problem that requires combining multiple facts to arrive at a solution. By studying this problem on Mistral and Gemma models, up to 27B parameters, we illuminate the core components the models use to solve such logic problems. From a mechanistic interpretability point of view, we use causal mediation analysis to uncover the pathways and components of the LLMs' reasoning processes. Then, we offer fine-grained insights into the functions of attention heads in different layers. We not only find a sparse circuit that computes the answer, but we decompose it into sub-circuits that have four distinct and modular uses. Finally, we reveal that three distinct models -- Mistral-7B, Gemma-2-9B and Gemma-2-27B -- contain analogous but not identical mechanisms.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）如何在其内部实现命题逻辑推理？具体来说，模型是否将推理分解为模块化步骤并顺序执行？是否存在稀疏的、可定位的“电路”负责不同子任务？
- **研究动机**：尽管LLM在多种推理任务上表现出色，但其内部工作机制仍不透明。现有 mechanistic interpretability 研究多局限于小型模型（如GPT-2）或单一单跳任务，缺乏对现代大型LLM多步推理过程的细致分析。
- **整体含义**：通过研究一个规范的两步命题逻辑问题（包含“逻辑运算符链”与“线性链”），论文试图揭示LLM内部是否存在可解释的、模块化的推理电路，并探究不同模型（Mistral-7B、Gemma-2-9B、Gemma-2-27B）是否共享相似机制。

## 2. 方法论：核心思想与关键技术细节

- **问题模板**：输入包含两条规则（如“A or B implies C”和“D implies E”）和三个变量的事实赋值，模型需推断查询变量（如C或E）的未知真值，并输出最小证明（以第一个回答令牌为分析焦点）。
- **因果中介分析（CMA）**：利用反事实提示（如翻转查询变量Q）构造“正常-反事实”提示对，通过激活修补（activation patching）测量各个注意力头对模型输出（logit差）的间接效应（IE），从而定位必要电路组件。
- **电路验证**：对补集电路进行激活冻结（仅允许候选电路C中的注意力头正常流动），观察能否恢复最大干涉效果（∆C/∆alt接近1），以此验证电路充分性。
- **细粒度子组件分析**：针对注意力头的 query、key、value 子组件分别修补，并结合注意力权重分析，将发现的注意力头分为四类：**查询规则定位头**、**查询规则移动头**、**事实处理头**、**决策头**。进一步通过交换规则位置、翻转事实赋值等特制反事实实验来验证每类头的角色。
- **对比分析**：将预训练LLM与从零训练的小型3层Transformer在任务上的机制进行对比，突出模块化程度差异。

## 3. 实验设计

- **数据集与场景**：使用自建的命题逻辑问题模板，变量名从A–Z随机采样，事实真值随机生成。每条提示包含4或6个少样本示例，然后附加一个待回答问题。模型需生成最小证明并给出查询值。
- **评估基准**：以第一个回答令牌的预测准确率作为主要衡量指标（因为该令牌要求模型综合所有推理步骤）。同时使用校准后的logit差作为因果效应的连续度量。
- **对比方法/模型**：
  - 目标模型：Mistral-7B-v0.1、Gemma-2-9B、Gemma-2-27B（均为开放权重模型）。
  - 对照基线：空电路（冻结所有注意力头）以及依次移除四类头族后的电路（消融分析）。
  - 额外对比：从零训练的小型Transformer（3层3头）作为结构简单、任务过拟合的对照。
- **实验充分性**：主要实验结果在60–400个样本上运行，部分关键实验（如规则交换干预）扩展到200样本以降低标准差。重复实验并报告均值与标准差（或误差条）。进行了多组消融（移除每类头族）和多种反事实干预（Q翻转、规则位置交换、事实翻转、逻辑运算符翻转）来系统验证每类头的作用。

## 4. 资源与算力

- 文中未明确给出训练或推理具体使用的GPU型号、数量和总耗时。仅提及小型Transformer实验在单张V100 GPU上运行，每个模型训练约2–3天（60k步，batch size 512）。
- 对于Mistral-7B和Gemma-2-9B/27B，仅说明使用了Google Research的内部计算资源，未提供详细算力信息（如GPU型号、数量、小时数）。

## 5. 实验数量与充分性

- **实验数量**：论文包含大量实验，具体包括：
  - 注意力头输出修补（必要性和充分性验证）各模型约覆盖30–40个注意力头层。
  - 子组件（key/value/query）修补实验每模型约20组。
  - 三组反事实干预（Q翻转、规则位置交换、事实翻转）各模型反复运行。
  - 电路充分性验证：对4组消融（移除每类头族）以及包含/不包含后处理头的变体。
  - 对比小型Transformer的线性探针、激活干扰等实验（附录C、D）。
- **充分性与公平性**：
  - 所有实验使用随机采样的测试样本，确保结果代表性。
  - 关键结论（如四类头族的存在）在三个不同模型（7B、9B、27B）上均得到复现，增强了泛化性。
  - 实验设计系统：先用必要性定位，再用充分性验证，最后用细粒度干预确认功能，逻辑链条完整。
  - 不足之处：缺少对更多随机种子、更多任务变体的测试（例如不同逻辑运算符、事实组合）；对Gemma-2-27B的分析受算力限制较简略；未进行严格的统计显著性检验（如置信区间），但报告了标准差。

## 6. 主要结论与发现

1. **LLM内部存在稀疏、模块化的推理电路**：在Mistral-7B、Gemma-2-9B、Gemma-2-27B中均发现四个功能明确的注意力头族，分别负责规则定位、规则移动、事实处理和决策，按照“Q → 相关规则 → 相关事实 → 决策”的顺序执行。
2. **推理是“懒惰”的**：大多数处理发生在模型看到查询令牌之后，而不是提前预处理所有输入。
3. **子电路复用**：模型在编写后续证明步骤时复用了同样的规则定位头和决策头（例如在写入规则引用时再次使用）。
4. **模型间类比但不同**：三个模型共享核心四类头，但Gemma-2-27B额外拥有“逻辑运算符头”来直接处理“and”/“or”；27B的电路表现出更多并行性（事实处理头对logits有直接效应，而9B中则需通过决策头）。
5. **与小型Transformer对比**：在3层从零训练的模型上，推理机制更加交织、模块性更差，注意模式在不同查询类型下会切换，缺乏专用的决策头。

## 7. 优点

- **问题选择精巧**：命题逻辑问题足够简单以进行因果分析，又包含多步推理和干扰信息，避免了过简单或过复杂的问题。
- **方法论系统**：结合“必要性+充分性+细粒度干预”的三阶段验证，形成了完整的电路发现与验证流程。
- **跨模型对比**：在三个差异较大的现成LLM上得到一致发现，增强了结论的稳健性和普适性。
- **开源代码**：提供GitHub仓库（https://github.com/guanzhehong/prop-logic-transformer-circuit），便于复现。
- **发现具有启发性**：揭示了LLM中“懒惰推理”和子电路复用行为，对理解模型内在工作原理有重要贡献。

## 8. 不足与局限

- **任务复杂性有限**：仅研究了2步推理（一条规则+直接事实），未推广到更长链或更复杂的逻辑结构。
- **模型范围有限**：仅分析了Mistral-7B和Gemma-2系列（9B、27B），未涉及其他架构或更大模型（如Llama、GPT-4）。其中Gemma-2-27B的分析因算力较粗略。
- **实验统计细节不充分**：部分实验仅报告均值，未计算置信区间或进行假设检验；结果的可重复性对随机种子敏感（在小型Transformer实验中提到不同种子可能不收敛）。
- **对MLPs的角色分析不深入**：虽然观察到MLPs的因果作用很弱，但未排除MLPs在更复杂推理中的重要性。
- **鲁棒性问题**：当提示格式改变（如“Facts”和“Rules”顺序调换）时，中间层头家族的功能会发生变化，说明发现的电路并非完全刚性。
- **没有涉及推理链（CoT）或思考模型**：论文仅分析了初始令牌的预测，未深入分析整个证明生成过程中的瞬态电路变化。

（完）
