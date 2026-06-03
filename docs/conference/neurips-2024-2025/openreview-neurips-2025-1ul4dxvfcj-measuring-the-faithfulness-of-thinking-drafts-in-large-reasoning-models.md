---
title: Measuring the Faithfulness of Thinking Drafts in Large Reasoning Models
title_zh: 测量大型推理模型中思考草稿的忠实度
authors: "Zidi Xiong, Shan Chen, Zhenting Qi, Himabindu Lakkaraju"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=1UL4dxvfcJ"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 直接测量大型推理模型中思考草稿的忠实度
tldr: 提出反事实干预框架评估大型推理模型思考草稿的忠实度。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 7688, \"height\": 2772}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-002.webp\", \"caption\": \"\", \"page\": 5, \"index\": 2, \"width\": 5256, \"height\": 2172}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 5252, \"height\": 2172}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 5256, \"height\": 2176}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-009.webp\", \"caption\": \"\", \"page\": 19, \"index\": 9, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-010.webp\", \"caption\": \"\", \"page\": 19, \"index\": 10, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-011.webp\", \"caption\": \"\", \"page\": 19, \"index\": 11, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-012.webp\", \"caption\": \"\", \"page\": 19, \"index\": 12, \"width\": 3343, \"height\": 2143}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-013.webp\", \"caption\": \"\", \"page\": 20, \"index\": 13, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-014.webp\", \"caption\": \"\", \"page\": 20, \"index\": 14, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-015.webp\", \"caption\": \"\", \"page\": 20, \"index\": 15, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-016.webp\", \"caption\": \"\", \"page\": 20, \"index\": 16, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-017.webp\", \"caption\": \"\", \"page\": 21, \"index\": 17, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-018.webp\", \"caption\": \"\", \"page\": 21, \"index\": 18, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-019.webp\", \"caption\": \"\", \"page\": 21, \"index\": 19, \"width\": 6462, \"height\": 2706}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-1ul4dxvfcj/fig-020.webp\", \"caption\": \"\", \"page\": 21, \"index\": 20, \"width\": 6462, \"height\": 2706}]"
motivation: 确保中间推理过程的忠实性对可靠监控、解释和控制至关重要。
method: 提出系统的反事实干预框架，评估草稿内和草稿间两个维度的忠实度。
result: 能够有效检测推理步骤中的合谋和误导性草稿。
conclusion: 该框架为评估大型推理模型的推理忠实度提供了系统方法。
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

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大型推理模型（LRM，如OpenAI o1/o3、DeepSeek R1、Claude 3.7 Sonnet等）通过引入中间“思考草稿”（thinking draft）进行多路径链式推理，但**这些中间推理步骤是否忠实反映了模型最终的决策过程**？即这些步骤是否真正因果地驱动了最终答案，而非事后合理化？
- **研究动机**：确保思考草稿的忠实性对于可靠监控（如用弱模型检查强模型推理）、有效控制（如人为插入思考内容）及可解释性至关重要。现有输入级操纵方法（如插入提示或调整选项顺序）虽能测量提示层面的影响，但无法评估草稿内部推理步骤的因果角色，可能造成“忠实性幻觉”。
- **研究意义**：首次针对LRM提出系统性的思考草稿忠实度评价框架，为实际监控和干预提供可靠基础。

## 2. 论文提出的方法论
### 核心思想
- 提出**反事实干预框架**，通过可控编辑思考草稿，观察模型行为变化，从而测量两个互补维度的忠实度：
  1. **草稿内忠实性（Intra-Draft Faithfulness）**：单个推理步骤是否因果性地影响后续步骤及最终草稿结论。
  2. **草稿到答案忠实性（Draft-to-Answer Faithfulness）**：最终答案是否逻辑一致且因果地依赖于思考草稿。

### 关键技术细节
- **草稿内忠实性**：
  - 使用GPT-4O-MINI将草稿分解为步骤，并标注类型：`CONTINUE`（前向推理）或`BACKTRACK`（回溯/修正）。
  - 在草稿不同位置（初始/中间/结束块）插入反事实步骤（两类干预：`Shift Mapping`重新映射选项标签；`Corrupt Option`污染选中选项）。插入步骤本身也分为“错误继续”或“回溯修正”。
  - 用Qwen2.5-32B-Instuct分类模型后续行为：**显式纠正**（检测并拒绝误导）或**步骤跟随**（无检测地采纳）。忠实度定义为：
    - 若纠正：最终结论不变。
    - 若跟随：结论按干预映射函数\phi变化。
  - 公式略，但核心思想是观测结论是否按预期被修改或保留。

- **草稿到答案忠实性**：
  - 对草稿最终结论进行两种反事实编辑：
    - **直接交替**：直接插入与正确结论相反的显式声明。
    - **合理交替**：用GPT-4O-MINI生成有逻辑性的替代结论。
  - 两个指标：
    - **草稿依赖度**：比较标准回答（带答案阶段推理）和立即回答（禁止额外推理）的答案一致性。
    - **草稿-答案一致性**：最终答案是否与编辑后草稿的显式结论一致。

## 3. 实验设计
### 数据集与场景
- **主要数据集**：
  - **GPQA Diamond**（198题，高难度推理）
  - **MMLU global facts子集**（88题，事实回忆类）
  - 额外验证：**MMLU College Math**（数学推理）
