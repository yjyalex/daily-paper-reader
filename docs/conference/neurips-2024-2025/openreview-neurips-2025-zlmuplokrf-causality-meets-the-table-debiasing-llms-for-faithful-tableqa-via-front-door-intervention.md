---
title: "Causality Meets the Table: Debiasing LLMs for Faithful TableQA via Front-Door Intervention"
title_zh: 因果遇见表格：通过前门干预使大语言模型对表格问答忠实
authors: "Zhen Yang, Ziwei Du, Minghan Zhang, Wei Du, Jie Chen, Fulan Qian, Shu Zhao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=zlMupLoKRf"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 通过去偏提高表格问答的忠实性
tldr: 提出因果干预表格问答，消除虚假相关性，提升忠实推理
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1180, \"height\": 777}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-002.webp\", \"caption\": \"\", \"page\": 24, \"index\": 2, \"width\": 1105, \"height\": 1111}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-003.webp\", \"caption\": \"\", \"page\": 24, \"index\": 3, \"width\": 1097, \"height\": 423}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-004.webp\", \"caption\": \"\", \"page\": 25, \"index\": 4, \"width\": 1257, \"height\": 1503}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-005.webp\", \"caption\": \"\", \"page\": 26, \"index\": 5, \"width\": 1231, \"height\": 1024}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-006.webp\", \"caption\": \"\", \"page\": 26, \"index\": 6, \"width\": 1286, \"height\": 1069}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-007.webp\", \"caption\": \"\", \"page\": 27, \"index\": 7, \"width\": 1286, \"height\": 770}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-008.webp\", \"caption\": \"\", \"page\": 27, \"index\": 8, \"width\": 1507, \"height\": 1085}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-zlmuplokrf/fig-009.webp\", \"caption\": \"\", \"page\": 28, \"index\": 9, \"width\": 1507, \"height\": 797}]"
motivation: 大语言模型在表格问答中依赖词共现的虚假相关性，而非真正推理。
method: 基于结构因果图，应用前门调整消除令牌共现偏差，构建因果干预表格问答（CIT）。
result: 在多个表格问答数据集上，CIT显著提升了准确性和推理忠实性。
conclusion: 因果干预是一种有效的去偏方法，可增强大语言模型在结构化数据上的推理能力。
---

## Abstract
Table Question Answering (TableQA) combines natural language understanding and structured data reasoning, posing challenges in semantic interpretation and logical inference. Recent advances in Large Language Models (LLMs) have improved TableQA performance through Direct Prompting and Agent paradigms. However, these models often rely on spurious correlations, as they tend to overfit to token co-occurrence patterns in pretraining corpora, rather than perform genuine reasoning. To address this issue, we propose Causal Intervention TableQA (CIT), which is based on a structural causal graph and applies front-door adjustment to eliminate bias caused by token co-occurrence. CIT formalizes TableQA as a causal graph and identifies token co-occurrence patterns as confounders. By applying front-door adjustment, CIT guides question variant generation and reasoning to reduce confounding effects. Experiments on multiple benchmarks show that CIT achieves state-of-the-art performance, demonstrating its effectiveness in mitigating bias. Consistent gains across various LLMs further confirm its generalizability.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLM）在表格问答（TableQA）中表现优异，但存在依赖词共现（token co-occurrence）虚假相关性的问题，导致推理不忠实。例如，当问题中出现“exactly”时，模型倾向于回答“yes”，即使正确答案是“no”。这种偏差源于预训练语料中的表层模式而非深层逻辑推理。
- **整体含义**：作者首次从因果角度建模TableQA中的偏差，提出通过前门调整（front-door adjustment）阻断由未观测混杂因子（词共现模式）引入的后门路径，从而提升LLM推理的鲁棒性和忠实性。

## 2. 论文提出的方法论

