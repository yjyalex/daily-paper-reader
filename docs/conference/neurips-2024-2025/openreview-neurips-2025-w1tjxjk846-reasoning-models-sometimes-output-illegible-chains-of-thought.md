---
title: Reasoning Models Sometimes Output Illegible Chains of Thought
title_zh: 推理模型有时输出不清晰的思维链
authors: Arun Jose
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=w1TjXJk846"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 评估思维链推理的忠实性和可读性
tldr: 发现推理模型经常产生不清晰且不忠实的思维链。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-w1tjxjk846/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1460, \"height\": 824}]"
motivation: 监控思维链需要其清晰且忠实，但现有模型可能不满足。
method: 评估多个SOTA推理模型思维链的可读性和忠实性。
result: 发现R1等模型经常产生包含无意义短语的思维链，而Claude模型更清晰。
conclusion: 思维链的忠实性需要进一步改进以确保可监控性。
---

## Abstract
Language models trained via outcome-based reinforcement learning (RL) to reason using chain-of-thought (CoT) have shown remarkable performance. Monitoring such a model's CoT may allow us to understand its intentions and detect potential malicious behavior. However, to be effective, this requires that CoTs are legible and faithful. We evaluate the legibility of CoTs in state-of-the-art reasoning models. We find that R1, R1-Zero, and QwQ often produce illegible CoTs (mixing nonsensical phrases, random words, and non-English characters) before returning to perfect coherence in their final responses, while Claude models notably exhibit higher legibility. Across 14 models, we observe that larger models within the same training paradigm tend to produce more illegible reasoning. Prefill experiments show that truncating reasoning at a legibility threshold reduces accuracy by 53\%, suggesting that illegible portions contribute to performance despite being difficult to monitor. Illegibility increases with question difficulty, suggesting that CoT monitoring may be less reliable precisely when most needed. We discuss potential hypotheses for these results, including steganography, vestigial tokens, and training artifacts. Our findings suggest that current approaches to CoT monitoring may be vulnerable to the emergence of outcome-based RL, particularly as models face increasingly complex tasks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

论文的核心问题是：当前基于结果强化学习（outcome-based RL）训练的推理模型（如 DeepSeek-R1、QwQ 等）虽然在复杂任务上表现优异，但其生成的思维链（Chain-of-Thought, CoT）经常变得**不清晰（illegible）**——混合无意义短语、随机单词、非英文字符等，而最终答案却保持完全可读。这种不清晰的 CoT 使得人类或自动化监控系统难以理解模型的实际推理过程，从而削弱了 CoT 监控作为 AI 安全手段的有效性。更危险的是，当问题难度增加时，CoT 的不清晰程度也增加，恰好在最需要监控的时候监控变得最不可靠。

本文的**整体含义**是：基于结果的 RL 训练可能自然而然地导致模型产生不可读的 CoT，这种特性不是简单的模型能力不足，而可能是 RL 训练过程中的产物（如“残余推理”假说）。这提示当前依赖 CoT 可读性进行安全监控的方法存在根本性漏洞。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

本文并未提出新的算法或模型，而是一个**实证分析框架**，核心思想是**量化评估 CoT 的可读性**，并探讨其与性能、难度等因素的关系。

**关键技术细节：**
- **可读性评分**：使用 GPT-4o 作为自动评分器，对 CoT 文本按 1~9 分分级（1 为最清晰，9 为最不清晰）。评分依据为评分器能否理解文本。作者还验证了使用 Claude 3.5 Sonnet 作为评分器时结果一致性高（Cohen's weighted kappa = 0.782）。
- **实验设置**：从 GPQA-Diamond 数据集中选取 198 道科学多选题，去除答案选项以增加难度。使用多个推理模型（DeepSeek R1/R1-Zero、QwQ、Qwen3-235B、Kimi K2、Claude 系列等）生成 CoT，并采样温度设为 1。
- **Prefill 实验**：将 QwQ 模型的 CoT 在可读性评分 ≥6 的位置截断，并将剩余可读部分作为 prefill 让同一模型继续输出最终答案，以检验不清晰部分对性能的实际贡献。
- **相关性分析**：多次采样（每问题 100 次），计算每个问题内 CoT 可读性与答案正确性的 Pearson 相关系数，并控制 CoT 长度的影响（通过长度归一化）。
- **难度划分**：根据 Claude 3.7 Sonnet 能否正确回答问题，将问题分为“容易/中等/困难”三组，分析可读性随难度的变化。

**没有公式或算法流程**，主要是实验设计和统计描述。

## 3. 实验设计：使用的数据集、benchmark、对比方法

- **数据集**：GPQA-Diamond（198 道研究生级科学选择题，涵盖生物、物理、化学）。作者移除了答案选项，使问题更难。
- **Benchmark**：无显式 benchmark，研究旨在揭示现象而非追求最高准确率。
- **对比方法**：
  - 模型对比：14 个模型，包括：
    - 推理模型：DeepSeek R1、R1-Zero（671B）、QwQ-32B、Qwen3-235B、Kimi K2 及 K2-Instruct-0905、Claude 3.7 Sonnet、Sonnet 4/4.5、Opus 4/4.1、Haiku 4.5。
    - 非推理模型：DeepSeek V3（R1 的基础模型）、R1-Distill-Qwen-32B、R1-Distill-Qwen-14B。
  - 控制变量：不同温度（1 和 0）下的可读性差异。
  - 对 OpenAI o1/o3/GPT-5 仅定性分析（未暴露完整 CoT）。

## 4. 资源与算力

