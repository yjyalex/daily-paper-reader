---
title: "SIGMA: Refining Large Language Model Reasoning via Sibling-Guided Monte Carlo Augmentation"
title_zh: SIGMA：通过兄弟引导的蒙特卡洛增强优化大语言模型推理
authors: "Yanwei Ren, Haotian Zhang, Fuxiang Wu, Jiayan Qiu, Jiaxing Huang, Baosheng Yu, Liu Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=tfbu0ITAez"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 使用蒙特卡洛树搜索生成高质量思维链数据，增强推理
tldr: 提出SIGMA通过MCTS中兄弟节点增强思维链数据，改进大模型推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 7292, \"height\": 4008}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 10630, \"height\": 4920}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 14140, \"height\": 6200}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 8022, \"height\": 4440}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-005.webp\", \"caption\": \"\", \"page\": 16, \"index\": 5, \"width\": 5127, \"height\": 3787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-006.webp\", \"caption\": \"\", \"page\": 16, \"index\": 6, \"width\": 5221, \"height\": 3787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-007.webp\", \"caption\": \"\", \"page\": 16, \"index\": 7, \"width\": 5221, \"height\": 3787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-008.webp\", \"caption\": \"\", \"page\": 16, \"index\": 8, \"width\": 5221, \"height\": 3791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-009.webp\", \"caption\": \"\", \"page\": 16, \"index\": 9, \"width\": 5221, \"height\": 3791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-010.webp\", \"caption\": \"\", \"page\": 16, \"index\": 10, \"width\": 5221, \"height\": 3791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-011.webp\", \"caption\": \"\", \"page\": 16, \"index\": 11, \"width\": 5221, \"height\": 3787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-012.webp\", \"caption\": \"\", \"page\": 16, \"index\": 12, \"width\": 5127, \"height\": 3787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-013.webp\", \"caption\": \"\", \"page\": 16, \"index\": 13, \"width\": 5127, \"height\": 3787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-014.webp\", \"caption\": \"\", \"page\": 16, \"index\": 14, \"width\": 5127, \"height\": 3791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-015.webp\", \"caption\": \"\", \"page\": 16, \"index\": 15, \"width\": 5127, \"height\": 3791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-016.webp\", \"caption\": \"\", \"page\": 16, \"index\": 16, \"width\": 5127, \"height\": 3791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-017.webp\", \"caption\": \"\", \"page\": 17, \"index\": 17, \"width\": 5576, \"height\": 4172}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-018.webp\", \"caption\": \"\", \"page\": 17, \"index\": 18, \"width\": 5576, \"height\": 4172}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-019.webp\", \"caption\": \"\", \"page\": 17, \"index\": 19, \"width\": 5576, \"height\": 4176}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-020.webp\", \"caption\": \"\", \"page\": 17, \"index\": 20, \"width\": 5576, \"height\": 4176}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-021.webp\", \"caption\": \"\", \"page\": 17, \"index\": 21, \"width\": 5576, \"height\": 4176}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-tfbu0itaez/fig-022.webp\", \"caption\": \"\", \"page\": 17, \"index\": 22, \"width\": 5722, \"height\": 4172}]"
motivation: 传统MCTS只保留最优轨迹，浪费了大量包含部分洞察和错误模式的兄弟节点。
method: 提出SIGMA框架，从MCTS树中重新整合被丢弃的兄弟节点作为高质量思维链数据。
result: 在多个推理基准上显著提升了模型性能。
conclusion: 利用搜索树中非最优分支的信息可以有效增强推理能力。
---

