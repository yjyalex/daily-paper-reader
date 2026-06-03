---
title: "MR-Ben: A Meta-Reasoning Benchmark for Evaluating System-2 Thinking in LLMs"
title_zh: MR-Ben：评估LLM系统二思维的元推理基准
authors: "Zhongshen Zeng, Yinhong Liu, Yingjia Wan, Jingyao Li, Pengguang Chen, Jianbo Dai, Yuxuan Yao, Rongwu Xu, Zehan Qi, Wanru Zhao, Linling Shen, Jianqiao Lu, Haochen Tan, Yukang Chen, Hao Zhang, Zhan Shi, Bailin Wang, Zhijiang Guo, Jiaya Jia"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=GN2qbxZlni"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 基于过程的CoT推理忠实性评估基准
tldr: 提出MR-Ben，通过检测CoT步骤中的错误来评估LLM的元推理，直接评估忠实性。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-004.webp\", \"caption\": \"\", \"page\": 3, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-005.webp\", \"caption\": \"\", \"page\": 3, \"index\": 5, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-006.webp\", \"caption\": \"\", \"page\": 22, \"index\": 6, \"width\": 2491, \"height\": 900}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-007.webp\", \"caption\": \"\", \"page\": 24, \"index\": 7, \"width\": 568, \"height\": 384}]"
motivation: 现有结果基准饱和，需要过程级基准来评估LLM的推理忠实性。
method: 构建过程级基准，要求LLM定位和分析自动生成推理步骤中的潜在错误。
result: MR-Ben能有效区分有能力和无能力的模型，揭示推理忠实性差异。
conclusion: 过程级评估是衡量CoT推理忠实性的有效方法。
---

## Abstract
Large language models (LLMs) have shown increasing capability in problem-solving and decision-making, largely based on the step-by-step chain-of-thought reasoning processes. However, evaluating these reasoning abilities has become increasingly challenging. Existing outcome-based benchmarks are beginning to saturate, becoming less effective in tracking meaningful progress. To address this, we present a process-based benchmark MR-Ben that demands a meta-reasoning skill, where LMs are asked to locate and analyse potential errors in automatically generated reasoning steps. Our meta-reasoning paradigm is especially suited for system-2 slow thinking, mirroring the human cognitive process of carefully examining assumptions, conditions, calculations, and logic to identify mistakes. MR-Ben comprises 5,975 questions curated by human experts across a wide range of subjects, including physics, chemistry, logic, coding, and more. Through our designed metrics for assessing meta-reasoning on this benchmark, we identify interesting limitations and weaknesses of current LLMs (open-source and closed-source models). For example, with models like the o1 series from OpenAI demonstrating strong performance by effectively scrutinizing the solution space, many other state-of-the-art models fall significantly behind on MR-Ben, exposing potential shortcomings in their training strategies and inference methodologies.

---

## 论文详细总结（自动生成）

# MR-Ben：评估LLM系统二思维的元推理基准 - 详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：现有的大语言模型（LLM）推理评估主要基于最终结果（如GSM8K、MATH），这些基准正在饱和，难以有效追踪模型在推理过程质量上的进步。许多模型即使给出正确最终答案，推理过程中可能包含逻辑错误、计算错误或不必要步骤，结果基准无法捕捉这些深层次问题。
- **核心问题**：如何设计一个过程导向的评估基准，衡量LLM在推理步骤中定位并分析错误的能力，从而反映其**系统二思维**（慢思考、仔细审视假设、条件、计算和逻辑）的成熟度。
- **整体含义**：提出MR-Ben，一个基于**元推理**（meta-reasoning）的基准，要求LLM扮演“教师”角色，评估自动生成的CoT解答，判断正确性、指出第一个错误步骤并解释原因。这直接评估模型对自身推理过程的监控与修正能力。

## 2. 论文提出的方法论

### 核心思想
- 采用**元推理范式**：模型不仅需要解决问题，还要分析他人（或其他模型）的推理过程，识别其中的错误并给出修正。这迫使模型进行更深入的递归推理，模仿人审视推理链条的过程。
- 每个数据点包含三个要素：**问题**、**CoT解答**（由GPT-3.5、Claude2、Mistral-Medium生成）、**错误分析**（由人类专家标注，包括错误步骤编号、错误原因、修正步骤）。

### 关键技术细节
1. **数据来源与构建**：
   - 问题来自MMLU（数学、物理、化学、生物、医学）、LogiQA（逻辑）、MHPP（编码），共5975个问题，覆盖高中到专业水平。
   - 使用LLM生成CoT解答（温度为1，要求分步输出），然后由专业标注团队完成错误标注。

2. **标注流程**（三阶段）：
   - **答案正确性**：自动匹配最终答案+手动检查推理过程是否错误（即使答案正确但推理错误也判定为错误）。
   - **错误步骤定位**：将每个步骤分为正确、中性、错误，标记第一个错误步骤。
   - **错误原因与修正**：提供详细错误分析和修正后的步骤。

3. **评估指标**：**MR-Score**，由三个子指标加权组合：
   - **MCC**（Matthews相关系数）：衡量整体解正确性分类性能。
   - **ACC_step**：在错误解答中正确识别第一个错误步骤的比例。
   - **ACC_reason**：在错误解答中同时正确识别错误步骤和错误原因的比例。
   - 权重w1、w2、w3通过实验优化选择，以最大化模型区分度。

4. **错误原因自动评分**：使用GPT-4-Turbo作为代理评估模型，与人类标注的92%一致率。