- **基准来源**：使用DeepSeek-R1和Qwen3-32B生成的思考草稿作为外部基准，同时使用模型自身生成的草稿。

### 对比模型
- **六大主模型**：
  - DeepSeek-R1-Distill系列：Llama-8B、Qwen-7B、14B、32B
  - RLVR后训练模型：QwQ-32B、Skywork-OR1-32B-Preview
- **额外评测模型**（附录）：DeepSeek-R1-Distill-1.5B、Skywork-OR1-7B、Qwen3-14B

### 实验类型
- **草稿内忠实性**：4种干预（Shift/Corrupt × CONTINUE/BACKTRACK）× 3个插入位置 = 12种场景/草稿。
- **草稿到答案忠实性**：2种结论修改（直接/合理）× 2种回答方式（标准/立即）。
- **控制变量**：模型体积、后训练方式（蒸馏 vs RLVR）、任务难度、草稿来源（外部/自生成）。

## 4. 资源与算力
- **文中未明确说明使用的GPU型号、数量或训练时长**。所有推理实验均使用贪婪解码（温度=0）以保证可复现性；DeepSeek-R1生成基准草稿时使用API默认采样（温度0.6）。评估过程使用外部API（GPT-4O-MINI）和本地模型（Qwen2.5-32B-Instuct）进行标注，但未公开具体计算开销。

## 5. 实验数量与充分性
- **实验总量**：在两大主数据集（GPQA/MMLU）上对6个主模型进行12种草稿内干预+2种草稿到答案干预，并包含自生成对比。额外在MMLU College Math和另3个小模型进行扩展验证。
- **消融覆盖**：步骤类型（继续/回溯）、插入位置（首/中/尾）、干预方式（映射/污染）、草稿来源（自生成/基准）。
- **可靠性校验**：由三名人类标注员对200条轨迹进行步骤分解和行为分类，Cohen's κ分别为0.83（几乎完美一致）和0.62（实质一致）。多LLM法官间一致率≥81%。
- **结论**：实验较为充分，考虑了多种影响因素，且有人类验证支持，客观性和公平性较好。

## 6. 论文的主要结论与发现
- **草稿内忠实性**：
  - 模型对**BACKTRACK步骤**更忠实（忠实率显著高于CONTINUE步骤）。
  - **显式纠正行为**的忠实率更高，表明模型能通过消除错误来保持路径连贯。
  - **位置效应**：早期步骤跟随影响结论更显著；后期纠正更有效。
  - 较大模型（如R1-32B）草稿内忠实度更高；RLVR训练无明显增益。
  - 简单任务（MMLU）忠实度高于复杂任务（GPQA）。
- **草稿到答案忠实性**：
  - **答案阶段引入了新推理**，而非简单总结草稿：标准回答与立即回答不一致率约30%（GPQA）。
  - **立即回答**更忠实于草稿结论（一致性更高），说明答案阶段会偏离草稿。
  - 较大模型更依赖**逻辑合理的结论**，小模型更依赖显式声明。
  - RLVR模型（QwQ、OR1）**草稿-答案一致性最低**，说明RLVR使模型更“坚信”内部计算而非草稿指导。
- **跨来源稳定性**：草稿依赖度跨自生成与基准稳定；但草稿-答案一致性波动较大，尤其对RLVR模型。

## 7. 优点
- **方法设计新颖**：首次针对LRM中间思考草稿提出因果性忠实度评估，区分草稿内和草稿到答案两个维度，填补现有研究空白。
- **反事实干预精心构造**：使用全局依赖的步骤（选项映射/污染）确保任何忠实推理都应受影响，避免细粒度依赖难以追踪的问题。
- **实验覆盖广泛**：涵盖多种模型规模、后训练策略、任务难度，并同时使用外部和自生成草稿，确保结果鲁棒。
- **人类验证+多重LLM法官**：保证标注可靠性，消除单一语言模型偏差。
- **分析深入**：不仅报告平均忠实率，还分解到步骤类型、行为类型、位置、模型因素，提供可解释的insights。

## 8. 不足与局限
- **评估任务局限**：主要依赖GPQA和MMLU（推理和事实类），未覆盖更开放式或创造性推理场景。数学推理（MMLU College Math）仅作为附加验证，不具备完整覆盖。
- **依赖外部LLM标注**：步骤分解、行为分类、反事实生成均依赖GPT-4O-MINI或Qwen2.5-32B，自身也有忠实性问题；虽有人类验证，但不能完全消除偏差。
- **反事实干预的生态效度**：插入的步骤（如选项表重映射）在实际监控中很少发生，是否能推广到自然错误仍需验证。
- **计算资源未报告**：无法评估实验的可重复成本，可能阻碍他人复现。
- **未探索因果机制**：忠实性测量仅停留在行为层面，未深入分析模型内部表示变化。
- **实际应用缺口**：论文指出低忠实度影响监控/控制，但未提供实际监控或控制实验来验证这些下游影响，停留在指标定义阶段。
- **对RLVR模型的解读有限**：RLVR模型忠实度低是否代表“更强大的内部推理”还是“不可控的偏差”？论文仅指出现象，未开展进一步归因。

（完）
