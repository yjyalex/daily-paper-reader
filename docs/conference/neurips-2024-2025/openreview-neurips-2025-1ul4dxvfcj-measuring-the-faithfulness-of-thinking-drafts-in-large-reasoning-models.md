---
title: Measuring the Faithfulness of Thinking Drafts in Large Reasoning Models
title_zh: 测量大型推理模型思考草稿的忠实性
authors: "Zidi Xiong, Shan Chen, Zhenting Qi, Himabindu Lakkaraju"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=1UL4dxvfcJ"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 系统性反事实框架评估思考草稿的忠实性
tldr: 提出反事实干预框架衡量大型推理模型中思考草稿的忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 7688, \"height\": 2772}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-002.webp\", \"caption\": \"\", \"page\": 5, \"index\": 2, \"width\": 5256, \"height\": 2172}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 5252, \"height\": 2172}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 5256, \"height\": 2176}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-009.webp\", \"caption\": \"\", \"page\": 19, \"index\": 9, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-010.webp\", \"caption\": \"\", \"page\": 19, \"index\": 10, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-011.webp\", \"caption\": \"\", \"page\": 19, \"index\": 11, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-012.webp\", \"caption\": \"\", \"page\": 19, \"index\": 12, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-013.webp\", \"caption\": \"\", \"page\": 20, \"index\": 13, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-014.webp\", \"caption\": \"\", \"page\": 20, \"index\": 14, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-015.webp\", \"caption\": \"\", \"page\": 20, \"index\": 15, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-016.webp\", \"caption\": \"\", \"page\": 20, \"index\": 16, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-017.webp\", \"caption\": \"\", \"page\": 21, \"index\": 17, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-018.webp\", \"caption\": \"\", \"page\": 21, \"index\": 18, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-019.webp\", \"caption\": \"\", \"page\": 21, \"index\": 19, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-020.webp\", \"caption\": \"\", \"page\": 21, \"index\": 20, \"width\": 6462, \"height\": 2706}]"
motivation: 确保推理过程忠实性对可靠监控和控制至关重要。
method: 提出反事实干预框架评估草稿内和草稿-答案忠实性。
result: 该框架有效检测了推理步骤的不忠实行为。
conclusion: 反事实干预是评估推理忠实性的有力工具。
---

## Abstract
Large Reasoning Models (LRMs) have significantly enhanced their capabilities in complex problem-solving by introducing a thinking draft that enables multi-path Chain-of-Thought explorations before producing final answers. 
Ensuring the faithfulness of these intermediate reasoning processes is crucial for reliable monitoring, interpretation, and effective control.   In this paper, we propose a systematic counterfactual intervention framework to rigorously evaluate *thinking draft faithfulness*. 
Our approach focuses on two complementary dimensions:
**(1) Intra-Draft Faithfulness**, which assesses whether individual reasoning steps causally influence subsequent steps and the final draft conclusion through counterfactual step insertions; and
**(2) Draft-to-Answer Faithfulness**, which evaluates whether final answers are logically consistent with and dependent on the thinking draft, by perturbing the draft’s concluding logic.
We conduct extensive experiments across six state-of-the-art LRMs. 
Our findings show that current LRMs demonstrate selective faithfulness to intermediate reasoning steps and frequently fail to faithfully align with the draft conclusions.
These results underscore the need for more faithful and interpretable reasoning in advanced LRMs.

---

## 论文详细总结（自动生成）

# 论文《Measuring the Faithfulness of Thinking Drafts in Large Reasoning Models》详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：大型推理模型（LRM）在生成最终答案前会输出一段中间的“思考草稿”（thinking draft），但当前缺乏方法判断这些草稿是否**忠实**——即中间推理步骤是否真实地、因果性地影响最终答案。
- **动机**：确保推理过程忠实性对于模型的**可靠监控**（如用弱模型检视强模型推理）和**有效控制**（如通过插入思考内容来引导行为）至关重要。现有工作多聚焦于输入层面的干预（如在提示中插入暗示），未评估思考草稿内部的因果依赖性以及最终答案对草稿结论的真实依赖，可能导致“忠实性幻觉”。
- **背景**：LRM（如OpenAI o1、DeepSeek R1）采用分两阶段生成：先产生非线性的思考草稿（含回溯、自我反思），再合成最终答案。这种解耦使得评估草稿的忠实性成为关键安全与可解释性问题。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**反事实干预框架**，系统操纵思考草稿中的特定内容，观察模型后续行为是否与干预因果一致，从而衡量忠实性。框架包含两个互补维度：
  - **草稿内忠实性**（Intra-Draft Faithfulness）：衡量单个推理步骤是否因果性地影响后续步骤及草稿最终结论。
  - **草稿到答案忠实性**（Draft-to-Answer Faithfulness）：衡量最终答案是否严格基于思考草稿的结论。

