---
title: Training Language Models to Reason Efficiently
title_zh: 训练语言模型高效推理
authors: "Daman Arora, Andrea Zanette"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=AiZxn84Wdo"
tags: ["query:cot-unfaith"]
score: 5.0
evidence: 训练LLM高效推理，最小化不必要计算
tldr: 一种训练推理模型以减少计算开销并保持性能的方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-aizxn84wdo/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-aizxn84wdo/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-aizxn84wdo/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-aizxn84wdo/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 512, \"height\": 512}]"
motivation: 长思维链推理的计算成本高，需要更经济的部署。
method: 通过激励模型最小化不必要的计算开销来训练高效推理。
result: 在保持性能的同时显著降低推理成本。
conclusion: 训练目标引导模型学习更高效的推理策略。
---

## Abstract
Scaling model size and training data has led to great advances in the performance of Large Language Models (LLMs). However, the diminishing returns of this approach necessitate alternative methods to improve model capabilities, particularly in tasks requiring advanced reasoning. Large reasoning models, which leverage long chain-of-thoughts, bring unprecedented breakthroughs in  problem-solving capabilities but at a substantial deployment cost associated to longer generations. Reducing inference costs is crucial for the economic feasibility, user experience, and environmental sustainability of these models.

In this work, we propose to train large reasoning models to reason efficiently. Our method incentivizes models to minimize unnecessary computational overhead while largely maintaining accuracy, thereby achieving substantial deployment efficiency gains. It  enables the derivation of a family of reasoning models with varying efficiency levels, controlled via a single hyperparameter. Experiments on two open-weight large reasoning models demonstrate significant reductions in inference cost while preserving most of the accuracy.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大型推理模型（如 OpenAI o1、DeepSeek-R1）通过长链式思维（Chain-of-Thought）显著提升了复杂推理能力，但这种能力以极高的推理计算成本为代价，影响经济可行性、用户体验和环境可持续性。
- **核心问题**：如何在不显著损失准确率的前提下，大幅降低推理模型在部署时的计算开销（即生成的 token 数量）。
- **整体含义**：本文提出一种训练方法，激励模型产生更短但正确的链式思维，从而实现推理效率的提升。该方法允许通过单一超参数控制效率-准确率权衡，不需要改变模型架构或牺牲太多性能。

### 2. 论文提出的方法论

- **核心思想**：在强化学习（RL）的奖励函数中引入对正确回答长度的惩罚，鼓励模型在保持正确的前提下使用更少的 token。
- **关键技术细节**：
  - 采用策略梯度方法（PPO + RLOO 优势估计器）优化目标。
  - 修改后的奖励函数为：  
    \[
    \mathbb{E}\left[ \mathbf{1}\{y = y^*(x)\} \left(1 - \alpha f(\text{LEN}(y))\right) \right]
    \]
    其中 \(\alpha \in [0,1)\) 是控制压缩强度的超参数，\(f(\cdot)\) 是长度的单调递增函数。
  - \(f(\cdot)\) 使用基于 sigmoid 的归一化：  
    \[
    f(\text{LEN}(y)) = \sigma\left( \frac{\text{LEN}(y) - \text{MEAN}(x)}{\text{STD}(x)} \right)
    \]
    其中均值和标准差从在线生成的正确回答中估计，从而对不同难度的问题按长度比例进行惩罚。
- **算法流程**（文字说明）：
  1. 对每个提示，从当前策略模型中采样多个回答。
  2. 根据正确性和长度计算每个回答的奖励（最短正确回答得最高奖励，错误回答得0分，正确但长的回答奖励较低）。
  3. 使用 RLOO 估计优势，然后执行 PPO 更新。
  4. 重复上述过程约 100 轮（约 200 梯度更新）即可收敛。
- **理论保证**（简化假设下）：优化该目标得到的种群最优模型会在每个提示上生成最短的正确回答，且不损失准确率。

### 3. 实验设计

- **使用的数据集**：
  - **训练集**：从 Numina Math 数据集的 MATH、cn_k12、AIME、AoPS、Olympiad 子集中筛选出约 3.2k 个有数值答案的数学问题。
  - **测试集**：GSM8K（小学水平）、MATH500（较难）、AIME2024（竞赛级），以及两个非数学推理基准：CommonSenseQA 和 Logical Deduction（来自 BIG-Bench）。
- **基准模型**：DeepSeek-R1-Distill-Qwen-1.5B 和 7B（目前唯一的公开推理蒸馏模型）。
- **对比方法**：
  - vLLM 生成截断（设置 token 上限：8k/16k/24k/32k）。
  - 拒绝采样 + SFT（选择最短正确回答进行监督微调）。
  - DPO（以最长 vs 最短正确回答作为偏好对）。
  - O1-Pruner（并发工作，离线RL）。
  - 原始推理模型（无压缩）。
  - 无推理的指令微调模型（Qwen2.5-Math-Instruct）。
