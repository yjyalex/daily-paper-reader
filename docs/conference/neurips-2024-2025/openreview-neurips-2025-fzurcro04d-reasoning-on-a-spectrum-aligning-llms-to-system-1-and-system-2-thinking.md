---
title: "Reasoning on a Spectrum: Aligning LLMs to System 1 and System 2 Thinking"
title_zh: 频谱推理：使大语言模型对齐系统1和系统2思维
authors: "Alireza Salkhordeh Ziabari, Nona Ghazizadeh, Zhivar Sourati, Farzan Karimi Malekabadi, Payam Piray, Morteza Dehghani"
date: 2025-05-12
pdf: "https://openreview.net/pdf?id=FZURCro04D"
tags: ["query:cot-unfaith"]
score: 5.0
evidence: 将大语言模型对齐到系统1和系统2推理
tldr: 将大语言模型对齐到系统1和系统2推理以研究准确率-效率权衡
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 2968, \"height\": 684}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-002.webp\", \"caption\": \"\", \"page\": 7, \"index\": 2, \"width\": 800, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-003.webp\", \"caption\": \"\", \"page\": 8, \"index\": 3, \"width\": 2048, \"height\": 420}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 1980, \"height\": 476}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-005.webp\", \"caption\": \"\", \"page\": 17, \"index\": 5, \"width\": 2718, \"height\": 692}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-006.webp\", \"caption\": \"\", \"page\": 19, \"index\": 6, \"width\": 12380, \"height\": 3052}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-007.webp\", \"caption\": \"\", \"page\": 20, \"index\": 7, \"width\": 5760, \"height\": 1916}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-008.webp\", \"caption\": \"\", \"page\": 20, \"index\": 8, \"width\": 7261, \"height\": 2415}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-009.webp\", \"caption\": \"\", \"page\": 21, \"index\": 9, \"width\": 4830, \"height\": 2055}]"
motivation: 大语言模型依赖逐步推理导致在非结构化任务上表现脆弱。
method: 构建包含两种推理风格有效回答的数据集，显式使模型与系统1或系统2推理对齐。
result: 系统2对齐模型在算术和符号推理上更优，系统1对齐模型在常识任务上更优。
conclusion: 根据任务特点灵活选择推理风格可提升模型表现。
---

## Abstract
Large language models (LLMs) demonstrate remarkable reasoning capabilities, yet their reliance on step-by-step reasoning can make them brittle when tasks do not align with such structured approaches. In contrast, human cognition flexibly alternates between fast, intuitive reasoning (System 1) and slow, analytical reasoning (System 2), depending on context. To bridge this gap, we curate a dataset of 2K examples, each with valid responses from both reasoning styles, and explicitly align LLMs with System 1 and System 2 reasoning. Evaluations across diverse reasoning benchmarks reveal an accuracy-efficiency trade-off: System 2-aligned models excel in arithmetic and symbolic reasoning, while System 1-aligned models perform better in commonsense tasks. A mechanistic analysis of model responses shows that System 1 models employ more definitive answers, whereas System 2 models demonstrate greater uncertainty. Interpolating between these extremes produces a monotonic transition in reasoning accuracy, preserving coherence. This work challenges the assumption that step-by-step reasoning is always optimal and highlights the need for adapting reasoning strategies based on task demands.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大语言模型（LLM）依赖逐步推理（如Chain-of-Thought）在非结构化任务上表现脆弱，而人类认知能根据任务灵活切换快速直觉（系统1）和慢速分析（系统2）两种模式。当前LLM开发隐式假设结构化逐步推理总是最优，但忽略了任务适配性。
- **整体含义**：本文旨在通过显式地将LLM对齐到系统1和系统2推理，研究两种推理范式在不同任务上的优劣势，挑战“逐步推理始终最优”的假设，并探索推理策略应根据任务需求自适应选择的理念。
- **背景**：借鉴认知心理学中双过程理论（System 1 / System 2）及神经科学中的习惯性与目标导向决策的频谱框架，将LLM推理视为可调的连续谱而非二元对立。

