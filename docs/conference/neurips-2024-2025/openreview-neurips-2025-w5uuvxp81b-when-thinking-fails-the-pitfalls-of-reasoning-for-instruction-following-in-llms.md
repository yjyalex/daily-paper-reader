---
title: "When Thinking Fails: The Pitfalls of Reasoning for Instruction-Following in LLMs"
title_zh: 当思考失败：推理对大语言模型指令遵循的陷阱
authors: "Xiaomin Li, Zhou Yu, Zhiwei Zhang, Xupeng Chen, Ziji Zhang, Yingying Zhuang, Narayanan Sadagopan, Anurag Beniwal"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=w5uUvxp81b"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 思维链推理降低指令遵循准确性
tldr: 发现显式思维链推理会损害LLM的指令遵循能力。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-001.webp\", \"caption\": \"\", \"page\": 9, \"index\": 1, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-007.webp\", \"caption\": \"\", \"page\": 22, \"index\": 7, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-008.webp\", \"caption\": \"\", \"page\": 22, \"index\": 8, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-009.webp\", \"caption\": \"\", \"page\": 22, \"index\": 9, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-010.webp\", \"caption\": \"\", \"page\": 22, \"index\": 10, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-011.webp\", \"caption\": \"\", \"page\": 22, \"index\": 11, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-012.webp\", \"caption\": \"\", \"page\": 22, \"index\": 12, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-013.webp\", \"caption\": \"\", \"page\": 22, \"index\": 13, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-014.webp\", \"caption\": \"\", \"page\": 22, \"index\": 14, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-015.webp\", \"caption\": \"\", \"page\": 22, \"index\": 15, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-016.webp\", \"caption\": \"\", \"page\": 22, \"index\": 16, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-017.webp\", \"caption\": \"\", \"page\": 22, \"index\": 17, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-018.webp\", \"caption\": \"\", \"page\": 22, \"index\": 18, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-019.webp\", \"caption\": \"\", \"page\": 23, \"index\": 19, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-020.webp\", \"caption\": \"\", \"page\": 23, \"index\": 20, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-021.webp\", \"caption\": \"\", \"page\": 23, \"index\": 21, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-022.webp\", \"caption\": \"\", \"page\": 23, \"index\": 22, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-023.webp\", \"caption\": \"\", \"page\": 23, \"index\": 23, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-w5uuvxp81b/fig-024.webp\", \"caption\": \"\", \"page\": 23, \"index\": 24, \"width\": 800, \"height\": 500}]"
motivation: 推理增强模型在指令遵循任务上表现反常。
method: 在IFEval和ComplexBench上评估20+模型并分析注意力模式。
result: CoT推理导致指令遵循准确性显著下降。
conclusion: 推理并非总是有益，需警惕其负面影响。
---

## Abstract
Reasoning-enhanced large language models (RLLMs), whether explicitly trained for reasoning or prompted via chain-of-thought (CoT), have achieved state-of-the-art performance on many complex reasoning tasks. However, we uncover a surprising and previously overlooked phenomenon: explicit CoT reasoning can significantly degrade instruction-following accuracy. Evaluating 20+ models on two benchmarks: IFEval (with simple, rule-verifiable constraints) and ComplexBench (with complex, compositional constraints), we consistently observe performance drops when CoT prompting is applied. Through large-scale case studies and an attention-based analysis, we identify common patterns where reasoning either helps (e.g., with formatting or lexical precision) or hurts (e.g., by neglecting simple constraints or introducing unnecessary content). We propose a metric, constraint attention, to quantify model focus during generation and show that CoT reasoning often diverts attention away from instruction-relevant tokens. To mitigate these effects, we introduce and evaluate four strategies: in-context learning, self-reflection, self-selective reasoning, and classifier-selective reasoning. Our results demonstrate that selective reasoning strategies, particularly classifier-selective reasoning, can substantially recover lost performance. To our knowledge, this is the first work to systematically expose reasoning-induced failures in instruction-following and offer practical mitigation strategies.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文内容生成的结构化、深入且客观的中文总结。

---

### 论文核心问题与整体含义（研究动机与背景）

*   **核心问题**：推理增强型大语言模型（Large Language Models, LLMs），尤其是通过思维链（Chain-of-Thought, CoT）进行推理的模型，在复杂推理任务上取得了巨大成功。然而，这篇论文首次系统地揭示了一个矛盾的、被长期忽视的现象：**显式的思维链推理会显著损害模型遵循用户指令的能力**。
*   **研究动机**：指令遵循（Instruction-Following）是衡量LLM对齐性、安全性和实用性的关键能力。尽管CoT被广泛认为能提升性能，但它在结构化任务（如指令遵循）上的负面影响尚不明确。作者旨在探索和解释这一“推理失灵”现象。
*   **整体含义**：该发现挑战了“更多推理总是更好”的普遍假设，表明推理存在副作用。它提示我们在使用LLM时不能简单地对所有任务都应用CoT，需要针对任务特性选择是否使用推理，以实现性能与可靠性的平衡。

