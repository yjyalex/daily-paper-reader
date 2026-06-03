---
title: "Deliberation on Priors: Trustworthy Reasoning of Large Language Models on Knowledge Graphs"
title_zh: 先验审议：基于知识图谱的大型语言模型可信推理
authors: "Jie Ma, Ning Qu, Zhitao Gao, Xing Rui, Jun Liu, Hongbin Pei, Jiang Xie, Lingyun Song, Pinghui Wang, Jing Tao, su zhou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=bulTwq5kNK"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 利用知识图谱先验知识增强LLM推理的忠实度
tldr: 基于先验知识的推理利用知识图谱先验提升LLM推理的忠实度和可靠性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 416, \"height\": 358}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 418, \"height\": 412}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 2406, \"height\": 575}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 2406, \"height\": 575}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 802, \"height\": 343}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-012.webp\", \"caption\": \"\", \"page\": 4, \"index\": 12, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-013.webp\", \"caption\": \"\", \"page\": 4, \"index\": 13, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-017.webp\", \"caption\": \"\", \"page\": 4, \"index\": 17, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-018.webp\", \"caption\": \"\", \"page\": 4, \"index\": 18, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-019.webp\", \"caption\": \"\", \"page\": 4, \"index\": 19, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-020.webp\", \"caption\": \"\", \"page\": 4, \"index\": 20, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-021.webp\", \"caption\": \"\", \"page\": 20, \"index\": 21, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-022.webp\", \"caption\": \"\", \"page\": 20, \"index\": 22, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-023.webp\", \"caption\": \"\", \"page\": 20, \"index\": 23, \"width\": 512, \"height\": 512}]"
motivation: 现有方法未能充分利用知识图谱的结构信息和约束，影响推理忠实度。
method: 提出DP框架，采用渐进知识蒸馏策略整合知识图谱先验到LLM推理中。
result: 在多个任务上提升了推理的忠实度和回答可靠性。
conclusion: 利用知识图谱先验能够有效增强LLM推理的可信度。
---

## Abstract
Knowledge graph-based retrieval-augmented generation seeks to mitigate hallucinations in Large Language Models (LLMs) caused by insufficient or outdated knowledge. However, existing methods often fail to fully exploit the prior knowledge embedded in knowledge graphs (KGs), particularly their structural information and explicit or implicit constraints. The former can enhance the faithfulness of LLMs' reasoning, while the latter can improve the reliability of response generations. Motivated by these, we propose a trustworthy reasoning framework, termed Deliberation over Priors (\texttt{DP}), which sufficiently utilizes the priors contained in KGs. Specifically, \texttt{DP} adopts a progressive knowledge distillation strategy that integrates structural priors into LLMs through a combination of supervised fine-tuning and Kahneman-Tversky Optimization, thereby improving the faithfulness of relation path generation. Furthermore, our framework employs a reasoning-introspection strategy, which guides LLMs to perform refined reasoning verification based on extracted constraint priors, ensuring the reliability of response generation. Extensive experiments on three benchmark datasets demonstrate that \texttt{DP} achieves new state-of-the-art performance, especially a H@1 improvement of 13% on the ComplexWebQuestions dataset, and generates highly trustworthy responses. We also conduct various analyses to verify its flexibility and practicality. Code is available at [https://github.com/mira-ai-lab/Deliberation-on-Priors](https://github.com/mira-ai-lab/Deliberation-on-Priors).

---

## 论文详细总结（自动生成）

### 论文完整总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大语言模型（LLMs）在知识密集型任务中容易产生幻觉（hallucination），原因是训练知识不足或过时。基于知识图谱（KG）的检索增强生成（RAG）虽然能缓解该问题，但现有方法未能充分利用KG中蕴含的先验知识，尤其是**结构信息**（如关系路径）和**显式/隐式约束**（如类型、多实体、时间等）。
- **研究动机**：利用KG的结构先验可提升LLM推理的忠实度（faithfulness），利用约束先验可提升生成响应的可靠性（reliability）。因此，作者提出名为 **Deliberation on Priors (DP)** 的可信推理框架，通过充分发掘KG中的先验知识来增强LLM的推理能力。

#### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将LLM的推理过程与KG的先验知识深度融合，分为**离线阶段**和**在线阶段**。
  - **离线阶段**：通过渐进知识蒸馏，将KG结构先验（关系路径）注入LLM。
  - **在线阶段**：通过推理-内省机制，利用约束先验对生成的推理路径进行验证和回溯，确保响应可靠性。

- **关键技术细节**：
  1. **Distillation（蒸馏）**：
     - 收集弱监督信号：对每个问题 `q`，使用Dijkstra算法在KG中找出从主题实体到答案实体的**所有最短关系路径**，形成一对多的“问题→路径”映射 `P_w(q)`。
     - 两阶段训练：
       - **SFT**：对LLM进行监督微调，最大化条件对数似然 `L_SFT(θ_s) = Σ log P_θ(r*_t | r*_<t, q, e_s)`，使其学会生成忠实的关系路径。
       - **KTO（Kahneman-Tversky Optimization）**：基于正负样本（通过路径截断、实体-路径交换、关系删除构造）进行偏好优化，损失函数为 `L_KTO(π_θ, π_ref) = E[λ_y - v(x, y)]`，`v(x,y)` 采用前景理论中的价值函数，能处理正负样本不平衡问题。
  2. **Planning & Instantiation（规划与实例化）**：
     - 用训练好的路径生成器为问题生成多个候选关系路径。
     - 根据语义相关性选择一条路径，然后从KG中检索实体，将关系路径实例化为包含具体实体的推理路径。
  3. **Introspection（内省）**：
     - 预定义5类约束（类型、多实体、显式时间、隐式时间、次序），对问题提取约束集合 `C(q)`。
     - 验证当前推理路径 `P` 是否满足约束：`J(q, e_s, P) = 1` 表示满足，否则给出反馈，触发回溯（重新选择路径、实例化、验证）。
     - 回溯循环直到找到满足约束的路径或候选集耗尽。

- **公式与流程文字说明**：
  - 弱监督路径收集：`P_w(q) = ShortestPath_Dijkstra(G_k(e_s), e_s, e_t)`
  - SFT目标：最大化路径生成的对数似然。
  - KTO目标：最小化损失函数，使LLM倾向正路径。
  - 4个模块：Distillation（训练）→ Planning（生成候选路径）→ Instantiation（检索实例化）→ Introspection（约束验证与回溯）。

#### 3. 实验设计：数据集、基准、对比方法
- **数据集**：三个多跳KGQA基准：
  - **WebQSP**：基于Freebase，2跳推理，训练2826/测试1628（作者从测试集中均匀采样500条）。
  - **ComplexWebQuestions (CWQ)**：复杂组合问题，最多4跳，训练27639/测试3531（采样500条）。
  - **MetaQA**：电影领域，1/2/3跳，总训练329282/测试30903（按每种跳数各采样200条，共600条）。
- **评估指标**：Hit（H）、Hits@1（H@1）、F1分数。
- **对比方法**：分为三类：
  - **监督学习（SL）**：EmbedKGQA、TransferNet、UniKGQA、RoG、AMAR、GNN-RAG。
  - **上下文学习（ICL）**：ToG（Think-on-Graph）、PoG（Plan-on-Graph）、Readi、DoG（Debate on Graph）。
  - **混合学习（HL）**：Interactive-KBQA、LightPROF。
- **本文方法变体**：DP搭配不同LLM后端（LLaMA3.1-8B、GPT-3.5、GPT-4、GPT-4o、GPT-4.1），并与上述baseline在相同条件下比较。

#### 4. 资源与算力
- 文中在附录B.1明确说明：使用**两个NVIDIA A800-80GB GPU**，精度为bfloat16。
- 训练细节：SFT和KTO均使用LoRA（秩16，alpha=32，dropout=0.1），SFT学习率5e-5，KTO学习率1e-5，batch size=4，SFT训练2个epoch，KTO训练1个epoch。
- 推理时的LLM调用和token消耗在Table 6中给出，但未说明端到端训练总时长。