## 2. 论文提出的方法论

### 核心思想
- 构建一个包含2000个推理问题的数据集，每个问题同时附带有效的系统1（启发式、直觉）和系统2（审慎、逐步）回答。通过偏好优化（DPO/SimPO）显式地将基础指令微调模型对齐到其中一种推理风格（或混合比例），从而研究不同对齐方式对下游任务性能的影响。

### 关键技术细节
- **数据构建**：
  - **生成阶段**：采用人工参与（human-in-the-loop）流程，使用GPT-4o基于专家编写的10种认知启发（如锚定效应、光环效应、过度自信等）的种子示例，通过单样本提示（one-shot）生成新的问题及对应的系统1和系统2回答。
  - **精炼阶段**：由于系统2回答天然更长，为避免对齐时模型偏好长回答，使用GPT-4o零样本提示将系统1和系统2回答的长度调整至大致相等（最终平均82.19 vs 83.93 tokens），并通过TOST等价检验确认长度差异消除。
  - **验证阶段**：领域专家手动修正约20%的回答，并通过主题建模（BERTopic）验证数据涵盖广泛主题。

- **对齐方法**：
  - 使用离线偏好优化方法：**DPO**（Direct Preference Optimization）和**SimPO**（Simple Preference Optimization）。
  - 对于系统1对齐：将系统1回答设为”赢家“，系统2设为”输家“；系统2对齐则相反。
  - 同时训练7个插值模型，其中赢家回答在系统1和系统2之间按不同比例混合（如0%系统1、20%系统1、...、100%系统1），以研究推理风格的连续过渡。

### 算法流程
- 步骤1：收集问题 → 步骤2：为每个问题生成系统1和系统2回答（GPT-4o + 专家种子） → 步骤3：长度调整 → 步骤4：专家验证 → 步骤5：使用偏好优化训练模型（DPO/SimPO，LoRA适配） → 步骤6：在13个基准上评估。

## 3. 实验设计

### 数据集与基准
- **训练数据**：自建的2000个问题数据集（80%训练，20%验证）。
- **测试基准（13个）**：
  - **算术推理**：MultiArith, GSM8K, AddSub, AQUA-RAT, SingleEq, SVAMP
  - **常识推理**：CSQA, StrategyQA, PIQA, SIQA, COM2SENSE
  - **符号推理**：Last Letter Concatenation, Coin Flip

### 对比方法
- **基础模型**：Llama-3-8B-Instruct 和 Mistral-7B-Instruct-v0.1（指令微调版本）
- **基线**：零样本直接回答 + 零样本CoT提示
- **对齐模型变体**：系统1-DPO、系统1-SimPO、系统2-DPO、系统2-SimPO
- **插值模型**：7个不同比例的系统1/系统2混合对齐模型

### 评估方式
- 两阶段评估：先让模型生成推理过程，再使用基准特定指令提取最终答案（精确匹配准确率）。
- 额外分析：输出长度差异、token级不确定性（logits）、hedge词比例、回答决定性（LLM-as-Judge）。

## 4. 资源与算力

- **GPU型号**：NVIDIA RTX A6000（48GB RAM）
- **训练时长**：总计约800 GPU小时
- **软件环境**：Python 3.10.12, PEFT 0.12.0, PyTorch 2.4.0, Transformers 4.44.2
- **具体配置**：
  - LoRA: rank=8, alpha=16, dropout=0.1
  - 训练5个epoch，batch size: 训练4，验证8
  - 学习率：DPO（Llama 7e-7，Mistral 5e-7），SimPO（Llama 1e-6，Mistral 5e-7）

## 5. 实验数量与充分性

- **实验数量**：大量实验覆盖多个维度——
  - 2种基础模型 × 2种对齐方法 × 2种对齐风格（系统1/系统2）= 8组主要对齐实验
  - 插值实验：7个混合比例模型 × 2种基础模型 = 14组
  - 所有模型在13个基准上评估，共对比数百个准确率数据点
  - 不确定性分析：token概率、hedge词、答辩决定性（使用Phi4做判断器）
