---
title: "A Implies B: Circuit Analysis in LLMs for Propositional Logical Reasoning"
title_zh: A蕴含B：大语言模型中命题逻辑推理的电路分析
authors: "Guan Zhe Hong, Nishanth Dikkala, Enming Luo, Cyrus Rashtchian, Xin Wang, Rina Panigrahy"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=M0U8wUow8c"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 大语言模型中命题逻辑推理的电路分析
tldr: 机理分析揭示了LLM求解命题逻辑问题的核心组件，在Mistral和Gemma模型上验证。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 1694, \"height\": 1946}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 2048, \"height\": 1887}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2048, \"height\": 1271}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-004.webp\", \"caption\": \"\", \"page\": 8, \"index\": 4, \"width\": 815, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-005.webp\", \"caption\": \"\", \"page\": 8, \"index\": 5, \"width\": 794, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 815, \"height\": 449}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 777, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 963, \"height\": 945}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-009.webp\", \"caption\": \"\", \"page\": 30, \"index\": 9, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-010.webp\", \"caption\": \"\", \"page\": 30, \"index\": 10, \"width\": 2048, \"height\": 1697}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-011.webp\", \"caption\": \"\", \"page\": 30, \"index\": 11, \"width\": 2048, \"height\": 1676}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-012.webp\", \"caption\": \"\", \"page\": 31, \"index\": 12, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-013.webp\", \"caption\": \"\", \"page\": 32, \"index\": 13, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-014.webp\", \"caption\": \"\", \"page\": 32, \"index\": 14, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-015.webp\", \"caption\": \"\", \"page\": 34, \"index\": 15, \"width\": 953, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-016.webp\", \"caption\": \"\", \"page\": 34, \"index\": 16, \"width\": 2010, \"height\": 1331}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-017.webp\", \"caption\": \"\", \"page\": 34, \"index\": 17, \"width\": 2010, \"height\": 1331}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-018.webp\", \"caption\": \"\", \"page\": 34, \"index\": 18, \"width\": 957, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-019.webp\", \"caption\": \"\", \"page\": 35, \"index\": 19, \"width\": 2048, \"height\": 1926}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-020.webp\", \"caption\": \"\", \"page\": 35, \"index\": 20, \"width\": 962, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-021.webp\", \"caption\": \"\", \"page\": 35, \"index\": 21, \"width\": 962, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-022.webp\", \"caption\": \"\", \"page\": 36, \"index\": 22, \"width\": 2048, \"height\": 1976}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-023.webp\", \"caption\": \"\", \"page\": 36, \"index\": 23, \"width\": 953, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-024.webp\", \"caption\": \"\", \"page\": 36, \"index\": 24, \"width\": 953, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-025.webp\", \"caption\": \"\", \"page\": 37, \"index\": 25, \"width\": 1243, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-026.webp\", \"caption\": \"\", \"page\": 37, \"index\": 26, \"width\": 1243, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-027.webp\", \"caption\": \"\", \"page\": 37, \"index\": 27, \"width\": 883, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-028.webp\", \"caption\": \"\", \"page\": 38, \"index\": 28, \"width\": 1969, \"height\": 1189}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-029.webp\", \"caption\": \"\", \"page\": 38, \"index\": 29, \"width\": 1676, \"height\": 1069}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-030.webp\", \"caption\": \"\", \"page\": 39, \"index\": 30, \"width\": 2415, \"height\": 1515}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-031.webp\", \"caption\": \"\", \"page\": 39, \"index\": 31, \"width\": 2415, \"height\": 1515}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-032.webp\", \"caption\": \"\", \"page\": 42, \"index\": 32, \"width\": 1047, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-033.webp\", \"caption\": \"\", \"page\": 42, \"index\": 33, \"width\": 1047, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-034.webp\", \"caption\": \"\", \"page\": 42, \"index\": 34, \"width\": 1612, \"height\": 2048}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-035.webp\", \"caption\": \"\", \"page\": 43, \"index\": 35, \"width\": 1473, \"height\": 940}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-036.webp\", \"caption\": \"\", \"page\": 43, \"index\": 36, \"width\": 943, \"height\": 1352}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-037.webp\", \"caption\": \"\", \"page\": 44, \"index\": 37, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-038.webp\", \"caption\": \"\", \"page\": 44, \"index\": 38, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-039.webp\", \"caption\": \"\", \"page\": 44, \"index\": 39, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-040.webp\", \"caption\": \"\", \"page\": 44, \"index\": 40, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-041.webp\", \"caption\": \"\", \"page\": 44, \"index\": 41, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-042.webp\", \"caption\": \"\", \"page\": 44, \"index\": 42, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-043.webp\", \"caption\": \"\", \"page\": 44, \"index\": 43, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-044.webp\", \"caption\": \"\", \"page\": 44, \"index\": 44, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-045.webp\", \"caption\": \"\", \"page\": 44, \"index\": 45, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-046.webp\", \"caption\": \"\", \"page\": 44, \"index\": 46, \"width\": 1931, \"height\": 1759}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-047.webp\", \"caption\": \"\", \"page\": 44, \"index\": 47, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-048.webp\", \"caption\": \"\", \"page\": 44, \"index\": 48, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-049.webp\", \"caption\": \"\", \"page\": 44, \"index\": 49, \"width\": 1570, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-050.webp\", \"caption\": \"\", \"page\": 45, \"index\": 50, \"width\": 2684, \"height\": 2336}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-051.webp\", \"caption\": \"\", \"page\": 46, \"index\": 51, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-052.webp\", \"caption\": \"\", \"page\": 46, \"index\": 52, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-053.webp\", \"caption\": \"\", \"page\": 46, \"index\": 53, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-054.webp\", \"caption\": \"\", \"page\": 46, \"index\": 54, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-055.webp\", \"caption\": \"\", \"page\": 46, \"index\": 55, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-056.webp\", \"caption\": \"\", \"page\": 46, \"index\": 56, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-057.webp\", \"caption\": \"\", \"page\": 46, \"index\": 57, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-058.webp\", \"caption\": \"\", \"page\": 46, \"index\": 58, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-059.webp\", \"caption\": \"\", \"page\": 46, \"index\": 59, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-060.webp\", \"caption\": \"\", \"page\": 46, \"index\": 60, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-061.webp\", \"caption\": \"\", \"page\": 46, \"index\": 61, \"width\": 2048, \"height\": 1782}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-062.webp\", \"caption\": \"\", \"page\": 46, \"index\": 62, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-063.webp\", \"caption\": \"\", \"page\": 46, \"index\": 63, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-064.webp\", \"caption\": \"\", \"page\": 46, \"index\": 64, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-065.webp\", \"caption\": \"\", \"page\": 46, \"index\": 65, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-066.webp\", \"caption\": \"\", \"page\": 46, \"index\": 66, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-067.webp\", \"caption\": \"\", \"page\": 46, \"index\": 67, \"width\": 2048, \"height\": 1773}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-068.webp\", \"caption\": \"\", \"page\": 46, \"index\": 68, \"width\": 2048, \"height\": 459}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-069.webp\", \"caption\": \"\", \"page\": 47, \"index\": 69, \"width\": 1536, \"height\": 1091}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-070.webp\", \"caption\": \"\", \"page\": 47, \"index\": 70, \"width\": 537, \"height\": 892}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-071.webp\", \"caption\": \"\", \"page\": 48, \"index\": 71, \"width\": 2967, \"height\": 2516}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-m0u8wuow8c/fig-072.webp\", \"caption\": \"\", \"page\": 55, \"index\": 72, \"width\": 2948, \"height\": 2888}]"
motivation: 理解大语言模型内部推理机制对于提升其可靠性至关重要。
method: 通过研究Mistral和Gemma模型在最小命题逻辑问题上的表现，揭示模型使用的核心组件。
result: 发现模型使用模块化的顺序推理步骤，并定位了关键注意力头。
conclusion: 大语言模型通过内部电路实现逻辑推理，可被机理分析解释。
---