#### 5. 实验数量与充分性
- **实验组数**：
  - 主实验（Table 2）：在3个数据集上对比多种baseline，DP报告了3次独立运行的平均值和标准差。
  - 灵活性验证（Table 2底部）：使用5种不同LLM后端。
  - 消融实验（Table 3）：在WebQSP和CWQ上移除KTO、移除各扰动类型、移除内省模块、移除预定义约束、移除反馈，共12个设置。
  - 路径生成与约束提取性能（Table 4）：在WebQSP和CWQ上报告路径生成和约束提取的H和F1。
  - 回溯分析（Table 5）：报告不同模型平均回溯步数。
  - 交互效率对比（Table 6）：与ToG、PoG、DoG比较LLM调用次数和token消耗。
  - 额外分析：附录B.3-B.6还包括不同LLM的详细性能表、样本数影响（图5）、不同LLM的token/calls（图6）以及案例研究。
- **充分性评价**：实验覆盖了多个数据集、多种指标、多组消融和对比，且报告了方差，结果客观。消融设计系统性地验证了各组件贡献。不足之处在于未在更大规模（如全量测试集）上验证（部分数据集仅采样500条），这可能引入采样偏差，但已被许多同类工作采用（如RoG、ToG），公平性可接受。

#### 6. 论文的主要结论与发现
- **DP在所有三个数据集上取得了新的SOTA**，尤其在CWQ上的H@1提升达**13%**（相对），且F1也显著提高。
- **渐进知识蒸馏**（SFT+KTO）能有效提升LLM对KG结构模式的感知，路径生成F1相对提升29.3%（WebQSP）和43.0%（CWQ）。
- **推理-内省策略**是确保响应可靠性的最关键组件：移除内省后性能下降最大。预定义约束比LLM自动总结约束效果更好（H@1下降2-3个百分点）。
- DP在**LLM调用次数和token消耗**上显著低于ICL型方法（如ToG、DoG），实用性更强。
- 不同LLM后端（从8B到GPT-4.1）均能从DP获益，表明框架灵活通用。

#### 7. 优点
- **方法论创新**：首次将KG中的结构先验和约束先验分别用于提升忠实度和可靠性，并设计了两阶段蒸馏和回溯验证机制。
- **训练数据高效**：利用弱监督（最短路径）自动构建“问题→路径”对，无需人工标注，且采用一对多映射（而非RoG的一对一），提高了路径覆盖度。
- **偏好优化设计**：使用KTO处理正负样本不平衡（正样本仅占1/4），比传统DPO更鲁棒。
- **实验全面且透明**：报告了多次运行的标准差，消融实验设计覆盖所有关键模块，对比了多类baseline，并在效率和交互成本上做了详实分析。
- **开源代码**：提供完整代码，可复现。

#### 8. 不足与局限
- **约束提取仍依赖人工预定义**：当迁移到垂直领域时，需要手动设计约束类型和模板，限制了可扩展性。作者在论文末尾承认该局限，并计划未来研究自动提取约束的方法。
- **路径生成存在误差**：案例研究（图9）显示生成的路径有时与真实路径存在微小差异（如错误使用了`currency_used`而非`currency_formerly_used`），说明结构先验的捕获仍有提升空间。
- **推理阶段偶尔过分依赖LLM内部知识**：即使路径正确，LLM可能遗漏正确答案（如图10案例），表明“推理”子模块还需增强。
- **采样偏差**：在WebQSP和CWQ上仅采样500个测试样本，虽然被许多先前工作采用，但可能存在不具代表性的风险。全量测试集结果未知。
- **算力需求**：虽然DP在推理时调用次数少，但离线训练仍需要双卡A800-80GB GPU资源，对硬件有一定要求（相对于纯ICL方法）。
- **未与最新SL方法（如采用更大LLM的GNN-RAG变体）全面比较**：部分baseline使用了不同LLM（如LLaMA2-7B/13B），直接比较时可能存在模型规模不对等。

（完）
