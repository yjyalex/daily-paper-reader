---
title: "Soft Thinking: Unlocking the Reasoning Potential of LLMs in Continuous Concept Space"
title_zh: 软思考：在连续概念空间中释放LLM的推理潜力
authors: "Zhen Zhang, Xuehai He, Weixiang Yan, Ao Shen, Chenyang Zhao, Xin Eric Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ByQdHPGKgU"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 一种无训练方法在连续概念空间中释放LLM推理潜力
tldr: 提出Soft Thinking无训练方法，通过生成抽象概念令牌改进思维链推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1728, \"height\": 360}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 3601, \"height\": 355}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 3601, \"height\": 355}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 3601, \"height\": 237}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 3601, \"height\": 237}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 3600, \"height\": 237}]"
motivation: 标准离散思维链限制推理表达能力。
method: 生成抽象概念令牌，模拟人类软推理过程。
result: 在多个推理任务上提升性能。
conclusion: 连续概念空间推理能增强LLM推理能力。
---

## Abstract
Human cognition typically involves thinking through abstract, fluid concepts rather than strictly using discrete linguistic tokens. Current Large Language Models (LLMs), however, are constrained to reasoning within the boundaries of human language, processing discrete token embeddings that represent fixed points in semantic space. This discrete constraint restricts the expressive power and upper potential of such reasoning models, often causing incomplete exploration of reasoning paths, as standard Chain-of-Thought (CoT) methods rely on sampling one token per step. In this work, we introduce Soft Thinking, a training-free method that emulates human-like ``soft'' reasoning by generating abstract concept tokens in a continuous concept space. These concept tokens are created by the probability-weighted mixture of token embeddings, which span the continuous concept space, enabling smooth transitions and richer representations that transcend traditional discrete boundaries. In essence, each generated concept token encapsulates multiple meanings from related discrete tokens, implicitly exploring various reasoning paths to converge effectively toward the correct answer. Empirical evaluations on diverse mathematical and coding benchmarks consistently demonstrate the effectiveness and efficiency of Soft Thinking, improving pass@1 accuracy by up to 2.48 points while simultaneously reducing token usage by up to 22.4\% compared to standard CoT. Qualitative analysis further reveals that Soft Thinking outputs remain highly interpretable and readable, highlighting the potential of Soft Thinking to break the inherent limits of discrete language-based reasoning.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：标准Chain-of-Thought（CoT）推理将LLM的输出限制在离散、预定义的token序列中，这本质上受限于人类语言的表达能力。每一步只采样一个token，导致推理路径探索不充分，容易陷入错误分支，且表达能力受限。
- **背景**：人类认知通常通过抽象、流体的概念进行思考，而非严格的离散语言符号。神经科学证据表明，人脑以抽象概念层次存储和表示信息，推理部分独立于语言网络。
- **动机**：打破LLM推理的离散token瓶颈，使其能够在连续概念空间中像人类一样进行“软”推理，同时隐式探索多条路径，提高准确性和效率。

## 2. 论文提出的方法论

### 核心思想
- 用**概念令牌（Concept Token）**替代离散token：概念令牌是模型在当前步骤输出的完整概率分布（softmax概率向量），而不是argmax后的单个token ID。
- 概念令牌的嵌入通过**概率加权混合所有token嵌入**得到，形成**连续概念空间**（凸组合集合）。
- 每一步将概念令牌的加权嵌入作为下一步输入，实现连续空间内的推理。

### 关键技术细节
- **概念令牌定义**：`ct := p`，其中p是LLM输出的概率分布。
- **连续概念空间**：`C = { Σ α_k e(k) : α ∈ Δ^{|V|-1} }`，即所有token嵌入的凸组合。
- **推理过程**：Soft Thinking仅在中间思考步骤生效。将概念令牌的embedding作为下一步输入：`˜e_next = Σ ct[k] e(k)`。当最可能token是结束符时停止推理，切换到标准离散解码输出答案。
- **Cold Stop机制**：基于熵的早期停止。计算每一步概念令牌的熵`H(p)`，若连续k步熵低于阈值τ，则插入`</think>`结束推理，避免OOD导致的重复或崩溃，提升效率。
- **理论分析**：通过线性近似将指数级的全路径求和转化为单个前向传播，保留完整分布信息。

### 算法流程（文字说明）
1. 输入问题，模型进入软思考模式。
2. 对于每个中间推理步：
   - 模型生成下一个token概率分布p。
   - 对p进行top-k top-p过滤，得到精简分布。
   - 计算每个Token嵌入的加权和作为新输入embedding。
   - 计算熵，Cold Stop检查是否提前终止。
