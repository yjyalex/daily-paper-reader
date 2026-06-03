---
title: Can Knowledge-Graph-based Retrieval Augmented Generation Really Retrieve What You Need?
title_zh: 基于知识图谱的检索增强生成真的能检索到你需要的吗？
authors: "Junchi Yu, Yujie Liu, Jindong Gu, Philip Torr, Dongzhan Zhou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=po0eyoYFUa"
tags: ["query:token"]
score: 7.0
evidence: 过程奖励模型用于检索对齐
tldr: 使用过程奖励模型对齐知识图谱检索与查询需求，提升准确性和多样性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-001.webp\", \"caption\": \"\", \"page\": 8, \"index\": 1, \"width\": 800, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-002.webp\", \"caption\": \"\", \"page\": 8, \"index\": 2, \"width\": 800, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-003.webp\", \"caption\": \"\", \"page\": 8, \"index\": 3, \"width\": 800, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-004.webp\", \"caption\": \"\", \"page\": 8, \"index\": 4, \"width\": 800, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-005.webp\", \"caption\": \"\", \"page\": 8, \"index\": 5, \"width\": 800, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 800, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-008.webp\", \"caption\": \"\", \"page\": 25, \"index\": 8, \"width\": 1528, \"height\": 803}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-009.webp\", \"caption\": \"\", \"page\": 28, \"index\": 9, \"width\": 818, \"height\": 629}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-010.webp\", \"caption\": \"\", \"page\": 28, \"index\": 10, \"width\": 862, \"height\": 635}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-011.webp\", \"caption\": \"\", \"page\": 28, \"index\": 11, \"width\": 820, \"height\": 633}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-012.webp\", \"caption\": \"\", \"page\": 28, \"index\": 12, \"width\": 831, \"height\": 633}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-013.webp\", \"caption\": \"\", \"page\": 28, \"index\": 13, \"width\": 822, \"height\": 620}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-014.webp\", \"caption\": \"\", \"page\": 28, \"index\": 14, \"width\": 820, \"height\": 635}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-015.webp\", \"caption\": \"\", \"page\": 28, \"index\": 15, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-po0eyoyfua/fig-016.webp\", \"caption\": \"\", \"page\": 29, \"index\": 16, \"width\": 600, \"height\": 400}]"
motivation: 现有KG-based RAG难以检索到准确且多样化的信息。
method: 提出GraphFlow框架，训练过程奖励模型来指导检索过程。
result: 在复杂查询上显著提升了检索质量和下游生成性能。
conclusion: 过程奖励模型能有效改善知识图谱检索的准确性。
---

## Abstract
Retrieval-Augmented Generation (RAG) based on knowledge graphs (KGs) enhances large language models (LLMs) by providing structured and interpretable external knowledge.
However, existing KG-based RAG methods struggle to retrieve accurate and diverse information from text-rich KGs for complex real-world queries.
Process Reward Models (PRMs) offer a way to align the retrieval process of KG-based RAG with query-specific knowledge requirements, 
but they heavily rely on process-level supervision signals that are expensive and hard to obtain on KGs.
To address this challenge, we propose GraphFlow, a framework that efficiently retrieves accurate and diverse knowledge required for real-world queries from text-rich KGs.
GraphFlow employs a transition-based flow matching objective to jointly optimize a retrieval policy and a flow estimator.
The flow estimator factorizes the reward of the retrieval outcome into the intermediate retrieval states.
Such reward factorization guides the retrieval policy to retrieve candidates from KGs in proportion to their reward.
This allows GraphFlow to explore high-quality regions of KGs that yield diverse and relevant results. 
We evaluate GraphFlow on the STaRK benchmark, which includes real-world queries from multiple domains over text-rich KGs. 
GraphFlow outperforms strong KG-RAG baselines, including GPT-4o, by 10\% on average in hit rate and recall. 
It also shows strong generalization to unseen KGs, demonstrating its effectiveness and robustness.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：现有基于知识图谱（KG）的检索增强生成（RAG）方法在处理复杂查询（如跨领域、多目标检索）时，难以同时保证检索的**准确性**和**多样性**。简单关系查询（如“Alice的父亲是谁”）可以通过单一跳转轻松解决，但复杂查询（如“列出A大学与B主题相关的论文”）需要融合结构化关系和文本内容，现有方法（包括基于检索的和基于智能体的）效果不佳。
- **整体含义**：论文旨在解决KG-based RAG在复杂查询下**检索质量不足**的问题，提出一种无需过程级奖励监督即可实现**对齐查询需求**的框架，提升检索结果的准确性和多样性，从而增强LLM在实际场景中的可靠性。

### 2. 方法论：核心思想、关键技术细节与算法流程

- **核心思想**：将KG上的检索过程建模为多步决策过程，采用**生成流网络（GFlowNet）** 的思想，学习一个检索策略，使得检索轨迹的抽样概率与其最终奖励成正比。这样，高奖励的轨迹（即检索到正确文档的路径）被更频繁地采样，从而自然实现准确且多样的检索。
- **关键技术细节**：
  - **问题形式化**：定义状态 \( s_t = (Q, \{D_j\}_{j=0}^t) \)（查询和已收集文档），动作 \( a_t \) 为从当前节点移动到邻居节点，过渡到新状态，最终奖励 \( R(\tau) \) 指示轨迹是否检索到目标文档。
  - **流估计与信用分配**：使用**详细平衡（Detailed Balance）** 条件：\( F(s_t) \cdot P(s_{t+1}|s_t) = F(s_{t+1}) \cdot P_B(s_t|s_{t+1}) \)，其中 \( F(s) \) 是状态流值，\( P \) 是前向策略，\( P_B=1 \) 设为固定向后策略（由于检索不可回溯）。通过优化该条件，将终端奖励分解到中间状态，无需过程级奖励。
  - **局部探索策略**：在每个非终止状态，除真实下一状态外，额外采样 k 个探索动作，生成多个候选下一状态，用于优化详细平衡损失。该策略避免访问低回报区域，提升训练效率。
  - **终止条件**：引入自环动作，允许策略在认为当前文档足够支持查询时停止检索，无需固定步数。
  - **LLM实现**：使用LLaMA3-8B-Instruct作为骨干，通过LoRA微调，并添加策略头和流头（均为MLP）。状态和状态-动作对通过提示模板编码，最终token嵌入用于预测。
