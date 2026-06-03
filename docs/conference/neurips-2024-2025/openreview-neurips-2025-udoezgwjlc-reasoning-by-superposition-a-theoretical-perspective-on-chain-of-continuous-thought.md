---
title: "Reasoning by Superposition: A Theoretical Perspective on Chain of Continuous Thought"
title_zh: 叠加推理：连续思维链的理论视角
authors: "Hanlin Zhu, Shibo Hao, Zhiting Hu, Jiantao Jiao, Stuart Russell, Yuandong Tian"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=UdOEZgWJLc"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 对LLMs中连续思维链推理的理论分析
tldr: 证明少步连续思维链可解决图可达问题，推进思维链理论。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有离散思维链的理论已充分，但连续思维链的理论缺乏，需解释其优势。
method: 利用两层Transformer模型证明连续思维链在固定步数内可解决有向图可达性问题。
result: 理论上证明了连续思维链相对离散思维链的优势。
conclusion: 连续思维链能够以较少步数实现更强的推理能力，为实际应用提供理论基础。
---

## Abstract
Large Language Models (LLMs) have demonstrated remarkable performance in many applications, including challenging reasoning problems via chain-of-thought (CoT) techniques that generate ``thinking tokens'' before answering the questions. While existing theoretical works demonstrate that CoT with discrete tokens boosts the capability of LLMs, recent work on continuous CoT lacks a theoretical understanding of why it outperforms discrete counterparts in various reasoning tasks, such as directed graph reachability, a fundamental graph reasoning problem that includes many practical domain applications as special cases. In this paper, we prove that a two-layer transformer with $D$ steps of continuous CoT can solve the directed graph reachability problem, where $D$ is the diameter of the graph, while the best known result of constant-depth transformers with discrete CoT requires $O(n^2)$ decoding steps where $n$ is the number of vertices ($D<n$). 
In our construction, each continuous thought vector is a superposition state that encodes multiple search frontiers simultaneously (i.e., parallel breadth-first search (BFS)), while discrete CoT must choose a single path sampled from the superposition state, which leads to a sequential search that requires many more steps and may be trapped in local solutions.
We also performed extensive experiments to verify that our theoretical construction aligns well with the empirical solution obtained via training dynamics. Notably, encoding of multiple search frontiers as a superposition state automatically emerges in training continuous CoT, without explicit supervision to guide the model to explore multiple paths simultaneously.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文内容，以下是对该论文的详细中文总结。

### 论文详细中文总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）

*   **研究动机**：大型语言模型（LLMs）在推理任务中表现出色，链式思维（CoT）通过生成中间“思考令牌”进一步提升了其能力。尽管关于离散令牌CoT的理论研究已较为充分，但近期提出的**连续思维链**（Chain-of-Continuous-Thought, CoCONUT）在诸多推理任务上（如数学推理GSM8K，特别是图推理问题）超越了离散CoT，其内在机制和表达能力却缺乏理论解释。尤其是CoCONUT能在更少的推理步骤下解决复杂问题。
*   **核心问题**：为什么连续思维链比离散思维链更具表达能力？其背后的工作机制是什么？
*   **研究意义**：该工作旨在为连续思维链的优越性提供首个严格的理论证明，并揭示其核心机制——**叠加态**推理。这不仅加深了对LLM推理能力的理解，也为设计更高效的推理架构提供了理论基础。

#### 2. 论文提出的方法论：核心思想、关键技术细节

*   **核心思想**：连续思维向量可以被视为一种**叠加态**，它能够在一次推理步骤中同时编码**多个搜索前沿**（即并行广度优先搜索 BFS）。而离散CoT必须从叠加态中“采样”出一个单一路径进行顺序搜索，这需要更多步骤且可能陷入局部解。作者将连续思维与离散思维的对比，类比于量子力学中的叠加态与坍缩态。
*   **关键技术细节**：
    *   **问题设定**：聚焦于**有向图可达性问题**。给定一个有向图、一个起始节点和两个候选目标节点，判断哪个是由起始节点可达的。问题被转化为一个适合Transformer处理的序列格式。
    *   **理论构造**：作者构造了一个**两层Transformer**模型，其参数与图的具体内容无关。
        *   第一层注意力：使用多个**注意力选择器**（Attention Chooser），根据不同的特殊令牌（如`<e>`, `<R>`, `<A>`）定位并复制相应的源节点、目标节点、候选节点信息到指定缓存空间。
        *   第二层注意力：当前一步的连续思维`[t_c]`（叠加态）作为查询，**平行地**关注所有源节点在当前可达集合`V_c`中的边令牌，将相应的目标节点信息聚合回当前思维，实现一步BFS扩展，得到新的叠加态`[t_{c+1}]`。
        *   MLP层：作为**噪声过滤器**。在注意力机制中，由于softmax的非理想性，叠加态中会混入噪声。MLP通过硬阈值激活函数（如`σ(x) = 1{x ≥ ε}`）过滤掉低权重噪音，并归一化所有可达节点的权重，使其保持均匀的叠加态。这是一个关键工程细节，确保了后续推理的精确性。
        *   最后预测：通过一个特殊的答案令牌`<A>`“测量”最终的叠加态`[t_C]`，通过比较`c1`和`c2`在叠加态中的信号强度（即它们是否在可达集`V_C`中）来输出答案。
