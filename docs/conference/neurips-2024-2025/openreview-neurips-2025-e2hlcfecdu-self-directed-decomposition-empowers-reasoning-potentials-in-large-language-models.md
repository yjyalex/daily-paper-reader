---
title: Self-Directed Decomposition Empowers Reasoning Potentials in Large Language Models
title_zh: 自我指导分解释放大语言模型的推理潜力
authors: "Chuan Tian, Yilei Zhang"
date: 2025-05-10
pdf: "https://openreview.net/pdf?id=e2hlcfECdu"
tags: ["query:cot-unfaith"]
score: 5.0
evidence: 自我指导分解增强推理
tldr: 提出提示策略使大语言模型自主分解推理问题为子任务
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-e2hlcfecdu/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1452, \"height\": 829}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-e2hlcfecdu/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 921, \"height\": 846}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-e2hlcfecdu/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 1111, \"height\": 792}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-e2hlcfecdu/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 1095, \"height\": 532}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-e2hlcfecdu/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 1113, \"height\": 546}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-e2hlcfecdu/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 1102, \"height\": 469}]"
motivation: 大语言模型在逻辑连贯性方面仍存在困难。
method: 提出自我指导分解提示策略，让模型自主将推理问题分解为子任务。
result: 该方法在演绎、归纳、数学等推理任务上显著提升性能。
conclusion: 自主分解有助于增强大语言模型在不同推理领域的逻辑连贯性。
---

## Abstract
Large Language Models (LLMs) have demonstrated remarkable advancements in natural language processing and reasoning tasks, yet often struggle with logical coherence during problem-solving. This paper introduces Self-Directed Decomposition (SD), a novel prompting strategy enabling LLMs to autonomously decompose reasoning problems into manageable sub-tasks without human intervention, allowing models to determine their own approach with adaptive flexibility across diverse reasoning domains. Experiments across seven reasoning tasks reveal that this methodology particularly enhances performance on deductive, inductive, mathematical, commonsense, and scientific reasoning tasks, while showing more modest benefits for abductive and causal reasoning tasks, achieving 62.26\% overall median accuracy compared to 49.64\% and 46.43\% for zero-shot and zero-shot Chain-of-Thought (CoT) approaches, respectively. Error and statistical analysis demonstrates that SD significantly transforms reasoning patterns by reducing wrong selection errors but increasing process mistakes for simpler variants, with only SD1 maintaining optimal balance. We discover a counterintuitive negative correlation between token consumption and accuracy ($R^2 = 0.162, p = 0.004$), challenging conventional resource-performance assumptions. Abductive reasoning demonstrates critical vulnerability to decomposition strategies, showing significant perspective errors increase ($R^2 = 0.66$). These findings explain why SD1 outperforms other variants: it balances different error types effectively while avoiding the complexity-accuracy trade-off that affects simpler decomposition strategies.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，以下是对该论文的详细、结构化中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）

**核心问题：** 大型语言模型（LLMs）在解决复杂推理问题时，常常缺乏逻辑连贯性，容易出错。

**研究动机与背景：**
- 虽然零样本思维链（Zero-shot CoT）等提示策略能提升模型推理能力，但其生成步骤可能不准确、跳跃，导致错误。
- 现有更复杂的提示方法（如 Least-to-Most、Tree of Thoughts）通常依赖预定义的示例或人工设计的算法，限制了模型的自主性。
- 因此，论文旨在探索一种新的提示策略，让 LLM 能够**自主**地将复杂问题分解为更易于处理的子任务，从而提高推理的准确性和逻辑性，而不需要过多的人为干预或结构约束。

### 2. 论文提出的方法论：核心思想、关键技术细节

**核心思想：** **Self-Directed Decomposition (SD，自我指导分解)**。该方法通过特定的提示指令，引导 LLM 在解决问题时，自主地进行问题分解并分析，将复杂的推理过程拆解为一系列有序的子任务。

**关键技术细节：**
- 论文提出了四个不同详细程度的 SD 变体，以探究分解引导的复杂性对性能的影响：
    1.  **SD1：** 最详细的版本。使用了如“应该（should）”的情态动词和“子任务（sub-tasks）”等结构化术语，提供最全面的分解指导。例如：“当你处理此类问题时，你应该像子任务一样进行问题分解来分析问题。”
    2.  **SD2：** 缩短指令，保留核心分解指令。例如：“当你处理一个问题时，分解问题并解决它。”
    3.  **SD3：** 添加了礼貌标记“请（please）”，以探索情感情境的影响。例如：“请分解问题并解决它。”
    4.  **SD4：** 最简版本。例如：“分解并解决。”

**算法/流程（文字描述）：**
1.  向 LLM 输入一个问题。
2.  在问题之前，附加一个 SD 系列的提示指令（例如 SD1）。
3.  模型接收到指令后，自主决定如何将问题分解成若干子任务。
4.  模型按顺序或逻辑结构执行这些子任务，逐步推理出最终答案。
5.  评估最终答案的正确性。

