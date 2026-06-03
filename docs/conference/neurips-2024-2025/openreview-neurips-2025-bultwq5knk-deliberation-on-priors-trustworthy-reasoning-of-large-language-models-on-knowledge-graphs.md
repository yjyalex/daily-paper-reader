---
title: "Deliberation on Priors: Trustworthy Reasoning of Large Language Models on Knowledge Graphs"
title_zh: 基于先验知识的深思：知识图谱上大语言模型的可信推理
authors: "Jie Ma, Ning Qu, Zhitao Gao, Xing Rui, Jun Liu, Hongbin Pei, Jiang Xie, Lingyun Song, Pinghui Wang, Jing Tao, su zhou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=bulTwq5kNK"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 利用知识图谱先验增强LLM推理的忠实性
tldr: 通过蒸馏知识图谱先验改进LLM推理的忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 416, \"height\": 358}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 418, \"height\": 412}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 2406, \"height\": 575}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 2406, \"height\": 575}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 802, \"height\": 343}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-012.webp\", \"caption\": \"\", \"page\": 4, \"index\": 12, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-013.webp\", \"caption\": \"\", \"page\": 4, \"index\": 13, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-017.webp\", \"caption\": \"\", \"page\": 4, \"index\": 17, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-018.webp\", \"caption\": \"\", \"page\": 4, \"index\": 18, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-019.webp\", \"caption\": \"\", \"page\": 4, \"index\": 19, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-020.webp\", \"caption\": \"\", \"page\": 4, \"index\": 20, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-021.webp\", \"caption\": \"\", \"page\": 20, \"index\": 21, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-022.webp\", \"caption\": \"\", \"page\": 20, \"index\": 22, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-bultwq5knk/fig-023.webp\", \"caption\": \"\", \"page\": 20, \"index\": 23, \"width\": 512, \"height\": 512}]"
motivation: 现有方法未能充分利用知识图谱中的先验信息，导致LLM推理不可信。
method: 提出渐进式知识蒸馏策略，从知识图谱中提取结构化先验约束LLM推理。
result: 在多个数据集上提升了推理的忠实性和可靠性。
conclusion: 利用知识图谱先验能有效增强LLM推理的忠实性。
---

## Abstract
Knowledge graph-based retrieval-augmented generation seeks to mitigate hallucinations in Large Language Models (LLMs) caused by insufficient or outdated knowledge. However, existing methods often fail to fully exploit the prior knowledge embedded in knowledge graphs (KGs), particularly their structural information and explicit or implicit constraints. The former can enhance the faithfulness of LLMs' reasoning, while the latter can improve the reliability of response generations. Motivated by these, we propose a trustworthy reasoning framework, termed Deliberation over Priors (\texttt{DP}), which sufficiently utilizes the priors contained in KGs. Specifically, \texttt{DP} adopts a progressive knowledge distillation strategy that integrates structural priors into LLMs through a combination of supervised fine-tuning and Kahneman-Tversky Optimization, thereby improving the faithfulness of relation path generation. Furthermore, our framework employs a reasoning-introspection strategy, which guides LLMs to perform refined reasoning verification based on extracted constraint priors, ensuring the reliability of response generation. Extensive experiments on three benchmark datasets demonstrate that \texttt{DP} achieves new state-of-the-art performance, especially a H@1 improvement of 13% on the ComplexWebQuestions dataset, and generates highly trustworthy responses. We also conduct various analyses to verify its flexibility and practicality. Code is available at [https://github.com/mira-ai-lab/Deliberation-on-Priors](https://github.com/mira-ai-lab/Deliberation-on-Priors).

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大型语言模型（LLM）在面对知识不足或过时信息时容易产生“幻觉”，生成看似合理但实际错误或过时的回答。尤其在法律、医疗等高风险领域，这严重损害了LLM的可靠性。
- **现有不足**：知识图谱增强的检索-生成方法（KG-RAG）虽能注入外部知识，但现有方法未能充分利用知识图谱（KG）中蕴含的**结构信息**（如连接主题实体与答案的关系路径）以及**显式/隐式约束**（如类型约束、多实体约束、时间约束等）。前者可提升推理的忠实性，后者可增强响应生成的可靠性。
- **动机**：提出一种可信推理框架 **DP（Deliberation on Priors）**，充分挖掘KG中的先验知识，使LLM推理更忠实、可靠。

## 2. 方法论
### 核心思想
- DP 由四个模块组成：**蒸馏（Distillation）**、**规划（Planning）**、**实例化（Instantiation）**、**反思（Introspection）**，分离线（训练）和在线（推理）两阶段工作。
- 离线阶段：通过渐进式知识蒸馏策略将KG的结构先验传递给LLM，包括监督微调（SFT）和 Kahneman-Tversky 优化（KTO）。
- 在线阶段：利用训练后的LLM生成候选关系路径，经选择、实例化后，通过反思模块基于预定义约束进行验证和回溯。

### 关键技术细节
- **弱监督采集**：对训练集中的每个问题，从主题实体出发，通过 Dijkstra 算法在 k-hop 子图中寻找连接到答案实体的最短关系路径，形成一对多的“问题→关系路径”映射。
- **SFT**：最大化条件对数似然，让LLM学习根据问题生成忠实的关系路径序列。
- **KTO 偏好优化**：自动构造偏好数据（利用路径截断、实体-路径交换、关系删除三种扰动生成负例，正负样本比例1:3），使用 KTO 损失函数优化，该函数基于人类决策的效用模型，能有效处理类不平衡。
- **分析与反思**：
  - 约束提取：LLM在少样本提示下从问题中提取5种预定义约束（类型、多实体、显式时间、隐式时间、序数）。
  - 路径选择：基于语义相关性选择候选路径。
  - 约束验证：判断实例化后的推理路径是否满足约束，若不满足则反馈信息触发回溯（重新选择路径），直到满足或候选集为空。

