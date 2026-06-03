---
title: "Reasoning by Superposition: A Theoretical Perspective on Chain of Continuous Thought"
title_zh: 通过叠加进行推理：连续思维链的理论视角
authors: "Hanlin Zhu, Shibo Hao, Zhiting Hu, Jiantao Jiao, Stuart Russell, Yuandong Tian"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=UdOEZgWJLc"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 思维链推理能力的理论分析
tldr: 证明连续思维链使transformer可解图可达性，优于离散思维链。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有理论缺乏对连续思维链优势的理解。
method: 通过理论分析证明两层transformer在多步连续思维链下可解图可达性问题。
result: 证明了连续思维链在特定推理任务上比离散思维链表达能力更强。
conclusion: 连续思维链在理论上具有优势，值得进一步探索。
---

## Abstract
Large Language Models (LLMs) have demonstrated remarkable performance in many applications, including challenging reasoning problems via chain-of-thought (CoT) techniques that generate ``thinking tokens'' before answering the questions. While existing theoretical works demonstrate that CoT with discrete tokens boosts the capability of LLMs, recent work on continuous CoT lacks a theoretical understanding of why it outperforms discrete counterparts in various reasoning tasks, such as directed graph reachability, a fundamental graph reasoning problem that includes many practical domain applications as special cases. In this paper, we prove that a two-layer transformer with $D$ steps of continuous CoT can solve the directed graph reachability problem, where $D$ is the diameter of the graph, while the best known result of constant-depth transformers with discrete CoT requires $O(n^2)$ decoding steps where $n$ is the number of vertices ($D<n$). 
In our construction, each continuous thought vector is a superposition state that encodes multiple search frontiers simultaneously (i.e., parallel breadth-first search (BFS)), while discrete CoT must choose a single path sampled from the superposition state, which leads to a sequential search that requires many more steps and may be trapped in local solutions.
We also performed extensive experiments to verify that our theoretical construction aligns well with the empirical solution obtained via training dynamics. Notably, encoding of multiple search frontiers as a superposition state automatically emerges in training continuous CoT, without explicit supervision to guide the model to explore multiple paths simultaneously.

---

## 论文详细总结（自动生成）

## 论文详细总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：大型语言模型（LLMs）在推理任务中通过思维链（Chain-of-Thought, CoT）生成中间“思考令牌”显著提升了能力。现有理论主要关注离散CoT（每个步骤输出离散文本 token），但对于近年提出的连续CoT（如COCONUT）为何能超越离散CoT，尤其在图可达性等推理任务中，缺乏理论理解。
- **核心问题**：连续CoT究竟如何提升表达能力？其内部机制是什么？
- **整体意义**：本文首次从理论上证明，连续CoT通过**叠加状态（superposition state）** 实现并行广度优先搜索（BFS），可以高效解决图可达性问题；而离散CoT只能进行串行搜索，所需步骤更多（O(n²) vs D步，n为顶点数，D为图直径）。这一发现解释了连续CoT在特定推理任务上的优势，并为理解连续思维表示提供了理论基础。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：每个连续思维向量（continuous thought）不是表示单一顶点的嵌入，而是编码了当前可达的所有顶点集合的**叠加状态**（即这些顶点嵌入的归一化和）。通过这种方式，模型在一个步骤内就能“并行”地扩展所有前沿节点，实现BFS。
- **关键技术细节**：
  - **模型架构**：一个**两层Transformer**，使用GPT-2风格的解码器（d_model=768, n_heads=8），从零训练。
  - **注意力选择器（Attention Chooser）**：作为构建块，使得特定token（如<e>）可以高注意力关注其源节点和目标节点，而其他token则关注序列开头（注意力汇集现象）。这依赖于正弦位置编码（或RoPE）的旋转性质。
  - **连续思维生成过程**：
    1. **第一层注意力**：通过五个注意力头，将每个边token的源节点和目标节点嵌入复制到该token的缓冲区空间。
    2. **第一层MLP**：过滤噪声，均衡权重。
    3. **第二层注意力**：当前连续思维向量（代表V_c）通过注意力机制指向其对应出边的边token，并将目标节点嵌入加回，形成新的扩展集合V_{c+1}的叠加。
    4. **第二层MLP**：再次滤波和归一化，得到下一个连续思维向量。
  - **最终预测**：通过测量答案token与候选目标节点的内积，选择信号更强的节点输出。
- **位置编码**：理论构造兼容正弦位置编码和RoPE，不需要为特定问题或长度定制，更具实用性。
- **数学表述**：连续思维向量 [t_c] = 1/√|V_c| Σ_{v∈V_c} u_v ，其中 u_v 是顶点v的嵌入。