## Abstract
Due to the size and complexity of modern large language models (LLMs), it has proven challenging to uncover the underlying mechanisms that models use to solve reasoning problems. For instance, is their reasoning for a specific problem localized to certain parts of the network? Do they break down the reasoning problem into modular components that are then executed as sequential steps as we go deeper in the model? To better understand the reasoning capability of LLMs, we study a minimal propositional logic problem that requires combining multiple facts to arrive at a solution. By studying this problem on Mistral and Gemma models, up to 27B parameters, we illuminate the core components the models use to solve such logic problems. From a mechanistic interpretability point of view, we use causal mediation analysis to uncover the pathways and components of the LLMs' reasoning processes. Then, we offer fine-grained insights into the functions of attention heads in different layers. We not only find a sparse circuit that computes the answer, but we decompose it into sub-circuits that have four distinct and modular uses. Finally, we reveal that three distinct models -- Mistral-7B, Gemma-2-9B and Gemma-2-27B -- contain analogous but not identical mechanisms.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLMs）能够解决复杂的推理任务，但其内部工作机制仍不透明。尤其是，模型是否将推理分解为模块化步骤、是否依赖特定局部组件、是否在不同规模/架构中共享相似的推理算法，这些问题尚未得到充分解答。
- **核心问题**：本文聚焦于理解LLMs如何解决一个**最小命题逻辑推理问题**——给定若干规则和事实，推断查询变量的真值。该问题需要模型定位相关规则、处理事实、做出决策，是一个典型的多步推理任务。
- **整体含义**：通过揭示LLMs内部的稀疏电路结构及模块化注意力头的功能，本文为理解深度学习模型的推理机制提供了重要视角，并展示了不同模型（Mistral、Gemma系列）之间虽不完全相同但高度相似的内部推理架构。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用**因果中介分析（Causal Mediation Analysis, CMA）** 来识别和验证LLMs中负责推理的因果电路。通过构建反事实提示（如改变查询变量、规则位置、事实值），测量模型内部组件（主要是注意力头）对输出的间接效应（IE），从而定位关键组件。
- **关键技术细节**：
    - **电路发现（必要性测试）**：将特定组件（如注意力头输出、键/值/查询激活）从原始输入切换到反事实输入，观察输出logit差异的变化。若切换后输出显著偏离原始答案，则说明该组件对推理至关重要。
    - **电路验证（充分性测试）**：冻结电路之外的所有组件为反事实激活，仅允许电路中组件正常运行，检查模型是否仍能正确输出。若电路单独足以维持原始行为，则验证了其充分性。
    - **子组件功能分析**：对注意力头的键、查询、值分别进行干预，结合注意力模式统计，推断其角色（如规则定位、信息移动、事实处理、决策）。
    - **跨模型比较**：在Mistral-7B、Gemma-2-9B、Gemma-2-27B上重复上述流程，对比不同规模和家族的模型是否具有类似的电路结构。
