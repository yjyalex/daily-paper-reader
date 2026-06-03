---
title: Enhancing the Outcome Reward-based RL Training of MLLMs with Self-Consistency Sampling
title_zh: 用自我一致性采样增强多模态大语言模型基于结果奖励的强化学习训练
authors: "Jiahao Wang, Weiye Xu, Aijun Yang, Wengang Zhou, Lewei Lu, Houqiang Li, Xiaohua Wang, Jinguo Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=cGkfMGQdCy"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 解决了强化学习训练中思维链不忠实轨迹的问题
tldr: 提出自我一致性采样来纠正多模态LLM基于结果奖励RL中的不忠实轨迹。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 325, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3960, \"height\": 2356}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-005.webp\", \"caption\": \"\", \"page\": 24, \"index\": 5, \"width\": 430, \"height\": 321}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-006.webp\", \"caption\": \"\", \"page\": 24, \"index\": 6, \"width\": 430, \"height\": 321}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-007.webp\", \"caption\": \"\", \"page\": 24, \"index\": 7, \"width\": 471, \"height\": 352}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-008.webp\", \"caption\": \"\", \"page\": 24, \"index\": 8, \"width\": 421, \"height\": 314}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-009.webp\", \"caption\": \"\", \"page\": 24, \"index\": 9, \"width\": 430, \"height\": 321}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-010.webp\", \"caption\": \"\", \"page\": 25, \"index\": 10, \"width\": 733, \"height\": 399}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-011.webp\", \"caption\": \"\", \"page\": 27, \"index\": 11, \"width\": 776, \"height\": 462}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-012.webp\", \"caption\": \"\", \"page\": 27, \"index\": 12, \"width\": 1848, \"height\": 338}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-013.webp\", \"caption\": \"\", \"page\": 28, \"index\": 13, \"width\": 1606, \"height\": 740}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-014.webp\", \"caption\": \"\", \"page\": 28, \"index\": 14, \"width\": 787, \"height\": 682}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-015.webp\", \"caption\": \"\", \"page\": 30, \"index\": 15, \"width\": 531, \"height\": 367}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-016.webp\", \"caption\": \"\", \"page\": 30, \"index\": 16, \"width\": 750, \"height\": 625}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-017.webp\", \"caption\": \"\", \"page\": 31, \"index\": 17, \"width\": 489, \"height\": 252}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-018.webp\", \"caption\": \"\", \"page\": 32, \"index\": 18, \"width\": 800, \"height\": 300}]"
motivation: 基于结果奖励的RL中，不忠实轨迹（错误推理后猜对答案）会获得相同奖励，阻碍学习。
method: 引入视觉扰动和重复截断重采样，通过轨迹一致性检测不忠实样本。
result: 有效过滤了不忠实轨迹，提升了RL训练效果。
conclusion: 自我一致性采样能纠正RL中的不忠实推理问题。
---

## Abstract
Outcome‑reward reinforcement learning (RL) is a common—and increasingly significant—way to refine the step‑by‑step reasoning of multimodal large language models (MLLMs). In the multiple‑choice setting—a dominant format for multimodal reasoning benchmarks—the paradigm faces a significant yet often overlooked obstacle: unfaithful trajectories that guess the correct option after a faulty chain of thought receive the same reward as genuine reasoning, which is a flaw that cannot be ignored. We propose Self‑Consistency Sampling (SCS) to correct this issue. For each question, SCS (i) introduces small visual perturbations and (ii) performs repeated truncation‑and‑resampling of a reference trajectory; agreement among the resulting trajectories yields a differentiable consistency score that down‑weights unreliable traces during policy updates. Plugging SCS into RLOO, GRPO, REINFORCE++ series improves accuracy by up to 7.7 percentage points on six multimodal benchmarks with negligible extra computation, offering a simple, general remedy for outcome‑reward RL in MLLMs.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：基于结果奖励的强化学习（Outcome‑reward RL）被广泛用于提升多模态大语言模型（MLLMs）的逐步推理能力。在多项选择问题这一多模态推理的主流形式中，标准做法仅根据最终选项是否匹配正确答案来赋予奖励。
- **核心问题**：这种奖励机制存在一个严重缺陷——模型可以通过**不忠实的推理轨迹**（即推理过程错误，但偶然猜对选项）获得与正确推理完全相同的奖励。这种“虚假成功”阻碍了模型学习真正的推理能力，导致模型在多项选择训练中的提升远低于开放问答场景（实验显示提升差距达6.4个百分点）。
- **研究动机**：需要一种方法区分忠实与不忠实的推理轨迹，从而引导模型生成更可靠的推理过程。

## 2. 方法论

### 2.1 核心思想
通过引入**一致性奖励（Consistency Reward）** 来惩罚不忠实的推理。假设在正确推理下，重复从推理前缀生成答案应得到一致的结果；而错误推理会导致答案多样性增加。通过衡量多个采答案的一致性，可以推断推理过程的可靠性。

### 2.2 关键技术细节
- **初始生成**：对于每个问题，模型先生成一个初始推理轨迹 \( \tau \)。
- **截断‑重采样（Truncation‑Resampling, TR）**：将初始轨迹按比例 \( k \) 截断，得到不完整轨迹 \( \tau_{<} \)。以该截断点为前缀，多次让模型继续生成新的答案。
- **视觉扰动（Visual Perturbation, VP）**：在每次重采样时，对输入图像添加独立的高斯噪声（噪声强度均匀采样），促使模型基于扰动的视觉证据进行推理。
- **一致性奖励计算**：将所有重采样得到的答案收集到集合 \( A \) 中，一致性奖励定义为：
  \[
  r_{\text{con}} = c \cdot (N - |A|)
  \]
  其中 \( N \) 是选项总数，\( c \) 是缩放系数。答案集合越小（即一致性越高），奖励越高。
