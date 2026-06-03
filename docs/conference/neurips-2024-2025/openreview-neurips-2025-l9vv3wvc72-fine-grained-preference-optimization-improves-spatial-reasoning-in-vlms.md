---
title: Fine-Grained Preference Optimization Improves Spatial Reasoning in VLMs
title_zh: 细粒度偏好优化提升视觉语言模型空间推理
authors: "Yifan Shen, Yuanzhe Liu, Jingyuan Zhu, Xu Cao, Xiaofeng Zhang, Yixiao He, Wenming Ye, James Matthew Rehg, Ismini Lourentzou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=L9vV3wVC72"
tags: ["query:rl-nlplr"]
score: 8.0
evidence: 使用细粒度DPO和MCTS进行空间推理，符合强化学习用于推理
tldr: SpatialReasoner-R1使用细粒度偏好优化提升VLM空间推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1024, \"height\": 812}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 761, \"height\": 470}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 802, \"height\": 1122}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 666, \"height\": 404}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-007.webp\", \"caption\": \"\", \"page\": 20, \"index\": 7, \"width\": 723, \"height\": 482}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-008.webp\", \"caption\": \"\", \"page\": 21, \"index\": 8, \"width\": 455, \"height\": 311}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-009.webp\", \"caption\": \"\", \"page\": 21, \"index\": 9, \"width\": 455, \"height\": 335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-010.webp\", \"caption\": \"\", \"page\": 21, \"index\": 10, \"width\": 455, \"height\": 315}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-011.webp\", \"caption\": \"\", \"page\": 21, \"index\": 11, \"width\": 455, \"height\": 339}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-012.webp\", \"caption\": \"\", \"page\": 22, \"index\": 12, \"width\": 1122, \"height\": 698}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-013.webp\", \"caption\": \"\", \"page\": 23, \"index\": 13, \"width\": 804, \"height\": 499}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-l9vv3wvc72/fig-014.webp\", \"caption\": \"\", \"page\": 24, \"index\": 14, \"width\": 1446, \"height\": 904}]"
motivation: 现有视觉语言模型在多步逻辑和空间对齐上表现不佳。
method: 设计多模型MCTS生成LongCoT轨迹，并采用细粒度DPO进行偏好优化。
result: 在空间推理任务上取得显著改进。
conclusion: 细粒度偏好优化能有效增强多步空间推理能力。
---

