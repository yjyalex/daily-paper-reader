---
title: Reasoning Models Sometimes Output Illegible Chains of Thought
title_zh: 推理模型有时输出难以辨认的思考链
authors: Arun Jose
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=w1TjXJk846"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 评估推理模型思考链的可读性和忠实性
tldr: 发现最先进推理模型常产生不清晰思考链，威胁忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-w1tjxjk846/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1460, \"height\": 824}]"
motivation: 监测模型思考链需要可读和忠实，但当前模型可能生成混乱内容。
method: 系统性评估多个推理模型输出思考链的可读性。
result: 发现R1、R1-Zero、QwQ等模型常出现非法字符和乱码。
conclusion: 模型规模和训练范式影响思考链可读性。
---

## Abstract
Language models trained via outcome-based reinforcement learning (RL) to reason using chain-of-thought (CoT) have shown remarkable performance. Monitoring such a model's CoT may allow us to understand its intentions and detect potential malicious behavior. However, to be effective, this requires that CoTs are legible and faithful. We evaluate the legibility of CoTs in state-of-the-art reasoning models. We find that R1, R1-Zero, and QwQ often produce illegible CoTs (mixing nonsensical phrases, random words, and non-English characters) before returning to perfect coherence in their final responses, while Claude models notably exhibit higher legibility. Across 14 models, we observe that larger models within the same training paradigm tend to produce more illegible reasoning. Prefill experiments show that truncating reasoning at a legibility threshold reduces accuracy by 53\%, suggesting that illegible portions contribute to performance despite being difficult to monitor. Illegibility increases with question difficulty, suggesting that CoT monitoring may be less reliable precisely when most needed. We discuss potential hypotheses for these results, including steganography, vestigial tokens, and training artifacts. Our findings suggest that current approaches to CoT monitoring may be vulnerable to the emergence of outcome-based RL, particularly as models face increasingly complex tasks.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：基于结果强化学习（outcome-based RL）训练的推理模型（如DeepSeek R1、OpenAI o1等）通过思维链（Chain-of-Thought, CoT）进行推理，性能显著。监控CoT有助于理解模型意图和检测恶意行为，但这要求CoT**可读（legible）且忠实（faithful）**。然而，研究发现许多最先进推理模型（如R1、R1-Zero、QwQ）的CoT经常出现无意义短语、随机词汇和非英文字符混杂，变得难以辨认，而最终答案却清晰可辨。这一现象威胁了基于CoT的监控可靠性。
- **核心含义**：基于结果RL的训练可能导致模型产生难以监控的推理过程，尤其是在任务难度增加时，监控反而在最需要时最不可靠。论文揭示了当前CoT监控方法可能存在的脆弱性。

#### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：系统评估不同推理模型CoT的**可读性**，分析可读性与模型性能、问题难度、模型规模的关系，并通过实验探究不清晰推理是否对性能有因果贡献。
- **关键技术细节**：
  - **可读性评分**：使用GPT-4o作为自动评分器，对每个CoT按1-9分评分（1最可读，9最不可读）。验证了与人类评分的一致性（Cohen's weighted kappa = 0.782）。
  - **数据集与任务**：使用**GPQA-Diamond**（198个研究生级别科学多选题），移除选项使问题更难。
  - **模型评估**：测试14个模型，包括DeepSeek R1、R1-Zero、QwQ-32B、Qwen3-235B、Kimi K2、Claude系列（Sonnet 4/4.5、Opus 4/4.1、Haiku 4.5等），以及非推理模型DeepSeek V3和R1蒸馏模型。
  - **Prefill实验**：截断可读部分（当可读性得分≥6时停止），然后用截断后的前缀强制模型直接输出答案，与完整CoT对比性能。
  - **可读性与问题难度分析**：根据Claude 3.7 Sonnet的回答正确性将问题分为“容易”“中等”“难”，比较不同难度下CoT的可读性。
  - **每问题相关性分析**：对每个问题，用QwQ采样100次，计算可读性与正确性之间的Pearson相关系数（对CoT长度归一化处理）。
  - **温度效应**：在温度0下评估部分模型的可读性变化。

#### 3. 实验设计：数据集、基准、对比方法
- **数据集**：**GPQA-Diamond**（198个生物学、物理学、化学多选题），移除选项以增加难度。
- **基准（Benchmark）**：以可读性分数作为主要评估指标，辅助指标包括回答正确性（GPT-4o评判）和部分正确性。
- **对比方法**：
  - 对比14个模型（见图2、表2），包括推理模型、非推理模型、不同规模、不同训练范式（结果RL vs SFT vs 蒸馏）。
  - 对比截断CoT（prefill） vs 完整CoT的性能（图3）。
  - 对比不同难度问题下的可读性（图4）。
  - 对比不同采样温度（温度1 vs 温度0，附录C）。
