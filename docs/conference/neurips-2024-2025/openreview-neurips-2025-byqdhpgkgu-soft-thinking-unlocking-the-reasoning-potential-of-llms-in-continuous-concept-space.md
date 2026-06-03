---
title: "Soft Thinking: Unlocking the Reasoning Potential of LLMs in Continuous Concept Space"
title_zh: 软思考：在连续概念空间中释放LLM的推理潜力
authors: "Zhen Zhang, Xuehai He, Weixiang Yan, Ao Shen, Chenyang Zhao, Xin Eric Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ByQdHPGKgU"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 提出通过生成连续概念令牌来增强LLM推理的方法
tldr: 提出一种无需训练的方法，在连续空间中生成抽象概念令牌以改进推理
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1728, \"height\": 360}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 3601, \"height\": 355}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 3601, \"height\": 355}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 3601, \"height\": 237}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 3601, \"height\": 237}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-byqdhpgkgu/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 3600, \"height\": 237}]"
motivation: 标准思维链受限于离散令牌，限制了推理的表达能力和完整性。
method: 提出Soft Thinking方法，通过生成连续概念令牌模拟人类软性推理。
result: 在推理任务上提高了性能，超越了标准思维链方法。
conclusion: 连续概念推理能更有效地探索推理路径，提升LLM的推理能力。
---

## Abstract
Human cognition typically involves thinking through abstract, fluid concepts rather than strictly using discrete linguistic tokens. Current Large Language Models (LLMs), however, are constrained to reasoning within the boundaries of human language, processing discrete token embeddings that represent fixed points in semantic space. This discrete constraint restricts the expressive power and upper potential of such reasoning models, often causing incomplete exploration of reasoning paths, as standard Chain-of-Thought (CoT) methods rely on sampling one token per step. In this work, we introduce Soft Thinking, a training-free method that emulates human-like ``soft'' reasoning by generating abstract concept tokens in a continuous concept space. These concept tokens are created by the probability-weighted mixture of token embeddings, which span the continuous concept space, enabling smooth transitions and richer representations that transcend traditional discrete boundaries. In essence, each generated concept token encapsulates multiple meanings from related discrete tokens, implicitly exploring various reasoning paths to converge effectively toward the correct answer. Empirical evaluations on diverse mathematical and coding benchmarks consistently demonstrate the effectiveness and efficiency of Soft Thinking, improving pass@1 accuracy by up to 2.48 points while simultaneously reducing token usage by up to 22.4\% compared to standard CoT. Qualitative analysis further reveals that Soft Thinking outputs remain highly interpretable and readable, highlighting the potential of Soft Thinking to break the inherent limits of discrete language-based reasoning.

---

## 论文详细总结（自动生成）

# 论文总结：Soft Thinking: Unlocking the Reasoning Potential of LLMs in Continuous Concept Space

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：标准 Chain-of-Thought (CoT) 推理将 LLM 的输出限制在离散的、预定义的 token 序列中，这受限于人类语言的表达能力，无法充分表示和操作抽象概念。此外，CoT 每次只采样一个 token，导致推理路径单一，容易陷入错误分支，且效率低下。
- **研究动机**：借鉴人类认知中通过抽象、流体的概念进行推理（而非严格的离散语言符号）的机制，旨在突破离散 token 推理的瓶颈，使 LLM 能够像人类一样同时考虑多种可能性、整合抽象概念，实现更灵活、并行且全面的推理。
- **整体含义**：提出一种无需训练的推理范式 Soft Thinking，将 LLM 的推理从离散 token 空间扩展到连续概念空间，通过概率加权混合 token 嵌入生成“概念 token”，从而保留完整概率分布，隐式探索多条路径，提升推理准确性和 token 效率。

## 2. 方法论：核心思想、关键技术细节

### 2.1 核心思想
- **概念 token (Concept Token)**：在每一步推理中，用 LLM 输出的完整概率分布 $p \in \Delta^{|V|-1}$ 作为概念 token，而非采样单一离散 token。
- **连续概念空间 (Continuous Concept Space)**：所有 token 嵌入的概率加权混合构成的凸组合集合 $C = \{ \sum_k \alpha_k e(k) : \alpha \in \Delta^{|V|-1} \}$。
- **推理过程**：每一步生成概念 token 后，通过公式 $\tilde{e}_{\text{next}} = \sum_k p[k] e(k)$ 计算下一个输入嵌入，将其喂入模型，重复直到检测到结束标记。输出阶段仍使用离散采样。
- **Cold Stop 机制**：为缓解 OOD 导致的生成崩溃，实时监控概念 token 的熵 $H(p) = -\sum p[k] \log p[k]$，若连续 $k$ 步熵低于阈值 $\tau$，则提前插入结束标记终止中间推理，切换至答案生成，从而提升效率并防止模型崩溃。

