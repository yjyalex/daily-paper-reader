---
title: Efficient Reasoning Through Suppression of Self-Affirmation Reflections in Large Reasoning Models
title_zh: 通过抑制大推理模型中的自我确认反射实现高效推理
authors: "kaiyuan liu, Chen Shen, Zhanwei Zhang, Junjie Liu, Xiaosong Yuan, Jieping Ye"
date: 2025-05-08
pdf: "https://openreview.net/pdf?id=A0AaeWG5NE"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 抑制自我确认反射以提高推理忠实性
tldr: 识别并抑制大模型推理中的自我确认冗余步骤，提高效率和忠实性。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1530, \"height\": 1879}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 1015, \"height\": 747}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 4413, \"height\": 2461}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 4413, \"height\": 2534}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 1293, \"height\": 750}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 1225, \"height\": 750}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-007.webp\", \"caption\": \"\", \"page\": 13, \"index\": 7, \"width\": 3000, \"height\": 2000}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-008.webp\", \"caption\": \"\", \"page\": 15, \"index\": 8, \"width\": 1216, \"height\": 750}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-009.webp\", \"caption\": \"\", \"page\": 15, \"index\": 9, \"width\": 2287, \"height\": 1150}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-a0aaewg5ne/fig-010.webp\", \"caption\": \"\", \"page\": 15, \"index\": 10, \"width\": 1293, \"height\": 751}]"
motivation: 大模型推理存在过度思考问题，尤其是自我确认反射步骤冗余且有害。
method: 分析推理模型中的自我确认反射模式，并通过压缩方法予以抑制。
result: 抑制自我确认反射后，推理输出更短且准确性保持或提升。
conclusion: 自我确认反射是导致推理不忠实和低效的重要原因，抑制它们可改善推理质量。
---

## Abstract
While recent advances in large reasoning models have demonstrated remarkable performance, efficient reasoning remains critical due to the rapid growth of output length. Existing optimization approaches highlights a tendency toward "overthinking", yet lack fine-grained analysis. In this work, we focus on Self-Affirmation Reflections: redundant reflective steps that affirm prior content and often occurs after the already correct reasoning steps. Observations of both original and optimized reasoning models reveal pervasive self-affirmation reflections. Notably, these reflections sometimes lead to longer outputs in optimized models than their original counterparts. Through detailed analysis, we uncover an intriguing pattern: compared to other reflections, the leading words (i.e., the first word of sentences) in self-affirmation reflections exhibit a distinct probability bias. Motivated by this insight, we can locate self-affirmation reflections and conduct a train-free experiment demonstrating that suppressing self-affirmation reflections reduces output length without degrading accuracy across multiple models (R1-Distill-Models, QwQ-32B, and Qwen3-32B). Furthermore, we also improve current train-based method by explicitly suppressing such reflections. In our experiments, we achieve length compression of 18.7\% in train-free settings and 50.2\% in train-based settings for R1-Distill-Qwen-1.5B. Moreover, our improvements are simple yet practical and can be directly applied to existing inference frameworks, such as vLLM. We believe that our findings will provide community insights for achieving more precise length compression and step-level efficient reasoning.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
大型推理模型（如 Deepseek-R1 系列）在数学推理等任务上表现优异，但输出长度迅速增长，导致高 token 使用和计算成本。现有优化方法虽识别出“过度思考”（overthinking）现象，但缺乏细粒度分析。本文聚焦于一种特定冗余行为——**自我确认反射（Self-Affirmation Reflection）**：模型在已经执行正确推理步骤后，仍会生成确认先前内容的反思性步骤。这种反射在原始模型和优化后的模型中普遍存在，甚至导致优化模型在某些实例上输出比原始模型更长。本文旨在通过抑制这种反射，在保持甚至提升性能的前提下实现输出长度压缩，从而提高推理效率。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：自我确认反射的起始词（leading words，即每句第一个词，如“Wait”“But”等）的概率分布与其他反射（如必要的核查反射）存在显著差异，其生成概率通常更低。利用这一特征，可以通过抑制低概率的反射起始词来减少不必要的反射步骤。
- **关键技术细节**：
  - 首先通过统计分析和模型 rollout 实验，确定“wait” (包括大小写) 为最显著的干扰目标。
  - **无训练方法（Train-free）**：在推理阶段，对每个生成步骤的 logits 进行处理：当“wait” token 的 softmax 概率低于阈值（如 0.3）时，将其 logit 设为负无穷，从而强制模型不生成该 token。
  - **基于训练的方法（Train-based）**：结合现有工作 EfficientReasoning [3] 的强化学习框架，在其 roll-out 阶段，以 25% 的概率对正样本（正确回答）进行干预（同样抑制低概率的“wait” token），使得模型学习生成更简洁的正确回答。
- **算法流程简述**：
  1. 加载模型和 tokenizer。
  2. 对每个生成步骤，计算当前 token 的 logits。
  3. 如果候选 token 属于指定干扰 token 集（如“wait”），且其 softmax 概率低于阈值，则将其 logit 设为 -inf。
  4. 继续正常解码。

