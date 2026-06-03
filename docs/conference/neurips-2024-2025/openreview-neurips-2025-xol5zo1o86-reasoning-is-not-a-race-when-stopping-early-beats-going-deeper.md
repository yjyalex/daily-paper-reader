---
title: "Reasoning Is Not a Race: When Stopping Early Beats Going Deeper"
title_zh: 推理不是赛跑：早停优于深入
authors: "Mohan Zhang, Jiaxuan Gao, Shusheng Xu, Yi Wu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=xoL5zo1O86"
tags: ["query:rl-nlplr"]
score: 9.0
evidence: 使用过程奖励模型指导长思维链推理
tldr: 提出ZGES通过检测步骤质量峰值改进PRM引导的思维链推理
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 3002, \"height\": 2381}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 4553, \"height\": 2581}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 2342, \"height\": 2158}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 2345, \"height\": 2158}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-005.webp\", \"caption\": \"\", \"page\": 4, \"index\": 5, \"width\": 2345, \"height\": 2158}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 912, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-009.webp\", \"caption\": \"\", \"page\": 8, \"index\": 9, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-010.webp\", \"caption\": \"\", \"page\": 8, \"index\": 10, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-011.webp\", \"caption\": \"\", \"page\": 8, \"index\": 11, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-012.webp\", \"caption\": \"\", \"page\": 16, \"index\": 12, \"width\": 1500, \"height\": 1050}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-013.webp\", \"caption\": \"\", \"page\": 16, \"index\": 13, \"width\": 1500, \"height\": 1050}]"
motivation: 过程奖励模型引导的长思维链推理中，步骤质量呈现递减趋势，导致标准PRM方法不如无PRM方法。
method: 提出Z分数引导的早停（ZGES），利用局部PRM奖励的Z分数在质量峰值处停止搜索。
result: 在多个数学基准和模型规模上，ZGES优于标准PRM引导束搜索和无PRM方法。
conclusion: 合理利用PRM的早期步骤信号可以提升长思维链推理的效果和效率。
---

## Abstract
We study the use of Process Reward Models (PRMs) for guiding Long Chain-of-Thought (CoT) reasoning in large language models. Although PRMs deliver fine-grained feedback in standard tasks, PRM-guided beam search does not consistently outperform PRM-free approaches in long CoT reasoning. We trace this shortfall to a "step quality degradation''—the expected step quality shows concave behavior, yielding unimodal or monotonically declining trends. To counteract this, we propose Z-Score Guided Early Stopping (ZGES), which halts search at the detected quality peak using local PRM-reward z-scores. Across multiple math benchmarks and model scales, ZGES outperforms both standard PRM-guided beam search and the PRM-free methods. Ablation studies further highlight the advantages and robustness of ZGES’s adaptive stopping mechanism.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLM）的长思维链（Long CoT）推理能力日益增强（如OpenAI o1、DeepSeek R1），但如何有效利用过程奖励模型（PRM）进行测试时搜索仍不明确。以往在短CoT任务中PRM引导的束搜索（Beam Search）优于无PRM方法（如多数投票），但在长CoT推理中，PRM引导的束搜索并未一致超越无PRM方法，甚至更差。
- **核心问题**：为什么PRM在长CoT推理中效果不佳？如何提升其效果？
- **整体含义**：论文发现束搜索过程中存在“步骤质量退化”（step quality degradation）——预期步骤质量呈现单峰或单调递减的趋势，并据此提出基于Z分数引导的早停策略（ZGES），在质量峰值处停止搜索，从而提升推理效果和效率。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：在长CoT推理的束搜索过程中，步骤质量在早期达到峰值后开始退化，因此选择在质量峰值处提前停止搜索，让LLM直接生成剩余轨迹和最终答案。
- **关键技术细节**：
  - 定义步骤质量 \(V^\pi(s)\) 为从当前状态正确完成推理的概率（等价于稀疏奖励强化学习中的状态价值函数）。
  - 通过蒙特卡洛 rollout 估计步骤质量，并观察其趋势：呈单峰或单调递减。
  - 理论上证明：由于PRM在束搜索后期重排序能力下降，步骤质量的期望呈凹性（第二阶差分为负）。
  - 提出**ZGES**：在束搜索每一步计算所有候选步骤的平均PRM奖励，然后计算该序列的局部z-score（基于当前及之前步骤的均值和标准差）。当局部z-score低于阈值λ时，触发早停，并从当前步骤的前一步候选开始直接解码后续内容。