3. 若检测到结束符或触发Cold Stop，切换到标准解码生成最终答案。

## 3. 实验设计

### 数据集与Benchmarks
- **数学推理**：Math500、AIME 2024、GSM8K、GPQA-Diamond（共4个）
- **代码生成**：HumanEval、MBPP、LiveCodeBench（共3个）

### 模型
- QwQ-32B（强化学习训练）
- DeepSeek-R1-Distill-Qwen-32B（监督蒸馏）
- DeepSeek-R1-Distill-Llama-70B（监督蒸馏）

### 对比方法
- **Standard CoT Thinking**：标准CoT，每步采样1个token，16个样本计算Pass@1
- **Standard Greedy CoT Thinking**：贪婪解码（温度0），单样本

### 指标
- **Pass@1准确率**（主要性能指标）
- **生成长度（token数）**（效率指标），仅计算正确答案的生成长度

## 4. 资源与算力

- **硬件**：8块NVIDIA H100 80GB GPU（在Section 4.1中明确说明）。
- **软件**：基于SGLang推理框架实现（v0.4.6.post1）。
- **训练**：Soft Thinking是**无训练方法**，无需额外训练资源或时间。推理时计算开销主要为top-k过滤和加权求和，复杂度为O(n·d)（n为过滤后token数，d为嵌入维度），熵计算O(|V|)但可忽略。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：在7个benchmark、3个模型上进行，共21组对比（每个模型-基准组合有3种方法：CoT、Greedy CoT、Soft Thinking）。
  - 消融实验：比较了4种策略（训练-free COCONUT、平均嵌入、Soft Thinking w/o Cold Stop、Soft Thinking w/ Cold Stop）在AIME 2024和LiveCodeBench上。
  - 超参数调整：对不同的top-n（5/10/15/20/30）、熵阈值τ（0.01/0.05/0.1/0.2）、连续步数阈值k（128/256/512/1024）进行了选择，最终报告最佳结果。
- **充分性**：实验覆盖了不同规模（32B/70B）、不同架构（Qwen/LLaMA）、不同训练范式（RL / 蒸馏）的模型，且同时评估了数学和代码两种领域。对比基线包括标准CoT和贪婪CoT，消融验证了各组件的贡献。结果重复一致（16样本平均），相对客观公平。

## 6. 主要结论与发现

- **准确率提升**：Soft Thinking在所有基准上一致提升Pass@1准确率，最高提升2.48个百分点（QwQ-32B数学平均），在AIME 2024上提升6.45个百分点。
- **Token效率显著提高**：生成长度减少11.6%~22.4%（数学）和16.1%~19.1%（代码），同时保持甚至提升准确性，打破了过去“性能-效率”之间的权衡。
- **对比其他方法**：Greedy CoT虽减少token但准确率大幅下降；训练-free COCONUT完全失败（生成最大长度、零正确率）；平均嵌入效果也很差。Cold Stop有效缓解OOD崩溃，同时提升准确率。
- **定性分析**：Soft Thinking生成的推理步骤保持高可读性和解释性，概率分布图显示在探索阶段分布较均匀，在精确计算阶段趋近one-hot，表明方法能灵活融合多条路径。

## 7. 优点

- **无训练、即插即用**：无需修改模型权重或架构，可直接集成到现有LLM的CoT流水线中，工程代价小。
- **双赢效果**：同时提升准确率和减少推理token数，而不是牺牲一方换取另一方。
- **理论基础扎实**：通过线性近似解释了方法如何隐式探索多条路径，并用熵作为置信度信号。
- **可解释性强**：定性可视化展示了概率分布在推理过程中的动态变化，便于理解模型行为。
- **通用性**：在多种模型（不同架构、规模、训练方式）和两个领域（数学、代码）上均有效。

## 8. 不足与局限

- **分布外（OOD）问题**：模型从未在训练中见过连续概念token，推理时输入分布发生偏移，可能导致不稳定性或重复生成。Cold Stop只能缓解，不能根本上消除。
- **性能提升边界有限**：在部分模型（如70B）上提升幅度较小（<1%），且依赖于超参数调节（τ、k、n），实际应用中可能需要调优。
- **实验覆盖不足**：未在更大规模模型（如>100B）或更多领域（如常识推理、知识问答）上验证，也未测试不同温度或采样策略的影响。
- **计算复杂度细节**：虽然指出复杂度比模型前向传播小，但未给出实际推理延迟对比，无法评估实际部署中的加速效果。
- **可扩展性**：作为无训练方法，无法通过进一步微调来完全对齐概念空间，未来需要训练式方法来弥补。

（完）