### 论文提出的方法论

#### 核心思想
1.  **现象发现与量化**：通过在指令遵循基准测试上进行广泛的CoT与非CoT对比实验，证实性能下降现象的普遍存在。
2.  **机制分析**：通过大规模案例研究和**约束注意力**（Constraint Attention）这一新指标，剖析CoT推理如何导致失败。
3.  **缓解策略**：提出并比较多种方法，旨在有选择性地应用推理，以避免其对指令遵循的损害。

#### 关键技术细节
*   **约束注意力（Constraint Attention）指标**：
    *   这是一个量化模型在生成回答时，对提示指令中**与约束相关的词元**（constraint-relevant tokens）关注程度的新指标。
    *   **计算过程**：
        1.  使用GPT-4o自动提取指令中每个约束对应的文本子串，并将其映射为提示词中的词元索引，定义集合C。
        2.  在模型生成的每一个步骤t，计算所有注意力头在各个层（layer）获得的层-步注意力权重 $a^{(l,t)}$。
        3.  计算当前步骤t对所有约束词元的平均注意力：$\alpha^{(l,t)} = \frac{1}{|C|} \sum_{j \in C} a^{(l,t)}_j$。
        4.  平均所有层的注意力，得到当前步的约束注意力：$\bar{\alpha}^{(t)} = \frac{1}{L} \sum_{l} \alpha^{(l,t)}$。
        5.  **核心发现**：通过可视化注意力轨迹，发现当使用CoT推理时，模型在生成回答阶段的约束注意力**普遍低于**不使用推理时的水平，特别是在推理导致性能下降的案例中。这为解释性能下降提供了有力的量化证据。
*   **四种缓解策略**：
    1.  **少样本上下文学习（In-Context Learning, FewShot）**：在提示中加入经过手工修正的、成功遵循指令的“思考-回答”示例。
    2.  **自我反思（Self-Reflection）**：模型先生成带有推理的初始回答，然后进行第二次推理，对自身回答进行反思和修正。
    3.  **自我选择性推理（Self-Selective Reasoning）**：模型自己判断对于当前指令是否需要推理，若需要则进行CoT，否则直接生成答案。
    4.  **分类器选择性推理（Classifier-Selective Reasoning）**：训练一个独立的二元分类器，根据指令文本预测使用CoT是否有益，从而决定是否启用推理。分类器基于一个预训练模型（如Qwen2.5-7B-Instruct）进行微调。

### 实验设计

*   **数据集**：
    1.  **IFEval**：包含541个提示，每个提示带有1-3个简单的、可自动验证的约束（如字数、格式、关键词）。评估采用指令级松散准确率。
    2.  **ComplexBench**：包含1150条指令，由多种操作（如逻辑与、链式、选择、嵌套）组合成复杂的组合约束，评估更为全面和困难。
*   **Benchmark**：这两个基准测试共同覆盖了从简单到复杂的指令遵循场景，使评估结果更具代表性和说服力。
*   **对比方法**：
    *   **基准对比**：对比每个模型**直接回答（非推理）** 与 **CoT提示回答** 的性能。
    *   **模型配对对比**：对比拥有“思考/推理”功能的模型与其基础版本（如Claude 3.7 Sonnet vs. Claude 3.7 Sonnet Think, DeepSeek-V3 vs. DeepSeek-R1, Qwen3系列）。
    *   **缓解策略对比**：在CoT性能的基础上，与四种提出的缓解策略（FewShot, Self-Reflection, Self-Selective, Classifier-Selective）进行对比。

### 资源与算力

*   **推理环境**：所有开源模型的推理均在4块 **NVIDIA H100-80GB** GPU上进行，未使用量化。
*   **分类器训练**：每个目标模型的分类器使用单块 **NVIDIA H100-80GB** GPU进行全参数微调。
*   **未明确说明**：论文未明确提及总训练或总推理的精确耗时，仅说明了环境配置。温度参数统一设置为0。

### 实验数量与充分性

