---
title: Precise Information Control in Long-Form Text Generation
title_zh: 长文本生成中的精确信息控制
authors: "Jacqueline He, Howard Yen, Margaret Li, Shuyue Stella Li, Zhiyuan Zeng, Weijia Shi, Yulia Tsvetkov, Danqi Chen, Pang Wei Koh, Luke Zettlemoyer"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=yv7zKaptjo"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 研究语言模型生成中的忠实性幻觉问题
tldr: 提出精确信息控制任务和基准，评估和控制长文本生成的忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-yv7zkaptjo/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}]"
motivation: 语言模型容易出现忠实性幻觉，即生成与输入不一致的信息。
method: 定义精确信息控制任务，要求模型仅使用输入声明生成内容，并建立基准。
result: 提出的PIC基准可用于评估模型的忠实性。
conclusion: 精确信息控制是衡量忠实性的有效方法。
---

## Abstract
A central challenge in language models (LMs) is faithfulness hallucination: the generation of information unsubstantiated by input context. To study this problem, we propose Precise Information Control (PIC), a new task formulation that requires models to generate long-form outputs grounded in a provided set of short self-contained statements, without adding any unsupported ones. PIC includes a full setting that tests a model’s ability to include exactly all input claims, and a partial setting that requires the model to selectively incorporate only relevant claims. We present PIC-Bench, a benchmark of eight long-form generation tasks (e.g., summarization, biography generation) adapted to the PIC setting, where LMs are supplied with well-formed, verifiable input claims. Our evaluation of a range of open and proprietary LMs on PIC-Bench reveals that, surprisingly, state-of-the-art LMs still hallucinate against user-provided input in over 70% of generations. To alleviate this lack of faithfulness, we introduce a post-training framework that uses a weakly supervised preference data construction method to train an 8B PIC-LM with stronger PIC ability—improving from 69.1% to 91.0% F1 in the full PIC setting. When integrated into end-to-end factual generation pipelines, PIC-LM improves exact match recall by 17.1% on ambiguous QA with retrieval, and factual precision by 30.5% on a birthplace fact-checking task, underscoring the potential of precisely grounded generation.

---

## 论文详细总结（自动生成）

# 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：语言模型（LM）在长文本生成中普遍存在“忠实性幻觉”（faithfulness hallucination），即生成与输入上下文不吻合的内容，包括遗漏、扭曲或编造信息。现有研究多依赖二元判断，无法精细评估和控制。
- **研究动机**：在高价值场景（如科学文献合成、医疗指南生成、动态信息更新）中，模型必须严格基于用户提供的上下文，而非依赖过时的参数记忆。忠实性幻觉的消除对可信赖生成至关重要。
- **整体含义**：提出**精确信息控制（PIC）** 任务框架，将忠实性幻觉问题转化为对可验证声明（verifiable claims）的精细控制，要求模型在长文本输出中完全基于输入声明集，不得引入任何未支持的声明。

# 2. 方法论
- **核心思想**：PIC将输入和输出均分解为可验证声明，通过声明级别的匹配来评估模型是否严格遵循用户提供的上下文。根据任务需求分为**完全PIC**（必须包含所有输入声明，无遗漏无添加）和**部分PIC**（允许选择性包含相关声明，但不得添加新声明）。
- **关键技术细节**：
  - **声明提取与验证**：使用自动化LLM（GPT-4o mini）从输出中提取可验证声明，并与输入声明集进行语义等价性检查。验证器函数`support(c, S)`返回True/False。
  - **评估指标**：完全PIC使用F1（平衡精度与召回），部分PIC使用精度，同时报告完美分数比例（零忠实性幻觉）。
  - **PIC-LM训练框架**（两阶段）：
    1. **监督微调（SFT）**：在PIC格式的多任务数据（No Robots、FLAN、CNN摘要、传记生成、长问答）上训练，并过滤出高PIC分数的样本。
    2. **偏好优化**：使用长度归一化DPO。偏好数据通过弱监督方式构建：对原始声明集随机丢弃部分声明得到扰动上下文，用SFT模型生成扰动响应；计算原始与扰动响应的每token对数概率归一化差值，根据阈值τ选择偏好对。
  - **算法流程**：输入`(I, C)` → 生成响应 → 提取声明 → 与输入声明比较精度/召回/F1。偏好数据构造使用归一化对数概率下降分数`σ(log p_θ(y_orig|I)/L - log p_θ(y_perturb|I)/L)`，并基于阈值τ选择更优响应。

# 3. 实验设计
- **数据集/场景**：
  - **PIC-Bench**：包含8个长文本生成任务（6个完全PIC + 2个部分PIC）。完全PIC任务：EntityBios（传记）、PopBios-P/PopBios-CF（标准/反事实传记）、AskHistorians、ELI5、ExpertQA。部分PIC任务：FACTS Grounding、XSUM。样本量从111到200不等，平均输入声明数13.5-63.5。
  - **下游应用**：ASQA（检索增强生成QA）、Birthplace验证任务、QAMParI（多答案开放域QA）。