- **关键技术细节**：
  - **步骤分解与类型标注**：使用GPT-4O-MINI将草稿分解为步骤，并标注为`CONTINUE`（普通前向推理）或`BACKTRACK`（回溯、修正）。
  - **草稿内忠实性评估**：
    - 在草稿的特定位置（初始、中部、末尾）插入一个**反事实步骤**（有两种类型：选项标签混淆“Shift Mapping”或选项内容破坏“Corrupt Option”），该步骤本身可以是错误`CONTINUE`或`BACKTRACK`。
    - 模型继续生成后续草稿。外部分类器（Qwen2.5-Instruct-32B）将模型行为归类为**显式纠正**（Explicit Correction）或**步骤跟随**（Step Following）。
    - 忠实性指标`δ_Intra`：
      - 若为纠正：要求模型最终结论与原始结论一致（`ANS(T') = ANS(T)`）。
      - 若为跟随：要求模型结论按干预逻辑相应改变（`ANS(T') = φ(ANS(T))`，φ为干预映射函数）。
  - **草稿到答案忠实性评估**：
    - 修改草稿结论（两种方式：**直接替换**，插入一个明确但无支持的结论；**合理替换**，使用GPT-4O-MINI生成逻辑自洽的错误理由）。
    - 测量两个指标：
      - **草稿依赖**（Draft Reliance）`δ_reliance`：比较标准回答（带答案阶段解释）与即时回答（无答案阶段解释）下答案是否一致——高依赖意味着答案阶段不引入新推理。
      - **草稿答案一致性**（Draft-Answer Consistency）`δ_consistency`：最终答案是否与草稿结论一致。
    - 在两种条件下计算：标准回答（自由生成答案阶段解释）与即时回答（强制直接输出答案）。

## 3. 实验设计
- **数据集**：
  - 主要数据集：**GPQA Diamond**（198 道研究生级推理选择题）和 **MMLU**（global facts 子集，88 道事实回忆题）。
  - 额外评估：**MMLU College Math** 子集（数学推理）。
- **思考草稿来源**：
  - **外部基准草稿**：由 DeepSeek-R1（使用默认采样）和 Qwen3-32B（贪心解码）生成。
  - **自生成草稿**：由被评估模型自身输出（贪心解码，温度=0）。
- **评估模型**（6 个主要模型 + 3 个额外模型）：
  - 蒸馏系列：DeepSeek-R1-Distill-Llama-8B、DeepSeek-R1-Distill-Qwen-7B、14B、32B（R1-7B/8B/14B/32B）。
  - 强化学习奖励（RLVR）后训练系列：QWQ-32B、Skywork-OR1-32B-Preview。
  - 额外模型：R1-1.5B、OR1-7B、Qwen3-14B。
- **对比方法**：论文未直接对比其他方法，而是通过改变步骤类型、位置、模型规模、后训练方法、任务难度、草稿来源等因素进行自对比分析。同时与反事实可模拟性方法（Chen et al., 2025）进行了结果比较。

## 4. 资源与算力
- 论文**未明确说明**使用的 GPU 型号、数量或训练时长。实验中所有模型均以**贪心解码**（温度=0）运行以确保可复现性，DeepSeek-R1 基准草稿通过 API 获取（温度=0.6）。算力细节未被披露。

## 5. 实验数量与充分性
- **实验数量**：
  - 草稿内忠实性：12 种场景/草稿（2 种干预类型 × 2 种步骤类型 × 3 种插入位置），评估 6 个主要模型，共约 12 × 198（GPQA）+ 12 × 88（MMLU）个条件，每个条件还比较不同草稿来源。
  - 草稿到答案忠实性：2 种结论修改方式 × 2 种回答条件（标准/即时），同样跨模型和数据集。
  - 额外实验：人类标注一致性检验（Cohen's κ）、跨模型分类器稳定性、条件困惑度分析、与反事实可模拟性方法对比、MMLU College Math 子集验证、额外模型评估。
