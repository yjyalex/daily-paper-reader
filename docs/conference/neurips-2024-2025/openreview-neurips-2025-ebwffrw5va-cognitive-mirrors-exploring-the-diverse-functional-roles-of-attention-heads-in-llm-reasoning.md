---
title: "Cognitive Mirrors: Exploring the Diverse Functional Roles of Attention Heads in LLM Reasoning"
title_zh: 认知镜像：探索LLM推理中注意力头的多样功能角色
authors: "Xueqi Ma, Jun Wang, Yanbei Jiang, Sarah Monazam Erfani, Tongliang Liu, James Bailey"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=EBwfFrw5VA"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 注意力头在LLM推理中的角色及CoT数据集
tldr: 提出可解释性框架分析推理中注意力头角色，使用基于CoT的数据集。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-ebwffrw5va/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1024, \"height\": 1024}]"
motivation: LLM内部机制不透明，理解推理过程需解释性。
method: 构建CogQA数据集分解问题为CoT子问题，采用多标签探针分析注意力头。
result: 发现注意力头承担检索、逻辑推理等不同认知功能。
conclusion: 注意力头功能专门化有助于理解LLM推理机制。
---

## Abstract
Large language models (LLMs) have achieved state-of-the-art performance in a variety of tasks, but remain largely opaque in terms of their internal mechanisms. Understanding these mechanisms is crucial to improve their reasoning abilities. Drawing inspiration from the interplay between neural processes and human cognition, we propose a novel interpretability framework to systematically analyze the roles and behaviors of attention heads, which are key components of LLMs. We introduce CogQA, a dataset that decomposes complex questions into step-by-step subquestions with a chain-of-thought design, each associated with specific cognitive functions such as retrieval or logical reasoning. By applying a multi-label probing method, we identify the attention heads responsible for these functions. Our analysis across multiple LLM families reveals that attention heads exhibit functional specialization, characterized as cognitive heads. These cognitive heads exhibit several key properties: they are universally sparse, and vary in number and distribution across different cognitive functions, and they display interactive and hierarchical structures.  We further show that cognitive heads play a vital role in reasoning tasks—removing them leads to performance degradation, while augmenting them enhances reasoning accuracy. These insights offer a deeper understanding of LLM reasoning and suggest important implications for model design, training and fine-tuning strategies.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

该论文研究的核心问题是：**大型语言模型（LLM）在复杂推理任务中，其内部的注意力头（attention heads）是否具有类似人脑认知功能的专门化角色？** 研究动机源于LLM虽然性能优异但内部机制不透明，难以解释其推理过程。背景方面，先前工作发现LLM的某些组件（如注意力头）在简单任务中表现出功能分化（如信息检索、一致性保持），但在多步推理场景下缺乏系统理解。作者借鉴认知科学中人类大脑执行不同认知功能（如检索、逻辑推理、决策）时具有模块化分工的特点，假设LLM中的注意力头也可能对应不同的认知功能，并通过构建可解释性框架来验证这一假设。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过构建一个带有认知功能标签的数据集CogQA，将复杂问题分解为链式思维（CoT）子问题，每个子问题标注所需的认知功能；然后利用探测（probing）方法从注意力头激活中学习分类器，识别出与各认知功能高度相关的注意力头（称为“认知头”）。

- **关键技术细节**：
  - **CogQA数据集构建**：从AQuA、CREAK、ECQA、e-SNLI、GSM8K五个推理基准中采样750个问题，使用GPT-4o按CoT范式分解为子问题，每个子问题带有答案和认知功能标签。经过两阶段人工验证过滤，最终得到570个主问题、3402个子问题三元组。认知功能分为低阶（检索、知识回忆、语义理解、句法理解）和高阶（数学计算、逻辑推理、推理、决策）共8种。
  - **注意力头特征提取**：对每个子问题，让LLM生成答案，提取每个生成token在所有层所有头的输出向量（value projection）；选择top-k语义重要token平均，并拼接所在层的平均激活，得到增强特征。
  - **多类探测分类器**：使用两层MLP（输入经线性投影降维至64维，隐层512维，ReLU，dropout 0.3，输出8类）进行训练；利用梯度×激活（Gradient×Activation）方法计算每个注意力头对每个认知功能的重要性分数。
  - **干预验证**：通过缩放输出（乘以小因子ε≈0.001）抑制特定头，评估性能下降；或沿功能方向激活增强（添加ασ·dir），观察性能提升。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - **主数据集**：CogQA（570个主问题，3402个子问题，用于探测和干预测试）。
  - **下游任务**：GSM8K_100（100个数学样本）和Extractive_QA（49个检索问答样本，由GPT-4o生成）。
