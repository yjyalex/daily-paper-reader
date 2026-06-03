---
title: "Thoughts Are All Over the Place: On the Underthinking of Long Reasoning Models"
title_zh: 思绪纷乱：论长推理模型的欠思考问题
authors: "Yue Wang, Qiuzhi Liu, Jiahao Xu, Tian Liang, Xingyu Chen, Zhiwei He, Linfeng Song, Dian Yu, Juntao Li, Zhuosheng Zhang, Rui Wang, Zhaopeng Tu, Haitao Mi, Dong Yu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=WcUo7Z2Jnh"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 长推理模型中的欠思考现象导致推理不忠实
tldr: 发现长推理模型中的欠思考现象：频繁切换思路导致推理深度不足和错误增加。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 长推理模型虽强但存在频繁切换思路导致推理不充分的问题。
method: 在多个挑战性数学测试集上系统分析开源长推理模型的行为。
result: 揭示思路切换频率与错误回答的相关性，并引入度量指标。
conclusion: 欠思考是长推理模型性能瓶颈之一。
---

## Abstract
Long reasoning models (LRMs) such as OpenAI's o1 and DeepSeek's R1 have demonstrated remarkable abilities in complex reasoning tasks by scaling test-time compute and exhibiting human-like deep thinking. However, we identify a phenomenon we term underthinking, where LRMs frequently switch between different reasoning thoughts without sufficiently exploring promising paths to reach a correct solution. This behavior leads to inadequate depth of reasoning and decreased performance, particularly on challenging mathematical problems. To systematically analyze this issue, we conduct experiments on three challenging test sets and two representative open-source LRMs, revealing that frequent thought switching correlates with incorrect responses. We introduce a novel metric to quantify underthinking by measuring token efficiency in incorrect answers. To address underthinking, we propose a decoding strategy with thought switching penalty (Tip)  that discourages premature transitions between thoughts, encouraging deeper exploration of each reasoning path. Experimental results demonstrate that our approach improves accuracy across challenging datasets without requiring model fine-tuning. Our findings contribute to understanding reasoning inefficiencies in LRMs and offer a practical solution to enhance their problem-solving capabilities. Our code is open-source and available at https://github.com/wangyuenlp/underthinking.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：长推理模型（LRMs，如 OpenAI o1、DeepSeek R1）通过扩展测试时计算展现了强大的复杂推理能力，但其推理过程中存在一个未被充分探索的问题——模型在生成答案时频繁在不同推理思路之间切换，却未对任何一条有希望的路径进行深入探索，导致推理深度不足。
- **问题的本质**：作者将此现象称为 **“欠思考”（Underthinking）**，即模型过早放弃有潜力的推理方向，转而探索大量其他方向，最终无法得出正确结论。
- **背景意义**：以往研究多关注“过度思考”（overthinking，即浪费资源在琐碎路径上），而本文聚焦于“欠思考”这一相反但同样关键的瓶颈，旨在提升 LRM 的推理效率与准确性。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- **定义“思考”**：将模型在一次解答中产生的中间认知步骤称为“思考”（thought），切换通常由特定词汇（如“alternatively”）触发。
- **欠思考的量化**：提出一个基于 token 效率的度量指标，衡量错误回答中有多少 token 没有被利用到正确思路上。
- **缓解方案**：设计一种带惩罚的解码策略，抑制过早的思路切换，鼓励模型在当前思路上深入探索。

### 关键公式与技术细节

1. **欠思考指标 ξUT**（公式 1）：
   \[
   \xi_{UT} = \frac{1}{N} \sum_{i=1}^N \left(1 - \frac{\hat{T}_i}{T_i}\right)
   \]
   - \(N\)：错误回答实例数。
   - \(T_i\)：第 \(i\) 个错误回答的总 token 数。
   - \(\hat{T}_i\)：从回答开始到第一个正确思考结束为止的 token 数。
   - 含义：值越高表示 token 效率越低（即欠思考越严重），值越低表示虽然答案错误但早期思路正确且浪费较少。

2. **思路切换惩罚（TIP）解码**：
   - **基本解码**：标准 softmax 概率 \(P(x_t=v|x_{<t}) = \exp(z_{t,v}) / \sum_{v'\in V}\exp(z_{t,v'})\)。
   - **惩罚机制**：对属于切换 token 集合 \(\hat{V}\)（如“alternatively”）的 token 的 logits 进行减法操作：
     \[
     \hat{z}_{t,v} = \begin{cases}
     z_{t,v} - \alpha, & \text{if } v \in \hat{V} \text{ 且 } t < \Psi + \beta \\
     z_{t,v}, & \text{otherwise}
     \end{cases}
     \]
   - \(\alpha\)：惩罚强度，控制对切换 token 的抑制程度。
   - \(\beta\)：惩罚持续时间，控制从当前思路开始后多少位置内施加惩罚。
   - \(\Psi\)：当前思路刚开始的位置。
   - 通过调整 \(\alpha\) 和 \(\beta\)，可以在不过度惩罚（允许策略性回溯）的前提下减少无意义的频繁切换。

### 算法流程（文字说明）
1. 对于每个生成步骤，检测当前是否处于新思路的起始阶段（距离上次切换不超过 β 个位置）。
2. 如果是，且当前 token 属于预定义的切换词集合，则在 logits 中减去 α 值，降低其生成概率。
3. 否则，保持原始 logits。
4. 最终通过调整后的概率分布采样下一个 token。

## 3. 实验设计

