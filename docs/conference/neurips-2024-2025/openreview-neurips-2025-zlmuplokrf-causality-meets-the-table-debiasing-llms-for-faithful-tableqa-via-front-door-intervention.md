---
title: "Causality Meets the Table: Debiasing LLMs for Faithful TableQA via Front-Door Intervention"
title_zh: 因果关系与表格相遇：通过前门干预去偏以实现忠实表问答
authors: "Zhen Yang, Ziwei Du, Minghan Zhang, Wei Du, Jie Chen, Fulan Qian, Shu Zhao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=zlMupLoKRf"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 通过因果干预提高表问答中LLM推理的忠实性
tldr: 提出因果干预去偏实现忠实表推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1180, \"height\": 777}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-002.webp\", \"caption\": \"\", \"page\": 24, \"index\": 2, \"width\": 1105, \"height\": 1111}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-003.webp\", \"caption\": \"\", \"page\": 24, \"index\": 3, \"width\": 1097, \"height\": 423}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-004.webp\", \"caption\": \"\", \"page\": 25, \"index\": 4, \"width\": 1257, \"height\": 1503}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-005.webp\", \"caption\": \"\", \"page\": 26, \"index\": 5, \"width\": 1231, \"height\": 1024}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-006.webp\", \"caption\": \"\", \"page\": 26, \"index\": 6, \"width\": 1286, \"height\": 1069}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-007.webp\", \"caption\": \"\", \"page\": 27, \"index\": 7, \"width\": 1286, \"height\": 770}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-008.webp\", \"caption\": \"\", \"page\": 27, \"index\": 8, \"width\": 1507, \"height\": 1085}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-009.webp\", \"caption\": \"\", \"page\": 28, \"index\": 9, \"width\": 1507, \"height\": 797}]"
motivation: LLM在表问答中依赖虚假相关而非真正推理。
method: 构建结构因果图并应用前门调整消除词共现偏差。
result: 方法提升了LLM在表问答中的推理忠实性。
conclusion: 因果干预有效促进LLM进行真正推理。
---

## Abstract
Table Question Answering (TableQA) combines natural language understanding and structured data reasoning, posing challenges in semantic interpretation and logical inference. Recent advances in Large Language Models (LLMs) have improved TableQA performance through Direct Prompting and Agent paradigms. However, these models often rely on spurious correlations, as they tend to overfit to token co-occurrence patterns in pretraining corpora, rather than perform genuine reasoning. To address this issue, we propose Causal Intervention TableQA (CIT), which is based on a structural causal graph and applies front-door adjustment to eliminate bias caused by token co-occurrence. CIT formalizes TableQA as a causal graph and identifies token co-occurrence patterns as confounders. By applying front-door adjustment, CIT guides question variant generation and reasoning to reduce confounding effects. Experiments on multiple benchmarks show that CIT achieves state-of-the-art performance, demonstrating its effectiveness in mitigating bias. Consistent gains across various LLMs further confirm its generalizability.

---

## 论文详细总结（自动生成）

# 论文总结：Causality Meets the Table: Debiasing LLMs for Faithful TableQA via Front-Door Intervention

## 1. 核心问题与整体含义（研究动机和背景）

- **动机**：LLM在表问答（TableQA）任务中表现优异，但常依赖预训练语料中的**词共现模式**（token co-occurrence patterns）来生成答案，而非进行真正的推理。这种虚假相关性（spurious correlations）导致模型在面对逻辑上等价但表达方式不同的问题时，容易做出不一致或错误的回答。例如，短语“exactly”经常与“yes”共现，导致LLM倾向于回答“yes”即使正确答案是“no”。
- **整体含义**：本文首次将因果干预引入LLM-based TableQA，通过构建结构因果图，将词共现建模为**潜在混杂因子（confounder）**，并应用**前门调整（front-door adjustment）** 来消除其影响，从而提升推理的忠实性和鲁棒性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将TableQA形式化为结构因果模型（SCM）：理想路径为 Q → E → A（问题→证据→答案），但词共现引入一条后门路径 Q ← C → A（混杂因子C）。通过前门调整，在不需显式观测C的情况下，估算因果效应 P(A | do(Q))。
- **关键技术细节**：
  1. **问题变体生成（Question Variant Generation）**：对每个原始问题Q，并行生成n个语义等价的变体{q'_i}（单次调用LLM），近似估计P(Q=q')（视为均匀分布，可省略）。
  2. **证据聚合（Evidence Aggregation）**：对每个问题变体提取对应证据，并取并集作为最终证据E，确保覆盖不同表述下的相关信息。
  3. **答案推理（Answer Inference）**：支持两种推理范式：
     - **直接提示（DP）**：在证据E和每个变体q'下，LLM以自回归方式生成推理步骤和答案。
     - **Agent（符号推理）**：LLM生成Python代码，执行后得到答案。
  4. **联合投票（Joint Voting）**：将DP和Agent的答案集合进行多数投票，得到最终预测。投票时若平局则随机选择。
