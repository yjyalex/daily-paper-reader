---
title: Causal Sufficiency and Necessity Improves Chain-of-Thought Reasoning
title_zh: 因果充分性和必要性改进思维链推理
authors: "Xiangning Yu, Zhuohan Wang, Linyi Yang, Haoxuan Li, Anjie Liu, Xiao Xue, Jun Wang, Mengyue Yang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=6tOKqqiyWE"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 因果充分性和必要性提升思维链忠实性
tldr: 提出因果框架提升思维链推理的充分性和必要性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 408, \"height\": 409}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 664, \"height\": 242}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 696, \"height\": 573}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 814, \"height\": 423}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-005.webp\", \"caption\": \"\", \"page\": 4, \"index\": 5, \"width\": 466, \"height\": 664}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 1030, \"height\": 444}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 626, \"height\": 628}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 1303, \"height\": 642}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-6tokqqiywe/fig-009.webp\", \"caption\": \"\", \"page\": 28, \"index\": 9, \"width\": 1020, \"height\": 608}]"
motivation: 现有思维链推理在充分性和必要性上存在不足，导致推理步骤覆盖不全或包含冗余步骤。
method: 引入因果推断中的充分性和必要性概率来衡量推理步骤对结论的因果贡献。
result: 所提方法能有效识别逻辑上充分或必要的推理步骤，提升推理质量。
conclusion: 因果视角有助于增强思维链推理的可信度和忠实性。
---

## Abstract
Chain-of-Thought (CoT) prompting plays an indispensable role in endowing large language models (LLMs) with complex reasoning capabilities. However, CoT currently faces two fundamental challenges: (1) Sufficiency, which ensures that the generated intermediate inference steps comprehensively cover and substantiate the final conclusion; and (2) Necessity, which identifies the inference steps that are truly indispensable for the soundness of the resulting answer. We propose a causal framework that characterizes CoT reasoning through the dual lenses of sufficiency and necessity. Incorporating causal Probability of Sufficiency and Necessity allows us not only to determine which steps are logically sufficient or necessary to the prediction outcome, but also to quantify their actual influence on the final reasoning outcome under different intervention scenarios, thereby enabling the automated addition of missing steps and the pruning of redundant ones. Extensive experimental results on various mathematical and commonsense reasoning benchmarks confirm substantial improvements in reasoning efficiency and reduced token usage without sacrificing accuracy. Our work provides a promising direction for improving LLM reasoning performance and cost-effectiveness. The code will be publicly available upon acceptance at: https://anonymous.4open.science/r/causalmath-1CEF.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究背景**：Chain-of-Thought (CoT) 能有效提升大语言模型（LLM）的复杂推理能力，但存在两个根本性问题：
  - **充分性（Sufficiency）**：中间推理步骤应完全支持最终结论，防止遗漏关键逻辑。
  - **必要性（Necessity）**：应识别出对答案的正确性真正不可或缺的步骤，避免冗余。
