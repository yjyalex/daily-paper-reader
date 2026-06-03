---
title: "Cognitive Mirrors: Exploring the Diverse Functional Roles of Attention Heads in LLM Reasoning"
title_zh: 认知镜像：探索注意力头在LLM推理中的多样功能角色
authors: "Xueqi Ma, Jun Wang, Yanbei Jiang, Sarah Monazam Erfani, Tongliang Liu, James Bailey"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=EBwfFrw5VA"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 使用思维链设计分解复杂问题以分析推理
tldr: 通过思维链分解数据集探测注意力头在推理中的角色
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-ebwffrw5va/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1024, \"height\": 1024}]"
motivation: LLM内部机制不透明，理解其推理行为至关重要。
method: 构建CogQA数据集并用多标签探测分析注意力头。
result: 识别出不同注意力头分别负责检索、逻辑推理等不同功能。
conclusion: 注意力头在推理中具有可区分的认知功能。
---

## Abstract
Large language models (LLMs) have achieved state-of-the-art performance in a variety of tasks, but remain largely opaque in terms of their internal mechanisms. Understanding these mechanisms is crucial to improve their reasoning abilities. Drawing inspiration from the interplay between neural processes and human cognition, we propose a novel interpretability framework to systematically analyze the roles and behaviors of attention heads, which are key components of LLMs. We introduce CogQA, a dataset that decomposes complex questions into step-by-step subquestions with a chain-of-thought design, each associated with specific cognitive functions such as retrieval or logical reasoning. By applying a multi-label probing method, we identify the attention heads responsible for these functions. Our analysis across multiple LLM families reveals that attention heads exhibit functional specialization, characterized as cognitive heads. These cognitive heads exhibit several key properties: they are universally sparse, and vary in number and distribution across different cognitive functions, and they display interactive and hierarchical structures.  We further show that cognitive heads play a vital role in reasoning tasks—removing them leads to performance degradation, while augmenting them enhances reasoning accuracy. These insights offer a deeper understanding of LLM reasoning and suggest important implications for model design, training and fine-tuning strategies.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

**研究动机**：大型语言模型（LLM）虽然在下游任务中表现出色，但其内部推理机制仍然“黑箱化”，缺乏透明理解。现有研究表明人类大脑在解决复杂推理任务时会激活多个专门化脑区（如前额叶负责检索、Broca区负责语义处理、顶叶负责逻辑推理），而LLM中多头注意力机制也可能存在类似的功能分工。然而，先前工作多聚焦于简单任务或单一功能，缺乏在复杂多步推理场景下对注意力头功能专门化的系统性分析。

**整体含义**：本文旨在探索LLM中的注意力头是否呈现出与人类认知功能相对应的专门化角色，并揭示其稀疏性、层次性等特性，从而为模型可解释性、训练策略优化提供理论基础。

## 2. 论文提出的方法论：核心思想、关键技术细节

**核心思想**：借鉴认知科学，将复杂推理任务分解为多步子问题，每个子问题对应一种认知功能；通过探测模型内部注意力头激活与认知功能标签的对应关系，识别出执行特定认知功能的“认知头”。

**关键技术细节**：

1. **数据集构建（CogQA）**：
   - 从AQuA、CREAK、ECQA、e-SNLI、GSM8K各取150个问题，共750个样本。
   - 使用GPT-4o以Chain-of-Thought方式将每个主问题分解为子问题-答案-认知功能三元组（subQAC）。
   - 经过两阶段人工验证（逻辑合理性、功能标签正确性）和自动答案校验（o4-mini模型+人工裁决），最终保留570个主问题、3402个子问题。

2. **认知功能分类**：低阶功能（检索、知识回忆、语义理解、句法理解）和高阶功能（数学计算、逻辑推理、推断、决策）。

3. **功能探测方法**：
   - **特征提取**：对每个子问题，模型生成答案时提取所有注意力头的输出值，取前k个语义重要token（由o4-mini挑选）的平均激活作为头级特征，并拼接层平均特征。
   - **多类分类器**：使用两层MLP（线性投影→64维→512维隐层→输出8类）训练，以交叉熵损失函数，Adam优化器，学习率1e-4，100轮。
   - **重要性计算**：对每个功能类c，使用梯度×激活方法计算每个注意头的贡献得分：\( I(c)_j = \mathbb{E} \left[ \frac{\partial \hat{y}_c}{\partial \bar{x}_j} \cdot \bar{x}_j \right] \)。重要性得分高的头被认定为该功能的认知头。

