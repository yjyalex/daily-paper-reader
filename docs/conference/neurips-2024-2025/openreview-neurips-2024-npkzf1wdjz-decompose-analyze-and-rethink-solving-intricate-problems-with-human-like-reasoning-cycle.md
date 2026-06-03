---
title: "Decompose, Analyze and Rethink: Solving Intricate Problems with Human-like Reasoning Cycle"
title_zh: 分解、分析与反思：以类人推理循环解决复杂问题
authors: "Shangzi Xue, Zhenya Huang, Jiayu Liu, Xin Lin, Yuting Ning, Binbin Jin, Xin Li, Qi Liu"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=NPKZF1WDjZ"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 类似人类的推理循环，包含分解与重新思考
tldr: 通过分解-分析-反思迭代构建推理树解决复杂问题。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-npkzf1wdjz/fig-001.webp\", \"caption\": \"\", \"page\": 9, \"index\": 1, \"width\": 456, \"height\": 383}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-npkzf1wdjz/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 455, \"height\": 381}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-npkzf1wdjz/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 454, \"height\": 331}]"
motivation: 现有方法难以处理需要逻辑规划和全局更新的复杂推理问题。
method: 采用树状问题分解与自然语言反馈全局更新推理步骤。
result: 在多个复杂推理任务上取得优异表现。
conclusion: 类人推理循环有效提升大模型解决复杂问题的能力。
---

## Abstract
In this paper, we introduce DeAR (_Decompose-Analyze-Rethink_), a framework that iteratively builds a reasoning tree to tackle intricate problems within a single large language model (LLM). Unlike approaches that extend or search for rationales, DeAR is featured by 1) adopting a tree-based question decomposition manner to plan the organization of rationales, which mimics the logical planning inherent
in human cognition; 2) globally updating the rationales at each reasoning step through natural language feedback. Specifically, the _Decompose_ stage decomposes the question into simpler sub-questions, storing them as new nodes; the _Analyze_ stage generates and self-checks rationales for sub-questions at each node evel; and the _Rethink_ stage updates parent-node rationales based on feedback from their child nodes. By generating and updating the reasoning process from a more global perspective, DeAR constructs more adaptive and accurate logical structures for complex problems, facilitating timely error correction compared to rationale-extension and search-based approaches such as Tree-of-Thoughts (ToT) and Graph-of-Thoughts (GoT). We conduct extensive experiments on three reasoning benchmarks, including ScienceQA, StrategyQA, and GSM8K, which cover a variety of reasoning tasks, demonstrating that our approach significantly reduces logical errors and enhances performance across various LLMs. Furthermore, we validate that DeAR is an efficient method that achieves a superior trade-off between accuracy and reasoning time compared to ToT and GoT.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有大语言模型（LLM）推理方法（如 Chain-of-Thought, Tree-of-Thoughts, Graph-of-Thoughts）在处理复杂推理任务时存在两类关键缺陷：
  - **结构僵化**：ToT/GoT 等方法扩展固定数量的思维分支，缺乏人类逻辑规划能力，易导致信息冗余或缺失。
  - **错误传播**：理由按顺序生成，中间错误（如“16-3+4=17”）无法及时纠正，会沿着路径传播到最终答案。
- **研究动机**：受认知科学中“推理简化理论”和“自我反思理论”启发，人类面对复杂问题时倾向于先分解为子问题，再逐步解答并反思更新先前结果。
- **整体含义**：提出 DeAR（_Decompose-Analyze-Rethink_）框架，通过构建推理树并引入“分解-分析-反思”循环，使 LLM 的推理过程更符合人类认知模式，从而提升复杂问题求解的准确性、可解释性和鲁棒性。

## 2. 方法论：核心思想、关键技术细节

### 2.1 核心思想
- 构建一个**推理树 (Reasoning Tree)** ，其中节点包含子问题、理由和连贯性分数，边表示父问题到子问题的分解关系。
- 通过**自顶向下的分解**（从原问题到子问题）和**自底向上的更新**（用子节点理由修正父节点理由）交替进行，模拟人类在推理中的规划‑反馈循环。

### 2.2 关键技术细节（DeAR 循环三阶段）