- **充分性与公平性**：
  - 控制长度偏差：通过长度调整消除了系统2回答更长的混淆因素。
  - 统计检验充分：使用独立样本t检验、配对t检验、McNemar检验、TOST等价检验、Mann-Whitney U检验等。
  - 对比基线合理：不仅对比指令微调模型，还对比了CoT提示，确保与当前主流方法比较。
  - 实验设计客观：所有超参数、数据分割、评估流程均详细披露，代码和数据集公开。
- **不足**：仅使用两种模型（Llama、Mistral）和两种对齐方法（DPO、SimPO），未涵盖更大规模模型或其他偏好优化方法；数据集仅覆盖10种认知启发，可能不全面。

## 6. 论文的主要结论与发现

1. **准确率-效率权衡**：系统2对齐模型在算术和符号推理上显著优于系统1模型和基线；系统1对齐模型在常识推理上显著优于系统2模型和基线。证实两种推理风格各有优势，而非单一模式通用。
2. **输出长度差异**：系统2模型生成更长的回答（尤其是在最终答案提取阶段），即使训练时已控制长度偏差。这暗示系统2的审慎性会导致多余输出，而系统1更简洁高效。
3. **推理风格的连续过渡**：从纯系统1到纯系统2插值模型的准确率在所有基准上呈现单调平滑变化（r²>0.9），说明LLM推理处于可调连续谱而非离散二分。
4. **不确定性分析**：
   - 系统2模型的token级log概率更低（更不确定），使用更多的hedge词（如“可能”“通常”）。
   - 系统1模型在常识任务上更快做出决定性回答，而在算术任务上两类模型差异不显著。
5. **失败模式**：系统2在需要高数值精度的题目上更可靠（如小数计算）；系统1在常识任务上更符合人类直觉，系统2常过度解析导致错误答案（如把“乐器”理解为隐喻）。
6. **挑战主流假设**：逐步推理并非总是最优，应根据任务需求自适应选择推理风格。

## 7. 优点

- **方法创新**：首次系统性地通过偏好对齐来显式引导LLM采用特定推理风格（系统1或系统2），而非仅仅评估已有推理行为。
- **控制变量严谨**：通过长度调整和多种统计检验确保实验公平，避免模型偏好长回答的偏差。
- **评估全面**：覆盖算术、常识、符号三种推理类型，并深入进行输出长度、不确定性、决定性等机械分析。
- **可解释性**：量化了系统1和系统2在不确定性、用词、回答果断性等方面的差异，结论与认知科学理论吻合。
- **实用性**：表明可以通过调整对齐数据中系统1/系统2比例来精细控制模型的推理风格，为自适应推理应用提供基础。

## 8. 不足与局限

- **数据集规模与覆盖有限**：仅2000示例，覆盖10种认知启发，可能不足以代表真实世界中所有推理场景。主题建模虽显示多样性，但样本量较小。
- **模型与对齐方法泛化性**：仅测试Llama-3-8B和Mistral-7B两种中等规模模型，以及DPO和SimPO两种离线方法。未验证大型模型或其他偏好优化（如RLHF）的表现。
- **不确定性度量手段**：采用token log概率和词汇级hedge分析作为代理，可能丢失更深层的推理不确定性（如置信度标定、多路径一致性）。
- **任务范围**：未涉及更复杂的动态决策、多轮对话、计划生成等场景，结果推广至这些领域需谨慎。
- **潜在偏差**：数据集由GPT-4o生成并人工修正，可能引入生成模型的固有偏见；专家修正仅20%，其余依赖自动生成质量。
- **伦理考量**：系统1模型可能过度自信导致错误，系统2模型可能过于谨慎延迟响应，在安全敏感场景需平衡。论文未深入讨论具体部署风险。
- **计算成本**：800 GPU小时对于大规模拓展可能较高，实验未探索更轻量的对齐方式（如提示工程）。

（完）
