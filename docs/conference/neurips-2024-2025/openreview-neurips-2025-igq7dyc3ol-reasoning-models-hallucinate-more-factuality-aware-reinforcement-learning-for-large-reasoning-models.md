---
title: "Reasoning Models Hallucinate More: Factuality-Aware Reinforcement Learning for Large Reasoning Models"
title_zh: 推理模型更容易产生幻觉：面向大型推理模型的事实感知强化学习
authors: "Junyi Li, Hwee Tou Ng"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Igq7Dyc3OL"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 揭示面向推理的RL微调增加幻觉，关注不忠实推理
tldr: 发现推理RL微调加剧幻觉，提出事实感知RL算法缓解。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 推理优化的RL微调显著增加了幻觉，需要解决这一关键缺陷。
method: 提出事实感知逐步策略优化（FSPO），在RL中引入显式事实验证。
result: FSPO在保持推理能力的同时有效降低了幻觉率。
conclusion: 推理模型的RL训练需要兼顾事实性以避免不忠实输出。
---

## Abstract
Large language models (LLMs) have significantly advanced in reasoning tasks through reinforcement learning (RL) optimization, achieving impressive capabilities across various challenging benchmarks. However, our empirical analysis reveals a critical drawback: reasoning-oriented RL fine-tuning significantly increases the prevalence of hallucinations. We theoretically analyze the RL training dynamics, identifying high-variance gradient, entropy-induced randomness, and susceptibility to spurious local optima as key factors leading to hallucinations. To address this drawback, we propose Factuality-aware Step-wise Policy Optimization (FSPO), an innovative RL fine-tuning algorithm incorporating explicit factuality verification at each reasoning step. FSPO leverages automated verification against given evidence to dynamically adjust token-level advantage values, incentivizing factual correctness throughout the reasoning process. Experiments across mathematical reasoning and hallucination benchmarks using Qwen2.5 and Llama models demonstrate that FSPO effectively reduces hallucinations while enhancing reasoning accuracy, substantially improving both reliability and performance.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **问题**：现有基于强化学习（RL）的推理优化（如DeepSeek-R1）虽然显著提升了数学、编程等复杂推理能力，但实证发现**面向推理的RL微调会大幅增加模型产生幻觉（hallucination）的风险**。这些幻觉常源于推理中间步骤的不正确或虚构陈述，即使最终答案偶尔正确，其可信度和可解释性也受到损害。
- **理论分析**：作者从理论上识别了三个关键因素：
  - **高方差梯度**：二元奖励使正确输出稀疏时梯度估计方差极大，训练不稳定。
  - **熵驱动的随机性**：为了探索正确输出，策略必须保持较高熵，增加了产生幻觉内容的概率。
  - **虚假局部最优**：模型可能收敛到确信但错误的答案，此时梯度为零，无法摆脱。
- **整体意义**：揭示了推理模型RL训练中事实性与推理能力之间的冲突，强调需要在奖励设计中引入中间步骤的事实反馈。

### 2. 论文提出的方法论
- **核心思想**：提出**Factuality-aware Step-wise Policy Optimization (FSPO)**，在RL训练中引入逐步事实验证信号，通过调整每个token的优势值，鼓励事实正确、惩罚事实错误，提供更密集的反馈。
- **关键技术细节**：
  - **奖励函数设计**：
    - **答案正确性奖励**（R_answer）：二进制（1/0），基于最终答案与真实答案匹配。
    - **逐步事实性奖励**（R_factuality）：将推理文本拆分为句子，使用自动验证器（HHEM-2.1）判断每个句子是否被给定证据（如Wikipedia段落）所蕴含（entail）、中立（neutral）或矛盾（contradict），分别赋分为1、0、-1。
    - **最终奖励**：R_final(y) = R_answer(y) + (1/N) Σ R_factuality(z_j)。
  - **Factuality-aware Advantage Adjustment**：基于GRPO框架，对每个输出yi计算群体优势Ai；然后根据每个token所属句子的R_factuality调整优势值ˆAi,t，使得事实正确的token即使整体答案错误也被鼓励，事实错误的token即使整体答案正确也被惩罚，从而避免虚假局部最优。
  - **优化目标**：采用GRPO形式，将调整后的优势ˆAi,t代入PPO的clip目标，更新策略πθ。