- **基准和模型**：在三个LLM家族、多种规模上测试——LLaMA（3.1-8B-instruct, 3.2-3B-instruct）、Qwen（3-8B, 3-4B）、Yi（1.5-9B, 1.5-6B）。共6个模型。
- **对比方法**：
  - **基准干预**：随机选择同等数量的注意力头进行掩码（random heads intervention）。
  - **消融设计**：
    - 不同掩码数量（16、32、64、128）的影响。
    - 交叉掩码：掩码检索头测试知识回忆，反之亦然。
    - 不同token提取位置（first, last, meaning_first, full, top-k）的消融。
    - 正干预（enhancement）与负干预（masking）对比。

### 4. 资源与算力

论文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力资源。仅在致谢中提及研究部分由澳大利亚研究委员会（ARC）项目支持。由于实验主要涉及模型推理和轻量级MLP训练，推测算力需求不大，但缺乏具体细节。

### 5. 实验数量与充分性

- **实验数量**：论文进行了多组实验，包括：
  1. 认知头重要性热力图可视化（6个模型×8功能）。
  2. 掩码干预表（Table 1：6模型×8功能，含两个指标：COMET和准确率）。
  3. 交叉掩码实验（Table 2：2模型×2功能×2指标）。
  4. 不同掩码数量趋势图（Figure 3：4功能×4数量）。
  5. 层次关系实验（Table 3：单一模型，8种低阶掩码组合对高阶功能影响）。
  6. 下游任务干预（Table 4：2任务×4模型×正负干预）。
  7. 消融实验（Appendix A.9：不同token位置）。
- **充分性与公平性**：
  - 覆盖了多个模型家族和规模，有利于验证泛化性。
  - 干预实验采用随机头掩码作为对照，因果验证设计合理。
  - 但论文未报告误差条或多次运行统计，削弱了结果的稳定性证据。作者在实验统计显著性部分回答“No”，认为实验不需要。且数据集构建依赖GPT-4o生成，可能存在偏差。

### 6. 论文的主要结论与发现

1. **存在认知头**：注意力头对特定认知功能具有显著重要性（重要性分数>0.001的头不足7%），且这种功能专门化在多个LLM家族中普遍存在。
2. **稀疏性与分布**：不同功能所需认知头数量差异大（如数学计算仅需59个头，推理需139个头），检索头主要集中在中层，数学头集中在高层。
3. **功能聚类与层次结构**：PCA显示同类型功能头聚类（推理、决策、逻辑推理头相近；数学头独立）；低阶头（检索、知识回忆）被抑制会显著损害高阶功能（决策、推理），而抑制句法理解头影响较小，揭示层次化组织。
4. **因果验证**：掩码认知头导致对应功能性能严重下降（准确率可降至0%），而掩码随机头影响很小；增强认知头激活可提升下游任务准确率。

### 7. 优点：方法或实验设计亮点

- **认知科学启发的分类框架**：将LLM内部组件行为与人类认知功能对齐，提供系统化分析视角。
- **多类探测与重要性归因**：不同于单类探测，该方法能同时捕获多头多功能的复杂关系，并利用梯度×激活计算归因分数。
- **正负双向干预验证**：既通过抑制（masking）证明必要性，又通过增强（enhancement）证明充分性，增强了因果性结论的可信度。
- **跨模型、跨规模验证**：在6个不同架构/大小模型上得到一致结论，表明发现的普遍性。
- **CogQA数据集设计**：链式分解并人工验证，确保子问题逻辑连贯且标注准确。

### 8. 不足与局限

- **功能覆盖不全**：仅考虑8种认知功能，可能无法涵盖LLM全部推理能力（如类比、空间推理等）。
- **单一功能标注假设**：每个子问题只标注一个认知功能，但实际推理可能同时涉及多种功能。
- **头功能单一假设**：认为每个注意力头只负责一个功能，但可能存在多头或多功能重叠，未深入研究。
- **缺乏统计显著性报告**：未提供误差条或多次运行结果，削弱实验稳健性。
- **数据集潜在偏差**：CogQA由GPT-4o生成，尽管有人工验证，但无法完全消除生成偏差或遗漏重要步骤。
- **算力信息缺失**：未披露实验计算资源，影响复现和能耗评估。
- **下游任务规模小**：GSM8K_100和Extractive_QA样本数较少，可能不足以代表一般性能。
- **未探索多个头同时干预的协同/竞争效应**：仅独立干预或简单组合，未系统研究多功能的联合作用。

（完）