- **流程描述**：
  1. 设置阈值λ和束搜索配置（束宽B×扩展因子E）。
  2. 运行束搜索，每一步记录平均PRM奖励 \(x_t\)。
  3. 计算 \(x_t\) 的局部z-score \(x'_t\)。
  4. 若 \(x'_t < λ\)，则停止搜索，从步骤 \(t-1\) 的候选直接解码；否则继续。
- **关键理论**：证明了平均PRM奖励与平均步骤质量之间存在强线性关系，因此z-score一致，可以可靠地指示步骤质量的变化趋势。

## 3. 实验设计

- **数据集/基准**：三个数学竞赛基准：AMC2023、AIME2024、AIME2025。
- **模型**：使用DeepSeek-R1-Distill-Qwen-1.5B和7B作为长CoT策略模型；PRM训练基于与策略模型一致的基座模型，采用Hard PRM和Soft PRM两种类型（论文主要报告Hard PRM的结果）。
- **对比方法**：
  - 无PRM基线：多数投票（Majority@N）。
  - 标准PRM引导的束搜索（Hard PRM和Soft PRM），使用加权最佳N（WBoN）选择最终答案。
  - 固定步数早停的束搜索。
  - 本文方法ZGES。
- **配置**：束搜索中扩展因子固定为2，束宽从4到64变化，控制总预算N=B×E与Majority@N的N相同。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅在附录中提到训练PRM时收集了9K道难题的数据集，并生成16条回答用于标注。
- **说明**：论文未提供详细的算力开销报告。

## 5. 实验数量与充分性

- **实验数量**：较为充分，包括：
  - 在两个模型规模（1.5B、7B）上，针对三个基准进行主要对比实验（共6种模型-基准组合）。
  - 对束宽（4,8,16,32,64）进行扩展实验。
  - 消融实验：超参数λ的敏感性分析（4种取值：-0.4, -0.6, -0.8, -1.0），以及对比固定步数早停。
  - 分析实验：PRM重排序能力随步骤下降（附录C.2），PRM奖励与步骤质量的相关性（表2、图3）。
- **充分性评价**：实验覆盖了不同模型规模、不同难度基准、不同搜索配置，对比了多种基线，并进行了理论分析和消融，整体比较客观公平。但未报告多次运行的方差/置信区间，可能缺乏统计显著性信息。

## 6. 论文的主要结论与发现

- PRM引导的束搜索在长CoT推理中不一定优于无PRM方法（如多数投票），原因在于步骤质量退化。
- 步骤质量的预期值在束搜索过程中呈凹性，表现为单峰或单调递减。
- 平均PRM奖励与平均步骤质量之间存在强线性关系，因此z-score可以反映步骤质量趋势。
- 提出的ZGES方法在几乎所有实验设置中均优于标准PRM束搜索和多数投票，且具有更好的计算效率（更少的PRM调用和令牌生成）。
- 动态早停优于固定步数早停。
- ZGES对阈值λ的选取具有鲁棒性。

## 7. 优点

- **问题洞察深刻**：通过实证和理论揭示长CoT中PRM束搜索效果不佳的根本原因——步骤质量退化，而非简单归因于PRM质量。
- **方法简洁有效**：ZGES仅需计算局部z-score，代码改动小，但效果提升显著，且降低计算开销。
- **理论支撑**：从概率论和强化学习角度证明了步骤质量的凹性和z-score的一致性，增强了方法可信度。
- **实验全面**：涵盖多个模型大小和难度基准，进行了消融和敏感性分析，验证鲁棒性。
- **实际价值**：在AIME2024等高难度基准上，1.5B模型使用ZGES（16束）准确率从60.0%提升至66.7%，且减少PRM调用50%以上。

## 8. 不足与局限

- **资源限制**：论文自述因计算资源有限，未能探索更广的λ值（如更激进的早停λ∈[0,1]），可能错过更优配置。
- **实验统计性**：未提供多次重复实验的误差棒或置信区间，无法判断结果是否显著。
- **步骤分割依赖启发式**：实际中步骤分割（step segmentation）基于符号标记和令牌长度阈值（约2000 tokens），可能影响PRM评分和z-score计算。
- **仅测试数学推理**：未在更广泛的推理任务（如代码合成、科学推理）中验证，泛化性未知。
- **假设限制**：理论推导基于两个假设（推理策略正确步骤更可能导致正确，PRM重排序能力逐渐下降），虽然文中提供了经验证据，但假设的轻微违反可能影响结论。
- **方法依赖PRM质量**：ZGES的有效性依赖于PRM奖励与步骤质量的强线性关系，若PRM训练不佳，可能失效。
- **未探索其他早停信号**：仅使用了z-score，未比较其他异常检测或变化点检测方法。

（完）
