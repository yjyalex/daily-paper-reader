---
title: Chain-of-Thought Reasoning Without Prompting
title_zh: 无需提示的思维链推理
authors: "Xuezhi Wang, Denny Zhou"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=4Zt7S0B0Jp"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 无需提示激发思维链推理
tldr: 表明通过改变解码而非提示可提取思维链推理路径。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 510}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2048, \"height\": 570}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 950, \"height\": 562}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 1086, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-006.webp\", \"caption\": \"\", \"page\": 17, \"index\": 6, \"width\": 830, \"height\": 610}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-007.webp\", \"caption\": \"\", \"page\": 17, \"index\": 7, \"width\": 822, \"height\": 608}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-4zt7s0b0jp/fig-008.webp\", \"caption\": \"\", \"page\": 17, \"index\": 8, \"width\": 822, \"height\": 614}]"
motivation: 现有CoT方法依赖手动提示工程。
method: 通过top-k替代解码而非贪婪解码来引出CoT路径。
result: 无需提示即可从预训练LLM中挖掘CoT推理。
conclusion: 解码过程本身蕴含CoT推理能力。
---

## Abstract
In enhancing the reasoning capabilities of large language models (LLMs), prior research primarily focuses on specific prompting techniques such as few-shot or zero-shot chain-of-thought (CoT) prompting. These methods, while effective, often involve manually intensive prompt engineering. Our study takes a novel approach by asking: Can LLMs reason effectively without any prompting? Our findings reveal that, intriguingly, CoT reasoning paths can be elicited from pre-trained LLMs by simply altering the \textit{decoding} process. Rather than conventional greedy decoding, we investigate the top-$k$ alternative tokens, uncovering that CoT paths are frequently inherent in these sequences. This approach not only bypasses the confounders of prompting but also allows us to assess the LLMs' \textit{intrinsic} reasoning abilities. Moreover, we observe that the presence of a CoT in the decoding path correlates with a higher confidence in the model's decoded answer. This confidence metric effectively differentiates between CoT and non-CoT paths. Extensive empirical studies on various reasoning benchmarks show that the proposed CoT-decoding effectively elicits reasoning capabilities from language models, which were previously obscured by standard greedy decoding.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有增强大语言模型（LLM）推理能力的方法主要依赖于特定的提示工程（prompting），如few-shot或zero-shot思维链（CoT）提示。这些方法虽然有效，但需要大量人工设计提示，且引入了人类先验，难以评估模型自身的推理能力。
- **核心问题**：LLM能否在不使用任何提示的情况下，仅通过改变解码过程就能有效进行推理？
- **整体意义**：作者发现，预训练LLM在标准贪婪解码时往往无法正确推理，但探索top-k替代token序列时，自然涌现出CoT推理路径。这表明LLM本身具备推理能力，只是被贪婪解码掩盖。该发现挑战了“LLM没有提示就无法推理”的主流观点，为无需提示的推理提供了新视角。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：不依赖任何提示（仅使用标准QA格式“Q: [question]\nA:”作为输入），通过修改解码过程（而非提示）来激发CoT推理路径。
- **关键技术细节**：
  - **CoT-decoding方法**：在第一个解码步骤，考虑top-k个替代token（k>0），而不是仅选择贪婪路径的最高概率token。之后继续使用贪婪解码生成完整输出。这样从不同起点出发，得到若干条解码路径。
  - **置信度指标（Δ）**：对于每条解码路径，计算最终答案token序列上模型概率的top-1与top-2之间的平均概率差。公式：Δ = (1/|answer|) Σ_{x_t in answer} [p(x_t^1 | x_<t) - p(x_t^2 | x_<t)]。该指标反映模型对解码答案的置信度。
  - **路径选择与聚合**：利用Δ值识别CoT路径：通常CoT路径的Δ显著高于非CoT路径。可直接选择Δ最高的路径作为输出，或对所有路径按答案聚合Δ：取使Σ Δ_{k,a}最大的答案a。
  - **与采样方法的区别**：直接采样难以产生CoT路径，因为模型倾向于输出直接答案。CoT-decoding通过强制第一个token多样性（top-k）来高效探索CoT空间。

## 3. 实验设计：数据集/场景、基准、对比方法
- **数据集**：
  - 数学推理：GSM8K（小学数学应用题）、MultiArith（多步算术）。
  - 常识推理：Year Parity（判断名人生日年份奇偶性）。
  - 符号推理：Coin Flip（硬币翻转状态追踪）、Web of Lies（真假陈述推理）、Multi-step Arithmetic（带括号的算术）。
  - Big-Bench-Hard相关任务：Sports Understanding、Object Counting等。