### 3. 实验设计：使用了哪些数据集/场景，对比了哪些方法

**数据集/场景 (7类推理任务)：**
- **演绎推理：** bAbI (task 15), EntailmentBank
- **归纳推理：** CLUTRR, bAbI (task 16)
- **数学推理：** Mathematics, SVAMP
- **科学推理：** SciBench
- **常识推理：** CommonsenseQA, PiQA, Pep-3K
- **溯因推理：** αNLI, ART
- **因果推理：** E-care, Balanced-COPA

**Benchmark / 对比方法：**
- **Zero-shot：** 直接提问，无额外提示。
- **Zero-shot CoT：** 在问题前加入“让我们一步步思考”（Let’s think step-by-step）。
- **Emotional EP02：** 使用情感刺激提示“这对我的职业生涯非常重要”（This is very important to my career）。
- **SD1, SD2, SD3, SD4：** 四个论文提出的方法。

### 4. 资源与算力

- **模型：** 使用了 **GPT-3.5-turbo** (通过 API)。
- **算力细节：** **论文未明确提及**使用了多少 GPU、训练时长等具体算力信息。作者强调了使用较小模型是为了证明改进提示策略比增大模型规模更有效。由于是 API 调用，通常不涉及本地 GPU 训练资源。

### 5. 实验数量与充分性

**实验数量：**
- 从每个数据集中手动随机抽取了**10个任务**（SciBench数据集中抽取了40个任务）。每个策略在每个任务上只运行一次。
- 在7类推理任务的14个数据集上进行了测试。

**充分性评估：**
- **优点：** 覆盖了广泛的推理类型，评估比较全面。引入了详细的错误分析框架（5种错误类型），并进行了严格的统计检验（如 R²、p值）。
- **局限性：**
    - **样本量偏小：** 每个数据集只有10-40个样本，可能无法完全代表真实数据分布，结论的泛化性有待验证。
    - **单次运行：** 每个任务只运行一次，而 LLM 的输出具有随机性（尽管温度设为0），缺乏多次运行的平均结果，这增加了结果的偶然性。
    - **模型单一：** 仅测试了 GPT-3.5-turbo 一个模型，结论在其他模型上的普适性需要更多验证。
    - **无消融实验：** 没有对 SD1 中不同的提示成分（如情态动词、子任务）进行独立的消融分析，以证明每个组件的贡献。

### 6. 论文的主要结论与发现

1.  **SD1 性能最佳：** 在所有提示方法中，**SD1** 获得了最高的**中位数准确率（62.26%）**，显著优于 Zero-shot（49.64%）和 Zero-shot CoT（46.43%）。
2.  **Token-准确性悖论：** 发现了一个反直觉的现象：消耗的 Token 数量与最终准确率呈**显著负相关**（R²=0.162, p=0.004）。这意味着生成更长的推理过程并不一定会带来更好的结果，反而可能引入更多错误。
3.  **错误类型权衡（WS-PM权衡）：** SD 策略有效地减少了**错误选择（Wrong Selection, WS）**错误，但增加了**过程错误（Process Mistake, PM）**。这是分解式推理共有的代价。**SD1 成功地在减少 WS 错误的同时，没有显著增加 PM 错误，实现了最佳平衡**。
4.  **任务特异性：** SD 在**演绎、归纳、数学、常识和科学推理**任务中提升显著，但在溯因和因果推理中提升有限，甚至有害。
5.  **溯因推理的脆弱性：** 溯因推理对分解策略**存在关键脆弱性**。使用 SD 变体时，视角错误（Perspective Mistake, PPM）显著增加（R²=0.66），分解破坏了其需要同时考虑多个假设的“并行”推理结构。

### 7. 优点

- **方法新颖：** “自我指导分解”的概念鼓励模型的自主性，区别于依赖预定义示例或外部算法的被动方法，思路清晰且有启发性。
- **分析深入：** 不仅仅比较准确率，还引入了**详尽的错误分析框架**（5种错误类型），深入分析了 SD 策略如何影响模型的推理过程，揭示了性能提升的根本原因。
- **统计严谨：** 使用了 R²、p 值等统计指标来量化关系，使结论更具可信度。
- **发现了反直觉的现象：** Token-准确性悖论和溯因推理的脆弱性都是很有价值的发现，为后续研究提供了新的视角和方向。

### 8. 不足与局限

- **样本量有限：** 如前所述，每个数据集仅抽取少量样本，可能不足以得出稳健的结论。
- **模型泛化性差：** 结论基于单一模型（GPT-3.5-turbo），未在其他 LLM（如 GPT-4、LLaMA）上验证，限制了结论的普适性。
- **性能上限：** 在科学推理等复杂任务上，即使 SD1 也仅有 37.5% 的最高准确率，说明方法受限于模型自身能力，天花板明显。
- **缺乏代码开源：** 论文未公开实验代码，这在一定程度上影响了实验的可复现性。
- **未讨论偏差风险：** 没有分析该方法在不同类型输入或社会偏见下可能存在的偏差，安全性和公平性分析不足。

（完）