- **算法流程（文字说明）**：
  1. 从初始节点开始，沿真实检索轨迹收集状态转换。
  2. 对于每个非终止状态，通过局部探索生成 k+1 个候选下一状态。
  3. 计算详细平衡损失：\( \mathcal{L}_{DBLE}(s_t) = \sum_{i} [\log F(s_t) - \log F(s'_{t+1,i}) + \log P(s_{t+1}=s'_{t+1,i}|s_t)]^2 \)。
  4. 设置边界条件 \( F(s_0)=F(s_T)=1 \)。
  5. 联合优化流头和策略头参数，以及LoRA适配器。

### 3. 实验设计

- **数据集/场景**：使用 **STaRK benchmark**，包含三个领域的文本丰富KG：
  - **STaRK-AMAZON**：电商KG，产品信息和属性关系。
  - **STaRK-MAG**：学术图谱（基于OGB和Microsoft Academic Graph），作者、机构、论文信息。
  - **STaRK-PRIME**：生物医学KG，药物、疾病、基因、通路等。
- **对比方法**：
  - 基于检索的方法：DenseRetriever、G-Retriever、SubgraphRAG。
  - 基于智能体的方法：ToG（以LLaMA3-8B和GPT-4o为骨干）、SFT（监督微调）、PRM（过程奖励模型）。
  - 所有方法均检索20次，使用Hit@1、Hit@5、MRR（准确性），Recall@20、De-duplicate Recall@20（多样性）作为指标。还额外使用Seper分数（∆Seper）评估检索质量。

### 4. 资源与算力

- 论文明确说明：所有实验在**8/16张NVIDIA A800-SXM4-80GB GPU**和**56核Intel(R) Xeon(R) Platinum 8336C CPU**上运行。训练GraphFlow约1个epoch，具体每轮训练时间未详细给出，但提到使用Lora微调，整体计算开销在可接受范围内。

### 5. 实验数量与充分性

- **实验组数**：包含**三个数据集**上的主实验（准确性表1、多样性表2、检索质量表3），以及**跨领域泛化实验**（图3）、**难度分级实验**（图4）、**训练动态图**（图5）、**消融/分析实验**（附录中更多结果如Hit@5泛化、不同难度详细图7-9）。主实验均对比了6种以上基线方法。
- **充分性与公平性**：实验覆盖了不同领域、不同难度（目标数量分级），并且GraphFlow在所有指标上显著优于包括GPT-4o在内的强基线。对比中，SFT和PRM也使用同样训练数据、同骨干模型（LLaMA3-8B），保证公平。但论文未做统计显著性检验（如误差条），仅报告单次结果，可能受随机性影响。此外，一些更先进基线（如QAGNN、RoG、ToG-2.0）因代码或不兼容而未能纳入对比，可能削弱实验完备性。

### 6. 主要结论与发现

1. **GraphFlow在准确性和多样性上均显著优于现有方法**：在STaRK三个数据集上，Hit@1和Recall@20平均提升约10%，尤其是PRIME数据集上D-R@20达到79.71%，远超ToG+GPT-4o（54.35%）。
2. **无需过程级奖励即可实现高质量检索**：仅使用轨迹的最终奖励（是否检索到目标文档），通过流估计实现隐式信用分配，效果超越需要过程监督的PRM。
3. **强大的跨领域泛化能力**：在未见过的KG（如从AMAZON和MAG训练后迁移到PRIME或反之）上，GraphFlow仍保持高Hit率，优于SFT和PRM。
4. **在复杂/困难查询上表现尤为突出**：当检索目标数增至15个以上时，GraphFlow的D-R@20远高于其他方法，展示了处理多样化、多目标查询的能力。

### 7. 优点

- **方法创新性**：将GFlowNet引入KG-based RAG检索过程，解决过程监督缺失问题，实现自适应奖励分解。
- **局部探索策略**：提升了训练效率，避免无效状态访问。
- **灵活终止条件**：自环动作使模型能根据上下文动态停止检索，更贴近实际需求。
- **实验全面且基线强**：不仅对比了传统检索/智能体方法，还对比了GPT-4o作为骨干的ToG，结果具有说服力。
- **开源代码**：论文提供了GitHub代码链接，便于复现和扩展。

### 8. 不足与局限

- **未报告统计误差**：主实验未列出多次运行的标准差或置信区间，结果稳定性未知。
- **基线覆盖不完整**：部分最新方法（如RoG、ToG-2.0、HybridRAG）因代码未公开或不可复现而未被纳入，可能影响结论的全面性。
- **计算资源需求较高**：依赖8-16张A800 GPU和Lora微调LLaMA3-8B，对硬件资源有一定要求，轻量级应用受限。
- **仅评估了检索阶段**：未与下游生成性能联合评估（如端到端QA准确率），虽然Seper分数提供了部分洞察，但实际RAG最终质量仍需验证。
- **领域局限性**：仅在三个特定领域（电商、学术、生物医学）上测试，对其他类型KG（如常识KG、时序KG）的适用性未知。

（完）
