---
title: Think or Not? Exploring Thinking Efficiency in Large Reasoning Models via an Information-Theoretic Lens
title_zh: 思考与否？通过信息论视角探索大型推理模型的思考效率
authors: "Xixian Yong, Xiao Zhou, Yingying Zhang, Jinlin Li, Yefeng Zheng, Xian Wu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=DpOSndSOZz"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 提出量化推理路径分歧和逐步贡献的指标
tldr: InfoBias和InfoGain指标揭示长推理链的低效和偏差
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-dposndsozz/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 513, \"height\": 321}]"
motivation: 大型推理模型生成的推理链过长且效率低下。
method: 提出信息偏差和信息增益两个指标量化推理效率。
result: 发现长推理链信息偏差增大、信息增益递减。
conclusion: 应避免不必要的冗长推理。
---

## Abstract
The recent rise of Large Reasoning Models (LRMs) has significantly improved multi-step reasoning performance, but often at the cost of generating excessively long reasoning chains. This paper revisits the efficiency of such reasoning processes through an information-theoretic lens, revealing a fundamental trade-off between reasoning length and semantic efficiency. We propose two metrics—InfoBias and InfoGain—to quantify divergence from ideal reasoning paths and stepwise information contribution, respectively. Empirical analyses show that longer reasoning chains tend to exhibit higher information bias and diminishing information gain, especially for incorrect answers. Motivated by these findings, we introduce an entropy-based Adaptive Think strategy that dynamically halts reasoning once confidence is sufficiently high, improving efficiency while maintaining competitive accuracy. Compared to the Vanilla Think approach (default mode), our strategy yields a 1.10% improvement in average accuracy and a 50.80% reduction in token usage on QwQ-32B across six benchmark tasks spanning diverse reasoning types and difficulty levels, demonstrating superior efficiency and reasoning performance. These results underscore the promise of entropy-based methods for enhancing both accuracy and cost-effiiciency in large language model deployment.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机和背景）
大型推理模型（LRMs）虽显著提升了多步推理能力，但往往生成过长的推理链，导致计算复杂度呈二次增长，且不符合人类认知经济原则。作者从香农通信三层面解释低效：技术层面，冗余位引入噪声；语义层面，信息增益递减；语用层面，收益递减且可能降低性能。核心问题：能否优化LRM推理模式，在保持性能的同时大幅缩短推理链？

### 2. 方法论
**核心思想**：采用信息论视角量化推理效率，提出两个度量指标，并基于熵设计自适应停止策略。

**关键技术细节**：
- **语义分割**：将输出推理路径分割为离散语义单元（如按段落或句子边界）。
- **InfoBias**：基于互信息衡量生成轨迹与理想轨迹的整体偏差。通过HSIC估计，并给出基于KL估计的上界，保证统计一致性。
- **InfoGain**：每步信息增益定义为熵减\(\Delta I_i = H_{i-1} - H_i\)，反映每步对答案不确定性的降低。还定义针对正确答案的目标信息增益。
- **Adaptive Think策略**：每步计算答案空间平均熵\(H_{avg}^i\)，若低于阈值\(\alpha \cdot \frac{1}{e \ln 2}\)则停止推理并直接输出答案。\(\alpha \in [0,1]\)控制严格程度。

### 3. 实验设计
- **数据集/场景**：6个基准，涵盖数学（GSM8K、AIME2025）、知识（MMLU-Pro）、叙事理解（MuSR）、逻辑（ProntoQA）、常识（CommonsenseQA）。
- **对比方法**：Vanilla Think（默认）、No-Think（强制跳过思考）、Gated Think（基于规则判断是否需要思考）、Adaptive Think（本文方法）。
- **评估模型**：3个推理模型（QwQ-32B, DeepSeek-R1-Distill-7B/32B）和5个标准模型（LLaMA3.1-8B, Phi-4, Qwen2.5-7B/32B, Yi-1.5-34B）。
- **评估指标**：准确率（Acc↑）和平均token数（#Token↓）。

### 4. 资源与算力
文中未明确说明使用的GPU型号、数量及训练时长。实验使用vLLM推理引擎进行高效推理，所有结果基于5次独立运行取平均。算力信息缺失，无法量化具体资源消耗。

### 5. 实验数量与充分性
实验较为充分：
- **主实验**：表1（数学）和表2（其他推理）覆盖所有模型与策略的对比，共约8类模型×4种策略×6个数据集=192组实验（不含非推理模型基线）。
- **参数分析**：图6展示了α从0.1到1.0对准确率和token数的影响，涵盖4个数据集。
- **决策分析**：图5展示了Gate Think模式下“思考”与“不思考”的比例，以及Adaptive Think的token使用。
- **消融分析**：Gate Think与Vanilla Think、No-Think的对比可视为部分消融。
- 实验考虑了不同难度、不同推理类型，多次运行取平均，保证统计意义。但部分数据集（如AIME2025）样本量较小（可能只有几十题），公平性上整体较好。

### 6. 主要结论与发现
1. **InfoBias与推理长度正相关**：更长推理链累积更大偏差，错误答案偏差更高、长度更不稳定。
2. **InfoGain递减**：随步数增加，每步信息增益迅速减小；模型在开始推理前已对正确答案有较高直觉信心。
3. **Adaptive Think效果显著**：相比Vanilla Think，在QwQ-32B上平均准确率提升1.10%，token使用减少50.80%（数学任务减少58.78%，非数学减少42.39%）。在多个基准上均优于No-Think和Gated Think。
4. **任务自适应能力**：模型能在困难任务（如AIME2025）上更多思考，在简单任务（如CommonsenseQA）上快速停止，体现动态调节能力。

### 7. 优点
- **理论创新**：从信息论角度系统分析推理效率，提出可量化、可解释的InfoBias和InfoGain指标。
- **方法简洁有效**：Adaptive Think不修改模型架构，仅基于熵阈值控制停止，实现显著加速且保持/提升准确率。
- **实验全面**：覆盖多种推理模型、标准模型、及多种推理类型任务，且与多种基线策略对比，验证了通用性。
- **深入分析**：讨论了不同任务对思考深度的需求差异，以及模型早期直觉信心现象，具有洞察力。
- **开源贡献**：提供了代码和数据（GitHub链接），保证可复现性。

### 8. 不足与局限
- **对闭源模型不友好**：需要访问token概率分布，闭源模型（如o1）需采样近似，增加复杂性。
- **自由生成任务需额外开销**：对于开放式数学问题，需要使用树搜索获取答案分布，增加了计算成本（附录B.2分析了阈值）。
- **未解决真正开放性问题**：方法适用于有确定答案的任务，对于无唯一“正确”答案的开放生成无法直接适用。
- **输出层面优化**：主要基于输出裁剪，未涉及模型架构或训练层面的效率改进（作者也承认这是未来方向）。
- **轻微性能下降**：在DeepSeek-R1-32B的MMLU-Pro和MuSR任务上，Adaptive Think准确率略低于Vanilla Think（-2.11%和-1.55%），说明蒸馏模型可能需要更细致阈值调整。
- **算力资源未明确**：缺乏GPU型号和训练时长信息，影响可复现性评估。

（完）
