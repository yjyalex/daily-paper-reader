---
title: "Thoughts Are All Over the Place: On the Underthinking of Long Reasoning Models"
title_zh: 思绪纷飞：论长推理模型的思维不足
authors: "Yue Wang, Qiuzhi Liu, Jiahao Xu, Tian Liang, Xingyu Chen, Zhiwei He, Linfeng Song, Dian Yu, Juntao Li, Zhuosheng Zhang, Rui Wang, Zhaopeng Tu, Haitao Mi, Dong Yu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=WcUo7Z2Jnh"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 分析长推理模型中的思维不足，与思维链忠实度相关
tldr: 发现长推理模型中的思维不足现象，频繁切换思路损害性能，与思维链忠实度相关。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 长推理模型在复杂推理中表现优异，但存在频繁切换思考路径而未深入探索的问题。
method: 通过实验分析思维切换频率与正确率的关系，并引入方法缓解。
result: 发现思维切换越频繁，回答越容易出错，尤其在数学问题上。
conclusion: 长推理模型存在思维不足，需改进推理深度。
---

## Abstract
Long reasoning models (LRMs) such as OpenAI's o1 and DeepSeek's R1 have demonstrated remarkable abilities in complex reasoning tasks by scaling test-time compute and exhibiting human-like deep thinking. However, we identify a phenomenon we term underthinking, where LRMs frequently switch between different reasoning thoughts without sufficiently exploring promising paths to reach a correct solution. This behavior leads to inadequate depth of reasoning and decreased performance, particularly on challenging mathematical problems. To systematically analyze this issue, we conduct experiments on three challenging test sets and two representative open-source LRMs, revealing that frequent thought switching correlates with incorrect responses. We introduce a novel metric to quantify underthinking by measuring token efficiency in incorrect answers. To address underthinking, we propose a decoding strategy with thought switching penalty (Tip)  that discourages premature transitions between thoughts, encouraging deeper exploration of each reasoning path. Experimental results demonstrate that our approach improves accuracy across challenging datasets without requiring model fine-tuning. Our findings contribute to understanding reasoning inefficiencies in LRMs and offer a practical solution to enhance their problem-solving capabilities. Our code is open-source and available at https://github.com/wangyuenlp/underthinking.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：尽管长推理模型（LRMs，如OpenAI o1、DeepSeek R1）通过扩展测试时计算能力在复杂推理任务中表现出色，但论文发现一个被忽视的问题——模型在推理过程中频繁切换不同的思考路径，却未对任何一条有希望的路径进行充分探索。这种行为被定义为 **“underthinking”（思维不足）**，它导致推理深度不足，尤其在高难度数学问题上性能下降。
- **整体含义**：LRM的“思维不足”是一种推理低效现象，类似于人类在思考中“浅尝辄止”。理解并缓解这一问题，对于提升长链推理模型的效率和准确性至关重要。

## 2. 论文提出的方法论

### 核心思想
- 识别出LRM在错误回答中比正确回答更频繁地切换“思考段落”（thoughts），且这些切换多为过早放弃尚有潜力的思路。因此，通过指标量化该现象，并提出一种解码策略抑制过早的思维切换，鼓励模型在当前路径上更深入地探索。

### 关键技术细节
1. **定义与分割思考段落**：使用Llama-3.3-70B模型自动将模型输出分割为多个“思考段落”，切换标志词如“alternatively”。
2. **量化指标——思维不足分数（ξ_UT）**：
   - 针对每个错误响应，计算 `ξ_UT = 1 - (第一个正确思考前的Tokens数 / 总Tokens数)`。
   - ξ_UT越高，表示无效切换浪费的token比例越大，思维不足越严重。正确响应的ξ_UT设为0。
3. **解码策略——Thought Switching Penalty (TIP)**：
   - 在标准解码的logits上，对代表思维切换的token（如“alternatively”、“messy”）施加惩罚：`ˆz_{t,v} = z_{t,v} - α`，若v属于切换token集且当前生成位置距上次切换未超过β步。
   - 参数α控制惩罚强度，β控制惩罚持续时间。通过惩罚降低这些token的生成概率，迫使模型在同一思路内停留更久。
   - TIP仅对高频切换模式惩罚，保留必要的战略性切换（如回溯）。