- **现有方法的不足**：当前CoT优化多采用相关性指标（如注意力权重、似然分数）或启发式压缩，无法可靠区分因果上必要的步骤，易导致“过度思考”（overthinking）或推理不完整。
- **本文动机**：引入因果推断中的 **Probability of Necessity and Sufficiency (PNS)** 概念，从因果视角量化每个推理步骤对最终答案的真实贡献，从而自动补充缺失步骤、剪除冗余步骤，在保持准确率的前提下大幅提升推理效率。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将CoT推理视为一种因果过程，为每个推理步骤定义**因果充分性 (PS)** 和**因果必要性 (PN)**，并组合成 **PNS** 指标。
- **形式化定义**：
  - **PS（充分性）**：给定CoT链 S，若插入该链能使错误答案变为正确，则定义为 \( PS(S,q) = P(A^{do(S)}=y | A \neq y, \bar{S}, q) \)。
  - **PN（必要性）**：针对特定步骤 \( s_t \)，若将其替换为错误版本后答案变为错误，则定义为 \( PN(S,s_t,q)=P(A^{do(\bar{s}_t)}\neq y | A=y, S, q) \)。
  - **PNS**：联合概率 \( PNS(S,s_t,q)=P(A^{do(S)}=y, A^{do(S')}\neq y) \)，其中 \( S' \) 为替换步骤后的反事实链。
- **可识别性**：在单调性假设或链充分性（PS=1）条件下，PNS可简化为 \( 1-P(A=y|do(S')) \) 或 \( P(A=y|do(S))-P(A=y|do(S')) \)，便于通过干预实验估计。
- **算法流程（Algorithm 1）**：
  1. 生成初始CoT链 \( S_{init} \)，评估其充分性（PS是否为1）。
  2. 若充分，则对每个步骤 \( s_t \) 进行k次**干预（rollout）**：用替代步骤 \( \tilde{s}_t \) 替换原步骤，并让模型生成后续内容，形成反事实链 \( S^{(i)} \)。
  3. 通过验证模型 \( V \) 评估每个 \( S^{(i)} \) 的答案正确性，用蒙特卡洛估计PNS：\( PNS \approx 1 - \frac{1}{k}\sum V(S^{(i)}) \)。
  4. 保留PNS高于阈值 \( \alpha \) 的节点，剪除冗余步骤，迭代直到所有保留步骤均满足充分且必要。
- **干预策略**：提供三种生成替代步骤的方法：
  - Direct Rollout：直接由基模型生成。
  - Prompt-Based Rollout：用结构化提示引导替换。
  - External Rollout：使用更强模型生成替换。
- **应用**：将优化后的CoT链用于**上下文学习（ICL）** 和**监督微调（SFT）**，使模型内化因果最小推理模式。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：
  - 数学推理：GSM-8k、MATH-500、AIME（含2025年竞赛题）。
  - 常识推理：CommonsenseQA。
- **评估指标**：推理效率（Token数、步骤数）、准确率（最终答案正确率）以及平均PNS值。
- **对比方法**：
  - **RQ1（PNS优化效果）**：比较优化前后的CoT性能（使用QwQ-32B-Preview/Qwen-2.5-72B和DeepSeek-R1/V3）。
  - **RQ2（ICL和SFT）**：
    - ICL基线：Standard、Fast-Solve、Reduction、CoD（Chain-of-Draft）。
    - SFT基线：Original（未微调）、Noncausal（使用原始CoT微调）。
  - 额外对比：SPIRIT、ReAct、Tree-of-Thoughts (ToT)。
- **模型规模**：
  - ICL实验：DeepSeek-V3、Qwen-2.5-72B/7B、Llama-3.1-8B。
  - SFT实验：DeepSeek-R1-Distill-Qwen-1.5B、DeepScaleR-1.5B-Preview、Phi-4-mini-reasoning。

## 4. 资源与算力
- **文中明确提及的算力资源**：
  - SFT实验在 **8×NVIDIA RTX 3090 GPU** 上运行，使用 **ZeRO-3** 优化、**bf16** 混合精度、**flash_attention_2**。
  - 训练3个epoch，每个GPU批次大小1，最大序列长度16384 tokens。
- **其他注意**：
  - PNS估计（rollout）阶段的算力未单独统计，但文中指出这是**一次性离线优化成本**，微调后推理时无需额外rollout。
  - 推理使用 **VLLM**，最大tokens设为16384。

## 5. 实验数量与充分性
- **主要实验组数**：
  - **RQ1**（表1）：含4个数据集 × 2个模型族（Qwen、DeepSeek）× 3种干预策略 = 24组主要对比，同时报告优化前后Token、Steps、Acc。
  - **PNS对比图**（Figure 3/5-8）：展示多个样本点优化前后PNS上升趋势，覆盖AIME和CommonsenseQA。
  - **RQ2-ICL**（表2）：4个数据集的5种对比方法 × 4种模型 = 80组结果。
  - **RQ2-SFT**（表3）：4个数据集 × 3种模型 × 3种设置（Original/Noncausal/Causal）= 36组。
  - **额外对比**（附录表7）：SPIRIT/ReAct/ToT vs Ours-ICL × 3个数据集 × 4个模型。
  - **消融与分析**：不同rollout策略、不同验证模型（Qwen-7B/72B, GPT-4o）、不同rollout次数k（1,3,5,10）的MAE对比（附录表6）。
  - **人类评估**：50个CoT样本由5位数学专家评判，统计充分性与必要性。
- **充分性评估**：
  - 实验覆盖了多种难度（从GSM-8k到AIME）、多种模型（7B至72B）、多种设置（ICL/SFT），且与多个代表性基线公平对比。
  - 所有实验均报告多个指标（Token、Steps、Acc），避免片面结论。
  - 对PNS优化效果进行了量化（表1）以及可视化（PNS散布图），并提供了消融和稳定性分析。

## 6. 主要结论与发现
- **PNS优化显著提升推理效率**：在所有数据集上，Token数和步骤数减少 **50%-90%**，同时准确率保持或提升（如GSM-8k上DeepSeek-V3从97.6%提升至99.9%，AIME上最高提升10个百分点以上）。
- **优化后的步骤因果必要性更强**：PNS值普遍上升（见图3），表明保留下的每一步对正确答案都更关键。
- **ICL中Ours-ICL表现最佳平衡**：与Standard/Fast-Solve/Reduction/CoD相比，在准确率最高的同时显著压缩推理过程，尤其对复杂任务（MATH-500）优势明显。
- **SFT中Causal-CoT优于Noncausal和Original**：微调后模型使用更少步骤（比原始减少40%-85%）即可达到相同或更高准确率，验证了因果优化训练数据的高质量价值。
- **人类评估验证**：84%的优化后CoT被判定为既充分又必要，仅6%存在不充分问题。

## 7. 优点
- **理论扎实**：将因果推断（PNS）与CoT合理结合，给出了严密的形式化定义和可识别性证明（附录A），突破了传统相关性方法的局限。
- **模型无关且通用**：方法不依赖特定LLM架构，适用于任何能生成CoT的模型，可自然嵌入ICL和SFT流程。
- **实验全面且有说服力**：
  - 覆盖多种难度、多种模型和多种使用场景。
  - 提供了详尽消融（干预策略、rollout次数、验证模型强度）。
  - 包含人工评估，增强结论可信度。
- **实用性高**：一次离线优化后，微调模型即可直接生成简洁准确的推理，无需在线干预，降低了部署成本。
- **开源承诺**：将公开发布代码和数据，便于复现与扩展。

## 8. 不足与局限
- **复杂任务上效果有限**：对AIME等高难度竞赛题，优化后Token减少幅度较小，且准确率提升不稳定（如Qwen-72B仅从16.7%提升至26.7%），可能因复杂推理步骤间高度耦合。
- **计算成本**：PNS估计需多次rollout（复杂度 \( O(k n^2) \)），对长链推理开销较大；虽然是一次性成本，但用户需权衡优化投入。
- **阈值α敏感**：剪枝阈值需手工设定，不同任务可能存在最优值差异，文中未提供自适应方案。
- **验证模型依赖**：PNS估计依赖验证模型V的质量，弱模型可能导致估计偏差；附录表6显示使用弱验证模型（如Qwen-7B）MAE显著增大。
- **数据集多样性有限**：实验主要集中于数学和常识QA，尚未在更广泛的任务（如代码、科学推理、逻辑谜题）上验证。
- **人工评估规模较小**：仅50个样本，且由专家评判，可能未能全面反映真实退化风险（如遗漏细微必要步骤）。

（完）
