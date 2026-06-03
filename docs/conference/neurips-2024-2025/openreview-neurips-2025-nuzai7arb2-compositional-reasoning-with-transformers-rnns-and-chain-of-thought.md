---
title: "Compositional Reasoning with Transformers, RNNs, and Chain of Thought"
title_zh: Transformer、RNN与思维链的组合推理能力
authors: "Gilad Yehudai, Noah Amsel, Joan Bruna"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=nUZaI7aRb2"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 跨架构的组合推理任务比较，包含思维链
tldr: 比较Transformer、RNN和带思维链的Transformer在组合推理上的表达能力，证明所有架构需超参数扩展。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 不同架构处理组合推理任务的能力差异尚不明确。
method: 定义组合推理问题族，理论上证明三种架构均需超参数随输入增长才能解决。
result: 提供了具体构造证明所有架构在某些假设下等价。
conclusion: 组合推理对任何架构都是非平凡的。
---

## Abstract
It is understood that different neural network architectures are suited to different tasks, but is there always a single best architecture for a given task? We compare the expressive power of transformers, RNNs, and transformers with chain of thought tokens on a simple and natural class of tasks we term Compositional Reasoning Questions (CRQ). This family captures multi-step problems with tree-like compositional structure, such as evaluating Boolean formulas. We prove that under standard hardness assumptions, *none* of these three architectures is capable of solving CRQs unless some hyperparameter (depth, embedding dimension, and number of chain of thought tokens, respectively) grows with the size of the input. We then provide constructions for solving CRQs with each architecture. For transformers, our construction uses depth that is logarithmic in the problem size. For RNNs, logarithmic embedding dimension is necessary and sufficient, so long as the inputs are provided in a certain order. For transformers with chain of thought, our construction uses $n$ CoT tokens. These results show that, while CRQs are inherently hard, there are several different ways for language models to overcome this hardness. Even for a single class of problems, each architecture has strengths and weaknesses, and none is strictly better than the others.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型被广泛用于逻辑推理等需要算法思维的任务，但不同神经网络架构（Transformer、RNN、带思维链的Transformer）在处理此类任务时各具特点。现有理论工作往往针对单一架构或单一任务进行表达性分析，缺乏对同一任务下不同架构的横向对比。
- **核心问题**：对于一类具有树状组合结构的推理问题——组合推理问题（Compositional Reasoning Questions, CRQ），这三种架构是否都能解决？是否必须依赖某些随输入规模增长的超参数？是否存在一种架构绝对优于其他架构？
- **背景**：CRQ 捕捉了布尔公式求值等典型的组合推理问题，其本质是 NC¹-完全问题。论文旨在揭示不同架构在解决同一类问题时固有的资源权衡。

## 2. 方法论：核心思想、关键技术细节

- **CRQ 定义**：一棵根树，每个节点用有限词汇中的向量标记，非叶子节点的答案递归定义为对子节点答案与自身向量的点积取 argmax。布尔公式求值是其特例。
- 针对三种架构分别给出了**充分性构造**（上界）和**必要性下界**（基于计算复杂度假设）：
  - **深度Transformer**：
    - 上界（定理4.1）：存在深度为 L-1、嵌入维度 O(d+log n) 的 Transformer，可解决深度不超过 L、节点数不超过 n 的所有 CRQ。核心是利用层级并行性，通过精心设计的注意力模式逐层同时求解同一深度的子问题。
    - 下界（定理4.3）：在 TC⁰ ≠ NC¹ 假设下，常数深度 Transformer 无法求解所有 CRQ。
  - **RNN**：
    - 上界（定理5.4）：如果节点按“记忆等级排序”（Memory-Rank Sort）输入，存在 5 层、隐藏维度 O(d log n) 的 RNN 能解决任意二叉 CRQ。核心是模拟栈机，通过辅助栈操作逐步计算结果。
    - 下界（定理5.2、定理5.5）：若节点输入次序敌意，RNN 需要 Ω(n) 隐藏状态；即使采用最优排序，隐藏状态大小也必须达到 Ω(mr(T))，其中 mr(T) 是树的记忆等级（对任意 n 节点树不超过 log n）。
  - **带思维链的浅层Transformer**：
    - 上界（定理6.1）：存在 2 层、嵌入维度 O(d+log n)、生成 n 个 CoT 令牌的 Transformer 可解决任意 n 节点 CRQ。核心是使用逆BFS顺序逐步生成结果，每个 CoT 令牌对应一个子问题的答案。
    - 下界（隐含）：在 TC⁰ ≠ NC¹ 假设下，生成 o(log n) 个 CoT 令牌不够；在有限精度下 Ω(n) 个 CoT 令牌是必要的。