### 流程说明（文字描述）
- 数据集构建：问题收集 → 多LLM生成CoT解答 → 人工标注错误（正确性、第一错误步、原因、修正）。
- 模型评估：输入问题+CoT解答 → 模型输出“解分析、解正确性、第一错误步、错误原因” → 自动计算MCC、ACC_step、ACC_reason → 加权得MR-Score。

## 3. 实验设计

- **数据集/基准**：MR-Ben（5975题，7个科目：数学、医学、生物、物理、化学、逻辑、编码）。
- **评估设置**：
  - 模型：涵盖闭源（GPT-3.5、GPT-4、Claude3、Mistral-Large、Yi-Large、Moonshot、o1-preview等）和开源（Gemma-2B、Phi-3、Llama3-8B/70B、Qwen1.5/2、DeepSeek系列等）。
  - 配置：主要报告0-shot和1-shot（一步式CoT示例）结果；还探索了few-shot、self-refine、解正确性先验等设置。
  - 提示方法：使用逐步CoT提示引导模型逐步分析，确保格式一致性。

- **对比方法**：主要对比不同规模和来源的模型在MR-Score上的表现，没有额外基线模型（因为MR-Ben是全新基准）。

- **消融/附加实验**：
  - **Few-shot提示**：测试不同shot数（0-3）的效果。
  - **Self-refine提示**：让模型自我验证和修正答案。
  - **解正确性先验**：在提示中告知模型解是错误的，观察性能变化。

## 4. 资源与算力

- 论文在附录G提及：本地推理使用**8×A800 GPU**的机器，对70B模型运行整个6000基准约需2小时（使用vLLM等加速库）。更小模型更快。
- 未详细说明所有实验的总GPU小时、训练时间等，因为实验均为推理测试，无训练过程。对于闭源模型通过API调用，算力消耗未量化。

## 5. 实验数量与充分性

- **主要实验**：表2报告了约20种模型在7个科目上的MR-Score（0-shot和1-shot），共约280个数据点（模型×科目×shot）。
- **附加实验**：
  - 表3：解正确性先验对比（4种模型，各100题）。
  - 表4：self-refine对比（4种模型）。
  - 表9（附录）：few-shot详细结果（4种模型×4 shot数）。
  - 附录B：错误理由评分鲁棒性分析（3个评分模型）。
- **充分性评价**：
  - 模型覆盖全面，包括大小、开源/闭源、专门训练（如DeepSeek-Coder）等。
  - 实验设计公平：所有模型使用相同提示模板，温度设为0（贪婪解码），确保结果可重复。
  - 消融实验探讨了提示策略、先验知识等影响因素，进一步验证基准的敏感性。
  - 局限：未报告多次运行的标准差（解码确定），但使用贪婪解码通常无需多次运行。总体实验设计客观、充分。

## 6. 论文的主要结论与发现

1. **o1-preview表现最佳**，在所有科目上MR-Score大幅领先，体现系统二思考（搜索、反思、解空间探索）的关键作用。
2. **大多数LLM在元推理上存在显著不足**：很多模型能正确解题，但无法识别自身或他人推理中的错误；尤其对数学和逻辑推理，错误步骤定位和原因解释能力薄弱。
3. **模型大小与性能正相关，但不绝对**：Phi-3（3.8B）凭借高质量合成数据训练，超越许多更大模型（如DeepSeek-Coder-33B）。o1模型通过test-time compute scaling显著提升。
4. **专门预训练不保证推理优势**：DeepSeek-Coder在编码任务上预训练，但其算法推理表现并不比通用模型突出。
5. **不同模型擅长不同推理范式**：知识、算术、算法、逻辑推理各有侧重，挑战了“领域增强必然带来全面提升”的假设。
6. **自优化效果有限**：self-refine对小型模型有害，对GPT-4等大型模型帮助甚微，且存在不一致性。

## 7. 优点

- **创新范式**：元推理直接评估推理过程，而不是最终答案，能揭示深层认知缺陷，与系统二思维紧密对应。
- **科目覆盖广泛**：涵盖自然科学、编码、逻辑等7个领域，难度从高中到专业级，比之前的工作（如MR-GSM8K、BIG-Bench Mistake）规模更大、领域更广。
- **数据质量严格**：三轮质量控制（双标注、二次审核、10%随机抽查），错误分析提供详细的步骤原因和修正，便于深入分析。
- **评估指标合理**：MR-Score综合解正确性、错误定位、错误解释三个层次，区分度好；利用GPT-4自动评分错误理由，效率高且与人类一致率高（92%）。
- **实验设计系统**：对比大量模型，考虑不同shot、self-refine、先验知识等设置，结论稳健。

## 8. 不足与局限

- **领域限制**：主要适用于自然语言可分解的学科，对于创造性、综合性任务（如写作、社会学）难以应用，因为无法精确分解为有序推理步骤。
- **语言限制**：仅包含英文问题，可能无法泛化到其他语言中的推理模式。
- **解答来源偏差**：错误解答仅来自GPT-3.5、Claude2、Mistral-Medium三个模型，可能未覆盖所有可能的错误模式。不同模型或人类会展示不同错误类型，基准的多样性有限。
- **评估依赖GPT-4**：错误原因评分使用GPT-4作为代理，尽管一致率高，但仍存在不可忽视的误差，尤其在复杂或模糊的案例中。
- **未报告统计显著性**：所有实验采用贪婪解码（温度=0），但未提供多次运行的结果或置信区间，可能无法完全反映性能的稳定性。
- **计算资源描述不完整**：仅简要提及使用A800 GPU，未详细说明每个实验的具体耗时、总GPU小时，影响可重复性评估。

（完）