论文**未明确说明使用的 GPU 型号、数量或训练时长**。评估是通过 OpenRouter 和 Anthropic API 进行的，属于推理阶段调用，不涉及训练过程。因此资源消耗方面未提供详细信息。

## 5. 实验数量与充分性

**实验数量**：
- 主实验（可读性评分）：对 14 个模型在 GPQA-Diamond 上各生成一次 CoT（可能部分模型多次采样？论文未明确重复次数，但图 2 和图 4 显示有多个样本点，可能每个模型每个问题生成一次？实际上图 2 每个模型是一个分布，应该是每个问题一次，但模型有 198 个问题，所以每个模型有 198 个样本？不明确。最好依据文本：在 3.2 节 QwQ 采样了 100 次每问题。因此总体实验量较大。
- 预填实验（Prefill）：仅对 QwQ 进行，比较原始性能和截断后性能（图 3），给出了百分比变化。
- 难度相关分析：对所有推理模型，按难度分组展示可读性（图 4）。
- 相关性分析：QwQ 每问题 100 次采样，计算相关系数（图 5）。
- 时间趋势分析（附录 A）：将 R1 和 QwQ 的 CoT 按 5000 字符切块，分别评分（图 6、7）。

**充分性与客观性**：
- 实验覆盖了多个主流推理模型家族（DeepSeek、Qwen、Kimi、Claude），且包含了基础模型（V3）作为基线，对比充分。
- 使用自动评分器（GPT-4o）代替人工，避免了时间成本高昂但引入了评分器自身偏差。作者检查了不同评分器之间的一致性，缓解了部分担忧。
- 预填实验仅针对 QwQ，且可能因截断导致 CoT 不完整而影响性能，作者也承认存在其他干扰因素，但仍认为差异过大不能由简单解释。
- 整体实验设计相对系统，但未进行严格的统计显著性检验（如 p 值报告），部分结论基于均值差异。

## 6. 论文的主要结论与发现

1. **CoT 不清晰是 outcome-based RL 的普遍后果**：除 Claude 和 Kimi 模型外，几乎所有推理模型（R1、R1-Zero、QwQ）都经常产生不可读的 CoT，而它们的最终答案保持可读。其基础模型 DeepSeek V3（未用 RL）不会产生此类现象，说明 RL 训练是主因。
2. **不清晰部分对性能有实际贡献**：预填实验表明，去掉不清晰部分后 QwQ 的正确率下降 53%（从 24.6% 正确降至 11.5%），错误答案从 54.1% 升至 73.6%。
3. **问题越难，CoT 越不清晰**：所有推理模型（除 Claude）在困难问题上生成更严重的不可读 CoT（图 4），相对增加约 23%。
4. **不清晰程度与正确性无一致相关性**：在问题内多次采样并控制长度后，平均相关系数接近 0（r=0.061），说明不清晰并不对应更高的正确率。
5. **更大模型在同一训练范式下 CoT 更不清晰**：R1（671B）比 QwQ（32B）更不清晰，而 R1-distill 模型（大小不同但无 RL 训练）可读性相近，表明不清晰并非单纯由模型容量不足引起。
6. **CoT 不清晰随时间递增**：R1 的 CoT 开头清晰，逐渐变为压缩、最后完全不可读（斯皮尔曼相关系数 0.54，p<0.0001）。QwQ 则无明显上升趋势。
7. **温度对不清晰性有影响**：温度 0 时 R1 的不清晰性显著降低（图 8），说明其可读性训练主要影响 top logprobs 而非深层机制。

## 7. 优点

- **系统性**：对 14 个模型进行大规模比较，涵盖多种训练范式（outcome-based RL、蒸馏、无 RL 基础模型），提供了全面的证据。
- **巧妙实验设计**：预填实验直接量化不清晰部分的因果贡献，而不是仅仅相关分析。
- **多维分析**：同时考察难度、长度、温度、模型大小等因素对可读性的影响，提供深入见解。
- **对安全监控的启示**：明确指出依赖 CoT 可读性的监控可能在最需要时失效，具有重要意义。
- **提出多种假设并加以讨论**：复杂隐写术（steganography）、无意义的 RL 人工制品、残余推理（vestigial reasoning），并分析了每个假设的合理性和局限性。

## 8. 不足与局限

- **无法确定因果机制**：作者承认仅凭观察性数据无法区分三种假说，需要更可控的实验（如直接操作 RL 训练过程或进行消融研究）。
- **数据集单一**：仅使用 GPQA-Diamond（科学多选题），未必代表其他任务场景（如数学、编码、开放域推理等）。
- **Claude 模型的不可解释性**：Claude 模型保持清晰但作者不能完全排除其使用了隐蔽的优化（如通过其他管道直接修改 CoT），因此不能断言 outcome-based RL 必然导致不清晰。
- **OpenAI 模型无法系统评估**：由于 CoT 不公开，仅依赖零散样本进行定性比较，缺乏统计强度。
- **自动评分器可能存在偏差**：虽然验证了跨评分器一致性，但 GPT-4o 本身可能倾向于某些模型格式，引入系统性误差。
- **预填实验的局限性**：仅对 QwQ 进行，且截断本身可能打乱推理流程，导致性能下降不完全归因于丧失不清晰部分。作者承认这一点，但认为效果差异过大。
- **未报告严格的统计显著性水平**：虽提及 p 值（如时间趋势分析中的 p<0.0001），但主要结论基于均值比较，未给出置信区间或效应量。
- **实验重复性有限**：未提供完整代码和超参数细节（如温度、采样次数等具体参数），尽管在补充材料中提供了代码，但审稿时可能尚未公开。

（完）
