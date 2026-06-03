---
title: Calibrating Reasoning in Language Models with Internal Consistency
title_zh: 用内部一致性校准语言模型的推理
authors: "Zhihui Xie, Jizhou Guo, Tong Yu, Shuai Li"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=udZKVMPf3S"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 研究思考链推理中的内部表示不一致性
tldr: 揭示思考链中内部表示不一致，削弱可靠性。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-udzkvmpf3s/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1188, \"height\": 932}]"
motivation: 语言模型常产生错误和矛盾，其推理可靠性存疑。
method: 分析中间层与最终层内部表示的一致性。
result: 发现生成的推理链虽提高准确率但内部表示不一致。
conclusion: 内部一致性可用于评估推理可靠性。
---

## Abstract
Large language models (LLMs) have demonstrated impressive capabilities in various reasoning tasks, aided by techniques like chain-of-thought prompting that elicits verbalized reasoning. However, LLMs often generate text with obvious mistakes and contradictions, raising doubts about their ability to robustly process and utilize generated rationales. In this work, we investigate reasoning in LLMs through the lens of internal representations, focusing on how these representations are influenced by generated rationales. Our preliminary analysis reveals that while generated rationales improve answer accuracy, inconsistencies emerge between the model’s internal representations in middle layers and those in final layers, potentially undermining the reliability of their reasoning processes. To address this, we propose internal consistency as a measure of the model’s confidence by examining the agreement of latent predictions decoded from intermediate layers. Extensive empirical studies across different models and datasets demonstrate that internal consistency effectively distinguishes between correct and incorrect reasoning paths. Motivated by this, we propose a new approach to calibrate reasoning by up-weighting reasoning paths with high internal consistency, resulting in a significant boost in reasoning performance. Further analysis uncovers distinct patterns in attention and feed-forward modules across layers, providing insights into the emergence of internal inconsistency. In summary, our results demonstrate the potential of using internal representations for self-evaluation of LLMs.

---

## 论文详细总结（自动生成）

## 论文中文详细总结

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型在推理任务中广泛采用链式思考（Chain-of-Thought, CoT）提示来生成逐步推理过程，从而提升答案准确率。然而，这些推理过程常包含明显错误或矛盾，使得模型生成的推理与其最终预测之间可能存在不一致（即推理不忠实），这严重影响了模型推理的可信度。
- **核心问题**：现有方法缺乏对模型**内部处理过程**的自我评估手段，无法有效判断推理路径的可靠性。因此，需要一种无需额外训练、能直接利用模型内在表示来校准推理的方法。
- **整体含义**：本文首次从内部表示视角探索推理校准。通过分析中间层与最终层表示的一致性，提出**内部一致性（Internal Consistency）**度量，并基于此对推理路径进行加权，从而提升推理性能。这不仅提供了新的校准手段，还揭示了 CoT 推理中内部不一致性的产生机制。

---

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：利用 Transformer 中间层的隐表示解码出潜在预测（latent predictions），计算这些潜在预测与最终预测的**一致程度**作为置信度指标。高内部一致性对应更可靠的推理路径。
- **关键技术细节**：
  - **潜在预测提取**：使用“logit lens”方法，将中间层隐藏状态通过 unembedding 得到词汇分布，取 argmax 得到该层的预测 token。
  - **校准偏差处理**：由于解码分布常偏向特定答案（如“True”），作者对每一层进行独立校准——计算数据集上该层预测为“True”的概率中位数，用中位数阈值平衡两类标签。
  - **内部一致性公式**：  
    \[
    \text{InternalConsistency}(x, \hat{y}) = \frac{1}{L-1} \sum_{\ell=1}^{L-1} \mathbb{1}\{\hat{y}^\ell = \hat{y}^L\}
    \]  
    其中 \(\hat{y}^\ell\) 是第 \(\ell\) 层的潜在预测，\(\hat{y}^L\) 是最终层预测。
  - **校准推理（SC+IC）**：基于自我一致性（Self-Consistency, SC）框架，对每个采样推理路径计算内部一致性，将所有路径按答案类别累加一致性分数，选择总分最高的答案。变体包括：
    - **SC+IC (tune)**：学习每层权重（L 个参数），优化一致性聚合。
    - **SC+IC (transfer)**：将在 PrOntoQA 上学到的层权重直接迁移到其他数据集。
- **算法流程**（文字描述）：
  1. 对每个测试问题，通过 CoT 采样生成 \(N\) 条推理路径。
  2. 对每条路径，在最终答案 token 处收集各中间层的潜在预测，计算内部一致性分数。
  3. 按答案类别（True/False）累加所有路径的一致性分数。
  4. 选择总分最高的答案作为最终预测。

---

### 3. 实验设计：使用了哪些数据集 / 场景，benchmark 是什么，对比了哪些方法

- **数据集**（全部转换为 True/False 二分类格式）：
  - **BoolQ**（阅读理解）
  - **CoinFlip**（符号推理，硬币翻转）
  - **PrOntoQA**（逻辑推理，3跳推理，虚构概念）
  - **ProofWriter**（逻辑推理，3跳推理，真实概念）