*   **实验规模**：实验非常充分，涵盖了：
    *   **20+个模型**：包括不同规模（1B-70B）和不同训练范式（通用 vs. 推理优化）的模型，以及多个闭源和开源模型。
    *   **两个互补基准**：覆盖简单和复杂的指令场景。
    *   **4种缓解策略**：进行了全面的比较实验。
    *   **消融与补充实验**：包括了注意力机制分析、推理长度相关性分析（附录J）和在推理模型上使用双重推理的测试（附录K）。
    *   **统计显著性验证**：对分类器选择性推理方法进行了蒙特卡洛交叉验证，并报告了均值和p值（附录I），证明了结果的鲁棒性。
*   **评判标准**：实验设计客观、公平。所有实验在温度0下进行以确保确定性。考虑了多种模型配对比较，并严格遵循基准测试的评估协议（包括依赖逻辑）。对比了推理型模型与其非推理版本，控制变量。
*   **结论**：实验数量充足，设计严谨，结果具有很强的说服力。

### 论文的主要结论与发现

1.  **核心发现**：显式的CoT推理普遍且显著地降低了LLM的指令遵循能力。在测试的14个模型中，13个在IFEval上性能下降，所有模型在ComplexBench上性能下降。
2.  **失败模式分类**：
    *   **推理有帮助**：主要在执行格式/结构约束（如输出JSON）和词汇/关键词精确约束（如指定字母出现次数）时有效。
    *   **推理有害**：在两种情况下最突出：
        1.  **过度关注高维内容而忽略简单机械约束**：当多约束共存时，推理倾向于进行内容规划，忘记了字数、大小写、重复原文等简单要求。
        2.  **引入违反约束的不必要内容**：推理过程产生的“帮助性”附加内容（如解释、翻译、评论）反而触发了约束违规（如在“无逗号”任务中引入逗号）。
3.  **机制解释**：**约束注意力**分析表明，CoT推理会将模型的注意力从指令中的约束相关词元上**转移**到推理过程中，导致生成回答时对约束的意识降低，从而增加违反风险。
4.  **缓解策略有效性**：
    *   **分类器选择性推理**是最有效的方法，在两个基准测试中为几乎所有模型带来了最佳或次佳的性能恢复，尽管需要为每个模型单独训练分类器。
    *   **自我反思**对简单指令和小模型效果显著，但对复杂指令和弱模型效果不佳。
    *   **自我选择性推理**展现了一定的有效性，但其决策倾向于**高召回率、低精确率**，即模型会过度使用推理。
    *   **少样本上下文学习**的提升有限，主要因为示例长度限制了数量，且可能存在偏差。
5.  **推理长度与性能无关**：推理词元的长度与指令遵循效果之间没有有意义的相关性，说明问题不在于“想的时间太长”，而在于“想的方式不对”。

### 优点

1.  **发现了反直觉但重要的现象**：首次系统性揭示了CoT推理在指令遵循任务上的负面影响，为该领域提出了一个重要且实用的新问题。
2.  **分析方法创新**：提出了“约束注意力”这一量化指标，为解释模型行为提供了直观且有力的工具，深化了对推理失败原因的理解。
3.  **实验完整且严谨**：进行了大规模的模型评估、深入的案例研究（包括详细的错误/成功案例）、统计显著性验证，提供了充分可靠的证据。
4.  **提供了可操作的缓解方案**：不仅诊断了问题，还提出了多种有效缓解策略，并对这些策略的优缺点、适用场景进行了详细分析和比较，给出了实践建议。
5.  **工作实用性强**：研究直接服务于提升LLM在实际应用中的可靠性和对齐性，具有很高的实用价值。

### 不足与局限

1.  **研究范围限定于指令遵循**：论文本身明确指出其主要局限在于仅研究了指令遵循任务。作者指出，虽然猜想推理可能在其他领域也产生类似负面影响，但这超出了当前工作的范围。
2.  **缓解策略的局限性**：
    *   **少样本学习**受限于示例长度和潜在的模型偏差。
    *   **自我反思**对弱模型和复杂指令效果不佳，且计算成本翻倍。
    *   **选择性策略**（自我/分类器）引入了额外的开销和复杂性，分类器方法需要为每个新模型重新训练。
    *   论文提到自我选择性推理的精确率低，暗示模型缺乏有效判断何时必须推理的能力。
3.  **计算成本考量**：论文分析指出推理会增加计算成本，但未在缓解策略评估中系统地量化成本与收益的权衡，特别是分类器训练带来的开销。
4.  **模型依赖性**：分类器选择性推理方法需要针对不同目标模型分别训练，缺乏跨模型的泛化能力。结论和缓解策略的有效性可能在未来的新模型上需要重新验证。
5.  **基准测试本身局限**：IFEval和ComplexBench都是合成或精心设计的基准测试，其是否能完美代表真实世界中种类繁多的指令遵循场景仍是一个问题。实验未能覆盖所有可能的指令类型（如多轮对话中的指令）。

（完）