- **Benchmark与对比方法**：
  - **开源基线**：Llama 3.1 8B/70B Instruct、Tulu 3 8B/70B、Ministral 8B Instruct、Hermes 3 8B/70B；32B推理模型：Qwen 3 32B、QwQ 32B、R1-Qwen-32B（带思考模式）。
  - **闭源模型**：GPT-4o、Claude 3.5 Sonnet。
  - **上下文感知方法**：CAD（上下文感知解码）、SelfCite 8B。
  - **消融实验**：SFT数据过滤、跳过SFT阶段、不同损失函数（DPO、SimPO）、阈值τ的选择（0.1-1.0）、随机采样对比。

# 4. 资源与算力
- 论文提到训练使用**40GB NVIDIA L40和A40 GPU集群**，结合DeepSpeed Zero-3 Offload技术。
- 未明确说明GPU数量、训练总时长。但提及SFT训练2个epoch，DPO训练1个epoch，批量大小128，使用了AdamW优化器。
- 评估使用GPT-4o mini进行声明提取和验证，单次评估成本低于7美元。

# 5. 实验数量与充分性
- **实验组数**：主实验包括PIC-Bench上12+个模型的对比（全设置和部分设置），以及两处下游应用（RAG、验证管道）的评估。消融实验涵盖SFT过滤、训练顺序、损失函数、阈值、LLM judge选择、思考模式等。
- **样本统计**：PIC-Bench每个任务样本数150-200，与长文本事实性文献一致；所有主实验报告了95% bootstrap置信区间，结果稳定。
- **客观性与公平性**：采用统一提示模板、统一评估流水线（GPT-4o mini做声明提取和验证，有人类验证确认一致性）。对比方法包括多个主流开源和闭源模型，基线设置合理。
- **充分性**：实验覆盖多种任务类型、模型规模、训练策略，并验证了泛化能力（BioASQ医学QA）。消融系统，但未进行多次独立训练以报告方差（仅报告置信区间）。

# 6. 主要结论与发现
- **忠实性幻觉普遍存在**：即使是GPT-4o，在完全PIC下完美F1比例仅27.9%，部分PIC完美精度比例67.7%。超过70%的生成仍存在幻觉。
- **PIC-LM显著提升**：8B PIC-LM在完全PIC上平均F1达91.0%，超越所有开源模型（Llama 3.3 70B为82.5%），接近GPT-4o（90.5%）。部分PIC精度93.3%，超越所有开源模型。
- **反事实设置极具挑战**：PopBios-CF（声明与参数知识冲突）是所有模型中难度最高的任务，完美F1均为个位数。
- **下游受益**：PIC-LM集成到RAG和自验证管道能有效提升事实准确性（ASQA EM提升17.1%，birthplace事实精度提升30.5%）。
- **指令遵循保持**：PIC-LM的Prometheus指令遵循评分与基线持平或更优（平均3.92 vs 3.75），未出现简单拼接声明的问题。
- **领域泛化**：在BioASQ医学QA上，PIC-LM精度达97.5%，远高于基线（最高88.9%）。

# 7. 优点
- **任务定义精细**：将忠实性幻觉问题分解为声明级控制，提供了更细粒度、可验证的评估框架。
- **方法简洁高效**：弱监督偏好数据构造无需人工标注或昂贵API，仅利用模型自身对数概率差异，成本低且可扩展。
- **全面实验设计**：涵盖多个任务类型、模型对比、下游应用、消融分析、人类验证，结论可靠。
- **实用性证明**：PIC-LM作为模块化组件可嵌入RAG和验证管道中，有效改善事实准确性，具有实际应用价值。
- **开放性**：所有代码、数据、模型均开源，利于复现和后续研究。

# 8. 不足与局限
- **强输入假设**：PIC要求输入声明集well-formed（非空、无重复、无矛盾、任务相关），实际场景中难以自动获得，限制了直接应用。
- **样本量偏小**：每个评估任务样本数在150-200之间，虽与同类工作一致，但统计精度仍有限。部分区间较宽（如Claude 3.5在反事实任务上）。
- **不覆盖复杂推理和创造性生成**：任务集中在事实性传递，未评估多步逻辑、创意写作等更复杂场景。
- **潜在安全副作用**：PIC-LM更易遵循输入声明，若输入包含有害或错误声明，模型可能输出有害内容，降低了原有的拒绝能力。作者认为这是可控设计的权衡，但仍需谨慎。
- **评估依赖LLM**：声明提取和验证均使用GPT-4o mini，虽有人类验证一致性高（81%），但LLM的偏差可能影响指标。
- **缺乏训练计算细节**：未提供精确的GPU数量、训练时间，影响可复现的成本估算。

（完）