*   **核心算法流程**（文字说明）：
    1.  将问题提示（包括图边、节点、根节点和候选节点）转换为输入序列。
    2.  对于第C步连续思维，Transformer以自回归方式生成`[t_{c+1}]`。关键点在于，`[t_c]`作为输入，其查询向量会与所有边令牌的键向量计算注意力分数，只有源节点在`V_c`中的边才获得高注意力，从而将其目标节点信息拉取到新的`[t_{c+1}]`中。
    3.  MLP过滤噪音并归一化，使`[t_{c+1}]`形成`V_{c+1}`的均匀叠加态。
    4.  经过D步（图的直径），最终叠加态`[t_D]`包含了所有可达节点。插入`<A>`令牌，模型比较两个候选节点在该状态中的权重，输出可达的那个。

#### 3. 实验设计

*   **数据集**：使用ProsQA数据集的一个子集，专门选取**需要3-4跳推理**的问题。每个图中的节点被用作词汇表中的专用令牌。
*   **基准方法 (Benchmark)**：
    *   **Coconut**：本文研究的连续思维链方法。
    *   **CoT**：传统的离散令牌链式思维。
    *   **CoT\***：层数更多（12层）的离散CoT模型，作为强基线。
    *   **No CoT**：不进行链式思维，直接输出答案。
*   **主要对比**：比较了Coconut与上述基线方法在图可达性问题上的**准确率**。

#### 4. 资源与算力

*   **算力**：论文正文未明确提及总训练算力，但在附录C.3中说明，**使用两个Nvidia A100 80GB GPU，每个COCONUT实验运行约需24小时**。

#### 5. 实验数量与充分性

*   **实验数量**：论文进行了多组实验，包括：
    *   **整体性能对比**：一个准确率对比图，展示了Coconut、CoT、CoT\*和No CoT的性能。
    *   **机制验证实验**：
        *   **注意力模式可视化**：展示了第一层和第二层注意力头的实际注意力分配图，验证其是否与理论构造（如复制边缘信息、关注可达节点）一致。
        *   **连续思维表示分析**：计算连续思维向量与不同类别节点（可达、不可达、前沿、最优）的**内积**，观察叠加态的构成，并绘制了直方图。
    *   **探索偏好消融实验**：引入**COCONUT-BFS**变体，其训练监督信号由“最优路径节点”改为“随机前沿节点”，以此探究多阶段训练对探索策略的影响。
    *   **稳定性验证**：附录C.2中，使用了**3个不同的随机种子**重复实验，报告了内积值的均值和标准差，以检验结果的稳定性。
*   **充分性与公正性**：
    *   **充分性**：实验设计较为全面，不仅覆盖了最终性能，还深入模型内部（注意力模式、表示空间）验证了理论机制，并通过消融实验探讨了不同监督信号的影响。使用多种随机种子保证了结果的稳定性。
    *   **客观公平**：在对比中，Coconut使用了较小的2层模型，而CoT基线中包含了更大的12层模型，这种对比能有力说明Coconut方法的效率优势。实验设置和超参数描述较为清晰。

#### 6. 论文的主要结论与发现

1.  **理论证明**：严格证明了**两层Transformer**配合**D步**（图的直径）连续思维链即可解决有向图可达性问题，而同等深度的离散CoT需要**O(n²)** 步。
2.  **核心机制**：连续思维链的卓越能力来源于**叠加态**的编码方式，它允许模型在一次前向过程中**隐式地执行并行广度优先搜索**。
3.  **涌现行为**：实验结果表明，仅使用最优路径的训练数据，模型就能自动学会在连续思维中编码**多个搜索前沿**，即叠加态的表示方式是**自发涌现**的，无需显式监督。
4.  **实证匹配理论**：实验中的注意力模式和表示分析结果与理论构造高度一致，验证了理论模型的有效性。

#### 7. 优点

*   **理论贡献突出**：首次为连续思维链的优越性提供了严谨的理论证明，特别是针对图推理这一重要领域。该理论清楚地揭示了连续和离散思维的本质效率差异。
*   **理论与实验结合紧密**：实验设计具有明确的理论指导性，不仅验证了最终性能，更深入剖析了模型内部是否在执行理论预设的“叠加态搜索”机制，形成了“理论预测-实验验证”的闭环。
*   **对通用位置编码的适用性**：理论构造不仅适用于正弦编码，还在附录中扩展到了更现代的**RoPE旋转位置编码**，增强了理论的普适性。
*   **工程启示**：通过揭示叠加态机制，为设计更高效的推理算法和模型架构提供了清晰的理论基础和优化方向。

#### 8. 不足与局限

*   **任务局限性**：论文的理论分析和实验主要集中在**有向图可达性**这一特定问题。虽然该问题具有广泛代表性，但其结论能否直接、完整地推广到更复杂的推理任务（如数学推理、代码生成）仍需进一步研究。
*   **模型深度限制**：理论构造和核心实验都基于**两层Transformer**。尽管这足以证明机制的优越性，但在更深、更现实的模型中，叠加态的维持和演化机制可能会更复杂，现有理论无法完全覆盖。
*   **训练动态分析不充分**：论文观察到模型会自发地探索“优质”或“前沿”节点，但**缺乏对训练动态的深层理论解释**，即为什么仅用最优路径监督就能涌现出这种“优先搜索”行为，这被作者保留为未来工作。
*   **实验规模与复杂度**：实验数据集（ProsQA子集）的规模有限，问题复杂度为3-4跳。在更大规模、更高跳数的图上，COCONUT的性能和资源消耗如何，论文未做详尽讨论。
*   **任务简化**：图可达性任务被简化为二分类（判断两个候选节点中的哪一个可达），这简化了输出空间。在更复杂的任务（如要求模型生成路径）中，叠加态机制的优势和表现还有待验证。

（完）