### 公式或算法流程（文字说明）
- 标准解码：softmax(logits) → 概率分布。
- TIP：对切换token的logits减去α，然后在惩罚窗口（β宽度）内重复应用；重新softmax得到新分布，从而降低切换概率。

## 3. 实验设计

- **数据集/场景**：
  - MATH500（按难度分5级，重点分析Level 5即MATH500-Hard）
  - GPQA Diamond（研究生级问答）
  - AIME 2022-2024（美国数学邀请赛）
- **基准模型**：
  - QwQ-32B-Preview
  - DeepSeek-R1-Preview
  - DeepSeek-R1-671B
  - 额外在消融中使用了DeepSeek-R1-Distill-Qwen-32B、DeepSeek-R1-Distill-Llama-70B以及Qwen3系列（4B/8B/14B/32B）。
- **对比方法**：
  - 标准解码
  - Self-Consistency（多数投票）
  - Laconic Decoding（选择最短答案）
  - TIP单独或与上述方法结合
- **评估指标**：Pass@1（32个样本/问题，温度0.7，top_p=0.95），以及UT分数。

## 4. 资源与算力

- **未明确说明**：论文未提及使用的GPU型号、数量、训练时长等具体计算资源。仅表示代码已开源，但无算力细节。

## 5. 实验数量与充分性

- **实验数量**：涵盖3个主要数据集，4种以上模型（含家族系列），多种解码策略组合，以及超参数网格搜索（α∈{3,5,10,20,30}，β∈{300,400,500,600,700}）。
- **充分性与客观公平**：
  - 使用固定超参数（α=3,β=600，在AIME2022-23开发集上调优）并在所有模型/数据集上复用，避免过拟合。
  - 报告了Pass@1和UT分数，并提供了多种采样策略下的对比（表3、表4）。
  - 补充了跨模型家族（Qwen3）和跨模态（GLM-4.1V-Thinking、MiMo-VL）的验证。
  - 实验设计较为系统，控制变量清晰，结果可复现。

## 6. 论文的主要结论与发现

1. **思维不足普遍存在**：在错误响应中，LRM的思维切换频率是正确响应的4倍以上，消耗更多token却未提高准确率。
2. **早期正确思路被放弃**：超过70%的错误响应中包含至少一个正确思路；这些正确思路常出现在早期，但模型未持续深入。
3. **TIP方法有效缓解思维不足**：在AIME2024上，QwQ-32B-Preview的Pass@1提升5.8%（38.3%→44.1%），DeepSeek-R1提升1.0%（73.8%→74.8%），同时UT分数下降。
4. **TIP与采样策略互补**：与Self-Consistency或Laconic Decoding结合后，准确率进一步提升，表明鼓励深入探索与多样性采样相辅相成。
5. **思维不足对数据集敏感**：更强大的模型（如DeepSeek-R1-671B）在某些数据集上UT分数高，说明能力增强并不必然带来推理效率提升。

## 7. 优点

- **问题新颖性**：首次系统定义并量化了LRM中的“思维不足”现象，补充了“过度思考”之外的另一推理低效视角。
- **方法轻量有效**：TIP作为解码阶段的后处理策略，无需模型微调，直接改造成本低，且与现有采样技术兼容。
- **指标设计合理**：UT分数通过token效率量化思维不足，直观且可解释，有利于评估推理过程质量。
- **实验覆盖全面**：涉及多种模型家族、难度梯度和模态（文本+视觉），增强了结论的普适性。
- **代码开源**：提供可复现资源。

## 8. 不足与局限

- **正确思路判定依赖外部模型**：UT分数计算依赖于Llama-3.3-70B等模型来标注“正确思路”，该判断存在误差（论文自测准确率约82%），可能影响指标可靠性。
- **超参数调优空间有限**：TIP的α和β仅在AIME2022-23上针对QwQ调优，跨模型/数据集虽有效，但可能非全局最优。
- **实验覆盖局限**：主要聚焦于数学与科学问答（MATH/GPQA/AIME），未在更广泛的任务（如代码生成、常识推理）中验证。
- **未探讨自适应机制**：论文未研究模型是否可以自身学习何时停止切换，而是依赖固定惩罚。
- **计算资源细节缺失**：未说明实验所需GPU型号与耗时，不利于估算成本。

（完）