- **公式与算法流程**（文字说明）：
    - 定义反事实提示对（原始/修改），对每个组件计算标准化logit差异分数：`(∆_intervened - ∆_orig) / (∆_alt - ∆_orig)`，分数接近1表示强间接效应。
    - 电路充分性验证：仅允许电路中组件在原始提示下正常计算，其余组件强制使用反事实激活，测量模型对原始答案的偏好程度。

## 3. 实验设计

- **数据集/场景**：使用**自定义命题逻辑问题**（长度2：一条逻辑操作链和一条线性链，共5个布尔变量）。输入格式为“规则+事实+问题”，要求模型输出最小证明（第一token即为关键事实）。模型通过**少样本学习**（4-6个示例）后回答新问题。
- **基准与对比方法**：
    - 主要研究三个LLM：**Mistral-7B-v0.1、Gemma-2-9B、Gemma-2-27B**，对比它们的内部电路。
    - 额外训练**小型GPT-like Transformer**（3层、3头、注意力仅结构）作为对照，观察其非模块化、自适应的推理策略。
- **评估指标**：首次token预测的logit差异、正确率（模型输出完整证明的正确比例）。对于小模型还采用线性探针（线性分类器）和激活干预。

## 4. 资源与算力

- 论文**未明确说明LLM实验的具体GPU型号、数量及总训练时长**。仅提及对LLM进行因果中介分析，需要多次前向传播，但未报告算力消耗。
- 对于**小模型训练**部分：使用**单块V100 GPU**，训练**3层3头Transformer**（共约200万参数），训练时长约**2-3天**（含多次随机种子实验）。
- 总体而言，算力细节不够透明，但小模型的训练开销已明确，LLM分析的计算需求可能更高（需多次激活缓存与干预）。

