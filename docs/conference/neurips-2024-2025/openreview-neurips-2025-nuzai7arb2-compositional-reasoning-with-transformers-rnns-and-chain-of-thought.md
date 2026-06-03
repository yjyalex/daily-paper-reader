---
title: "Compositional Reasoning with Transformers, RNNs, and Chain of Thought"
title_zh: Transformer、RNN和思维链的组成推理
authors: "Gilad Yehudai, Noah Amsel, Joan Bruna"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=nUZaI7aRb2"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 分析思维链在组合推理中的表达能力
tldr: 证明Transformer、RNN和思维链在组合推理任务上的理论极限
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 不同神经网络架构在任务上的表达能力尚未被系统比较，尤其是在需要多步推理的组成推理问题上。
method: 理论分析，定义组成推理问题（CRQ）并证明各架构需要超参数随输入增长才能解决。
result: 证明在标准硬度假设下，三种架构均无法高效解决CRQ，除非深度、嵌入维度或思维链令牌数随输入增长。
conclusion: 为理解思维链在推理中的必要性和局限性提供了理论基础。
---

## Abstract
It is understood that different neural network architectures are suited to different tasks, but is there always a single best architecture for a given task? We compare the expressive power of transformers, RNNs, and transformers with chain of thought tokens on a simple and natural class of tasks we term Compositional Reasoning Questions (CRQ). This family captures multi-step problems with tree-like compositional structure, such as evaluating Boolean formulas. We prove that under standard hardness assumptions, *none* of these three architectures is capable of solving CRQs unless some hyperparameter (depth, embedding dimension, and number of chain of thought tokens, respectively) grows with the size of the input. We then provide constructions for solving CRQs with each architecture. For transformers, our construction uses depth that is logarithmic in the problem size. For RNNs, logarithmic embedding dimension is necessary and sufficient, so long as the inputs are provided in a certain order. For transformers with chain of thought, our construction uses $n$ CoT tokens. These results show that, while CRQs are inherently hard, there are several different ways for language models to overcome this hardness. Even for a single class of problems, each architecture has strengths and weaknesses, and none is strictly better than the others.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

大型语言模型在逻辑推理等需要算法思维的任务中越来越广泛使用。然而，不同神经网络架构（Transformer、RNN、带思维链的 Transformer）究竟谁更适合这类任务？现有工作常选用利于某一架构的特定任务进行比较，缺乏对同一任务全面、公平的对比。本文提出一类统一的组合推理问题（Compositional Reasoning Questions, CRQ），它捕捉了多步推理中固有的树状组合结构（如布尔公式求值）。通过严格的理论分析，论文证明了在标准计算复杂性假设（TC⁰ ≠ NC¹）下，三种架构都不能在不扩大超参数（深度、嵌入维度、CoT令牌数）的情况下解决任意CRQ；但同时每种架构都提供了一种低资源消耗的解决途径。结论是：**对于同一类推理任务，没有一种架构绝对优于其他，每种架构都有其独特的资源权衡**。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **CRQ 定义**：一棵有根树，每个节点 \(v\) 有一向量标签 \(x_v\)（来自有限词汇）。叶节点的答案为 \(x_v\)；非叶节点的答案由其子节点的答案经 \(\arg\max\) 运算得到：\(A(v)=\arg\max_{u\in C(v)} \langle A(u), x_v\rangle\)。该定义可覆盖布尔公式求值等经典问题。
- **Transformer 深度必要性与充分性**：
  - **充分性**：用 \(L-1\) 层 Transformer（嵌入维度 \(O(d+\log n)\)）解决深度为 \(L\) 的 CRQ。每层自注意力让同一深度的子问题并行求解，然后通过 MLP 更新节点状态，逐层向上计算。
  - **必要性**：常数深度 Transformer 属于 TC⁰ 复杂度类，而 CRQ 是 NC¹ 完全的，因此在 TC⁰ ≠ NC¹ 假设下，常数深度无法解决所有 CRQ。