### 公式/算法流程（文字说明）
1. 对每个问题，从训练标签中提取最短路径集合作为弱监督信号。
2. 用SFT微调LLM（生成关系路径）。
3. 构造偏好数据，用KTO进一步优化LLM。
4. 在线推理时，LLM生成多条候选关系路径。
5. 选择一条路径（基于语义相似度），并从KG中检索实体进行实例化。
6. 提取问题中的约束，验证实例化路径是否满足；若满足则生成最终回答；若不满足则反馈并回溯到路径选择步骤。

## 3. 实验设计
### 数据集与场景
- **WebQSP**：基于Freebase的2跳问答，约2,826训练、1,628测试（采样500）。
- **ComplexWebQuestions (CWQ)**：复杂组合问题，最多4跳，27,639训练、3,531测试（采样500）。
- **MetaQA**：电影领域，1/2/3跳问答，329,282训练、30,903测试（各跳各采样200，共600）。

### Benchmark 与评价指标
- 指标：**Hit（H）**（任意正确答案）、**Hits@1（H@1）**（首答正确）、**F1**（兼顾多正确答案）。
- 对比方法分为三类：
  - 监督学习（SL）：EmbedKGQA、TransferNet、UniKGQA、RoG、AMAR、GNN-RAG。
  - 上下文学习（ICL）：ToG、PoG、Readi、DoG。
  - 混合学习（HL）：Interactive-KBQA、LightPROF。

## 4. 资源与算力
- 训练在 **2块 NVIDIA A800-80GB GPU** 上进行，使用 bfloat16 精度。
- 采用 LoRA（rank=16，alpha=32，dropout=0.1）微调 LLaMA3.1-8B-Instruct。
- 训练轮次：SFT 2 epoch，KTO 1 epoch；初始学习率分别为 5e-5 和 1e-5，batch size 为 4。
- **论文未明确给出训练总时长**，仅提及使用两路A800。在线推理部分调用LLM的次数和token消耗在表6中给出（非常少，如CWQ平均2.9次调用）。

## 5. 实验数量与充分性
- 共进行了**多组实验**：
  - 主实验：表2展示了DP在三个数据集上使用多种LLM（LLaMA3.1-8B、GPT-3.5、GPT-4.0、GPT-4o、GPT-4.1）与18种基线方法的对比，结果取三次运行均值±标准差。
  - 消融实验（表3）：评估了去除KTO、三种扰动策略、反思模块、预定义约束、反馈机制的影响。
  - 路径生成与约束提取效果（表4）：对比一对多 vs 一对一映射。
  - 错误分布分析（图4）：归纳五类错误。
  - 回溯步骤统计（表5）：不同模型下回溯平均次数。
  - 效率分析（表6）：与ToG、PoG、DoG对比LLM调用次数和token消耗。
  - 样例分析（附录B.6）：成功与失败案例。
- **充分性判断**：实验覆盖多个数据集、多种LLM、多种扰动、多种消融场景，结果统计有方差，对比方法覆盖主流，实验设计客观公平。唯一不足是未在更大参数规模的LLM（如70B）或更多领域上验证。

## 6. 主要结论与发现
- DP 在三个数据集上均达到新 SOTA，尤其在 CWQ 上 H@1 相比最佳基线提升13%。
- 渐进式蒸馏（SFT+KTO）有效提升了关系路径生成的忠实性（路径F1相对提升29.3%～43.0%）。
- **反思（Introspection）模块最关键**：其移除导致性能下降最大（表3）。
- 预定义约束较LLM自动总结约束效果更优（H@1下降0.8%～2.8%）。
- 回溯机制能纠正错误路径，且更强的LLM（GPT-4.1）更善于触发回溯，贡献更高质量。
- DP 在交互效率上显著优于其他 ICL 方法（例如CWQ仅需2.9次LLM调用，ToG需22.6次）。

## 7. 优点
- **方法创新**：首次系统性地利用KG的结构先验和约束先验进行推理，蒸馏+偏好优化+反思回溯设计合理。
- **实验全面**：覆盖多种LLM、多数据集、多指标、多消融，具有良好可重复性（开源代码）。
- **效果显著**：H@1和F1大幅领先，尤其复杂问答。
- **实用性强**：在线推理仅需少量LLM交互，Token消耗低，便于部署。
- **灵活性好**：可适配不同LLM（开源与商业），且无需大量手工工程。

## 8. 不足与局限
- **依赖人工定义约束**：约束类型和提取模板需要人工预定义，扩展至垂直领域时需要额外人力。论文中消融实验（w/o CPD）也显示自动总结约束效果下降。
- **路径生成仍有误差**：错误分布（图4）显示路径生成错误占比高（CWQ约36.9%），生成的路径虽逼近真实但仍可能不精确，需更鲁棒的结构学习。
- **计算资源需求**：虽然在线推理高效，但离线蒸馏阶段仍需要2块A800训练，对于无资源团队可能门槛较高。
- **实验规模有限**：仅在 Freebase 和 Wiki-Movie 两个KG上验证，未涉及更大或更异构的KG（如Wikidata），且LLM规模最大为8B（LLaMA），未探索70B以上模型的兼容性。
- **潜在偏差风险**：弱监督路径来源于最短路径，可能遗漏合理但非最短的路径；偏好数据构造依赖人工扰动规则，可能存在偏差。

（完）