- **关键概念**：记忆等级是衡量树规模所需栈深度的一种递归定义，用于指导 RNN 的输入顺序。

## 3. 实验设计

- **论文未包含任何实验**。所有结论均基于严格的理论证明和复杂度归约，属于理论计算机科学与机器学习交叉的纯理论工作。
- 没有使用数据集、benchmark 或对比基线方法。

## 4. 资源与算力

- **未涉及任何实际计算资源**（GPU、训练时间等）。论文是理论分析，不涉及模型训练或推理的实验性验证。

## 5. 实验数量与充分性

- **无实验**。因此无法评价实验数量或充分性。但论文的证明体系完整：对每种架构都给出了上界构造和下界证明，且下界依赖于标准的计算复杂度假设（TC⁰ ≠ NC¹），保证了理论结果的严谨性。

## 6. 主要结论与发现

- **没有一种架构在不增长超参数的情况下能解决所有 CRQ**：Transformer 深度必须随树深度增长；RNN 隐藏维度必须至少随记忆等级增长；带 CoT 的 Transformer 需要生成至少 Ω(log n) 个 CoT 令牌（实际上界为 n）。
- **每种架构在某种资源上具有优势**：
  - 深度 Transformer：高度并行，但模型规模（深度）依赖问题大小。
  - RNN：参数规模小、运行时间近线性，但依赖输入顺序且不可并行。
  - 带 CoT 的 Transformer：参数规模仅为对数级，但运行缓慢且不可并行，CoT 令牌数需线性。
- **具体对比**：在 n 节点平衡二叉树（深度 log n）情形下，三种架构的复杂度如表 1 所示（原文 Table 1）：深度 Transformer 参数 O(L log² n)、运行时 O(L n² log n)、并行运行时 O(L)；RNN 参数 [O(log n), O(n)]、运行时 [O(n log n), O(n²)]、并行运行时 O(n)；带 CoT 的 Transformer 参数 O(log² n)、运行时 O(n² log² n)、并行运行时 O(n)。
- **结论**：即使对于单一任务，也不存在绝对最优的架构；研究必须考虑不同资源的权衡。

## 7. 优点

- **统一框架**：提出 CRQ 作为研究组合推理的简洁形式化框架，能自然捕获布尔公式求值（NC¹-完全问题）等重要问题。
- **全面对比**：同时在深度 Transformer、RNN 和带 CoT 的浅层 Transformer 三种架构上进行上界构造和下界证明，提供了完整的资源权衡图谱（如表 1 所示）。
- **新颖的见解**：揭示了 RNN 对输入顺序的敏感性——敌意顺序导致 Ω(n) 记忆，但利用记忆等级排序可将隐藏维度降至对数；带 CoT 的 Transformer 在生成令牌数上相比 Feng et al. (2024) 从 O(n²) 改进到 O(n)。
- **理论深度**：所有证明严谨，使用了复杂度理论（TC⁰、NC¹）、通信复杂度、概率方法等工具。

## 8. 不足与局限

- **无实验验证**：理论构造虽然严谨，但未在真实模型上进行数值验证，无法说明实际训练中能否学到这些构造。
- **假设限制**：结果依赖于标准但未被证明的复杂度假设（TC⁰ ≠ NC¹），部分结论（如 CoT 下界）在有限精度下更优，但在 log 精度下是否最优仍开放。
- **CRQ 定义的范围**：当前定义基于点积的 argmax，虽然能够嵌入布尔逻辑和算术（见附录 F），但实际推理任务可能涉及更复杂的操作（如集合运算、条件分支），需要进一步扩展。
- **未讨论泛化能力**：论文只关注表达能力，不涉及模型能否通过训练学习到这些解决方案，以及是否能在不同树结构、大小上泛化。
- **CoT 令牌数量最优性悬而未决**：上界为 n，下界仅为 Ω(log n)，存在很大 gap，有待后续工作填补。

（完）