- **基准（benchmark）**：各数据集的标准准确率。
- **对比方法**：
  - 贪婪解码（greedy decoding）。
  - 多种采样解码：top-k采样、top-p（nucleus）采样、波束搜索、温度采样。
  - 无提示的自一致性（self-consistency without CoT prompt，采样10条路径后多数投票）。
  - 零样本CoT提示（zero-shot CoT prompting）、少样本CoT提示（few-shot CoT prompting），以及CoT-decoding与这些提示方法的组合。
- **模型**：PaLM-2（四个规模：X-Small, Small, Medium, Large；以及指令微调版本）、Mistral-7B（预训练版和指令微调版）、Gemma-7B。

## 4. 资源与算力
- 论文在附录中说明了计算资源：
  - Mistral-7B和Gemma-7B：使用A100 GPU（40GB显存），每个任务约10-20小时。
  - PaLM-2：使用TPU v4，根据模型规模和任务，耗时从几小时（小模型）到几天（大模型）。
- 未提供具体的GPU数量、总训练时长等详细信息，但指出实验规模合理。

## 5. 实验数量与充分性
- **实验数量**：覆盖多个模型家族（PaLM-2、Mistral、Gemma）、多种规模（PaLM-2的XS到Large）、多个推理任务（数学、常识、符号、合成任务）。包含消融实验（如k值影响、不同解码策略对比、路径聚合对比、与提示方法的结合等）。手动检查GSM8K前100个样本验证Δ与CoT的相关性（88%）。整体实验量较充分。
- **充分性和公平性**：
  - 对比了多种主流解码方法（贪婪、采样、波束搜索等），并列出了准确率（见表4）。
  - 对不同模型家族和规模进行了重复验证，结果一致。
  - 消融实验系统分析了k值、路径聚合的影响。
  - 对指令微调模型也做了实验，显示CoT-decoding仍能提升。
  - 不足之处：部分合成任务（如多步算术）使用了100个样本，规模较小；Year Parity任务依赖模型自身提取出生年份作为真实标签（可能引入偏差），但作者对此进行了说明和处理。

## 6. 论文的主要结论与发现
- **主要发现**：预训练LLM在不使用任何提示的情况下，通过改变解码过程（探索top-k替代token）可以自然产生CoT推理路径，且这些路径通常具有更高的答案置信度（Δ值大）。
- **性能提升**：CoT-decoding在多个数据集和模型上显著提升推理准确率，有时是贪婪解码的2-3倍。例如，Mistral-7B在GSM8K上从9.9%提升到25.1%，Year Parity从35.0%提升到66.0%。
- **无提示挖掘推理能力**：CoT-decoding能够更真实地评估模型的固有推理能力，不受人类提示干扰。在简单任务上（如一年级数学、常识问题），模型固有推理能力较强；在复杂或高度合成任务上，固有CoT路径较少，提示更多扮演“教学”角色。
- **置信度指标的有效性**：Δ值能可靠地区分CoT路径与非CoT路径，优于模型原始概率或长度归一化概率。
- **与提示的方法兼容**：CoT-decoding可与零样本CoT提示结合，进一步提升性能（如GSM8K上Mistral-7B从17.5%提升到40.2%）。

## 7. 优点
- **方法创新性**：首次提出无需提示、仅通过解码修改即可激发CoT推理，挑战了主流观点。方法简单、无需额外模型或训练。
- **去除了人为偏差**：避免了提示工程引入的人类先验，能更客观地评估模型固有的推理能力。
- **置信度指标的实用性**：Δ值简单有效，且与CoT路径强相关，可作为可靠选择路径的依据。
- **广泛的实验验证**：在多个模型家族、规模、任务上验证，结果一致，具有泛化性。
- **与现有方法兼容**：可轻松与零样本/少样本CoT提示结合，进一步提升性能。

## 8. 不足与局限
- **计算成本增加**：探索top-k路径需要多次解码，增加了计算开销（k通常取10，约10倍于贪婪解码）。虽然作者提到可通过未来研究优化效率，但目前成本较高。
- **对复杂任务效果有限**：在需要多步状态追踪或高度合成任务上，固有CoT路径较少，改善幅度有限，提示仍起主导作用。任务越困难，找到正确CoT路径的k值往往更大，效率下降。
- **答案识别依赖格式**：Δ计算需要识别答案跨度，目前采用提取最后数值或特定选项等方法，对于开放式答案可能不够精确。作者也承认这是局限。
- **仅分支第一步**：目前只在第一个token进行分支，后期分支可能找到更多正确路径，但计算成本更高。论文仅初步比较了后期分支的影响。
- **可能的不公平性风险**：Year Parity任务中，使用模型自身提取的出生年份作为真实标签，可能引入系统误差（模型若无法正确回忆年份，则无法评估）。作者已处理大部分情况，但仍存潜在偏差。
- **未深入探讨安全性或公平性影响**：虽然方法整体正面，但未讨论可能被误用或对社会的影响。

（完）