- **基准场景**：零样本（Zero-shot）、少样本（Few-shot）、少样本+CoT、以及 Least-to-Most (L2M) 提示。
- **对比方法**：
  - **Greedy**：贪心解码单条路径。
  - **SC**（Self-Consistency）：多数投票。
  - **SC+Δ**（Wang & Zhou, 2024）：基于路径置信度（top-1与top-2 logit差值）加权投票。
  - **SC+IC**（本文）：基于内部一致性加权投票。
  - **SC+IC (tune)**、**SC+IC (transfer)**：可学习的层加权变体。
- **模型**：Llama-2-7B、Llama-2-13B、Mistral-7B、Mixtral-8×7B（MoE）。

---

### 4. 资源与算力

- **文中说明**：所有实验在配备8张 Nvidia GPU 和 512GB 内存的计算节点上执行。重现主要实验所需总计算时间**不超过 100 GPU 小时**。但未明确 GPU 型号（如 A100、V100 等）。论文整体研究项目所需算力多于报告中实验。

---

### 5. 实验数量与充分性

- **实验数量**：
  - 在 4 个数据集、4 个模型、2 种提示方式（CoT 和 L2M）下评估，共产生 4×4×2 = 32 个主要结果表（Table 1 中显示）。
  - 每个设置使用 10 种不同随机种子运行，报告平均校准准确率。
  - 抽样路径数量：40 条。
  - 额外消融实验包括：不同投票数（图4）、层加权分析（tune/transfer）、组件分析（图5）、以及内部一致性分布的详细可视化（附录 C 多图）。
- **充分性与公平性**：
  - 模型覆盖了不同规模（7B、13B）和架构（标准 Transformer、MoE），具有代表性。
  - 对比了贪心、self-consistency、logit 置信度等多种基准，设置公平。
  - 使用了校准准确率（Calibrated Accuracy）以避免标签不平衡偏差。
  - 但实验仅限 decoder-only 模型，未涉及 encoder-decoder 或更大规模模型（如 70B+）。
  - 提示方式仅考虑 vanilla CoT 和 L2M，未涉及更复杂的技术（如 Tree-of-Thought）。

---

### 6. 论文的主要结论与发现

1. **CoT 推理导致内部不一致**：虽然 CoT 提高最终答案准确率，但中间层与最终层表示的一致性下降，表明模型可能未充分利用中间层捕获的正确信息。
2. **内部一致性有效区分正确/错误推理路径**：正确的推理路径通常具有更高的内部一致性，分布差异显著（图3）。
3. **基于内部一致性的加权可提升推理性能**：SC+IC 在所有模型和数据集上一致优于 SC 和 SC+Δ，尤其在符号推理和逻辑推理任务上提升明显（Table 1）。
4. **层权重可学习且可迁移**：SC+IC (tune) 进一步改进性能；在 PrOntoQA 上学到的权重可直接迁移到其他任务，效果接近单独训练，表明存在通用模式。
5. **内部不一致的根源**：中间层自注意力高效聚焦于查询和推理步骤，但后续 FFN 层的作用位置与这些高注意力层不匹配，导致信息未有效传递至最终预测（图5）。FFN 层的值向量（value vectors）在后期层主导最终输出。

---

### 7. 优点

- **新颖视角**：首次利用内部表示一致性进行无监督推理校准，无需额外训练或人工标注。
- **即插即用**：方法可直接应用于现有 LLM，无需修改模型或训练过程，计算开销小（仅需少量前向传播获取中间层 logits）。
- **揭示机制**：通过分析注意力和 FFN 层，提供了内部不一致性产生的合理解释，有助于理解 LLM 推理的局限性。
- **实验全面**：覆盖多种模型规模、架构、数据集类型，并进行了详细的消融和可视化分析。
- **实用价值**：SC+IC 在复杂推理任务上提升明显，尤其在逻辑推理（PrOntoQA、ProofWriter）上显著优于多数投票法。

---

### 8. 不足与局限

- **模型范围有限**：仅评估 decoder-only 架构（Llama、Mistral 系列），未包括 encoder-decoder 模型（如 T5）或更大模型（≥70B）。虽然文中提到可用 decoder lens 扩展，但未验证。
- **推理方法单一**：仅实验了 vanilla CoT 和 L2M，未涉及 Tree-of-Thought、Program-aided 等高级提示方法，内部一致性在这些场景下的表现未知。
- **任务格式限制**：所有任务均转化为 True/False 二分类，使用单 token 答案，降低了分析的普适性。对于多 token 或开放式生成答案的推理任务，如何定义内部一致性尚需探索。
- **校准偏差处理依赖数据集**：对每层预测进行中位数均衡需要数据集整体统计，部分依赖全局信息，在少量样本或流式场景下可能受限。
- **层权重学习需少量标注数据**：SC+IC (tune) 需要 500 条 held-out 样本，虽然迁移学习效果不错，但并非完全无监督。
- **可迁移性分析**：尽管跨任务迁移有效，但仅测试了从一个数据集到其他任务的迁移，未测试跨模型或跨领域的迁移鲁棒性。

（完）
