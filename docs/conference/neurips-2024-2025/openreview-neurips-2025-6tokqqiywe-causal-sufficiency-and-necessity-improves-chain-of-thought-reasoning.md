---
title: Causal Sufficiency and Necessity Improves Chain-of-Thought Reasoning
title_zh: 因果充分性和必要性提升思维链推理
authors: "Xiangning Yu, Zhuohan Wang, Linyi Yang, Haoxuan Li, Anjie Liu, Xiao Xue, Jun Wang, Mengyue Yang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=6tOKqqiyWE"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 因果充分性与必要性框架用于CoT忠实性
tldr: 因果框架量化CoT步骤的充分性和必要性以提升忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 408, \"height\": 409}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 664, \"height\": 242}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 696, \"height\": 573}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 814, \"height\": 423}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-005.webp\", \"caption\": \"\", \"page\": 4, \"index\": 5, \"width\": 466, \"height\": 664}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 1030, \"height\": 444}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 626, \"height\": 628}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 1303, \"height\": 642}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-009.webp\", \"caption\": \"\", \"page\": 28, \"index\": 9, \"width\": 1020, \"height\": 608}]"
motivation: CoT推理面临步骤充分性和必要性的挑战。
method: 引入因果概率框架，评估推理步骤的充分性和必要性。
result: 能够识别逻辑上充分和必要的步骤，提升推理质量。
conclusion: 因果视角有助于提升CoT推理的忠实性和可解释性。
---

## Abstract
Chain-of-Thought (CoT) prompting plays an indispensable role in endowing large language models (LLMs) with complex reasoning capabilities. However, CoT currently faces two fundamental challenges: (1) Sufficiency, which ensures that the generated intermediate inference steps comprehensively cover and substantiate the final conclusion; and (2) Necessity, which identifies the inference steps that are truly indispensable for the soundness of the resulting answer. We propose a causal framework that characterizes CoT reasoning through the dual lenses of sufficiency and necessity. Incorporating causal Probability of Sufficiency and Necessity allows us not only to determine which steps are logically sufficient or necessary to the prediction outcome, but also to quantify their actual influence on the final reasoning outcome under different intervention scenarios, thereby enabling the automated addition of missing steps and the pruning of redundant ones. Extensive experimental results on various mathematical and commonsense reasoning benchmarks confirm substantial improvements in reasoning efficiency and reduced token usage without sacrificing accuracy. Our work provides a promising direction for improving LLM reasoning performance and cost-effectiveness. The code will be publicly available upon acceptance at: https://anonymous.4open.science/r/causalmath-1CEF.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：思维链（Chain-of-Thought, CoT）在提升大语言模型复杂推理能力中至关重要，但面临两大核心挑战：
  - **充分性（Sufficiency）**：生成的中间推理步骤是否全面支持最终结论。
  - **必要性（Necessity）**：哪些步骤对于答案的正确性是不可或缺的。
- **现存问题**：现有方法多基于相关性（如注意力权重、似然分数）来评估步骤重要性，缺乏因果层面的严格性，容易保留冗余步骤或遗漏关键步骤，导致“过度思考”（overthinking）或推理不完整。
- **论文主张**：引入因果概率框架（概率必要性充分性，PNS），从因果干预角度定量评估并优化CoT步骤，使推理链既充分又必要，从而提升推理效率和准确性。

## 2. 论文提出的方法论
### 核心思想
- 基于Pearl的因果理论，将CoT推理步骤视为因果变量，通过干预和反事实推理量化每个步骤的因果贡献。
- 定义三个关键量：
  - **PS（充分性概率）**：给定推理链S，其能产生正确答案的概率。
  - **PN（必要性概率）**：某一步骤st被替换为错误版本后，答案变为错误的概率。
  - **PNS（必要且充分概率）**：同时满足PS和PN的联合概率。
### 关键技术细节
1. **链级PS估计**：若完整链S得到正确答案则PS=1，否则PS=0。
2. **节点级PN估计**：当PS=1时，对每个步骤st，利用rollout模型生成反事实链（替换st并生成后续步骤），通过蒙特卡洛估计PNS = 1 - (1/k) Σ V(S⁽ⁱ⁾)，其中V为验证模型（评估正确性）。
3. **迭代剪枝算法（Algorithm 1）**：
   - 输入：初始CoT链、正确答案、问题、阈值α。
   - 若链充分，则依次处理每个步骤st：
     - 生成替代步骤st̃并rollout得到k条反事实链。
     - 计算PNS值，若大于α则保留st，否则丢弃。
   - 输出优化后的最小充分必要链。
4. **三种rollout策略**：直接rollout、基于提示的rollout、外部模型rollout。
### 公式与算法流程（文字说明）
- 利用干预分布P(A | do(S), q)来替代观测概率。
- 在单调性和外生性假设下，PNS可识别为P(A=y|do(S)) - P(A=y|do(S′))或1 - P(A=y|do(S′))。
- 优化后的CoT链用于后续的上下文学习（ICL）和监督微调（SFT）。