## Abstract
Current Vision-Language Models (VLMs) struggle with fine-grained spatial reasoning, particularly when multi-step logic and precise spatial alignment are required. In this work, we introduce SpatialReasoner-R1, a vision-language reasoning model designed to address these limitations. To construct high-quality supervision for spatial reasoning, we design a Multi-Model Monte Carlo Tree Search (M3CTS) method that generates diverse, logically consistent Long Chain-of-Thought (LongCoT) reasoning trajectories. In addition, we propose a fine-grained Direct Preference Optimization (fDPO) method that introduces segment-specific preference granularity for descriptive grounding and logical reasoning, guided by a spatial reward mechanism that evaluates candidate responses based on visual consistency, spatial grounding, and logical coherence. Experimental results demonstrate that fDPO achieves relative performance gains of 4.1% and 9.0% over standard DPO on spatial qualitative and quantitative tasks, respectively. SpatialReasoner-R1, trained with fDPO, sets a new SoTA on SpatialRGPT-Bench, outperforming the strongest baseline by 9.4% in average accuracy, while maintaining competitive performance on general vision-language tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前视觉语言模型（VLM）在细粒度空间推理方面存在明显不足，尤其当任务需要多步逻辑推理和精确的空间对齐时，模型表现不佳。典型场景包括复杂物体排列、遮挡、相对方向判断以及距离/尺寸的定量估计。
- **研究动机**：空间推理是机器人、自动驾驶、增强现实等应用的基础能力，现有VLM往往依赖直接输出答案的范式，缺乏显式推理步骤；即使使用思维链（CoT）提示，生成的推理轨迹也往往过于简短或抽象，无法捕捉精细的空间逻辑。因此需要一种能生成结构化、可解释的长链条思维（LongCoT）推理，并通过细粒度优化提升推理质量的方法。
- **整体含义**：本文通过提出 SpatialReasoner-R1 模型，首次将细粒度偏好优化（fDPO）与多模型蒙特卡洛树搜索（M3CTS）结合，系统性地提升了VLM在多步空间推理任务上的性能，同时保持了通用视觉语言能力。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **整体架构**：SpatialReasoner-R1 基于 Sa2VA 架构（InternVL2.5 变体），接受图像、文本查询和视觉提示（区域掩码）作为输入，输出带有 `<description>` 和 `<reasoning>` 标签的 LongCoT 响应。
- **核心方法**：
  - **多模型蒙特卡洛树搜索（M3CTS）**：用于生成高质量、多样化的 LongCoT 推理数据。具体流程包括：
    - **Expand**：同时使用多个 VLM（如 Gemini 1.5 Pro、Qwen2.5VL-72B 等）扩展搜索树中的节点，生成候选推理状态。
    - **Simulate**：对每个候选状态，由两个评估模型在三个维度（视觉描述准确性、空间关系正确性、逻辑推理连贯性）上打分（+1/0/-1），保留非负分节点。
    - **Backprop**：将模拟得分递归向上传播，更新父节点的价值估计和访问计数。
    - **Select**：基于上置信界（UCB）策略选择最有前途的节点继续扩展，平衡探索与利用。
  - **细粒度直接偏好优化（fDPO）**：将 LongCoT 响应分解为描述部分（R_desc）和推理部分（R_reason）。核心原则：
    - 原则1：优化强度应根据描述和推理组件的内在复杂度与质量差异动态平衡。
    - 原则2：偏好差分更大的组件应获得更大的优化权重。
    - 实现：计算每个片段的偏好差分 ΔR，通过 softmax 映射得到权重 w_desc 和 w_reason，再通过缩放因子 α 调整基础 β 参数，得到片段特定的 β_desc 和 β_reason。最终损失函数为：
      ```
      L_fDPO(θ) = -E[ log σ( β_desc * F_desc + β_reason * F_reason ) ]
      ```
      其中 F_s 是片段级别的似然比。
  - **细粒度空间奖励机制**：评估候选推理路径的三个维度：
    - **视觉一致性奖励（R_vc）**：评价描述部分的存在性、属性准确性、完整性和适当性（0-4分）。
    - **深度引导空间奖励（R_sp）**：利用深度图验证描述和推理中的每个空间断言，引入不确定性权重（0.8-1.0）和上下文权重（0.8-1.0），分别对描述和推理部分计算得分 R_sp,desc 和 R_sp,reason。
    - **逻辑连贯奖励（R_lc）**：评价推理部分的事实一致性、逻辑连贯性、规则应用正确性和结论有效性（0-4分）。
    综合得分：score(R_desc) = R_vc + R_sp,desc；score(R_reason) = R_lc + R_sp,reason。

## 3. 实验设计：数据集、基准与对比方法

- **主要基准**：SpatialRGPT-Bench，包含 657 个定性 VQA 对（分类问题如高低、左右、宽窄等）和 749 个定量 VQA 对（数值估计如直接距离、水平距离、垂直距离、宽度、高度、方向）。
- **通用视觉语言基准**：MME、POPE、SEED-Bench、AI2D、SQA-TEST、MMMU_V、MMStar、HallusionBench。
- **对比方法**：
  - **通用大型 VLM**：Gemini 2.0 Flash、Llama 4 Maverick、Gemini 1.5 Pro、ChatGPT-4o（零样本/少样本）。
  - **专门化 VLM**：SpatialBot-3B、SpaceThinker Qwen2.5VL-3B、InternVL2.5-78B、Sa2VA (4B/8B)、SpatialRGPT-8B。
  - **SpatialReasoner-R1 变体**：SFT 版、DPO 版、fDPO 版（4B 和 8B 参数规模）。
- **评估协议**：定性问题由 GPT-4o 判断回答与标准答案语义是否一致，给予 0/1 分；定量问题提取数值并标准化为米，计算预测值在真值 ±25% 内视为成功。

## 4. 资源与算力