## 3. 实验设计：数据集、场景、基准、对比方法
- **数据集**：
  - 数学推理：MATH500（500 题）、AIME24（2024 美国邀请赛数学考试）、AMC23（2023 美国数学竞赛）、GSM8K（小学算术）。
  - 域外通用推理：GPQA-Diamond（198 题研究生级 STEM 多选题）。
- **基准模型**：
  - R1-Distill-Qwen-1.5B/7B/32B、QwQ-32B、Qwen3-32B。
- **对比方法**：
  - 基线（原始模型）。
  - Underthink [40]：通过调整固定窗口内反思起始词的 logits 来鼓励探索。
  - EfficientReasoning [3]：基于强化学习的长度奖励压缩方法。
  - 本文方法在不同阈值下的变体。
- **评估指标**：准确率（Acc）和平均输出长度（LEN）。

## 4. 资源与算力
- **硬件**：所有实验在 **NVIDIA L20 GPU** 上运行。具体数量、训练时长未在文中明确说明。
- **训练设置**：训练实验仅针对 R1-Distill-Qwen-1.5B 模型，采用 EfficientReasoning 的原始训练环境（Dockerfile）和 roll-out 预算（AIME24 为 10 次，MATH500 为 3 次，GSM8K 为 1 次）。未提供总训练时长或 GPU 卡数。

## 5. 实验数量与充分性
- **实验组数**：非常丰富，主要包括：
  - 无训练实验：5 个模型 × 4 个标准数据集 × 5 个阈值（0.1~0.9）+ 域外数据集，共约 25×5=125 种配置，部分结果以表格呈现。
  - 训练实验：R1-Distill-1.5B 在 3 个数据集上，比较不同 α 值（0.05,0.1,0.2,0.4）和不同阈值/干预概率的消融实验。
  - 对比 Underthink：对 R1-Distill-1.5B 在 4 个数据集上测试α=1,3,10 和 β=200,600,1000,+∞ 共 16 种参数组合。
  - 额外对比干扰不同 token 的影响（“wait”、“wait+alternatively”、“wait+alternatively+but”）。
- **充分性**：实验覆盖了多种主流推理模型、多个数学和通用推理数据集，以及关键超参数（阈值、干预概率、干扰 token 组合）。但缺乏多次运行的标准差或置信区间报告，也未在更大模型（如 70B）上测试训练方法（限于算力）。总体而言，实验设计较全面、客观，但可复现性细节需依赖代码。

## 6. 论文的主要结论与发现
- 自我确认反射在原始模型和优化模型中普遍存在，且其起始词“Wait”的概率显著低于其他反射类型。
- 通过抑制低概率的“Wait” token，可以实现无训练下的长度压缩（R1-Distill-1.5B 压缩 18.7%）和训练下的更大压缩（50.2%），同时准确率保持或略有提升。
- 该方法简单有效，可直接集成到 vLLM 推理框架，无需修改模型结构。
- 在域外数据集 GPQA-Diamond 上也验证了方法的通用性。
- 消融实验表明，干预概率 25% 和阈值 0.3 为较优设置；过度干预会损害性能。

## 7. 优点
- **方法简洁**：仅需在推理时对少数 token 的 logits 进行后处理，训练时只需在采样阶段进行简单干预，无需修改模型架构或增加额外训练成本。
- **实用性强**：可直接嵌入现有推理框架（如 vLLM），具备即插即用能力。
- **效果显著**：在多个模型和数据集上均实现长度压缩，且性能不降，甚至略有提升。
- **分析细致**：首次聚焦自我确认反射这一具体冗余现象，并通过统计分析和 rollout 实验验证起始词的概率偏差，为后续工作提供新视角。
- **训练/无训练双轨验证**：既提供了无需训练的快速方案，也展示了如何增强现有训练方法。

## 8. 不足与局限
- **识别候选 token 的系统性不足**：本文仅聚焦于“wait” token，尚缺乏自动发现其他可能干扰 token 的方法；多 token 间的依赖协调问题也未解决。
- **实验覆盖的局限性**：
  - 训练实验仅针对 1.5B 较小模型，未验证在更大模型（如 32B+）上的训练效果（主要受算力限制）。
  - 所有实验均缺乏多次独立运行的误差线或统计显著性检验，结果的稳定性未明确报告。
  - 未在更多非数学推理任务（如常识推理、逻辑推理）上验证泛化性。
- **潜在风险**：抑制某些“wait” token 可能会误伤必要的核查反思，尽管实验显示可通过其他高概率 token 补偿，但在某些复杂问题上仍可能损害推理质量。阈值选择对性能敏感，需要针对不同模型或任务进行调优。
- **计算资源描述不充分**：未提供 GPU 数量、训练时长等详细信息，影响可复现性。
- **其他局限**：对自我确认反射产生的根本机制（如模型为何会陷入循环）仍缺乏深入理解。

（完）