### 2.2 技术细节
- **无需训练**：直接复用模型现有嵌入矩阵，无额外参数或层，不更新权重。
- **轻量计算**：每步仅需对 top-k（如 10-15）个 token 进行矩阵-向量乘法（复杂度 $O(n \cdot d)$），熵计算复杂度 $O(|V|)$，但相较于模型前向传播可忽略。
- **理论支撑**：将标准 CoT 的指数级路径求和（Eq.7）通过一阶线性近似递归地压缩为单一路径，概念 token 对应概率分布的期望，保留所有路径信息。

## 3. 实验设计

### 3.1 数据集与场景
- **数学推理**：MATH500、AIME 2024、GSM8K、GPQA-Diamond（共 4 个）。
- **编程推理**：HumanEval、MBPP、LiveCodeBench（共 3 个）。

### 3.2 Benchmark 对比方法
- **CoT Thinking（标准 CoT）**：step-by-step 推理，每个问题采样 16 次计算 pass@1。
- **CoT Thinking (Greedy)**：贪心解码，温度设为 0，每次只采样 1 个结果。
- **Soft Thinking（本文方法）**：使用概念 token 推理，参数：top-n（n=5~30），熵阈值（0.01~0.2），连续步数阈值（128~1024），温度 0.6，top-k=30，top-p=0.95。

### 3.3 模型
- **QwQ-32B**（强化学习训练），**DeepSeek-R1-Distill-Qwen-32B**（监督蒸馏），**DeepSeek-R1-Distill-Llama-70B**（监督蒸馏）。覆盖 32B 和 70B 参数规模，Qwen 和 LLaMA 架构。

## 4. 资源与算力
- **硬件**：8 张 NVIDIA H100 80GB GPU（未明确提及训练时长，因其为无需训练的方法，仅推理）。
- **框架**：基于 SGLang 实现快速推理。
- **计算开销**：每步额外开销为 top-k 加权平均和熵计算，相比单次前向传播很小。

## 5. 实验数量与充分性
- **实验数量**：在 7 个数据集上测试了 3 个模型（共 21 个实验组合），每个方法在数学和代码任务上均有结果，并报告了 accuracy 和 generation length 两个指标。
- **消融实验**：比较了三种策略：1) COCONUT-TF（训练-free 版本直接使用隐藏状态）；2) Average Embedding（简单平均 top-n 嵌入）；3) Soft Thinking（概率加权）。同时对比了有无 Cold Stop 的效果。
- **充分性评估**：实验覆盖了多种规模、架构、训练范式的模型，数据集涵盖数学和编程领域，消融实验验证了各组件的必要性。但未报告误差条或统计显著性检验（论文提到 16 次采样计算 pass@1，但未显示误差范围）。总体公平客观，但可进一步强化统计可靠性。

## 6. 主要结论与发现
- **性能提升**：Soft Thinking 在几乎所有数据集和模型上一致提升 pass@1 准确率，最高提升 2.48 个百分点（QwQ-32B 数学平均），在 AIME2024 上提升达 6.45%。
- **Token 效率提升**：同时显著降低生成长度（最多降低 22.4%），表明推理更简洁高效。
- **贪心解码的缺陷**：贪心 CoT 虽然减少 token，但性能大幅下降，而 Soft Thinking 在提升效率的同时保持甚至提升准确率，打破了性能与效率的传统权衡。
- **可解释性**：定性分析显示，Soft Thinking 生成的中间步高度可读，且概率分布显示其在探索阶段分布较均匀，在精确计算阶段趋于 one-hot，体现了路径探索能力。
- **Cold Stop 有效性**：消融表明，缺少 Cold Stop 时模型易因 OOD 崩溃（生成无限重复），Cold Stop 有效抑制崩溃并缩短无效计算。

## 7. 优点
- **无需训练**：即插即用，可直接应用于任何 LLM 的 CoT 流程，无需修改模型架构或权重。
- **性能与效率双赢**：同时提升准确率和效率，这是许多方法难以实现的。
- **理论严谨**：提供了数学推导说明如何通过线性近似将指数路径求和压缩为单一路径。
- **可解释性强**：概念 token 的概率分布可视化清晰展示了推理过程的不确定性和决策演变。
- **易于复现**：基于开源 SGLang 实现，关键代码在附录公开。

## 8. 不足与局限
- **OOD 问题**：模型从未在连续概念 token 上训练，推理时存在分布外（OOD）问题，虽用 Cold Stop 缓解但未从根本上解决。未来需探索训练策略使模型适应连续空间。
- **缺乏统计显著性报告**：未提供误差条或置信区间，仅报告单次平均结果（虽然用了 16 采样，但未体现方差）。
- **实验覆盖**：仅测试了数学和代码任务，未涉及常识推理、逻辑推理、多模态等其他领域。
- **Cold Stop 阈值灵敏度**：需手动调参（熵阈值和连续步数阈值），不同任务可能需不同设置，鲁棒性未充分探讨。
- **可扩展性**：仅测试了 32B/70B 模型，未在更大模型（如 100B+）或小模型（如 7B）上验证，结论泛化性受限。
- **计算开销对比**：虽声称每步开销很小，但未与标准 CoT 进行严格延迟或吞吐量对比，仅报告 token 长度缩减。

（完）