## 3. 实验设计
### 数据集与场景
- **数学推理**：GSM-8k（小学算术）、MATH-500（中等难度）、AIME（高级竞赛题，至2025年）。
- **常识推理**：CommonsenseQA（多项选择题）。
### Benchmark与对比方法
- **RQ1（因果优化效果）**：比较优化前后的CoT链（token数、步骤数、准确率、平均PNS）。
- **RQ2（下游任务提升）**：
  - **ICL基线**：Standard、Fast-Solve、Reduction、Chain-of-Draft (CoD)、Ours-ICL。
  - **SFT基线**：Original（基模型）、Noncausal（用原始CoT微调）、Causal（用PNS优化后CoT微调）。
  - **额外对比**：SPIRIT、ReAct、Tree-of-Thoughts (ToT)。
### 使用模型
- **RQ1**：Qwen-2.5-72B-Instruct、QwQ-32B-Preview、DeepSeek-V3、DeepSeek-R1。
- **RQ2 (ICL)**：Qwen-2.5-72B-Instruct、Qwen-2.5-7B-Instruct、Llama-3.1-8B-Instruct、DeepSeek-V3。
- **RQ2 (SFT)**：DeepSeek-R1-Distill-Qwen-1.5B、DeepScaleR-1.5B-Preview、Phi-4-mini-reasoning。

## 4. 资源与算力
- 论文在附录D中明确说明：
  - **SFT训练**：使用8 × NVIDIA RTX 3090 GPU，采用ZeRO-3优化器、bf16混合精度、flash_attention_2，每个GPU batch size为1，最大序列长度16384，训练3个epoch。
  - **推理（Inference）**：通过VLLM进行，max-tokens设为16384，但未指定GPU类型和数量。
- 未提供总计算时长或能耗估计。

## 5. 实验数量与充分性
- **实验数量**：非常充分，包括：
  - RQ1在4个数据集上、2个模型族、3种rollout策略下的全面对比（表1）。
  - RQ2在ICL设置中覆盖4个模型、3个数据集、5种基线（表2）。
  - RQ2在SFT设置中覆盖3个微调模型、4个数据集（表3）。
  - 额外对比SPIRIT、ReAct、ToT（附录K，表7）。
  - 人类评估50个CoT样本（附录I）。
  - 消融实验：不同k值、不同验证器（附录J）。
  - 多组PNS对比图（图3、附录G）。
- **公平性与客观性**：
  - 所有实验均使用公开数据集和标准分割。
  - 对比方法均为近期代表性工作，且使用相同推理环境。
  - 在ICL中，Ours-ICL与基线采用相同的few-shot设置（示例数量一致）。
  - 但未报告统计显著性检验（如置信区间或p值）。
- **总体评价**：实验设计严谨，覆盖多种任务难度和模型规模，结果具有较强说服力。

## 6. 论文的主要结论与发现
1. **PNS优化显著降低推理冗余**：token数和步骤数减少50%-80%，同时准确率提升或保持。
2. **ICL中优化CoT提升效果**：Ours-ICL在几乎全部设置中优于所有基线，特别是GSM-8k上准确率接近100%，且token数仅为标准CoT的30%-50%。
3. **SFT中因果优化效果突出**：即使在小型推理模型（1.5B）上，使用PNS筛选的少量数据（1229条）微调，即可在准确率和效率上超越基线。
4. **因果必要性增强**：优化后CoT的每步平均PNS值高于原始CoT，表明保留的步骤更关键。
5. **人类评估验证质量**：84%的优化后CoT被专家评为既充分又必要。

## 7. 优点
- **理论创新**：首次将PNS因果概率引入CoT推理，提供了严谨的数学定义和可识别性证明。
- **模型无关性**：适用于任何能生成CoT的LLM，不依赖模型内部结构。
- **双层次优化**：链级充分性与节点级必要性协同，确保逻辑完整和简洁。
- **实验全面**：覆盖多种任务、模型规模和微调范式，结果一致。
- **成本效益**：一次性的PNS筛选成本，但微调后推理时无需额外因果计算，实际部署效率高。
- **可解释性**：优化的CoT链中每一步都有明确的因果贡献，便于理解模型决策。

## 8. 不足与局限
- **高复杂度任务性能下降**：在AIME等极难任务上，优化后准确率提升有限甚至持平（表1中部分结果），且步骤减少幅度较小。
- **阈值选择敏感**：算法依赖阈值α，论文未系统研究其最优选择或自适应方法。
- **PNS估计成本高**：对每个步骤需进行k次rollout，n较长时间复杂度为O(k·n²)，可能不适用于实时或长链场景。
- **反事实生成质量**：反事实链由rollout模型生成，若生成质量差（如语义不连贯）会引入偏差。论文未充分讨论这一风险。
- **实验局限**：
  - 未测试更大规模模型（如70B以上）或更多领域（如代码生成、科学推理）。
  - 未提供统计显著性检验，部分改进幅度较小（如AIME上从16.7%到26.7%），可能受随机性影响。
  - 人类评估仅50个样本，且仅在优化后CoT上评估，未对基线进行平行评估。
- **应用限制**：ICL中优化CoT对示例选择和提示格式敏感；SFT仅使用1229条数据，泛化到其他模型或任务需重新筛选。

（完）