### 3. 实验设计
- **训练数据集**：
  - 事实性部分：从HotpotQA和2WikiMultiHopQA中随机选取2K样本（含问题、答案、Wikipedia证据）。
  - 推理部分：采用SimpleRL (8K数学问题) 进行传统RL训练。
- **评估基准**：
  - **推理能力**：GSM8K、MATH-500、AIME 2024、AIME 2025（Pass@1）。
  - **幻觉评估**：TruthfulQA（生成任务，用GPT-3评判）、HaluEval-QA（二选一判断）、HalluQA（中文）。
- **对比方法**：
  - API模型：DeepSeek-V3/R1、GPT-4o、GPT-o1。
  - 推理模型：QwQ-32B、R1-Distill系列（Qwen-7B/14B/32B、Llama-8B）。
  - 开源模型：Qwen2.5-7B-Base/Instruct、Llama3.1-8B-Instruct。
- **训练细节**：使用verl框架，在Qwen2.5-7B-Base/Instruct和Llama3.1-8B-Instruct上训练，batch size=8，每组prompt采样8条轨迹，最大长度2048 tokens，minibatch 1024，学习率4e-7，KL系数1e-3，clip ratio 0.2，温度1.0，训练1个epoch。

### 4. 资源与算力
- 论文明确提及：使用 **4 × H100 80G GPU**，每次训练约 **24小时**。

### 5. 实验数量与充分性
- **主要实验**：在7个基准（4推理+3幻觉）上比较了10余个基线模型，结果呈现在表1。
- **消融分析**：
  - 对比GRPO（仅答案奖励）、GRPO w/ factuality（含事实奖励但无优势调整）、FSPO（完整方法），在MATH-500和HaluEval-QA上展示训练曲线（图4）。
  - 对比不同RL算法（GRPO vs Reinforce++）的适应性（图5）。
  - 分析训练样本数量（1K/2K/4K/8K）的影响（图6）。
  - 分析响应长度与事实性分数（图7）。
- **充分性**：实验覆盖了推理和幻觉的多方面指标，消融实验验证了各组件贡献，对比基线全面（包括API、蒸馏、开源模型），统计上可靠。但未提供多次运行的标准差或置信区间，可能影响随机性评估。

### 6. 论文的主要结论与发现
- FSPO在所有三个幻觉基准上显著优于所有开源和推理模型，甚至超过部分API模型（如TruthfulQA上58.4% vs GPT-4o 59.0%接近）。
- 在推理基准上，FSPO（基于7B模型）达到或超过更大模型（如GSM8K 89.5% vs QwQ-32B 88.6%），且同时降低幻觉，证明**事实感知训练不仅能抑制幻觉还能保持甚至提升推理性能**。
- 消融实验证实，逐步事实性奖励和事实感知优势调整均不可或缺（FSPO > GRPO w/ factuality > GRPO）。
- 该方法仅需少量事实性数据（2K样本）即可显著改善事实性，且不损害推理能力。

### 7. 优点
- **理论贡献**：首次从梯度方差、熵约束、虚假局部最优三个角度系统分析RL二元奖励导致幻觉的机制，为后续研究提供理论基础。
- **方法创新**：引入逐步事实性验证和基于句子层级的优势调整，提供比最终答案更密集的反馈，有效解决了稀疏奖励问题。
- **实用性**：方法轻量（仅需自动验证器，无需人工标注），在7B/8B模型上即可取得显著效果，具有良好的扩展性。
- **实验设计全面**：覆盖多种模型规模、多种基准、多种消融变量，且公开代码和数据集。

### 8. 不足与局限
- **理论分析局限**：仅针对二元奖励，未严格推广到连续奖励；证明依赖特定假设，实际泛化性待验证。
- **实验规模局限**：受计算资源限制，仅测试7B/8B模型，未在更大模型（如14B/32B）上验证；推理benchmark仅包含数学和开放域QA，未覆盖编程、医疗等高风险领域。
- **事实验证器依赖**：采用HHEM-2.1作为自动验证器，其质量直接影响奖励准确性；验证器本身也可能存在偏差或错误。
- **训练数据有限**：事实性训练仅使用2K样本，虽然有效，但更大量数据的边际收益和潜在灾难性遗忘（如对数学推理的损害）未充分探讨。
- **统计报告**：未给出多次运行的标准差或显著性检验，结果稳健性存疑。

（完）
