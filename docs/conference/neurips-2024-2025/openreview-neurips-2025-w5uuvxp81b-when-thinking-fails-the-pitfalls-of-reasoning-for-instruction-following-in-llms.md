---
title: "When Thinking Fails: The Pitfalls of Reasoning for Instruction-Following in LLMs"
title_zh: 当思考失败：推理在大语言模型指令遵循中的陷阱
authors: "Xiaomin Li, Zhou Yu, Zhiwei Zhang, Xupeng Chen, Ziji Zhang, Yingying Zhuang, Narayanan Sadagopan, Anurag Beniwal"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=w5uUvxp81b"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 揭示思维链推理可能降低指令遵循的忠实性
tldr: 表明显式思维链推理会损害大语言模型的指令遵循准确性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-001.webp\", \"caption\": \"\", \"page\": 9, \"index\": 1, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-007.webp\", \"caption\": \"\", \"page\": 22, \"index\": 7, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-008.webp\", \"caption\": \"\", \"page\": 22, \"index\": 8, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-009.webp\", \"caption\": \"\", \"page\": 22, \"index\": 9, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-010.webp\", \"caption\": \"\", \"page\": 22, \"index\": 10, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-011.webp\", \"caption\": \"\", \"page\": 22, \"index\": 11, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-012.webp\", \"caption\": \"\", \"page\": 22, \"index\": 12, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-013.webp\", \"caption\": \"\", \"page\": 22, \"index\": 13, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-014.webp\", \"caption\": \"\", \"page\": 22, \"index\": 14, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-015.webp\", \"caption\": \"\", \"page\": 22, \"index\": 15, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-016.webp\", \"caption\": \"\", \"page\": 22, \"index\": 16, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-017.webp\", \"caption\": \"\", \"page\": 22, \"index\": 17, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-018.webp\", \"caption\": \"\", \"page\": 22, \"index\": 18, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-019.webp\", \"caption\": \"\", \"page\": 23, \"index\": 19, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-020.webp\", \"caption\": \"\", \"page\": 23, \"index\": 20, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-021.webp\", \"caption\": \"\", \"page\": 23, \"index\": 21, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-022.webp\", \"caption\": \"\", \"page\": 23, \"index\": 22, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-023.webp\", \"caption\": \"\", \"page\": 23, \"index\": 23, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-024.webp\", \"caption\": \"\", \"page\": 23, \"index\": 24, \"width\": 800, \"height\": 500}]"
motivation: 思维链推理在复杂推理任务上表现优异，但可能影响指令遵循任务。
method: 在多个模型和基准上系统评估思维链推理对指令遵循准确性的影响。
result: 思维链推理会使简单规则的指令遵循准确率下降。
conclusion: 思维链推理并非万能，在某些任务中可能产生负面效果。
---

## Abstract
Reasoning-enhanced large language models (RLLMs), whether explicitly trained for reasoning or prompted via chain-of-thought (CoT), have achieved state-of-the-art performance on many complex reasoning tasks. However, we uncover a surprising and previously overlooked phenomenon: explicit CoT reasoning can significantly degrade instruction-following accuracy. Evaluating 20+ models on two benchmarks: IFEval (with simple, rule-verifiable constraints) and ComplexBench (with complex, compositional constraints), we consistently observe performance drops when CoT prompting is applied. Through large-scale case studies and an attention-based analysis, we identify common patterns where reasoning either helps (e.g., with formatting or lexical precision) or hurts (e.g., by neglecting simple constraints or introducing unnecessary content). We propose a metric, constraint attention, to quantify model focus during generation and show that CoT reasoning often diverts attention away from instruction-relevant tokens. To mitigate these effects, we introduce and evaluate four strategies: in-context learning, self-reflection, self-selective reasoning, and classifier-selective reasoning. Our results demonstrate that selective reasoning strategies, particularly classifier-selective reasoning, can substantially recover lost performance. To our knowledge, this is the first work to systematically expose reasoning-induced failures in instruction-following and offer practical mitigation strategies.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：增强推理能力的大语言模型（如DeepSeek-R1、Claude、OpenAI O系列）在数学推理、多跳问答等复杂任务上表现优异，通常依赖于链式思维（CoT）提示。然而，CoT是否对指令遵循能力（Instruction Following）有益尚不清楚。指令遵循是模型对齐、安全性和实用性的核心能力，要求模型严格遵守用户指定的约束（如字数、格式、关键词等）。
- **核心发现**：作者通过系统性实验发现，显式的CoT推理反而会显著降低模型的指令遵循准确性，这是一个此前被忽视的现象。
- **整体含义**：推理并非万能，在某些任务中可能产生负面效果，需要审慎使用；同时为构建更可靠的指令遵循系统提供了新的研究方向。