- **RNN 隐藏维度与输入顺序**：
  - **输入顺序至关重要**。任意顺序可能导致隐藏维度需 \(\Omega(n)\)（归约自集合不相交问题）。若使用**内存秩排序**（Memory-Rank Sort）按后序遍历顺序输入（优先处理内存秩大的子节点），则可用隐藏维度 \(O(d\log n)\) 的 RNN 解决所有二叉 CRQ。内存秩是树的最小所需栈大小的度量，对于 \(n\) 节点树不超过 \(\log n\)。
  - 构造上，RNN 通过 5 层 MLP 模拟栈操作：根据当前节点与栈顶深度关系决定压栈或弹栈计算，最终根节点结果即为答案。
- **带 CoT 的 Transformer**：
  - 使用 2 层 Transformer，嵌入维度 \(O(d+\log n)\)，生成 \(n\) 个 CoT 令牌（每个对应一个非叶子节点解）。通过逆向 BFS 位置编码，使每步自注意聚焦于下一个待解子问题，逐层自底向上求解。
  - 下界：在 TC⁰ ≠ NC¹ 下，\(O(\log n)\) 个 CoT 令牌不够；在有限精度下，\(\Omega(n)\) 个 CoT 是必要的（Amiri et al., 2025）。

### 3. 实验设计

本文为纯理论论文，未进行任何实验。所有结果均为确定性构造与证明，不涉及数据集、基准方法或经验对比。仅通过复杂度归约和紧的下界来说明结论。

### 4. 资源与算力

文中未提及任何计算资源（GPU 型号、数量、训练时长等）。因无实验，不涉及实际算力消耗。

### 5. 实验数量与充分性

无实验。但理论分析覆盖了三种架构的正、反两面，给出了紧的上下界（见表 1），逻辑完整，充分证明了核心论点。

### 6. 论文的主要结论与发现

- **三种架构面对 CRQ 均需资源随输入规模增长**，但各有侧重：
  - 深度 Transformer：所需深度为树深（对数），参数规模大但并行时间常/对数。
  - RNN：所需隐藏维度为对数，使用适当输入顺序可线性时间，并行时间线性。
  - 带 CoT 的 Transformer：参数对数，需生成 \(n\) 个 CoT 令牌，无并行性。
- **输入顺序对 RNN 影响巨大**：不利顺序需要 \(\Omega(n)\) 隐藏维度；利用内存秩排序可降至 \(O(\log n)\)。
- **CRQ 是 NC¹ 完全的**，是衡量 LLM 推理能力的理论标尺。
- **结论**：即便在同一推理任务上，各架构表现依赖超参数选择，没有架构绝对最优，提示应结合多种思路（深层 Transformer、高效 RNN、测试时缩放 CoT）改进 LLM 推理。

### 7. 优点

- **理论清晰且严格**：给出紧的上下界，揭示不同架构的本质 trade-off。
- **任务定义统一**：CRQ 兼顾简单性与表达能力，可覆盖布尔公式、算术运算等常见推理。
- **构造精巧**：Transformer 的逐层并行、RNN 的内存秩排序、CoT 的逐令牌求解均具启发性。
- **公平比较**：在相同任务上对比三种架构，避免以往选择利于某架构的任务的偏颇。
- **面向开放问题**：明确指出了 CoT 令牌数下界与上界之间的 Gap 等未来方向。

### 8. 不足与局限

- **仅理论表达性**：论文只分析表达能力，未涉及可学习性（优化过程、泛化能力），实际训练中模型未必学到这些构造。
- **假设理想化**：假设硬最大注意力、精确计算、输入顺序可控等，与真实 Softmax 注意力和随机初始化训练有差距。
- **CoT 令牌数 Gap 未完全解决**：理论上界为 \(n\)，下界在对数到线性之间（对数精度下仅知 \(\Omega(\log n)\)，有限精度下为 \(\Omega(n)\)），最优性仍开放。
- **未覆盖其他架构**：未考虑 SSM、Hybrid 模型等最新变种。
- **任务局限**：CRQ 虽具代表性，但实际推理任务可能包含更多嵌套、语境、模糊性，简单树结构不足以完全刻画。

（完）