- **评估指标**：Pass@k（GSM8K: k=1, MATH500: k=3, AIME2024: k=10）；温度=0.6；最大 token 限制=32k；使用 vLLM 批量推理；解析器由 Qwen 团队提供以确保正确性判断。
- **忠实性评估**：在 MMLU 子集上，通过插入“斯坦福教授认为答案是D”这样的提示，检查链式思维是否明确提及该提示（参考 Chen et al., 2025 等方法）。
- **消融实验**：
  - 优势归一化对压缩效果的影响（发现非归一化更容易导致准确率骤降）。
  - 在基础模型（Qwen2.5-3B）上直接应用该方法（在 Countdown 任务中对比 vanilla RLOO）。

### 4. 资源与算力

- 硬件：GH200 GPUs
  - 1.5B 模型：4 个 GPU（一个低密度节点）
  - 7B 模型：8 个 GPU（分布两个低密度节点，每节点 4 GPU）
- 训练时间：约 20 小时
- 技术细节：ZeRO Stage 2（1.5B）/ Stage 3（7B），bfloat16 精度，批次大小 128（32 prompts × 8 生成），学习率 5e-6（1.5B）/ 2e-6（7B），Adam 优化器，KL 系数 1e-3。
- 注：论文未明确计算总 GPU 小时数，但提供了上述细节。

### 5. 实验数量与充分性

- 对每个 α（0, 0.05, 0.1, 0.2, 0.4）均使用 3 个不同随机种子运行，报告均值和误差。
- 在 5 个测试集上评估。
- 与 4 种基线方法（截断、SFT、DPO、O1-Pruner）进行系统比较。
- 额外进行了忠实性分析、CoT 行为分析（验证/回溯/探索关键词计数）、消融实验（优势归一化、基础模型训练）。
- **充分性评价**：实验设计较为全面，覆盖了不同难度、不同领域的数据集，对比了多个相关基线，并提供了消融和统计分析。但缺乏对更多基础模型（如 LLaMA 系列）或更大规模模型（>7B）的验证，也未在非数学推理任务（如代码生成、科学推理）上充分测试。

### 6. 论文的主要结论与发现

- 本文方法能够有效导航精度-计算量的 Pareto 前沿：在保持大部分准确率的前提下大幅减少推理 token 数。例如 7B 模型上，token 减少 50% 时准确率仅下降 <5%。
- 压缩效果与问题难度相关：简单问题（GSM8K）上 token 减少最多（达 83%），困难问题（AIME）上压缩较少（~27%），表明模型自动区分问题难度。
- 与 SFT、DPO、O1-Pruner 相比，本文方法在同样 token 预算下实现更高的准确率。
- 即使不施加长度惩罚（α=0），由于 RLOO 实现中的长度偏差，仍观察到一定压缩。作者通过纠正该偏差证实了这一点。
- 压缩导致 CoT 的忠实性下降（但仍显著优于非推理指令模型），同时减少了验证、回溯和探索等宏观行为的发生频率。
- 对基础模型直接训练时，虽然收敛更慢且峰值性能稍低，但在相同性能下能产生更短的回答。

### 7. 优点

- **方法简洁有效**：只需修改几行强化学习代码即可实现，且训练成本低（约 100 RL 步）。
- **可调性**：通过单个超参数 α 可连续调节压缩程度，适应不同部署场景。
- **鲁棒性强**：在多种难度和类型的数据集上均表现出稳定的效率提升。
- **兼容性好**：与现有的系统级加速（如 vLLM、推测解码）和模型级压缩（量化、剪枝）正交，可叠加使用。
- **理论基础**：在简化设定下证明了最优性保证，增加了方法的可靠性。

### 8. 不足与局限

- **训练复杂性**：相比于简单的 SFT 或 DPO，RL 训练仍需要更多实现和调参工作。
- **无法精确控制生成长度**：α 只能影响压缩强度，不能直接指定目标 token 数；后续工作（Aggarwal & Welleck, 2025）解决了此问题。
- **性能轻微损失**：所有压缩方案均伴随少许准确率下降（虽然很小），是否存在无损失压缩仍需探索。
- **忠实性下降**：压缩导致 CoT 更少提及外部提示，可能降低可解释性和可信度。
- **通用性未充分验证**：实验主要聚焦数学推理（GSM8K、MATH、AIME），虽然在 CommonSenseQA 和 Logical Deduction 上也有测试，但未进一步扩展到更广泛的 NLP 任务（如代码、科学问答）。
- **基础模型训练效果有限**：直接在基础模型上应用本方法收敛慢、峰值性能较低，更适合作为后训练步骤用于已有推理能力的模型。
- **仅使用蒸馏模型**：实验基于 DeepSeek-R1-Distill 系列，未在完全从头训练的推理模型上验证，可能影响结论的泛化性。

（完）