## Abstract
Enhancing large language models by simply scaling up datasets has begun to yield diminishing returns, shifting the spotlight to data quality. Monte Carlo Tree Search (MCTS) has emerged as a powerful technique for generating high-quality chain-of-thought data, yet conventional approaches typically retain only the top-scoring trajectory from the search tree, discarding sibling nodes that often contain valuable partial insights, recurrent error patterns, and alternative reasoning strategies. This unconditional rejection of non-optimal reasoning branches may waste vast amounts of informative data in the whole search tree. We propose SIGMA (Sibling Guided Monte Carlo Augmentation), a novel framework that reintegrates these discarded sibling nodes to refine LLM reasoning. SIGMA forges semantic links among sibling nodes along each search path and applies a two-stage refinement: a critique model identifies overlooked strengths and weaknesses across the sibling set, and a revision model conducts text-based backpropagation to refine the top-scoring trajectory in light of this comparative feedback. By recovering and amplifying the underutilized but valuable signals from non-optimal reasoning branches, SIGMA substantially improves reasoning trajectories. On the challenging MATH benchmark, our SIGMA-tuned 7B model achieves 54.92\% accuracy using only 30K samples, outperforming state-of-the-art models trained on 590K samples. This result highlights that our sibling-guided optimization not only significantly reduces data usage but also significantly boosts LLM reasoning.

---

## 论文详细总结（自动生成）

# SIGMA论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：随着大语言模型（LLM）规模增大，单纯扩大训练数据量带来的收益逐渐递减，数据质量成为提升推理能力的关键。蒙特卡洛树搜索（MCTS）能生成高质量的思维链（CoT）数据，但传统MCTS方法只保留搜索树中得分最高的单一轨迹，丢弃了大量兄弟节点（sibling nodes），这些节点往往包含部分正确的推理步骤、重复的错误模式和替代推理策略，造成了信息浪费。
- **整体含义**：本文提出SIGMA框架，通过重新利用被丢弃的兄弟节点来优化LLM推理路径，实现数据高效、性能提升的训练数据合成，从而降低对海量数据的依赖。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

**核心思想**：将MCTS搜索树中每个决策点的兄弟节点作为文本梯度信号，通过两阶段（批判+修订）过程对最优轨迹逐步细化，产生更高质量的CoT数据。

**关键技术细节**：
- **MCTS路径选择**：采用UCT（上置信界）准则平衡探索与利用，公式为：  
  `c = argmax_{j in C(n)} [V_c + cp * sqrt(ln(N_n)/N_c)]`  
  选择高价值分支。最终选出最优路径 T* = {p^(1), p^(2), ..., p^(D)}。
- **兄弟引导的文本损失与梯度**：在每个深度d，定义文本损失 L_text = Φ(T_p, {T_s | s in S(p)})，其中T_p是选中节点文本，S(p)是同父兄弟节点集合。批判模型CLLM作为符号梯度计算器，输出自然语言批评G = ∂L_text / ∂T_p = CLLM(T_p, {T_s})。
- **文本梯度下降修订**：修订模型RLLM根据当前文本T_p和梯度G生成改进版本：˜T_p = TGD.step(T_p, G) = RLLM(T_p, G)。逐深度进行，完成一轮坐标下降，优化整个路径。

**算法流程**（文字说明）：
1. 使用MCTS对每个问题构建搜索树，选择最优路径。
2. 对最优路径的每一步，收集其兄弟节点。
3. 批判模型比较选中节点与兄弟节点，输出自然语言批评（文本梯度）。
4. 修订模型根据批评修正该步骤文本，形成新的推理路径。
5. 所有步骤修订完毕后，输出改进后的完整CoT作为训练数据。

## 3. 实验设计：使用的数据集 / 场景、benchmark、对比方法

- **训练数据生成**：使用Qwen2.5-Math-7B作为生成模型，从MATH和GSM8K提示生成搜索树，每个节点采样3个候选，树最大深度16。生成两个温度（0.4和0.7）各15K样本，共30K训练集。批判和修订模型使用GPT-4o-mini。
- **基准测试**：**In-domain**：GSM8K、MATH。**Out-of-domain**：CollegeMath、DeepMind Mathematics、OlympiadBench-Math、TheoremQA，共6个数学推理基准。另外在常识推理任务（CommonsenseQA、StrategyQA、ARC-Challenge）上也进行了评估。
- **对比方法**：MetaMath、WizardMath、MMIQC、MathScale、RefAug、DART-Math、MathFusion等，以及无增强的标准微调基线。对比不同数据量（15K~2.3M）下的性能。

