---
title: "Reasoning on a Spectrum: Aligning LLMs to System 1 and System 2 Thinking"
title_zh: 推理光谱：将LLM对齐到系统1和系统2思维
authors: "Alireza Salkhordeh Ziabari, Nona Ghazizadeh, Zhivar Sourati, Farzan Karimi Malekabadi, Payam Piray, Morteza Dehghani"
date: 2025-05-12
pdf: "https://openreview.net/pdf?id=FZURCro04D"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 将LLM对齐到系统1和系统2思维，在推理基准上评估
tldr: 将LLM对齐到两种推理风格，展示了准确率与效率的权衡。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 2968, \"height\": 684}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-002.webp\", \"caption\": \"\", \"page\": 7, \"index\": 2, \"width\": 800, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-003.webp\", \"caption\": \"\", \"page\": 8, \"index\": 3, \"width\": 2048, \"height\": 420}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 1980, \"height\": 476}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-005.webp\", \"caption\": \"\", \"page\": 17, \"index\": 5, \"width\": 2718, \"height\": 692}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-006.webp\", \"caption\": \"\", \"page\": 19, \"index\": 6, \"width\": 12380, \"height\": 3052}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-007.webp\", \"caption\": \"\", \"page\": 20, \"index\": 7, \"width\": 5760, \"height\": 1916}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-008.webp\", \"caption\": \"\", \"page\": 20, \"index\": 8, \"width\": 7261, \"height\": 2415}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-fzurcro04d/fig-009.webp\", \"caption\": \"\", \"page\": 21, \"index\": 9, \"width\": 4830, \"height\": 2055}]"
motivation: LLM依赖逐步推理导致脆弱性，需灵活切换推理风格。
method: 收集2000个包含两种推理风格答案的样本，并对LLM进行对齐训练。
result: 系统2对齐的模型在算术和符号推理上表现更好，系统1对齐的模型在常识任务上更优。
conclusion: 不同推理风格有各自的适用场景，需要根据任务灵活选择。
---

## Abstract
Large language models (LLMs) demonstrate remarkable reasoning capabilities, yet their reliance on step-by-step reasoning can make them brittle when tasks do not align with such structured approaches. In contrast, human cognition flexibly alternates between fast, intuitive reasoning (System 1) and slow, analytical reasoning (System 2), depending on context. To bridge this gap, we curate a dataset of 2K examples, each with valid responses from both reasoning styles, and explicitly align LLMs with System 1 and System 2 reasoning. Evaluations across diverse reasoning benchmarks reveal an accuracy-efficiency trade-off: System 2-aligned models excel in arithmetic and symbolic reasoning, while System 1-aligned models perform better in commonsense tasks. A mechanistic analysis of model responses shows that System 1 models employ more definitive answers, whereas System 2 models demonstrate greater uncertainty. Interpolating between these extremes produces a monotonic transition in reasoning accuracy, preserving coherence. This work challenges the assumption that step-by-step reasoning is always optimal and highlights the need for adapting reasoning strategies based on task demands.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前大型语言模型（LLMs）在许多推理任务上表现出色，但主要依赖逐步、结构化的推理（如 Chain-of-Thought）。这种单一模式在需要灵活应对的任务（如常识判断）中显得脆弱，甚至出现“过度思考”（overthinking）现象。相比之下，人类认知能够根据情境灵活切换快速直觉推理（系统1）和慢速分析推理（系统2），实现效率与准确性的平衡。
- **核心问题**：LLMs 是否也能模拟这种双过程推理？不同的推理风格是否在不同任务中各有优势？系统1（快速、启发式）和系统2（慢速、分析式）在 LLMs 中如何对齐并影响性能？
- **整体含义**：挑战“逐步推理总是最优”的假设，提出应根据任务需求适配推理策略，从而开发更灵活、高效的 LLM 推理系统。

### 2. 论文提出的方法论：核心思想、关键技术细节

**核心思想**：通过偏好优化（Preference Optimization）将 LLMs 显式地对齐到系统1或系统2的推理风格，研究不同对齐方式对多种推理任务的影响。

**关键技术细节**：
- **数据集构建**：构建包含2000个问题的数据集，每个问题对应一个系统1（启发式、直觉）和一个系统2（分析、逐步）的答案。这些答案基于10种认知启发式（如锚定效应、光环效应等），由领域专家设计种子示例，并利用 GPT-4o 扩展，经人工校验和长度平衡（消除因长度差异导致的偏好偏差）后得到。
- **对齐方法**：使用两种离线偏好优化算法：**DPO**（Direct Preference Optimization）和 **SimPO**（Simple Preference Optimization）。将系统1答案作为偏好对中的“赢家”训练系统1对齐模型，将系统2答案作为“赢家”训练系统2对齐模型。通过 LoRA 进行参数高效微调。
- **推理光谱插值**：为研究从系统1到系统2的连续过渡，训练了7个中间模型，按不同比例混合系统1和系统2作为偏好答案，观察性能的变化规律。