- 文中在附录 D 中明确说明：训练在 **2 块 NVIDIA H100 GPU** 上进行，每阶段（SFT 和 DPO）各约 **2.5 天**。使用 AdamW 优化器，SFT 学习率 4e-5，DPO 学习率 1e-7，权重衰减 0.05。评估和奖励计算使用了外部模型（GPT-4o、Gemini 1.5 Pro、Qwen2.5VL-72B 等）。

## 5. 实验数量与充分性

- **空间推理主实验**（表1）覆盖 12 个分类/数值子任务，对比了 10 个基线模型和 6 个自身变体。
- **通用视觉语言实验**（表2）覆盖 8 个通用基准，与最强基线 SpatialRGPT-8B 对比。
- **消融实验**：
  - α 参数在 10%-40% 范围内的 5 组实验（表3）。
  - λ 参数在 0.2-0.8 范围内的 4 组实验（表4）。
- **定性示例**：提供 4 个具体案例（图4、图7、图8、图9），展示推理过程对比和失败案例。
- **充分性评价**：实验覆盖了主要空间推理维度（方向、距离、尺寸、高度等）和通用能力，消融实验验证了关键超参数的影响。但缺少统计显著性检验（如置信区间或多次运行的平均值），且未报告随机种子影响。总体而言，实验设计较为全面、客观，对比公平。

## 6. 论文的主要结论与发现

- **fDPO 优于标准 DPO**：fDPO 在空间定性任务上平均提升 4.1%，定量任务提升 9.0%。
- **SpatialReasoner-R1 fDPO 8B 达到 SOTA**：在 SpatialRGPT-Bench 上平均准确率 95.59%（定性）和 77.30%（定量），分别比最强基线 SpatialRGPT-8B 提升 2.9% 和 15.8%，平均提升 9.4%。
- **参数高效**：4B 模型（fDPO）超越了 78B 的 InternVL2.5，展示了细粒度优化的有效性。
- **通用能力保持**：在 MME、POPE、SEED-Bench 等通用基准上，SpatialReasoner-R1 也优于 SpatialRGPT-8B，表明空间推理增强未损害其他能力。
- **消融发现**：α=30%、λ=0.6 时获得最优性能；过大的 α 或 λ 会导致性能下降。

## 7. 优点：方法与实验设计亮点

- **方法创新**：
  - 首次在空间推理中引入段级偏好粒度（fDPO），区分描述和推理的优化需求，比传统 DPO 更细致。
  - M3CTS 利用多个 VLM 协同探索，生成多样化且逻辑一致的 LongCoT 数据，填补了高质量空间推理训练数据的缺失。
  - 奖励机制结合视觉一致性、深度验证和逻辑连贯，提供多维度的反馈信号。
- **实验设计亮点**：
  - 对比了多种规模的模型（3B-78B）和不同训练策略（SFT、DPO、fDPO），验证了方法的可扩展性。
  - 提供了定性和定量双重评估，以及详细的消融实验和案例研究（包括失败案例），分析深入。
  - 开源计划（附录提及）有利于复现和后续研究。

## 8. 不足与局限

- **输入依赖**：模型需要显式的区域提示（掩码）来区分参考物体，无法从纯文本描述中自动定位物体，这在真实应用中可能受限。
- **空间维度限制**：当前聚焦于 2D 图像中的空间推理，未扩展到 3D 或具身场景。
- **训练数据偏差**：训练数据来自 OpenSpatial 数据集（400K 样本），场景和物体类别有限，可能影响泛化性。负样本通过仅修改结论构造，可能简单化。
- **奖励模型依赖**：奖励计算依赖 GPT-4o 和深度估计模型（Depth Anything），这些外部模型本身的偏差和错误可能传播。
- **计算成本**：M3CTS 数据生成和奖励评估需要多次调用多个大模型，开销较大。
- **统计报告不足**：未报告多次运行的标准差或置信区间，实验结果可能存在随机性影响。
- **局限性讨论**：附录 H 和 G 指出，错误的空间推理在安全关键领域可能带来风险，且模型对分布偏移的鲁棒性未充分验证。

（完）