### 核心思想
- 将TableQA建模为结构因果图（SCM）：理想因果路径为 Q → E → A（问题→证据→答案），但词共现模式作为混杂因子 C 引入后门路径 Q ← C → A。
- 应用前门调整公式：
  \[
  P(A = a \mid do(Q = q)) = \sum_e P(E = e \mid Q = q) \sum_{q'} P(A = a \mid E = e, Q = q') P(Q = q')
  \]
  无需直接观测混杂因子 C，仅通过可观测变量估计因果效应。

### 关键技术细节（CIT框架四步）
1. **问题变体生成**（对应 \(P(Q = q')\)）：
   - 对每个原始问题 Q，通过单次LLM调用生成 n 个语义等价但表层形式不同的变体 \(\{q'_i\}_{i=1}^n\)，假设变体概率均匀。
2. **证据聚合**（对应 \(\sum_e P(E = e \mid Q = q)\)）：
   - 使用单次LLM调用同时为所有变体提取证据，合并为统一证据集 \(e = \bigcup_{i=1}^n e_{q'_i}\)。
3. **答案推理**（对应 \(\sum_{q'} P(A = a \mid E = e, Q = q') P(Q = q')\)）：
   - 两种推理范式：
     - **Direct Prompting (DP)**：链式思维（CoT）生成自然语言推理步骤和答案。
     - **Agent**：生成并执行Python代码进行符号运算。
   - 采用单次调用处理所有变体，提高效率。
4. **联合投票**：
   - 将DP和Agent的多轮输出（n轮DP + m轮Agent）整体进行多数投票选出最终答案；平局时随机选择。

### 算法流程简述
输入问题Q和表格T → 生成问题变体集 → 对各变体联合提取证据并聚合 → 分别用DP和Agent推理 → 多数投票输出最终答案。总调用次数：变体生成1次 + 证据提取1次 + 推理1次（共3次API调用）。

## 3. 实验设计

### 数据集
- **WikiTableQuestions (WTQ)**：4344个测试样本，涉及聚合、比较、算术推理。
- **TabFact**：2024个样本，事实验证任务。
- **FetaQA**：2003个样本，自由形式问答，需长文本生成。

### 评估指标
- WTQ和TabFact：精确匹配准确率（exact match accuracy）。
- FetaQA：BLEU分数。

### 对比方法
- 涵盖预训练模型：TAPAS-large、TAPEX-large、T5-3B等。
- LLM-based方法：Codex、BINDER、DATER、StructGPT、CHAIN-OF-TABLE、ReAcTable、Cabinet、SYNTQA、TIDE等。
- 基线包括直接提示、Agent范式以及两者联合的方法（Mix-SC、TIDE）。

### 主要对比设置
- 主要实验使用GPT-3.5，泛化性实验测试了多种LLM（开源：LLaMA 2-7B/13B/70B、DeepSeek-R1；闭源：GPT-4、GLM 4、Gemini 1.5、Claude 3.5），温度设为0.8。

## 4. 资源与算力

- **未详细说明**：论文主要基于LLM API调用（如GPT-3.5/4、Gemini等）和开源模型推理（LLaMA 2等），未提及使用的GPU型号、数量或训练时长。
- **间接信息**：作者提到CIT只需3次API调用（变体生成、证据提取、推理各一次），远少于基于迭代的方法（如CHAIN-OF-TABLE需N*3次）。对于开源模型，可部署于私有环境，但计算资源未量化。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：三个数据集（WTQ、TabFact、FetaQA）上对比众多基线方法（表1-3）。
  - 消融实验：分别移除“问题变体”和“证据聚合”两个组件，验证每个模块贡献。
  - 参数影响：分析变体数量(n,m)对性能和token消耗的影响（表4、图3）。
  - 泛化性实验：9种不同LLM（开源+闭源）上验证CIT的通用性（表6）。
  - 因变量分析：按问题类型、表格大小、模型规模等分组分析性能（图4、图5）。
  - 错误案例分析：手工检查100个错误样本，分类错误类型。
  - 数据污染分析：对比直接QA与CIT的准确率，排除数据泄露影响（表8）。
- **充分性评估**：实验设计全面，覆盖标准基准、消融、泛化、效率、错误分析等维度，对比方法充分（20+个基线），结果重复性和统计不确定性未报告（无误差棒），但论文声称进行了多次运行取平均。整体上实验较为充分、客观、公平。

## 6. 论文的主要结论与发现

- **状态达到（SOTA）**：CIT在三个数据集上均超越所有对比方法（WTQ：76.38% vs 此前最优TIDE 75.00%；TabFact：91.30% vs TIDE 89.82%；FetaQA：BLEU 36.34% vs UniTabPT 33.12%）。
- **因果方法有效**：通过前门调整消除词共现偏差，显著提升忠实推理能力。
- **泛化能力强**：在多种LLM上均有一致提升（如GPT-4提升6.20%，Claude 3.5提升3.54%）。
- **组件贡献**：问题变体是核心，移除后性能下降最多（如CIT-DP在TabFact上下降6.77%）；证据聚合也有辅助作用。
- **DP与Agent互补**：联合投票优于单一模式，Agent在结构化推理上更强，DP在自由生成上更优。
- **效率高**：仅3次API调用即可实现SOTA，相比迭代方法显著降低开销。

## 7. 优点

- **创新性**：首次将因果干预（前门调整）引入LLM-based TableQA，提出无需观测混杂因子的去偏方法。
- **方法简洁高效**：通过生成问题变体实现因果估计，单次调用策略降低计算成本，便于实际部署。
- **全面实验验证**：在多样数据集、多LLM、多种设置下验证有效性，消融和错误分析充分。
- **鲁棒性强**：对数据污染、表格大小变化、问题类型等均表现稳定。
- **可解释性**：因果图直观展示偏差来源，前门调整公式清晰可推导。

## 8. 不足与局限

- **依赖变体质量**：问题变体生成的质量直接影响去偏效果，语义不一致或多样性不足可能削弱因果估计。
- **未考虑多模态表格**：方法仅处理结构化文本，未涉及图像表格或混合模态。
- **计算资源未量化**：API调用成本虽低，但未给出具体的GPU/时间消耗，不利于复现和成本评估。
- **对表格大小敏感**：在超大表格上DP模式性能下降明显（Agent相对更好），需进一步优化长上下文处理。
- **错误分析有限**：仅人工检查100例，样本量较小，可能遗漏系统性错误模式。
- **缺乏理论严格性**：虽然给出了前门调整推导，但未证明该公式在LLM场景下的实际等价性（如变体均匀分布假设的合理性）。
- **仅针对TableQA**：方法设计高度依赖于表格-问题结构，泛化到其他任务（如文本QA、视觉QA）需重新设计因果图。

（完）