## 2. 方法论

- **核心思想**：对比“直接回答”（Base）与“先思考后回答”（CoT）两种模式下的指令遵循表现，分析推理为何降低遵循能力，并提出选择性使用推理的策略来缓解问题。
- **关键技术细节**：
  - **约束注意力（Constraint Attention）**：提出一个量化指标，计算模型在生成答案时对指令中约束相关的token的平均注意力权重。定义为：
    - 对于每个约束 \( c_r \)，获取其在提示中的token索引集合 \( C_r \)，全集 \( C = \bigcup C_r \)。
    - 在生成步骤 \( t \)，层 \( l \)、头 \( k \) 的注意力权重 \( A^{(l,t)}_k \)，层平均注意力向量 \( a^{(l,t)} = \frac{1}{H}\sum_k A^{(l,t)}_k \)。
    - 层-步约束注意力：\( \alpha^{(l,t)} = \frac{1}{|C|}\sum_{j \in C} a^{(l,t)}_j \)。
    - 层平均约束注意力：\( \bar{\alpha}^{(t)} = \frac{1}{L}\sum_{l=0}^{L-1} \alpha^{(l,t)} \)。
    - 进一步计算答案阶段平均约束注意力 \( \bar{\beta}^{(l)} \)，并定义注意力下降 \( \Delta\beta = \bar{\beta}_{\text{Base}} - \bar{\beta}_{\text{CoT}} \)。
  - **缓解策略**（四种）：
    1. **少样本上下文学习**：在提示中插入人工修正的失败案例（正确推理+正确回答），引导模型。
    2. **自反思（Self-Reflection）**：模型首先生成CoT回答，然后通过第二次推理自我评估并修正，如果满意则保留，否则修改。
    3. **自选择推理（Self-Selective Reasoning）**：模型先评估指令是否适合使用推理（YES/NO），再决定是否执行CoT。
    4. **分类器选择性推理（Classifier-Selective Reasoning）**：训练一个外部二分类器（基于Qwen2.5-7B-Instruct），判断每条指令是否应该使用CoT，然后仅对需要推理的指令应用CoT。

## 3. 实验设计

- **数据集**：
  - **IFEval**：541条提示，包含1-3个可验证的简单约束（如字数、格式、关键词频率）。采用指令级宽松准确率。
  - **ComplexBench**：1150条指令，包含通过“AND”、“Chain”、“Selection”、“Nested”等操作组合的复杂约束。结合规则和LLM评估，翻译为英文版本。
- **基准方法（Baseline）**：
  - **原始（Original）**：不使用CoT，直接回答。
  - **CoT**：使用标准CoT提示（先思考后回答）。
- **评估模型**：20+个模型，包括：
  - 通用模型：Llama系列（1B/3B/8B/70B）、Mixtral 8x7B、Qwen2.5系列、DeepSeek-V3。
  - 推理增强模型：Claude 3.5/3.7 Sonnet（普通与Think模式）、DeepSeek-R1、Qwen3 Base vs Think、Qwen2.5-Math等。
  - 闭源与开源混合，参数规模从1B到70B。
- **对比方法**：四种缓解策略分别与CoT基线比较，报告准确率和提升/下降量。另外还有9组配对比较（推理版本 vs 非推理版本）。
- **消融与分析**：
  - 大规模人工案例研究（541+1000样本），总结推理帮助/伤害的四种模式。
  - 约束注意力可视化分析，展示注意力下降与失败的相关性。
  - 额外实验：双推理（ThinkCoT）、推理长度相关性、非推理任务质量评估（LLM-as-judge）。

## 4. 资源与算力