## 5. 实验数量与充分性

- **实验数量**：丰富且系统。
    - **LLM电路发现**：对每个模型进行了60-200个样本的激活干预实验（不同反事实类型如改变QUERY、规则位置、事实值），并重复3次以上验证统计显著性。
    - **细粒度干预**：对注意力头的键、查询、值分别干预，并计算多个指标（校准logit差异、交叉熵损失变化等）。
    - **充分性测试**：在Gemma-2-9B和Mistral-7B上分别完成了不同电路组合的验证（空电路、完整电路、移除某一族头等）。
    - **跨模型比较**：三个LLM均做了类似分析，并额外对Gemma-2-27B进行了逻辑算子转换的干预。
    - **小模型研究**：训练了多个不同层数和头数的变体，并进行了线性探针、激活干预、注意力模式统计等。
- **充分性与公平性**：实验设计较为完备——使用随机采样的测试集、报告均值与标准差、多次重复实验。反事实构造合理（仅改变单一因素如QUERY），控制了混淆变量。结论基于因果证据而非相关性。但**小模型仅在单一任务上训练**，其与LLM的可比性有限（训练数据分布差异大）。

## 6. 论文的主要结论与发现

1. **LLMs中存在稀疏、模块化的推理电路**：四个功能明确的注意力头家族——**查询规则定位头（Queried-Rule Locators）、查询规则移动头（Queried-Rule Movers）、事实处理头（Fact Processors）、决策头（Decision Heads）**，依次实现“QUERY→定位规则→移动规则信息→处理事实→决策”的推理链路。MLP贡献较小。
2. **跨模型一致性**：Mistral-7B、Gemma-2-9B、Gemma-2-27B均包含类似结构的电路，但机制不完全一致。例如Gemma-2-27B的电路更具并行性，且拥有专门的**逻辑算子头**（关注“and”/“or”），这在9B模型中不明显。
3. **“惰性推理”现象**：电路主要在模型看到QUERY token后才激活，不会提前处理规则和事实，体现了按需处理的特点。
4. **子电路复用**：同一组定位头和决策头在生成证明中多次复用（不仅用于首token的预测，也用于后续规则调用）。
5. **与小模型的对比**：在3层Transformer中，推理过程更不模块化，注意力头的功能随输入情境变化而动态调整，缺乏LLM中的专用化组件。

## 7. 优点

- **方法新颖**：将因果中介分析应用于LLM推理的多步逻辑问题，系统识别并验证了四个功能明确的子电路，是首个在7B~27B规模模型上完成此类分析的工作。
- **实验设计严谨**：使用多种反事实（改变QUERY、规则位置、事实值、逻辑算子）进行必要性/充分性测试，并辅以注意力模式统计和细粒度子组件干预，结论有强力因果支撑。
- **跨模型比较**：对比了不同家族（Mistral vs. Gemma）和不同规模（7B/9B/27B）的模型，揭示了共性（模块化顺序推理）和差异（并行度、逻辑算子头），具有普遍性和洞察力。
- **提供开源工具**：公开GitHub代码仓库，包含完整的实验Notebook，可复现核心结果。

## 8. 不足与局限

- **任务复杂性有限**：仅限于长度2的命题逻辑问题（5个变量、2条规则），更长的推理链或更复杂的逻辑形式（如一阶逻辑）尚未研究。结论能否推广到更现实、更复杂的推理场景（如数学应用题）尚不确定。
- **LLM算力细节不透明**：未报告LLM实验所需的GPU时间，不利于其他研究者评估资源需求。
- **小模型对照的局限性**：小模型是在合成逻辑任务上从头训练的，而LLM是预训练后在少样本设置下表现，两者训练数据分布差异巨大，其机制差异可能源于预训练而非模型大小本身。
- **电路定义的主观性**：电路边界（哪些头必须包括、哪个token位置）存在一定主观选择，虽经充分性验证，但“完全”电路还原度仅86%~98%，仍有少量因果效应未被解释。
- **潜在偏差**：模型输出正确率并非100%（Mistral-7B约70%，Gemma-2-9B约83%），分析可能偏向于正确样本，错误样本的机制可能存在不同。另外，仅分析了首个token的预测，未深入覆盖完整证明的后续token。

（完）
