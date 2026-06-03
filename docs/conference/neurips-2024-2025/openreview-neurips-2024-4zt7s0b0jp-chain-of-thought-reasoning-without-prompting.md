---
title: Chain-of-Thought Reasoning Without Prompting
title_zh: 无需提示的思维链推理
authors: "Xuezhi Wang, Denny Zhou"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=4Zt7S0B0Jp"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 通过解码引发思维链路径，与忠实性评估相关
tldr: 通过改变解码方式无需提示即可引发思维链推理
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 510}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2048, \"height\": 570}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 950, \"height\": 562}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 1086, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-006.webp\", \"caption\": \"\", \"page\": 17, \"index\": 6, \"width\": 830, \"height\": 610}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-007.webp\", \"caption\": \"\", \"page\": 17, \"index\": 7, \"width\": 822, \"height\": 608}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-008.webp\", \"caption\": \"\", \"page\": 17, \"index\": 8, \"width\": 822, \"height\": 614}]"
motivation: 现有CoT方法依赖繁琐的提示工程。
method: 通过探索top-k替代token代替贪婪解码来引发CoT路径。
result: 发现CoT路径常存在于这些替代序列中。
conclusion: 无需提示即可评估LLM的CoT推理能力。
---

## Abstract
In enhancing the reasoning capabilities of large language models (LLMs), prior research primarily focuses on specific prompting techniques such as few-shot or zero-shot chain-of-thought (CoT) prompting. These methods, while effective, often involve manually intensive prompt engineering. Our study takes a novel approach by asking: Can LLMs reason effectively without any prompting? Our findings reveal that, intriguingly, CoT reasoning paths can be elicited from pre-trained LLMs by simply altering the \textit{decoding} process. Rather than conventional greedy decoding, we investigate the top-$k$ alternative tokens, uncovering that CoT paths are frequently inherent in these sequences. This approach not only bypasses the confounders of prompting but also allows us to assess the LLMs' \textit{intrinsic} reasoning abilities. Moreover, we observe that the presence of a CoT in the decoding path correlates with a higher confidence in the model's decoded answer. This confidence metric effectively differentiates between CoT and non-CoT paths. Extensive empirical studies on various reasoning benchmarks show that the proposed CoT-decoding effectively elicits reasoning capabilities from language models, which were previously obscured by standard greedy decoding.

---

## 论文详细总结（自动生成）

# 论文《Chain-of-Thought Reasoning without Prompting》中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有大语言模型（LLM）的推理能力主要通过两类方式激发：一是依赖人工设计的提示（如 few-shot 或 zero-shot chain-of-thought 提示），二是通过大量 CoT 监督数据对模型进行微调。前者需要繁琐的提示工程，且引入了人类先验，难以评估模型的**内在推理能力**；后者则成本高昂且依赖标注数据。
- **核心问题**：LLM 能否**完全不需要任何提示**，仅通过改变解码过程就能自发地进行推理？
- **主要发现**：在标准的问答格式下（`Q: [问题]\nA:`），经典贪婪解码往往无法产生 CoT 路径。然而，若在第一个解码步探索 top-k 替代 token，然后继续贪婪解码，**CoT 路径会自然出现在这些替代序列中**。这表明 LLM 在预训练后内部已经具备了推理能力，只是被贪婪解码所掩盖。

## 2. 论文提出的方法论

- **核心思想**：放弃贪婪解码，通过**改变解码策略**（探索 top-k 替代 token）来揭示模型内在的 CoT 推理路径，并使用**答案置信度**作为筛选指标。
- **关键技术细节**：
  - **CoT-decoding 流程**：
    1. 输入格式：`Q: [问题]\nA:`。
    2. 在第一个解码步骤，选取模型输出的 top-k 个 token（例如 k=10）。
    3. 对每一个 top-k token，继续采用贪婪解码生成完整的响应序列，共得到 k 条解码路径。
    4. 对每条路径，提取其中的答案（例如数学题中的最后一个数字，或选项“even/odd”）。
    5. 计算每个路径的**答案置信度** Δ。
  - **置信度 Δ 定义**（min-margin 方法）：
    \[
    \Delta_{k, \text{answer}} = \frac{1}{|\text{answer}|} \sum_{x_t \in \text{answer}} \left( p(x_t^1 | x_{<t}) - p(x_t^2 | x_{<t}) \right)
    \]
    其中 \(x_t^1\) 和 \(x_t^2\) 是第 t 个解码步中 top-2 的 token（在答案范围内的 token）。Δ 衡量模型在生成答案 token 时的**确定性**——差距越大，置信度越高。
  - **路径选择**：默认选取 Δ 最大的路径所对应的答案。也可使用**加权聚合**：取使得 \(\sum_k \Delta_{k,a}\) 最大的答案 a。
- **算法特点**：完全无监督，无需任何提示或微调，仅通过解码过程与 logits 计算即可实施。