- **公式**：最终估计为 P(A|do(Q)) = Σ_e P(E=e|Q) Σ_{q'} P(A=a|E=e,Q=q') P(Q=q')。实际实现中，P(Q=q')视为常数，通过聚合多个变体来近似。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - WikiTableQuestions (WTQ)：4,344个测试样本，涉及聚合、比较、算术推理。
  - TabFact：2,024个样本，表事实验证任务。
  - FetaQA：2,003个样本，自由形式问答，要求长文本生成。
- **评估指标**：WTQ和TabFact使用精确匹配准确率（Exact Match Accuracy）；FetaQA使用BLEU分数。
- **对比方法**：
  - 预训练模型：TAPAS-large, TAPEX-large, T5-3B等。
  - LLM-based方法：Codex, BINDER, DATER, StructGPT, CHAIN-OF-TABLE, ReAcTable, Cabinet, SYNTQA, TIDE等（共20余种）。
  - 本文方法CIT报告了DP、Agent及联合投票三种模式的结果。
- **LLM选择**：
  - 主实验使用GPT-3.5。
  - 泛化性实验涵盖开源模型（LLaMA 2-7B/13B/70B, DeepSeek-R1）和闭源模型（GPT-4, GLM 4, Gemini 1.5, Claude 3.5）。

## 4. 资源与算力

- 论文未明确说明使用的GPU型号、数量或训练时长。文中提到多数实验通过API调用完成（GPT-3.5等），开源模型（LLaMA系列）可能需要本地GPU部署，但未给出具体算力描述。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：在3个数据集上对比了多种基线，共报告3张主表（表1-3）。
  - 消融实验：对DP、Agent及联合投票三种模式，分别移除“问题变体”和“证据聚合”组件（表5），覆盖所有数据集。
  - 超参数分析：探索变体数量（图3）、投票轮数n/m（表4）、表大小影响（图5）。
  - 泛化性实验：在8个不同LLM上验证（表6）。
  - 额外分析：问题类型影响（图4）、错误案例分析（附录F）、数据污染验证（表8）。
- **充分性**：实验设计较为全面，从多个维度验证了方法的有效性。对比基线涵盖近年主流方法，消融实验清晰，泛化性测试覆盖不同规模和来源的LLM。但未报告多次运行的标准差或置信区间，可能忽略随机性影响。

## 6. 主要结论与发现

- CIT在三个基准数据集上均取得**最先进（SOTA）性能**：WTQ上准确率76.38%（比之前最好提升2.73%），TabFact上91.30%（提升2.21%），FetaQA上BLEU 36.34%。
- **问题变体生成和证据聚合是核心组件**：去除变体导致性能显著下降（如WTQ上DP模式下降3.74%），去除证据聚合影响较小。
- **联合投票（DP+Agent）优于单一模式**：Agent在结构化任务（WTQ/TabFact）表现更好，DP在长文本生成（FetaQA）更有优势。
- **泛化性良好**：在所有8个LLM上应用CIT均带来提升（+0.69%至+6.20%），其中GPT-4提升最大（+6.20%）。
- **方法有效缓解词共现偏差**：在双否扰动测试中，CIT准确率显著高于直接推理（76.38% vs 48.70%），说明其依赖真正推理而非表面模式。

## 7. 优点

- **创新性**：首次将因果干预（前门调整）引入LLM-based TableQA，提出系统化的去偏差框架。
- **实用性**：无需访问模型内部或显式建模混杂因子，仅需三个API调用，效率高（表7对比显示API调用数远少于迭代方法）。
- **鲁棒性**：对表大小变化、问题类型差异、不同LLM均表现稳定。
- **充分的分析**：包含消融实验、超参数影响、错误案例分析、数据污染验证等，提供了深入理解。

## 8. 不足与局限

- **对变体质量的依赖**：CIT有效性依赖于生成的问题变体语义等价且多样；若生成质量低（如偏离原意），可能引入噪声。论文未提出控制或筛选变体的机制。
- **未报告统计显著性**：所有结果仅报告单次运行值，未提供标准差或多次重复实验的误差范围，削弱了结论的可靠性。
- **泛化性测试限于8个模型**：对更多不同类型（如多模态模型）或更大规模（>100B）的LLM未验证。
- **只考虑词共现一种混杂**：LLM中的虚假相关可能还有其他来源（如格式偏差、数据集偏差等），本文未覆盖。
- **Agent模式的代码幻觉**：错误案例分析发现Agent易产生幻觉性约束（如添加不存在的条件），可能影响可靠性。
- **可复现性**：尽管附了代码链接（论文声称），但论文正文未详细列出所有超参数（如具体提示词模板仅放在附录，但部分细节可能仍不够）。此外，闭源API响应存在非确定性，复现需谨慎。

（完）