- **最终奖励**：\( r = r_{\text{acc}} + r_{\text{format}} + r_{\text{con}} \)，其中 \( r_{\text{acc}} \) 是最终选项的准确率奖励，\( r_{\text{format}} \) 是格式奖励。
- **策略更新**：使用标准策略梯度方法（如RLOO、GRPO、REINFORCE++）优化策略，优势函数基于归一化的奖励。

### 2.3 算法流程（文字说明）
1. 对每个问题，采样初始回答和推理轨迹，计算准确率奖励。
2. 进行 \( m \) 次重采样：
   - 截断初始轨迹至比例 \( k \)；
   - 对图像添加随机高斯噪声；
   - 从截断前缀继续生成，得到新答案，加入集合 \( A \)。
3. 根据答案集合计算一致性奖励。
4. 计算总奖励并减去基线。
5. 计算策略梯度，更新模型参数。

## 3. 实验设计

### 3.1 数据集与场景
- **训练数据**：三个开源数据集——M3CoT（通用）、ScienceQA（科学）、Geometry3K（几何），仅保留含图像的多项选择题。
- **评估Benchmarks**：六个主流多模态基准：
  - M3CoT、ScienceQA、MathVision、We-Math、MMMU、MathVerse
  - 涵盖数学、科学、医学等多个领域，均使用多项选择题子集。

### 3.2 对比方法
- **Baselines**：原始预训练模型（Qwen2.5-VL-7B-Instruct等）、监督微调（SFT）。
- **RL算法**：GRPO、REINFORCE++‑baseline、REINFORCE++、RLOO，分别使用标准结果奖励（仅准确率+格式）以及结合SCS的变体。
- **模型规模与架构**：主要在Qwen2.5-VL-7B-Instruct上进行，同时用Qwen2.5-VL-3B-Instruct和InternVL3-8B验证泛化性。

## 4. 资源与算力

- **硬件**：每个实验使用 8 张 A800 GPU。
- **训练时长**：约 24 小时。
- **额外开销**：引入SCS后训练时间增加约 38%（从12.5小时到17.2小时），但作者指出使用vLLM等推理引擎可并行化，实际影响可控。

## 5. 实验数量与充分性

- **实验组数**：涵盖多种算法、模型、超参数设置，包括：
  - 4种RL算法 × 是否使用SCS，共8组主要对比。
  - 三个不同模型上的RLOO+SCS实验。
  - 组件消融（TR vs VP vs 两者组合）。
  - 超参数敏感性实验（截断比例、重采样次数）。
  - 统计稳定性实验（3次重复运行，计算95%置信区间）。
  - 推理可靠性量化（人工+强LLM判定100个正确样本）。
- **充分性评价**：实验设计较为全面，覆盖了主要算法、模型变体、关键组件和超参数，并提供了统计显著性验证。对比基准包括SFT和多种RL基线，确保客观公平。

## 6. 主要结论与发现

- SCS在多种RL算法上均带来一致提升：RLOO提升最大（+7.7百分点），REINFORCE++ +2.0，REINFORCE++‑baseline +1.7，GRPO +0.9（均基于Qwen2.5-VL-7B）。
- 在较小模型（3B）和不同架构（InternVL3-8B）上，SCS也分别带来+3.2和+1.6的提升。
- 推理可靠性分析表明，SCS训练后模型不忠实推理的发生率下降约14-15%。
- 截断‑重采样和视觉扰动两个组件均不可或缺，结合使用时效果最佳。
- 超参数选择（截断比例约0.8，重采样次数约4）可取得最佳平衡，性能波动控制在4个百分点以内，方法稳健。

## 7. 优点

- **简单高效**：无需训练额外的奖励模型（如过程奖励模型），仅通过采样阶段引入一致性检查，计算开销可接受。
- **通用性强**：可即插即用于多种主流RL算法（RLOO、GRPO、REINFORCE++系列），适用于不同规模和多架构的MLLMs。
- **直接解决核心问题**：针对性克服了结果奖励RL中推理不忠实的问题，实验不仅验证了精度提升，还通过人工和LLM判定直接证明了推理质量的改善。
- **分析深入**：提供了丰富的定性、定量分析和消融实验，使结论扎实可信。

## 8. 不足与局限

- **实验泛化范围有限**：作者承认SCS尚未在纯文本LLM和更多MLLM上充分验证，当前结论主要基于Qwen2.5-VL系列和InternVL3-8B。
- **计算开销**：虽然可控，但训练时间增加约38%，在资源受限场景下可能成为瓶颈。
- **超参数依赖**：方法的最优超参数（截断比例、重采样次数、噪声范围）需针对任务调整，文中虽给出推荐值，但未提供自动化选择策略。
- **仅针对多项选择问题**：目前设计依赖于选项数量固定的场景，对开放生成任务（如数学计算、代码）需要额外适配。
- **假设限制**：方法假设正确推理→确定正确答案，但现实中部分问题可能存在多个正确推理路径，假设的严格性可能影响部分场景适用性。

（完）