4. **干预验证**：
   - **负干预**：将认知头输出乘以小因子ϵ（如0.001）进行掩码。
   - **正干预**：基于正确/错误样本的激活差异计算方向dir，将头激活沿该方向偏移：\( x_{hl}(i) \leftarrow x_{hl}(i) + \alpha \sigma_{hl} dir_{hl} \)。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - 主数据集：CogQA（570主问题，3402子问题），用于训练探测分类器和识别认知头。
  - 下游任务：GSM8K_100（100个数学题）和Extractive_QA（49个检索题，答案从原文抽取）。

- **基准与对比方法**：
  - **负干预基线**：掩码相同数量的随机头（random heads）与掩码认知头（cognitive heads）比较。
  - **正干预基线**：不进行干预的原始模型（Base）。
  - **跨功能干预**：掩码retrieval头测知识回忆，掩码知识回忆头测检索，验证专门性。
  - **层级结构实验**：掩码低阶功能头（如检索、语法理解）后测量对高阶功能（如数学、决策）的影响。

- **评估指标**：准确率（BLEU>0.8或ROUGE/语义相似度>0.6视为正确）、COMET分数（用于生成质量评估）。

## 4. 资源与算力

论文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提到实验在LLaMA、Qwen、Yi等多个模型上进行，且所有实验均为推理和微调（训练探测分类器），但未提供具体计算资源细节。

## 5. 实验数量与充分性

**实验组数**约10组，包括：
- 主干预实验（表1）：6个模型×8个功能×2种掩码条件（认知头vs随机头）×2个指标（comet, acc）→ 大量数据点。
- 跨功能掩码（表2）：2个模型×2个功能组合。
- 不同掩码数量曲线（图3）：4个功能×4种数量×2种指标。
- 层级结构（表3）：4个条件×5个功能。
- 正负干预（表4）：2个任务×6个模型×多种条件。
- 消融实验（附录A.9）：不同token位置选择。
- PCA聚类可视化（图4）：3个模型。

**充分性评价**：
- 覆盖了3大模型家族（LLaMA, Qwen, Yi）共6个不同规模模型，具有较好代表性。
- 对比了随机掩码基线，实验设计公平。
- 使用了多种评估指标（BLEU, ROUGE, COMET, 语义相似度），减少单一指标偏差。
- 消融实验验证了特征提取策略（token选择）的影响。
- **不足**：未在更多主流模型（如GPT-4、Gemini等）上验证，也未比较其他可解释性方法（如注意力可视化、causal tracing）。

## 6. 论文的主要结论与发现

1. **认知头的存在与稀疏性**：每个认知功能仅激活少量高重要性注意力头（<7%），不同功能激活头数差异显著（如推断需139个，数学仅需59个）。
2. **普遍性与跨模型一致性**：该现象在多个LLM家族和规模中普遍存在，且相似模型结构内分布模式相似。
3. **层次功能组织**：低阶功能（如检索）头多分布在中间层，高阶功能（如数学）头集中在高层；低阶头受损会显著影响高阶任务性能。
4. **功能聚类**：PCA显示数学、推理、决策头聚为一类，低阶功能（检索、语法）形成另一类，反映模块化组织。
5. **因果关系验证**：掩码认知头导致对应功能性能大幅下降（甚至降至零），而随机掩码几乎无影响；增强认知头激活可提升下游任务准确率。

## 7. 优点：方法或实验设计上的亮点

- **数据集设计精良**：CogQA通过多阶段人工验证保证质量，将复杂推理分解为单功能子问题，为细粒度分析提供基础。
- **方法论创新**：将人类认知功能分类引入LLM分析，并采用多类探测方法同时分析多个功能，克服了以往单类分析的限制。
- **实验全面且系统**：涵盖正负干预、跨功能影响、层级结构、聚类分析、消融实验，从多种角度验证认知头的功能意义。
- **开源可复现**：提供了代码和数据集（GitHub），增强可信度。
- **实际价值**：提出正向干预可提升模型性能，为模型微调、推理增强提供新思路。

## 8. 不足与局限

- **功能覆盖有限**：仅考虑8种预定义功能，可能未涵盖LLM全部认知能力（如创造力、类比推理）。
- **标注简化**：每个子问题仅标记单一功能，实际推理可能涉及多重认知操作同时进行。
- **单功能假设**：假设一个注意力头只对应一种功能，忽略了头可能承担多个上下文依赖角色的情况。
- **数据集规模较小**：仅570个主问题，可能不足以完全代表复杂推理分布。
- **实验局限性**：未在更大规模模型（如70B以上）或闭源模型（如GPT-4）上验证；未与其他可解释性方法（如注意力模式分析、Causal Tracing）对比。
- **算力信息缺失**：未报告计算资源消耗，影响可重复性细节。
- **潜在偏差**：数据集构建依赖GPT-4o，可能引入该模型自身的推理偏见。

（完）