## 4. 资源与算力

- **MCTS生成**：使用1块RTX 4090，生成15K样本约需42 GPU小时。
- **修订阶段**：使用GPT-4o-mini API，15K样本消耗33.6M prompt tokens + 11.4M completion tokens，成本约11.7美元；60K样本成本约47.6美元。若用开源Qwen2.5-7B-Instruct替代，总成本约70美元。
- **微调**：4块NVIDIA H100 GPU，采用DeepSpeed ZeRO、混合精度FP16、AdamW优化器，训练3个epoch。学习率：DeepSeekMath-7B 5e-5，Llama3-8B 1e-5，Mistral-7B 4e-6。

## 5. 实验数量与充分性

- 实验覆盖3个基座模型（DeepSeekMath-7B、Llama3-8B、Mistral-7B），在6个数学推理基准上报告准确率。还额外评估了Qwen2.5-Math-7B以及常识推理任务，验证泛化性。
- 进行了多组消融实验：
  - 对比未经细化的原始MCTS路径（MCTS-15K）和黑盒GPT-4o-mini直接生成CoT（Blackbox-15K）。
  - 更换批判/修订模型（Qwen2.5-7B-Instruct、Qwen2.5-72B-Instruct、GPT-4o-mini）观察影响。
  - 不同数据规模（15K、30K、60K）对比。
- 实验设计较为充分，对比了多个SoTA方法，控制数据量一致，使用零样本贪婪解码，结果客观。但未报告多次运行的统计误差（如方差），公平性上采用官方评估协议。

## 6. 论文的主要结论与发现

- SIGMA-15K在所有3个基座模型上超越所有30K规模基线，表明极高数据效率。
- SIGMA-30K性能优于或持平60K规模方法，例如DeepSeekMath-7B平均48.2%，超过MathFusion-30K的45.7%，接近DART-Math-60K的47.4%。
- 在Qwen2.5-Math-7B上，SIGMA-60K平均65.6%，显著优于Rstar、AceMath等。
- 消融证明：相比原始MCTS路径，SIGMA提升约10+绝对百分点；相比黑盒CoT生成，提升约1-2个百分点。
- 批判/修订模型的泛化性：不同教师模型（包括开源7B）均能带来显著提升，GPT-4o-mini最佳。
- 结论：通过回收搜索树中非最优分支的信息，可以显著提升推理数据质量，减少训练数据需求。

## 7. 优点

- **数据高效**：仅用15K~60K样本即可达到甚至超越数百万样本训练的模型性能，计算成本大幅降低（相比DART-Math降低30倍以上）。
- **方法创新**：首次系统性地利用MCTS中丢弃的兄弟节点作为对比信号，引入文本梯度下降概念，在语言空间进行优化，无需额外rollout或外部奖励模型。
- **模型无关与易于集成**：批判和修订模型可选择不同规模和来源（包括开源模型），可无缝接入现有数据生成流水线。
- **泛化性强**：在多种基座模型、数学推理和常识推理任务上均取得一致提升，证明框架的通用性。

## 8. 不足与局限

- **批判/修订模型依赖性**：当前使用GPT-4o-mini（私有API），虽实验表明开源模型也可用，但性能仍有差距，可能受限教师能力。未来可探索更强开源模型。
- **规模有限**：仅对7B~8B基座模型进行全参数微调，未验证更大模型（如70B）或LoRA等轻量方法上的表现。
- **未进行多次运行统计**：主要报告单次结果，缺少误差棒或置信区间，结果稳健性需进一步验证。
- **未覆盖非数学推理任务**：仅在数学和少量常识推理上测试，对代码、科学推理等领域的泛化性尚待验证。
- **MCTS生成成本**：虽然总成本低，但生成搜索树仍需一定GPU时间，且在生成过程中温度等超参数可能影响后续质量。

（完）
