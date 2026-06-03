---
title: "On the Bias of Next-Token Predictors Toward Systematically Inefficient Reasoning: A Shortest-Path Case Study"
title_zh: 下一词元预测器倾向于系统性低效推理：最短路径案例研究
authors: "Riccardo Alberghi, Elizaveta Demyanenko, Luca Biggio, Luca Saglietti"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=l8razJItEy"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 分析LLM在最短路径任务中的推理效率
tldr: 下一词元预测器偏好冗余长链而非最优推理链。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 测试时计算增加会引入冗余推理，需研究推理效率。
method: 在分层图中的最短路径任务上训练解码器Transformer，比较最优和长推理路径。
result: 模型倾向生成冗余但非最优的推理链。
conclusion: 下一词元预测存在系统性低效推理偏差。
---

## Abstract
Recent advances in natural language processing highlight two key factors for improving reasoning in large language models (LLMs): (i) allocating more test-time compute tends to help on harder problems but often introduces redundancy in the reasoning trace, and (ii) compute is most effective when reasoning is systematic and incremental, forming structured chains of thought (CoTs) akin to human problem-solving. To study these factors in isolation, we introduce a controlled setting based on shortest-path tasks in layered graphs. We train decoder-only transformers on question–trace–answer triples using a custom tokenizer, comparing models trained on optimal bottom-up dynamic programming traces with those trained on longer, valid traces involving backtracking. Surprisingly, under the same training-token budget, the latter models generalize better to unseen graphs. This benefit is not due to length alone—injecting arbitrary redundancy into reasoning traces fails to help and can even hurt performance. Instead, we find that generalization correlates with the model's confidence in next-token prediction, suggesting that long, coherent, and locally incremental traces make the training signal easier to optimize.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 现代大型语言模型（LLM）在推理任务中表现出色，其成功部分归因于**链式思维（CoT）**和**测试时计算（test-time compute）**的扩展。
- 然而，更长的CoT常引入冗余，且并非所有CoT都同样有效。研究者们尚不清楚：**哪种结构的推理轨迹最适合下一词元预测架构的学习**。
- 本文针对此问题，设计了一个**可控的最短路径任务**作为测试平台，系统探究推理效率、轨迹结构、可预测性对模型泛化的影响。
- 核心假设：**全局最优的推理策略（如动态规划）可能并非训练的最佳选择；相反，系统性、局部增量但更长的推理轨迹可能更符合下一词元预测的归纳偏差**。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过控制推理轨迹的**效率参数η**，生成从高效（外层循环、动态规划风格）到低效（深度优先、频繁回溯）的不同推理路径，训练Transformer模型，并分析其泛化性能。
- **技术细节**：
  - **任务**：基于分层有向无环图（DAG）的最短路径问题，图参数（层数L=7、每层节点数K=6、边成本C=5、连接概率p_e=0.6）。
  - **token化**：自定义token字典，包含分隔符、节点标签、成本、BoT/EoT等特殊token，将图、推理轨迹和答案序列化为token序列。
  - **推理轨迹生成**：使用一个队列算法维持待探索路径，优先级权重取决于温度参数η（η>0偏好短路径/层优先，η<0偏好长路径/深度优先）。轨迹满足基本正确性约束（路径、成本一致，只基于当前最优部分路径构建）。
  - **模型**：Phi3小语言模型（3层、12头、768隐藏维度、28.5M参数），从头训练，掩码问题部分，只学习预测轨迹和答案。
  - **训练细节**：AdamW优化器，恒定学习率2e-5，批量大小16k token，上下文长度4096（有轨迹）或256（无轨迹）。
  - **评估指标**：答案正确性（路径合法、成本最优）、推理步数、下一词元置信度。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：合成生成大量唯一图实例（无重复），按9:1划分训练/测试集。图大小分布与训练一致（层数3/5/7均出现）。