- **充分性与公平性**：
  - **优点**：实验设计系统，覆盖多个因素；使用外部基准草稿和自生成草稿双重验证；进行人工校对和多重 LLM 分类器交叉验证；消融分析（位置、行为类型）细致。
  - **不足**：GPQA 和 MMLU 样本量较小（198 和 88），可能影响统计显著性；干预类型限于选项操作，未覆盖所有误推理场景；所有实验基于贪心解码，未测试采样温度变化的影响。

## 6. 主要结论与发现
- **草稿内忠实性**：
  - 模型对 **BACKTRACK 步骤**比 CONTINUE 步骤更忠实，因为回溯可能作为“注意重置”信号。
  - **显式纠正行为**比步骤跟随更忠实，能更可靠地恢复原始推理。
  - **位置效应**：早期步骤跟随影响结论更大，而后期显式纠正更有效。
  - **更大模型**（如 R1-32B）忠实性更高；**RLVR 调优**（QWQ、OR1）对草稿内忠实性无明显提升，表明该能力更与内在容量相关。
  - **简单任务**（MMLU）比复杂任务（GPQA）忠实性更高；草稿来源影响很小（差异 < 10%）。
- **草稿到答案忠实性**：
  - **答案阶段经常引入新推理**，并非简单重述草稿——所有模型（除 QWQ）在 GPQA 上有约 30% 的答案改变。
  - **即时回答（无答案阶段解释）** 显著提高草稿-答案一致性，说明答案阶段解释本身可能偏离草稿。
  - **RLVR 调优模型**（QWQ、OR1）草稿-答案一致性最低（如 GPQA 上仅 19.54% 和 29.94%），说明 RLVR 增强了答案阶段的独立计算，降低了对外部草稿导引的敏感性。
  - **更大模型**更偏好逻辑合理的结论修改，小模型则更服从直接明确的声明。
  - **简单任务**上蒸馏模型的草稿依赖和一致性更高。
- **总体**：当前 LRM 表现出**选择性忠实性**，对回溯步骤较诚实，但答案阶段常常偏离草稿结论，削弱了草稿的监控和可控价值。

## 7. 优点（方法与实验设计亮点）
- **首创性**：首次系统定义并量化思考草稿的忠实性，包含草稿内和草稿到答案两个正交维度，填补了现有研究空白。
- **方法论健壮性**：反事实干预设计经过精心构造（确保可传播、有意义、可验证），且采用外部 LLM 分类器与人类标注交叉验证（Cohen κ 达 0.83 和 0.62），结果可信。
- **广泛基准**：覆盖多种模型族（蒸馏 vs. RLVR）、规模（1.5B–32B）、任务类型（复杂推理 vs. 事实回忆），并考虑自身草稿与外部草稿，结论具有较高泛化性。
- **深入分析**：系统探究了步骤类型、行为类型、位置、模型大小、后训练方法、任务难度等因素的影响，提供了细粒度洞察。
- **补充比较**：与反事实可模拟性方法对比，展示了两类指标反映不同忠实性侧面（言语化 vs. 因果依赖），提供了综合评价视角。

## 8. 不足与局限
- **干预覆盖有限**：草稿内忠实性仅测试了选项混淆和内容破坏两类反事实场景，未涵盖更丰富的误推理类型（如数字计算错误、逻辑跳跃）。
- **样本量较小**：GPQA（198）和 MMLU 子集（88）规模有限，可能影响统计稳定性，通用性需更大规模验证。
- **算力与可复现性**：未报告算力消耗，且依赖 GPT-4O-MINI（商业 API）和外部草稿提供方（DeepSeek API），部分实验不完全可复现。
- **忠实性与正确性的张力**：论文承认偏离草稿有时可提升最终准确性（如草稿错误但答案正确），但坚持忠实性为安全前提——这一权衡未深入讨论。
- **RLVR 模型解释有限**：发现 RLVR 调优降低草稿依赖性，但未深入分析其机制（如是否因为奖励信号强化了内部答案计算）。
- **评估外推性**：所有模型均基于开源系列，未包括 GPT-4、Claude 等闭源模型；且干预均基于文本层面，未涉及模型内部隐藏状态的真实因果分析，可能存在“忠实性假象”。

（完）