- **额外样本**：引用OpenAI o1、o3、GPT-5的公开样本进行定性比较。

#### 4. 资源与算力
- **文中未明确说明使用的GPU型号、数量、训练时长等具体算力信息。**
- 仅提到通过OpenRouter（评估大多数模型）和Anthropic API（评估Claude模型）进行推理，未提及训练资源。

#### 5. 实验数量与充分性
- **实验数量**：
  - 主要可读性评估：对14个模型各在GPQA-Diamond上采样一次（或多次）并评分，呈现平均分和标准差。
  - Prefill实验：对QwQ进行截断对比（图3）。
  - 难度分析：对所有模型按问题难度分组比较（图4）。
  - 每问题相关性：对QwQ在每个问题上采样100次（图5）。
  - 时间序列分析：将CoT分块（每5000字符）分析可读性随时间变化（附录A，图6-7）。
  - 温度效应：对部分模型在温度0下重复评估（附录C，图8）。
- **充分性与客观性**：
  - 实验覆盖多个家族、多种规模，但仅限GPQA-Diamond一个数据集（科学问答），缺乏其他任务（如代码、数学、对话）的验证。
  - 无法系统评估OpenAI模型（CoT不公开），仅引用第三方样本，是一个局限。
  - 使用了自动评分器，虽然验证了与人类的一致性，但可能存在偏差。
  - Prefill实验可能因过早截断引入误差，作者提到已参考相关研究排除干扰。
  - 总体实验设计较为充分，但因果结论仍需更严格的控制实验。

#### 6. 论文的主要结论与发现
1. **多数推理模型产生不清晰CoT**：R1、R1-Zero、QwQ等模型常有高分数的不可读推理，但最终答案可读。Claude系列（Sonnet 4/4.5、Opus 4/4.1、Haiku 4.5）可读性显著更高。
2. **模型规模效应**：同一训练范式下，更大模型倾向于产生更不清晰的CoT（例如R1 671B比QwQ 32B更差），说明非能力局限所致。
3. **不清晰推理对性能有贡献**：Prefill实验显示截断不清晰部分后，正确率下降53%（从24.6%降至11.5%），说明不清晰部分并非无用。
4. **可读性与问题难度负相关**：问题越难，CoT越不清晰（图4）。
5. **可读性与正确性无一致相关**：对QwQ每问题100次采样，平均相关系数接近零（r=0.061），表明不清晰推理并不比清晰推理更高效。
6. **时间模式**：CoT可读性随时间递减（R1），后期出现更密集的压缩推理。
7. **温度效应**：R1在温度0下可读性显著提高，但R1-Zero不变，说明R1的清晰性训练主要锐化分布，而非改变机制。

#### 7. 优点
- **系统性评估**：涵盖14个主流模型，包括多个家族和规模，对比了不同训练范式。
- **使用自动评分器并验证可靠性**：GPT-4o评分与人类评分一致性较高（Cohen's kappa=0.782），降低了人工成本。
- **因果探察实验**：Prefill实验直接测试不清晰部分的因果贡献，而非仅观察相关性。
- **区分多个假设**：讨论了隐写术、遗留推理、训练伪像等三种假说，并基于证据评估其可能性。
- **发现温度效应和难度效应的实际意义**：指出监控在最需要时（困难问题）最不可靠，为安全研究提供警示。
- **提供公开代码和补充材料**：促进可重复性。

#### 8. 不足与局限
- **数据集单一**：仅使用GPQA-Diamond（单轮科学问答），无法代表现实监控任务（如多轮对话、代码生成、代理行为）。结论推广性有限。
- **无法系统评估OpenAI模型**：o1/o3/GPT-5的CoT未公开，仅引用第三方样本，缺乏定量比较。
- **未确定因果机制**：不清晰CoT为何仍然有用？作者提出了多个假说但未通过干预实验（如干预RL训练或进行消融）来区分。
- **自动评分器可能的偏差**：尽管验证了与人类一致性，但GPT-4o对Claude模型评分可能偏低（Claude平均分1.5左右），存在系统性偏差。
- **Prefill实验的局限性**：截断操作可能导致模型进入不熟悉的分布，影响结果，作者虽提及但未完全排除。
- **未探讨忠实性问题**：可读性不等于忠实性，论文仅关注可读性，未评估CoT是否真实反映了模型推理过程。
- **未解释Claude为何可读**：论文指出Claude模型可读性高，但未深入分析其训练技术，也未说明是否牺牲了性能或忠实性。

（完）
