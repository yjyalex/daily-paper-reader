---
title: Calibrating Reasoning in Language Models with Internal Consistency
title_zh: 用内部一致性校准语言模型中的推理
authors: "Zhihui Xie, Jizhou Guo, Tong Yu, Shuai Li"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=udZKVMPf3S"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 通过检测CoT内部不一致性校准推理
tldr: 通过分析内部表示与生成理由的一致性来校准LLM推理。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-udzkvmpf3s/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1188, \"height\": 932}]"
motivation: LLM常产生明显错误和矛盾的文本，怀疑其处理和使用生成理由的能力。
method: 从内部表征角度研究推理，分析中间层与最终层之间的不一致性。
result: 发现生成理由虽提高答案准确性，但内部表征出现不一致，损害可靠性。
conclusion: 需要关注内部一致性以提升推理可靠性。
---

## Abstract
Large language models (LLMs) have demonstrated impressive capabilities in various reasoning tasks, aided by techniques like chain-of-thought prompting that elicits verbalized reasoning. However, LLMs often generate text with obvious mistakes and contradictions, raising doubts about their ability to robustly process and utilize generated rationales. In this work, we investigate reasoning in LLMs through the lens of internal representations, focusing on how these representations are influenced by generated rationales. Our preliminary analysis reveals that while generated rationales improve answer accuracy, inconsistencies emerge between the model’s internal representations in middle layers and those in final layers, potentially undermining the reliability of their reasoning processes. To address this, we propose internal consistency as a measure of the model’s confidence by examining the agreement of latent predictions decoded from intermediate layers. Extensive empirical studies across different models and datasets demonstrate that internal consistency effectively distinguishes between correct and incorrect reasoning paths. Motivated by this, we propose a new approach to calibrate reasoning by up-weighting reasoning paths with high internal consistency, resulting in a significant boost in reasoning performance. Further analysis uncovers distinct patterns in attention and feed-forward modules across layers, providing insights into the emergence of internal inconsistency. In summary, our results demonstrate the potential of using internal representations for self-evaluation of LLMs.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：大语言模型（LLMs）在推理任务中表现优异，尤其是通过思维链（Chain-of-Thought, CoT）提示生成逐步推理。然而，LLMs 经常生成明显错误或自相矛盾的文本，质疑其处理和使用生成推理的鲁棒性。已有研究指出 CoT 推理存在“不忠实”问题（如推理与最终预测矛盾），亟需有效的校准方法来评估推理路径的可靠性。
- **核心问题**：CoT 推理虽然提高了答案准确率，但导致模型中间层与最终层内部表示之间出现不一致，这种内部不一致可能损害推理过程的可靠性。如何利用内部表示来校准 LLM 的推理？
- **整体含义**：通过探究内部表示与生成推理的一致性，提出一种无需额外训练、即插即用的自评估方法，能够有效区分正确与错误的推理路径，并用于提升推理性能。

## 2. 论文提出的方法论

- **核心思想**：在 CoT 推理过程中，模型的中间层（隐藏层）内部表示包含潜在的推理信息，通过解码这些中间层的“潜在预测”（latent predictions）并与最终输出比较，可以衡量模型对推理结果的置信度。内部一致性越高，推理路径越可靠。
- **关键技术细节**：
  - **内部一致性定义**：对于一条推理路径，在最终答案 token 处收集所有中间层（第 1 层到第 L-1 层）的潜在预测（通过 logit lens 解码），计算这些潜在预测与最终预测（第 L 层）一致的比率：  
    `InternalConsistency(x, ŷ) = (1/(L-1)) Σ_{ℓ=1}^{L-1} 1{ŷ^ℓ = ŷ^L}`。  
    其中 ℓ 为层索引，L 为总层数。
  - **平衡潜在预测**：由于各层预测可能存在偏差（如倾向于某一类答案），作者为每层分别计算中位数阈值来校准，使得正负类分布更均衡。
  - **校准推理方法（SC+IC）**：基于自一致性（Self-Consistency）框架，对多条采样的推理路径，计算每条路径的内部一致性分数，并将分数作为权重累加到对应答案上，最终选择加权和最高的答案。变体包括：
    - **SC+IC（tune）**：学习各层的权重向量 w（仅 L 个参数），在 500 个样本上优化。
    - **SC+IC（transfer）**：将 PrOntoQA 上学到的权重直接迁移到其他任务。
- **公式/算法流程（文字说明）**：
  1. 输入问题，使用 CoT 提示采样 N 条推理路径（温度采样，top-p 核采样）。
  2. 对每条路径，在生成最终答案 token 时，记录每个中间层的 logits，经平衡后得到潜在预测。
  3. 计算每条路径的内部一致性分数。
  4. 对于每个候选答案，累加所有支持该答案的路径的内部一致性分数。
  5. 选择总分最高的答案作为最终输出。

## 3. 实验设计