### 数据集与基准
- **MATH500**（Level 5 子集作为 Hard 变体）：高中数学竞赛题。
- **GPQA Diamond**：研究生级问答基准。
- **AIME 2022-2024**：美国邀请数学竞赛题目。
- **AIME 2022-2023** 用作开发集进行超参数调优。
- **OlympiadBench（多模态子集）**：用于验证泛化性。

### 评估模型
- **主模型**：QwQ-32B-Preview、DeepSeek-R1-671B、DeepSeek-R1-Preview（后两者为闭源但可见 CoT）。
- **额外模型**：DeepSeek-R1-Distill-Qwen-32B、DeepSeek-R1-Distill-Llama-70B（用于正确性评估）。
- **对比方法**：
  - 标准解码（无惩罚）。
  - 自一致性（Self-Consistency）：多个样本投票。
  - 简洁解码（Laconic Decoding）：选择最短答案。
  - TIP + 上述方法的各种组合。

### 实验设置
- 每个问题生成 32 个样本，temperature=0.7，top_p=0.95。
- 超参数 α 和 β 在 AIME 2022-2023 上网格搜索（α∈{3,5,10,20,30}，β∈{300,400,500,600,700}），最终选定 α=3, β=600 并固定用于所有后续实验。
- Pass@1、Pass@4、Pass@8、Pass@16 等指标。

## 4. 资源与算力

- **文中未明确说明使用的 GPU 型号、数量、训练时长**。
- 仅提及方法为纯解码策略，无需模型微调，因此算力需求主要来自推理阶段（32 次采样 × 多个问题）。
- 作者未报告具体硬件信息或计算成本，这算是一个信息缺失。

## 5. 实验数量与充分性

### 实验组数概览
- **主要实验**：在三个主流测试集（MATH500-Hard、GPQA Diamond、AIME2024）上对主模型进行标准解码和 TIP 对比，报告 Pass@1 和 UT 分数（表3）。
- **最佳-of-N 采样实验**：结合自一致性和简洁解码，在 4/8/16 样本设置下评估（表4）。
- **多模态泛化实验**：在 OlympiadBench 上评估两个视觉语言模型（表6），验证欠思考不限于文本。
- **消融与调优**：对 α 和 β 的网格搜索（表5）。
- **内部正确性评估**：使用两个蒸馏模型验证“正确思考”的标注准确率（82.9% 和 81.8%）。
- **额外模型系列验证**：在 Qwen3 系列（4B/8B/14B/32B）上验证欠思考趋势（表2）。

### 充分性评价
- **优点**：跨多个难度级别、多个模型系列、多种解码策略的实验，覆盖较全面；参数调优基于独立开发集，避免数据泄露；多模态泛化实验增强了结论的普适性。
- **不足**：主要聚焦于数学推理任务，尚未在更广泛的 NLP 推理任务（如常识推理、科学问答）中验证；局限于开源/可见 CoT 的模型，未覆盖闭源 o1 等；超参数 α 和 β 的选择对任务和模型敏感，但论文仅展示了一组固定值的通用结果。

## 6. 主要结论与发现

1. **欠思考普遍存在**：在多个 LRM 和不同难度任务中，错误回答比正确回答包含更多思路切换、更长 token 消耗，但准确率不增反降。
2. **早期思路正确但被放弃**：超过 70% 的错误回答中至少包含一个正确思路，模型未能坚持深入。
3. **欠思考与准确率关系复杂**：具体表现因数据集而异——有些模型准确率提高但欠思考也加重，有些则同时改善。
4. **TIP 方法有效**：引入思路切换惩罚后，在几乎所有设置下均能显著提升 Pass@1，降低 UT 分数，并且能与自一致性、简洁解码等策略协同。
5. **泛化到多模态**：在视觉-语言模型的数学推理中也观察到欠思考现象，说明该问题是跨模态的普遍瓶颈。

## 7. 优点

- **问题新颖**：首次系统定义和量化“欠思考”，填补了推理效率研究中“过度思考”之外的重要空白。
- **度量合理**：提出的 ξUT 指标基于 token 效率，直观且可解释，能与准确率互补。
- **方法轻量实用**：TIP 仅需修改解码 logits，无需微调模型，易于集成到现有推理流程。
- **实验严谨**：使用了多个具有挑战性的标准基准，并进行了充分的消融和参数分析，结果可靠。
- **开源代码**：提供代码仓库，便于复现和后续研究。
- **跨模型/跨模态验证**：在多个模型系列和视觉-语言任务上验证了结论，增强了泛化性。

## 8. 不足与局限

- **切换词依赖**：当前方法依赖手工定义的切换 token 集合（如“alternatively”），可能无法覆盖所有切换模式，且不同模型可能使用不同的表达。
- **正确思路判定成本**：度量需要依赖外部模型判断每个思路是否正确，评估过程复杂且可能引入噪声。
- **超参数敏感**：α 和 β 需要针对具体任务调优，文中仅固定一组值，未讨论自适应调节策略。
- **任务范围有限**：实验主要限于数学竞赛类问题，尚未在更广泛推理场景（如编程、常识）中验证。
- **资源信息缺失**：未报告计算资源（GPU 型号/数量/时间），不利于他人估计复现成本。
- **未对比其他缓解策略**：如动态早停、思路探索预算等，缺乏与更多基线方法的比较。
- **局限性未充分讨论**：论文仅简要提及但未展开分析欠思考与模型能力的关系（如为何更大模型在某些数据集上欠思考更严重）。

（完）
