---
title: "On the Bias of Next-Token Predictors Toward Systematically Inefficient Reasoning: A Shortest-Path Case Study"
title_zh: 关于下一个词预测器偏向系统低效推理的偏见：以最短路径为案例研究
authors: "Riccardo Alberghi, Elizaveta Demyanenko, Luca Biggio, Luca Saglietti"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=l8razJItEy"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 使用最短路径任务和思维链研究LLM的低效推理
tldr: 表明下一个词预测器偏向更长的推理轨迹而非最优的
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 研究LLM在推理时产生冗余的原因，以更好地理解思维链的效率和忠实性。
method: 在分层图最短路径任务上训练解码器专用Transformer，比较最优动态规划轨迹与较长轨迹。
result: 发现模型偏向更长的推理轨迹，暗示推理效率低下与不忠实的问题。
conclusion: LLM的下一词预测训练目标可能导致系统性的低效推理，影响思维链的忠实性。
---

## Abstract
Recent advances in natural language processing highlight two key factors for improving reasoning in large language models (LLMs): (i) allocating more test-time compute tends to help on harder problems but often introduces redundancy in the reasoning trace, and (ii) compute is most effective when reasoning is systematic and incremental, forming structured chains of thought (CoTs) akin to human problem-solving. To study these factors in isolation, we introduce a controlled setting based on shortest-path tasks in layered graphs. We train decoder-only transformers on question–trace–answer triples using a custom tokenizer, comparing models trained on optimal bottom-up dynamic programming traces with those trained on longer, valid traces involving backtracking. Surprisingly, under the same training-token budget, the latter models generalize better to unseen graphs. This benefit is not due to length alone—injecting arbitrary redundancy into reasoning traces fails to help and can even hurt performance. Instead, we find that generalization correlates with the model's confidence in next-token prediction, suggesting that long, coherent, and locally incremental traces make the training signal easier to optimize.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：大语言模型（LLM）在推理时常产生冗余的思维链（CoT），但并非所有冗余都同样有害。本文研究 next-token 预测器的归纳偏置是否倾向于系统性的低效推理，以及这种偏置对模型泛化能力的影响。
- **整体含义**：揭示了“最优算法轨迹”不一定是最佳训练信号；相反，更长、更结构化、局部递增的轨迹更易被 next-token 预测器学习，从而获得更好的泛化性能。这对理解 LLM 推理效率与忠实性有重要意义。

## 2. 方法论
- **核心思想**：在受控的合成最短路径任务中，通过调节搜索策略生成不同效率的推理轨迹（从高效动态规划到低效深度优先搜索），训练 decoder-only transformer，并分析模型性能与 next-token 置信度之间的关系。
- **关键技术细节**：
  - 任务：分层有向无环图（DAG），节点数、边权随机生成，任务为输出最小成本路径。
  - 轨迹生成：基于优先级队列的探索算法，通过效率参数 η 控制探索顺序（η>0 偏向层序，η<0 偏向深度优先；η=0 为随机顺序）。
  - 轨迹最优性保证：所有轨迹均包含正确路径-成本声明，且构建过程逐步递增。
  - 自定义分词器：使用特殊标记（BoS, EoS, BoT, EoT, 节点标签, 整数, 分隔符等）编码图结构、推理轨迹和答案。
  - 模型架构：Phi3 小型语言模型（3层，12注意力头，768隐藏维度，2850万参数），从头训练 next-token 预测任务。
  - 训练策略：固定 token 预算（32M/128M），屏蔽问题部分的损失，仅计算轨迹和答案上的损失；使用 AdamW 优化器，学习率 2e-5，批量大小约 16K token。
- **算法流程**：轨迹生成算法（图2a）：初始化队列和最佳成本表；每次按权重概率选择路径；若发现更优路径则更新并扩展；权重 = exp(-η·层数)。η 值决定探索顺序。