### 3. 实验设计：数据集、Benchmark、对比方法

- **数据集**：13个推理基准，分为三类：
  - 算术推理：MultiArith, GSM8K, AddSub, AQUA-RAT, SingleEq, SVAMP
  - 常识推理：CSQA, StrategyQA, PIQA, SIQA, COM2SENSE
  - 符号推理：Last Letter Concatenation, Coin Flip
- **对比方法**：系统1对齐模型、系统2对齐模型 vs. 基础指令调优模型（Llama-3-8B-Instruct、Mistral-7B-Instruct-v0.1）及其零样本和零样本 CoT 提示变体。
- **评估方式**：两阶段评估：先让模型自由回答，再以原问题+模型回答+基准特定指令进行格式化输出，计算精确匹配准确率。

### 4. 资源与算力

- 论文在附录L中说明：实验使用 NVIDIA RTX A6000 GPU（48GB RAM）进行，总计算时间约 800 GPU 小时。训练基于 PyTorch 2.4.0，使用 LoRA（rank=8, alpha=16, dropout=0.1），训练5个epoch，early stopping。

### 5. 实验数量与充分性

- **实验数量**：在13个基准上评估了两种模型（Llama 和 Mistral）的两种对齐方法（DPO 和 SimPO），共约 13×2×2 = 52 组主实验；另外还进行了长度分析、光谱插值（7个中间模型×13基准）、不确定性分析（token概率、修饰词、确定性判断）和错误模式分析（AddSub、CSQA示例）。
- **充分性判断**：实验覆盖了三种不同类型的推理任务，对比了主流基线（含CoT），并进行了统计检验（t检验、Mann-Whitney U检验、McNemar检验等），结果呈现了一致的趋势。但仅使用了两种模型（8B和7B）和两种对齐方法，未在更大模型或更多对齐方法上验证，可能存在泛化性局限。整体而言实验设计较为系统，但对于完全结论的充分性尚可，但广度可提升。

### 6. 论文的主要结论与发现

- **准确率-效率权衡**：系统2对齐模型在算术和符号推理上显著优于系统1模型；系统1对齐模型在常识推理上胜过系统2模型和CoT基线。
- **推理长度差异**：系统2模型生成更长、更详细的响应（尤其在第二阶段），即使训练时控制了长度；系统1模型更简洁。
- **光谱过渡**：从系统1到系统2的插值模型显示单调、平滑的准确率变化（所有基准的 R² > 0.9），表明推理风格是可调连续谱，而非二分。
- **不确定性与确定性**：系统2模型在token概率、修饰词使用上表现出更高的不确定性；系统1模型更早做出确定性的回答（尤其在常识任务中）。
- **失败模式**：系统2在算术中更擅长处理高精度计算；在常识中易过度分析导致答案偏离常规人类判断。

### 7. 优点

- **创新的研究角度**：将认知科学的双过程理论系统性地应用于LLM对齐，挑战了“逐步推理总是最优”的主流观点。
- **精致的数据集构建**：基于10种认知启发式，由专家生成种子并利用LLM扩展，再经均衡长度和人工校验，为后续研究提供了可靠资源。
- **完整的实验设计**：不仅对比系统1/2与基线，还分析了光谱插值、不确定性、错误模式，提供了深层次的机制理解。
- **统计严谨性**：对主要结果进行了统计检验，确认了差异的显著性。
- **实际启示**：揭示了任务适配推理策略的重要性，为 LLM 的灵活推理和效率优化提供了指导。

### 8. 不足与局限

- **数据集规模有限**：仅2000个样本，涵盖10种启发式，未能覆盖更广泛的推理场景；作者自承未来需要更大规模。
- **模型与对齐方法泛化性**：仅在两种8B/7B模型和两种对齐方法上验证，未测试更大模型（如70B）或其他偏好优化方法（如RLHF），通用性存疑。
- **评估指标简化**：主要使用精确匹配准确率，未系统评测响应质量、流畅性或实际应用中的动态决策表现。
- **不确定性测量依赖代理信号**：使用token概率和修饰词分析，可能无法完全反映内在不确定性。
- **对齐可能导致偏见或错误**：系统1模型可能过于自信而犯错，系统2模型可能过度思考导致效率下降，文中伦理声明也承认了这些风险。
- **未探索动态切换**：当前研究是静态对齐（全系统1或全系统2），未实现任务感知的动态切换，这是未来方向。

（完）