1. **分解阶段 (Decompose)**
   - 输入：当前节点的问题 \( q_t \)。
   - 提取“逻辑启发 (Logic Heuristics)”：从预先构建的分解演示池中检索与原始问题 \( Q \) 最相似的 \( K \) 个问-分解对，作为提示的一部分。
   - 若当前节点连贯性分数 \( s_t \) 大于阈值 \( \epsilon_1 \)，则提示 LLM 将 \( q_t \) 自适应地分解为若干子问题 \( \{q_{t+1}^j\} \)，不预设数量（但受最大分支限制）。
   - 分解结果作为子节点添加。

2. **分析阶段 (Analyze)**
   - 为每个新子问题 \( q_{t+1}^j \) 执行：
     - **求解 (Solve)**：生成原始理由 \( r_{t+1}^j \)。
     - **自我检查 (Self-Check)**：LLM 检查 \( r_{t+1}^j \) 中的错误并修正得到 \( \hat{r}_{t+1}^j \)。
     - **评分 (Score)**：LLM 评估修正后理由与问题的逻辑连贯性，输出分数 \( s_{t+1}^j \)（范围为 0–1，通过直接生成数值获得）。
   - 将 \( (q_{t+1}^j, \hat{r}_{t+1}^j, s_{t+1}^j) \) 存入子节点。

3. **反思阶段 (Rethink)**
   - 若子节点分数 \( s_{t+1}^j > \epsilon_2 \)，则 LLM 从当前路径以上所有节点中**提取**与子问题最相关的 \( k \) 个祖先节点。
   - 使用子节点的理由 \( \hat{r}_{t+1}^j \) **更新**这些祖先节点的理由，替换原理由。
   - 更新后根节点理由被视为最终解决方案，从中提取答案。

### 2.3 算法流程
- 初始化：根节点 \( n_0 \) 包含原始问题 \( Q \)。
- 循环（直到所有节点为叶子或达到最大深度）：
  1. 从队列取出当前节点 \( n_t \)。
  2. 若 \( s_t > \epsilon_1 \) 则执行分解。
  3. 对每个新子节点执行分析：求解、自我检查、评分入队。
  4. 若子节点分数 \( > \epsilon_2 \) 则执行反思：提取相关祖先节点并更新其理由。
- 最终从根节点理由中提取答案。

## 3. 实验设计

### 3.1 数据集与场景
- **ScienceQA**（知识推理）：多模态科学问答，21,208 道选择题。
- **StrategyQA**（逻辑推理）：开放式问题，需要隐式推理步骤，2,780 个示例。
- **GSM8K**（数学推理）：小学数学应用题，8.5K 训练 + 1K 测试。
- 任务类型覆盖知识推理、逻辑推理、数学推理。

### 3.2 对比方法（Baselines）
- **主对比**：Few-shot prompting, Chain-of-Thought (CoT), Tree-of-Thoughts (ToT), Graph-of-Thoughts (GoT)。
- **额外对比**：Least-to-most Prompting, SelfCheck, ToT-variant, CoT+Self-Consistency（部分实验）。
- 骨干模型：GPT-3.5 (gpt-3.5-turbo-1106)、LLaMA2-7B、ChatGLM3-6B；部分额外实验使用 GPT-4。

### 3.3 评估指标
- 主指标：准确率 (Accuracy)。
- 辅助指标：ROSCOE 套件中的 Source-Consistency (SC) 和 Reasoning Alignment (RA)；人类评估（多数投票，Kappa=0.70）；推理时间（秒/问题）和 API 调用次数。

## 4. 资源与算力

- **文中未明确报告 GPU 型号、数量或训练时长**。
- 实验方式：
  - GPT-3.5 通过 OpenAI API 调用，未涉及本地 GPU 训练。
  - LLaMA2-7B 和 ChatGLM3-6B 从 Hugging Face 加载预训练模型**直接推理**（无微调），因此算力消耗主要来自推理时的 GPU 资源分配，但论文未具体说明使用了几块 GPU 或型号。
- 仅提供了推理时间（以 ChatGLM3-6B 为例）及 API 调用次数的比较，显示 DeAR 在效率上占优。

## 5. 实验数量与充分性