## 3. 实验设计
- **数据集/场景**：合成分层图，参数固定为 {L=7, K=6, C=5, p_e=0.6}，图实例不重复，训练/测试比 9:1，测试集为未见过的图。
- **基准（Benchmark）**：最短路径任务，评估 answer accuracy（路径是否最优、一致、可能）和 next-token 置信度。
- **对比方法**：
  - 无轨迹（直接问题→答案）
  - 不同效率轨迹：η=+5（DP，高效）、η=0（随机探索）、η=-5（DFS，低效回溯）
  - 冗余注入：确定性重复（DR）和随机重复（RR），η=+5 基础
  - 不同采样温度（T=0, 0.5, 1.0, 等）
  - 模型深度对比（3层 vs 6层）
  - 训练数据规模对比（16M, 32M, 128M token）

## 4. 资源与算力
- 文中明确说明：使用 Nvidia A100 80GB 或 RTX4090 24GB GPU，搭配 32 CPU 核心；FP16 精度，显存约 14GB（3层模型）；采用 PyTorch、HuggingFace、vLLM 框架。
- **未明确说明**：具体训练时长、GPU 数量、总计算量。论文仅提及“most training runs have been executed on hardware configurations featuring either...”，未给出精确时间。

## 5. 实验数量与充分性
- **实验组数**：涵盖多种对比（无轨迹 vs 有轨迹；不同 η；冗余注入；温度扫描；模型深度；数据量）约 10 余组，每组有 3-5 个随机种子，误差棒为 ±1σ。
- **充分性**：实验设计系统，控制变量清晰，结果可复现。但所有实验基于合成短路径任务和单一小模型，未在真实 NLP 任务或更大模型上验证，外部有效性有限。消融实验（冗余长度 vs 结构）设计合理，表明长度本身不是关键，结构更重要。

## 6. 主要结论与发现
- **链式思维（CoT）至关重要**：无轨迹时模型无法泛化到较大图；有轨迹时泛化显著提升。
- **结构优于全局最优**：低效但系统性的 DFS 轨迹（η=-5）训练的模型比高效 DP 轨迹（η=+5）模型泛化更好，即使前者看到的图实例更少（因轨迹更长）。
- **Next-token 置信度是学习能力的代理**：泛化性能与模型在推理过程中的平均 top-token 概率高度相关；DFS 轨迹的可预测性最高（低信息熵），因此更易优化。
- **任意冗余无益**：对 DP 轨迹简单重复（DR 或 RR）不仅不提升性能，还可能引入循环导致更差结果。
- **模型偏向更长轨迹**：在零采样温度下，η=0 和 RR 模型产生比预期更长的轨迹，需通过较高采样温度正则化。
- **温度调节效果反直觉**：较高采样温度反而抑制了无限循环，使轨迹长度收敛到训练分布。

## 7. 优点
- **控制性好**：合成环境可精确调节轨迹效率，分离变量（长度与结构），结果清晰可解释。
- **反直觉发现**：证明“更高效算法轨迹不一定是最好的训练数据”，对实际 CoT 教学有指导意义。
- **引入置信度度量**：将 next-token 概率作为学习难度的代理，提供新分析角度。
- **实验较全面**：包含冗余注入、温度扫描、模型规模、数据量等变量，误差棒和种子更增可信度。
- **代码开源**：促进可复现性。

## 8. 不足与局限
- **任务与模型局限性**：仅使用合成最短路径任务和 28.5M 参数的小型 Transformer，结论是否推广到自然语言推理、大型模型（如 GPT-4、DeepSeek-R1）未知。
- **算法域单调性**：其他算法任务（排序、SAT、定理证明）可能呈现不同最优-低效权衡。
- **未探索架构多样性**：未测试非自回归模型或不同注意力机制，归纳偏置可能不同。
- **训练数据生成**：所有轨迹均遵循最优性条件（不构建于次优子路径上），这在真实人类思维链中可能不成立。
- **无机制解释**：虽观察到置信度差异与“灵光一现”损失跳变，但未进行机械可解释性分析验证内部电路。
- **资源报告不完整**：未提供具体训练时长和总能耗。

（完）