## 3. 实验设计

- **数据集与场景**：
  - **数学推理**：GSM8K（多步数学应用题）、MultiArith（多步算术）。
  - **常识推理**：Year Parity（判断名人生于偶数年还是奇数年）。
  - **符号推理**（Big-Bench-Hard 子集）：Coin Flip（硬币翻转状态跟踪）、Web of Lies（谎言推理）、Multi-step Arithmetic（多步算术运算）、Sports Understanding、Object Counting。
- **Benchmark**：采用标准准确率（Accuracy）作为评价指标。
- **对比方法**：
  - 贪婪解码、top-k 采样、top-p（nucleus）采样、beam search、温度采样、自一致性（无 CoT 提示）、零样本 CoT 提示、自一致性+零样本 CoT 提示。
- **模型**：PaLM-2（XS/Small/Medium/Large 及指令微调版）、Mistral-7B（预训练 & 指令微调）、Gemma-7B。

## 4. 资源与算力

- **Mistral-7B 与 Gemma-7B**：使用 NVIDIA A100 GPU（40 GB RAM），每个任务约需 10–20 小时（取决于样本数）。
- **PaLM-2 系列**：使用 TPU v4，任务时长从数小时（小模型）到数天（大模型）不等。
- 文中未明确给出使用的 GPU/TPU 数量、总计算量等详细数字。

## 5. 实验数量与充分性

- **实验数量**：涵盖三大模型家族、多种规模、多个数据集/难度级别、多种解码对比、消融研究（k 值影响、分支步骤、聚合策略等），以及 CoT-decoding 与提示方法的组合实验。总计几十组实验。
- **充分性**：
  - **正面**：对比方法全面，覆盖了主流解码策略；在不同模型上趋势一致；消融实验详细（如 k 值从 1 到 50 的影响）。
  - **局限性**：部分符号推理任务（如 Coin Flip 的 3/4 轮、Web of Lies 的 5 轮）样本量较小（100 例或现有数据集）；Year Parity 只用了 100 个名人姓名，可能代表性不足。但整体上，实验设计客观、公平，结论可信。

## 6. 论文的主要结论与发现

- **结论 1**：LLM **无需任何提示**，仅通过探索 top-k 替代解码路径就能自发产生 CoT 推理，且这些路径常被贪婪解码所掩盖。
- **结论 2**：CoT 路径的存在与模型对最终答案的**高置信度**（Δ 值高）高度相关。利用此现象提出的 CoT-decoding 方法可可靠地筛选出 CoT 路径。
- **结论 3**：CoT-decoding 显著优于贪婪解码及其他标准解码方法（如温度采样、beam search），在 GSM8K 上预训练 Mistral-7B 准确率从 9.9% 提升至 25.1%；PaLM-2 Large 从 34.8% 提升至 63.2%。
- **结论 4**：CoT-decoding 可**缩小预训练模型与指令微调模型之间的推理差距**（部分任务上预训练+CoT-decoding 达到接近指令微调的性能）。
- **结论 5**：CoT-decoding 揭示了 LLM 内在的推理脆弱性：复杂符号任务（如多步状态跟踪、运算顺序遵循）中正确 CoT 路径难出现，表明模型在这些领域仍受限。

## 7. 优点

- **无需人工先验**：完全不需要提示或微调，真正评估模型的**内在推理能力**，避免了提示工程带来的偏见。
- **方法简单易行**：仅需改变解码第一步的 token 选择并计算 logits 差异，无需额外模型或训练。
- **可组合性强**：CoT-decoding 可与零样本 CoT 提示等现有方法结合，进一步提升性能。
- **具有诊断价值**：通过分析 CoT 路径出现的困难程度，可以定位模型在特定任务（如状态跟踪、运算顺序）上的弱点。

## 8. 不足与局限

- **计算成本较高**：需要解码 k 条路径（例如 k=10），相比贪婪解码增加了数十倍的计算开销。论文未讨论如何利用高效解码技术（如推测解码）来降低开销。
- **答案开放性限制**：对于答案非固定选项（如开放式生成）的任务，利用 top-2 token 概率差作为置信度指标可能不精确。论文虽提到了使用“So the answer is”等扩展方式，但在更开放的场景中仍需改进。
- **分支范围局限**：当前仅考虑在第一个解码步骤分支，后续步骤分支可能更优（尤其在任务需要中期修正时）。论文指出最优分支点可能因任务而异，但未系统探索。
- **依赖预训练分布**：对于高度合成或预训练中罕见的多步任务，正确的 CoT 路径可能完全不在 top-k 中，导致方法失效。这限制了方法在复杂符号推理上的适用性。
- **实验多样性不足**：部分符号推理任务样本量较小，且未涵盖更多真实世界复杂推理（如科学推理、逻辑谜题等）。此外，仅测试了三个模型家族，通用性有待更多模型验证。

（完）