- **benchmark/对比方法**：
  - **无CoT baseline**：模型直接从问题回答最优路径。
  - **不同效率的CoT**：
    - η=+5（高效DP）：层优先、最小步数。
    - η=0（随机探索）：步数中等，但顺序完全随机。
    - η=-5（低效DFS）：深度优先、大量回溯、步数最长（约比DP长75%）。
  - **冗余注入**：
    - η=+5+确定性重复（DR）：每步重复一次。
    - η=+5+随机重复（RR）：每步以1/2概率重复（期望长度与DR相同但结构不规则）。
  - **不同采样温度**：0.0, 0.2, 0.5, 0.7, 1.0, 1.2, 1.5, 1.7, 2.0。
  - **不同模型深度**：3层 vs 6层。
  - **不同训练规模**：16M、32M、128M token。
- **所有实验均在相同图分布上评估，保证公平**。

## 4. 资源与算力

- **硬件**：Nvidia A100 (80GB) 或 RTX4090 (24GB)，搭配32个CPU核心。
- **显存占用**：约14GB（FP16精度，3层模型）。
- **训练时间**：未明确给出具体时长，但提到使用vLLM推理，单次训练约需若干小时至一天不等（基于GPU型号和配置）。
- **框架**：PyTorch、HuggingFace Transformers、vLLM。

## 5. 实验数量与充分性

- **组数**：至少进行了以下主要对比实验：
  - 无CoT vs 有CoT（图3）。
  - 三种效率η=+5、0、-5（图4、图1）。
  - 两种冗余注入DR和RR（图5）。
  - 不同采样温度（图5、图6）。
  - 不同模型深度（附录图10）。
  - 不同训练token预算（16M、32M、128M）。
- **充分性评估**：
  - 每个实验包含**5个随机种子**，报告1-sigma误差条。
  - 测试集规模约10000个新图。
  - 训练过程跟踪至收敛（超过20个epoch）。
  - 详细分析了轨迹质量指标（子问题最优性、重复步骤等，附录图7、表1）。
- **评价**：实验设计系统、控制变量充分、重复性高，结论可信。但主要局限在合成任务和小模型，未在自然语言或大模型上验证。

## 6. 论文的主要结论与发现

1. **CoT至关重要**：没有CoT时模型无法泛化到较大图；有CoT后性能大幅提升。
2. **结构优于全局最优性**：低效但系统性（DFS）的轨迹训练出的模型，泛化能力显著优于高效DP训练出的模型，尽管低效模型在相同token预算下看到的图更少。
3. **长度不是关键，可预测性是关键**：长度相近但随机探索（η=0）或随机重复（RR）的轨迹性能差；而可预测性高的轨迹（η=-5）使模型下一词元置信度更高，优化更顺畅。
4. **注入无结构冗余反而有害**：确定性重复（DR）微幅降效；随机重复（RR）导致模型产生无限循环，需更高采样温度正则化。
5. **模型存在“高冗长偏差”**：在低温度下，模型生成的轨迹长度倾向于比训练分布更长（模仿低效DFS），需较长训练时间或非零温度才能收敛。
6. **下一词元置信度可作为可学习性的有力代理**：在所有设置中，轨迹平均top-1概率与答案准确性正相关。

## 7. 优点：方法或实验设计上的亮点

- **极强的可控制性**：通过单一参数η就能平滑调节推理效率，精确研究“效率-可预测性”权衡。
- **干净的任务设计**：最短路径任务有明确最优解，便于量化推理质量；token化简单，可排除自然语言的干扰。
- **引入“下一词元置信度”作为解释工具**：直接证明了低效但系统性轨迹为何更有效。
- **系统消融实验**：分别控制轨迹长度、结构规则性、测试时温度，剥离不同因素影响。
- **开源代码**：GitHub上提供完整代码和配置，可复现。

## 8. 不足与局限

- **合成任务**：最短路径问题高度结构化，与自然语言推理有本质差异，结论外推性未知。
- **小模型**：仅使用28.5M参数的Phi3，大模型是否表现出相同偏差需进一步验证。
- **自定义tokenizer**：可能缺乏现实LLM中常见的语义组合性，影响偏差的普遍性。
- **仅验证一个算法域**：排序、SAT、定理证明等其他领域可能展示不同最优-低效权衡。
- **未进行可解释性分析**：虽然提出猜想（如“eureka”阶段、电路涌现），但未具体分析注意力头部或隐藏状态。
- **缺乏RL训练探索**：未尝试通过强化学习或课程学习引导模型向更高效推理收敛。

（完）