### 3. 实验设计
- **数据集**：采用**ProsQA**（Hao et al., 2024）的子集，要求解决方案需要3-4推理步。数据集统计：训练14785例、验证257例、测试419例，平均每图约22.8个顶点、36.5条边，方案长度3.5。
- **Benchmark**：图可达性判定问题（给定有向图、根节点、两个候选节点，判断哪个可达）。
- **对比方法**：
  - **COCONUT**（连续CoT）：本文方法，两层Transformer。
  - **CoT**（离散CoT）：同样两层Transformer，但每步生成离散token。
  - **CoT***：12层12头的大模型离散CoT。
  - **No CoT**：直接预测答案，无中间步骤。
- **评估指标**：准确率（Accuracy），随机基线为50%。

### 4. 资源与算力
- **硬件**：每次运行使用**两张Nvidia A100 80GB GPU**。
- **训练时长**：每个COCONUT运行约**24小时**（训练300个epoch，每阶段25epoch，多阶段训练）。
- **备注**：文中明确给出该信息（附录C.3）。

### 5. 实验数量与充分性
- **主要实验组**：
  - 整体准确率对比（图4）：所有方法在测试集上评估。
  - 注意力模式分析（表1、图5）：量化第二层注意力对不同类型边（可达/不可达/前沿/最优）的分配。
  - 连续思维表示可视化（图6）：计算连续思维与各节点嵌入的内积，按节点类型分组。
  - 探索优先级实验（COCONUT-BFS）：用随机前沿节点而非最优节点进行监督训练，验证搜索行为是否保持不变。
  - 稳定性测试（附录表5）：使用3个随机种子重复实验，报告内积均值的标准差。
- **充分性评价**：实验设计较为全面：
  - 既对比了离散CoT（包括大模型），也对比了无CoT基线。
  - 不仅报告最终准确率，还深入分析了内部表示和注意力模式，验证理论机制。
  - 通过COCONUT-BFS消融，证明并行搜索行为会自动涌现，而非仅因最优路径监督。
  - 多次运行确保结果稳定性。
- **客观性**：结果清晰，连续CoT显著优于离散CoT，且可视化与理论预测一致。实验设置公开，代码已开源，可复现。

### 6. 论文的主要结论与发现
- **主要结论**：连续CoT通过**叠加状态**（superposition）实现并行BFS，使得两层Transformer只需**D步**（D为图直径）即可解决图可达性问题；而离散CoT需O(n²)步，且可能陷入局部解。
- **实证发现**：
  - 两层COCONUT准确率接近**100%**，远高于两层CoT（约75%）和12层CoT*（约83%）。
  - 连续思维向量确实编码了当前可达顶点集，且更强调前沿节点和最优路径节点。
  - 模型自动学会并行搜索，即使训练只提供最优路径，注意力仍会均匀分配给所有前沿节点。
  - 探索优先级（对最优节点的偏向）源自多阶段课程训练，但即使使用随机BFS监督，模型仍能学会并行BFS且准确率不变。

### 7. 优点
- **理论贡献突出**：首次为连续CoT提供严格的理论解释，证明其通过叠加状态实现高效并行搜索，与离散CoT形成严格区分。
- **构造实用**：理论构造使用了实际中普遍采用的正弦/旋转位置编码，无需为特定问题定制，更易推广。
- **实验验证充分**：不仅验证准确率，还深入分析注意力模式和表示空间，直接支撑理论机制；包含消融实验和多次运行，结论可靠。
- **揭示涌现行为**：发现并行搜索能力会自动涌现，即使训练信号仅包含单一最优路径，无需显式多路径监督，具有启发性。
- **开源代码**：便于后续研究复现和扩展。

### 8. 不足与局限
- **问题范围有限**：理论分析和实验仅聚焦于*图可达性问题*，虽然该问题具有代表性，但未推广到更复杂的推理任务（如数学推理、规划等）。文中所提机制是否适用于其他场景仍需验证。
- **理论假设较强**：构造需要嵌入维度与词汇表大小线性相关（d=O(|Voc|)），在实际大模型中使用可能不具经济性；且假定token嵌入是正交的，实际训练中难以保证。
- **训练依赖多阶段课程**：多阶段训练策略需要人工设计课程顺序，可能引入偏见；文中也提到训练数据提供最优路径，导致模型偏向最优节点，这可能在真实无监督场景中不成立。
- **缺乏离散CoT的严格下界**：虽然文中指出最佳已知结果需O(n²)步，但未证明离散CoT所需步数不可能少于Ω(n²)或Ω(D)的某种下界；因此严格分离结论有待补充。
- **实验规模较小**：图平均仅22个节点，推理步仅3-4步，未测试更大规模或更复杂图上的泛化能力。
- **未讨论训练动态理论**：虽然观察到并行搜索行为自动涌现，但未从训练动力学角度解释该过程，仅作为未来工作。

（完）