### 5.1 实验数量
- **主实验**（表 1）：3 个数据集 × 3 种骨干模型 × 4 种主要基线 + DeAR = 15 个条件，另附 Least-to-most 和 SelfCheck 结果。
- **消融研究**：
  - 自我检查模块消融（表 10）。
  - 逻辑启发消融（表 6）。
  - 不同阈值 \( (\epsilon_1, \epsilon_2) \) 组合对准确率的影响（图 4）。
  - 随机更新 vs. 选择性更新（表 3）。
- **效率分析**（图 5）：不同分支/深度设置下的时间-准确率散点图。
- **推理树特性分析**（表 2）：平均分支数、深度、理由长度。
- **自动一致性评估**（表 3）：ROSCOE 分数。
- **人类评估**（图 3）：每数据集 100 个样本，10 名标注者。
- **额外基线**（表 9）：MATH 数据集 + GPT-4 骨干，对比 ToT-variant、CoT+SC 等。

### 5.2 充分性与公平性
- **实验覆盖全面**：三种不同推理类型，三种不同规模/来源的 LLM，多种对比方法（包括最先进的 ToT 和 GoT）。
- **消融设计合理**：验证了自我检查、逻辑启发、反思阶段中更新策略的有效性。
- **统计显著性**：报告了与基线的配对 t 检验（p < 0.05），结果显著。
- **人机评估**：采用盲评、多数投票、Kappa 一致性（0.70），保证了主观评价的可靠性。
- **参数选择**：阈值通过验证集选择，且测试集结果与验证集一致，避免了过拟合。
- **公平性**：对比方法按照原论文设置，分支/深度等超参数与 DeAR 平均特性对齐（如 b=2,d=3 等），避免不公平比较。

## 6. 主要结论与发现

1. **准确率显著提升**：在全部三个数据集及三种骨干模型下，DeAR 均超越所有基线，提升具有统计显著性（p<0.05）。
2. **逻辑一致性更强**：ROSCOE（SC, RA）指标和人类评估均显示 DeAR 生成的理由逻辑性更好，减少中间错误。
3. **全局更新有效**：与“随机更新”对比，DeAR 基于 LLM 自主选择更新节点更优；且无需更新全部节点，节省计算。
4. **效率优势**：在相近准确率水平下，DeAR 推理时间更短、API 调用次数更少，实现了准确率与效率的更好 trade-off。
5. **推理树结构揭示了任务特性**：ScienceQA 需要更深分解（平均深度 3.62），StrategyQA 分支更多（2.43），GSM8K 理由更长（85.27 字符）。
6. **阈值调优反映任务需求**：GSM8K 需要更低的反思阈值（0.4），表明该任务需要更频繁的更新才能达到最佳效果。

## 7. 优点

- **方法创新**：
  - 将问题分解与理由更新有机结合，模仿人类“先分解后反思”的认知过程，优于纯扩展/搜索的方法。
  - 自适应分解（不预设子问题数量）和基于 LLM 自主选择更新节点，灵活性强。
- **实验全面详实**：
  - 跨多种任务、模型、评估维度，消融充分，统计分析严谨。
  - 引入人类评估和自动合理性指标，多角度验证。
- **实用性强**：在保证准确率的同时，推理效率优于 ToT/GoT，更具应用潜力。
- **可解释性好**：推理树结构直观展示推理路径，案例展示清晰的错误修正过程。

## 8. 不足与局限

- **推理时间仍然较长**：尽管效率优于对比方法，但迭代循环仍带来可观开销，对于对实时性要求极高的场景可能不够理想。
- **依赖额外标注**：分解阶段需要“逻辑启发”示例，虽然可利用现有训练集，但扩展到新领域需人工标注或自动生成。
- **真实世界验证有限**：仅在三个学术基准上测试，未在更复杂（如多模态、开放域）或实际业务场景中验证。
- **阈值选择依赖验证集**：虽然验证集选择与测试集一致，但阈值调优过程可能引入一定偏差，迁移到新任务需重新调整。
- **最大深度/分支限制**：为控制计算设置了最大深度 4 和最大分支 3，对极端复杂问题可能不足，但实验表明平均深度/分支均未超过限制。

（完）