- 论文明确说明：
  - 所有开源模型推理使用**4块NVIDIA H100-80GB GPU**，不使用量化。
  - 分类器选择性推理中，每个目标模型单独训练一个二分类器，使用**单块NVIDIA H100-80GB GPU**，完整微调。
  - 训练超参数（通过网格搜索选择）：学习率1e-5，训练3个epoch，优化器未明确说明（推测为AdamW类）。
  - 验证集比例10%，训练数据为每模型50%样本（其余50%用于评估下游效果）。
  - 未报备总训练时间或总计算量，但根据模型规模和数据集大小（IFEval 541条，ComplexBench 1150条），估计每个分类器训练时间在数十分钟至数小时内。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：14个模型在IFEval和ComplexBench上的原始/CoT对比，以及四种缓解策略结果（表1）。
  - 配对比较：9个模型对的对比（表2）。
  - 案例研究：手动检查全部541个IFEval样本和超过1000个ComplexBench样本。
  - 注意力分析：多模型、多层、多数据集的可视化和量化（图1-8）。
  - 额外实验：双推理（表5）、推理长度相关性（表4）、非推理任务质量（表6）、蒙特卡洛交叉验证（表3）、自选择推理决策分析（图9）。
- **充分性**：实验覆盖广泛，模型规模跨越多个数量级，包含不同训练范式（通用、推理增强、蒸馏），且在两个不同复杂度的benchmark上验证。结果一致且显著，足以支持核心结论。但未在更多种类的任务上（如安全性、对话）进行验证，存在一定局限性。
- **公平性**：所有模型使用固定温度0，提示模板统一（附录F），评估指标一致。对比条件严格（相同指令、相同评估）。但CoT提示可能引入额外token（思考过程），在评估时是否将思考过程计入约束检查？论文明确说明：约束检查基于最终答案，思考过程不参与评分（但某些约束如“无逗号”可能因思考过程引入而违规，这在案例中已指出）。总体公平性良好。

## 6. 主要结论与发现

- **结论1**：显式CoT推理显著降低指令遵循准确性。13/14模型在IFEval下降，所有模型在ComplexBench下降。例如Llama3-8B从75.2%降至59.0%。
- **结论2**：推理增强模型（如DeepSeek-R1、Claude Think）通常比非推理版本更差地遵循指令（表2）。
- **结论3**：推理帮助的场景：格式化/结构遵循、词汇精确约束；推理伤害的场景：过度关注高级内容而忽略简单约束、引入多余内容导致违反约束。
- **结论4**：推理降低了模型对约束token的注意力（约束注意力下降），且下降幅度在失败案例中更大。
- **结论5**：四种缓解策略中，分类器选择性推理效果最好且最稳定，自反思在简单指令和较大模型上有效，自选择推理在复杂指令上有效，少样本学习效果有限。
- **结论6**：推理长度与指令遵循性能无有意义的相关性（Pearson系数接近0）。

## 7. 优点

- **创新性**：首次系统揭示并量化CoT推理对指令遵循的负面影响，此前领域多认为推理有益。
- **分析深入**：结合大规模案例研究和注意力机制分析，提供了因果解释（注意力转移），而不仅仅是现象描述。
- **缓解策略全面**：提出四种从简单到复杂的方法，并分析适用场景，提供决策指引。
- **实验设计严谨**：覆盖多个模型族、参数规模、训练范式，并在两个互补的benchmark上验证，结果一致。
- **开源可重复**：代码和数据已公开（GitHub），提示模板和训练细节完整提供。

## 8. 不足与局限

- **实验覆盖局限**：仅专注于指令遵循任务（IFEval和ComplexBench），未在更广泛的NLP任务（如对话质量、安全合规、开放域生成）上验证。作者在局限部分已承认这一点。
- **缓解策略的泛化性**：分类器选择性推理需为每个目标模型单独训练一个分类器，成本较高；自反思对弱模型效果差；少样本学习受限于上下文长度和示例偏差。
- **评估粒度**：所有实验基于约束满足的二元或比例指标，未考虑违反约束的严重程度或实际实用影响（例如，忽略一个格式约束 vs 出现事实错误）。
- **可能偏差风险**：IFEval和ComplexBench均为合成或手动构建，可能不充分代表真实用户指令的多样性和噪声。CoT提示固定，不同提示风格可能影响结果（但通过标准提示控制了变量）。
- **计算成本**：未详细报告总实验算力消耗，仅提及单次推理硬件。为大规模复现可能需要大量GPU时间。

（完）