- **数据集与场景**：共 4 个推理数据集，均转化为“True/False”二分类格式：
  - **BoolQ**（阅读理解）：基于段落回答是/否问题。
  - **CoinFlip**（符号推理）：硬币翻转序列后的朝向。
  - **PrOntoQA**（逻辑推理）：基于虚构概念的 3 跳演绎推理。
  - **ProofWriter**（逻辑推理）：基于真实概念的 3 跳推理。
- **基准方法（Baseline）**：
  - **Greedy**：贪心解码。
  - **SC**：标准自一致性（多数投票）。
  - **SC+Δ**：基于 logit 置信度（最高概率与次高概率之差）加权。
  - **SC+IC**：本文提出的基于内部一致性加权。
  - **SC+IC（tune）** 和 **SC+IC（transfer）** 为加权变体。
- **模型**：Llama-2-7B、Llama-2-13B、Mistral-7B、Mixtral-8×7B（MoE）。
- **实验设置**：4 个数据集 × 4 个模型 × 两种提示方式（CoT 和 Least-to-Most） × 多个基线，报告校准后的准确率（balanced accuracy），每次实验 10 个随机种子取平均，采样 40 条推理路径。

## 4. 资源与算力

- 论文在附录 B 中说明：所有实验使用一个拥有 8 张 Nvidia GPU 卡和 512GB 内存的计算节点。
- 重现文中主要实验的总估算计算时间小于 **100 GPU 小时**（整个研究项目需要更多计算，但报告中仅包含核心实验）。

## 5. 实验数量与充分性

- **实验数量**：主实验包含 4 个模型 × 4 个数据集 × 2 种提示方式（CoT 和 L2M） × 多个基线（Greedy, SC, SC+Δ, SC+IC 等），共产生大量表格（表 1）和多个图（图 3-4, 6-10）。此外还有消融实验（路径数变化）、分层加权实验、注意力与 FFN 分析实验（图 5）以及语义分析。
- **充分性评估**：
  - 实验覆盖了不同规模、不同架构的模型（7B/13B、MoE），多个推理类型任务，结果具有较好的泛化性。
  - 使用多个随机种子，报告准确率，统计合理性较好。
  - 对比了多种主流基线方法，公平性尚可。
  - 不足之处：仅使用单一指标（准确率），未对其他校准指标（如 ECE、可靠性图）进行系统比较；未在更复杂的推理任务（如数学、常识推理）上验证。

## 6. 论文的主要结论与发现

1. **CoT 推理导致内部不一致**：线性探针实验表明，CoT 虽提高准确率，但中间层与最终层的表示出现不一致。
2. **内部一致性可有效校准推理**：内部一致性分数在正确与错误推理路径间分布有明显差异（正确路径一致性更高），且与预测正确概率呈正相关。
3. **基于内部一致性的加权提升性能**：提出的 SC+IC 方法在所有模型和数据集上持续优于贪心解码、标准自一致性以及基于 logit 的置信度方法，尤其是在符号推理和逻辑推理任务上提升显著（1.8%～4.9%）。
4. **分层权重可迁移**：通过学习各层权重（仅 L 个参数）进一步改进性能，且跨任务迁移效果良好，说明层重要性模式具有通用性。
5. **内部不一致源于注意力与 FFN 层的错配**：中间层的注意力集中在查询和推理步骤上，但 FFN 层在后期才主导最终预测，导致中间层的正确信息未被充分利用。

## 7. 优点

- **方法新颖且简洁**：首次从内部表示一致性角度校准 CoT 推理，无需额外训练或人工标注，即插即用。
- **强泛化性**：在多种模型架构（包括 MoE）和多个推理任务上均有效，且学习到的层权重可跨任务迁移。
- **可解释性**：通过分析注意力权重和 FFN 值向量，揭示了内部不一致产生的可能机制（中间层与后期层功能脱节）。
- **实验全面性**：覆盖了不同模型规模、多种基线、两种提示方式（CoT 和 L2M），统计严谨（10 个随机种子）。

## 8. 不足与局限

- **实验覆盖有限**：
  - 仅测试了 decoder-only 模型（Llama, Mistral），未涉及 encoder-decoder 模型（如 T5）。
  - 仅使用 vanilla CoT 和 Least-to-Most 两种简单提示方法，未在更复杂的 prompting 技术（如 Tree-of-Thought, Program-Aided Language Models）上验证。
  - 仅使用 True/False 二分类任务，未扩展到多选或生成式推理任务。
- **偏差风险**：内部一致性度量依赖于 logit lens 解码，可能受到模型内部表示偏差的影响（已有校准步骤缓解，但未完全消除）。
- **应用限制**：需要访问中间层隐藏状态，对于 API-only 模型（如 GPT-4）不可直接使用；计算额外开销（需存储所有中间层 logits）可能较大。
- **未深入讨论失败案例**：表 2 虽给出实例，但未系统分析内部一致性方法在哪些场景下失效。

（完）
