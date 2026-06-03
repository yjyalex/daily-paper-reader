---
title: Latent Chain-of-Thought for Visual Reasoning
title_zh: 用于视觉推理的潜在思维链
authors: "Guohao Sun, Hang Hua, Jian Wang, Jiebo Luo, Sohail Dianat, MAJID RABBANI, Raghuveer Rao, Zhiqiang Tao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=0i8ClSr3kQ"
tags: ["query:token"]
score: 8.0
evidence: 使用强化学习为潜在思维链提供token级稀疏奖励
tldr: 基于token级强化学习和贝叶斯推理扩展的潜在思维链视觉推理方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 320, \"height\": 562}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-003.webp\", \"caption\": \"\", \"page\": 10, \"index\": 3, \"width\": 721, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-004.webp\", \"caption\": \"\", \"page\": 15, \"index\": 4, \"width\": 424, \"height\": 591}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-005.webp\", \"caption\": \"\", \"page\": 17, \"index\": 5, \"width\": 800, \"height\": 557}]"
motivation: 现有视觉推理训练算法泛化性差且依赖有偏奖励模型。
method: 将推理视为后验推断，使用多样化的强化学习算法和token级稀疏奖励训练潜在思维链。
result: 所提方法在多个视觉推理任务上优于传统方法。
conclusion: 潜在思维链结合token级强化学习能有效提升视觉推理的泛化性和可靠性。
---

## Abstract
Chain-of-thought (CoT) reasoning is critical for improving the interpretability and reliability of Large Vision-Language Models (LVLMs). However, existing training algorithms such as SFT, PPO, and GRPO may not generalize well across unseen reasoning tasks and heavily rely on a biased reward model. To address this challenge, we reformulate reasoning in LVLMs as posterior inference and propose a scalable training algorithm based on amortized variational inference. By leveraging diversity-seeking reinforcement learning algorithms, we introduce a novel sparse reward function for token-level learning signals that encourage diverse, high-likelihood latent CoT, overcoming deterministic sampling limitations and avoiding reward hacking. Additionally, we implement a Bayesian inference-scaling strategy that replaces costly Best-of-N and Beam Search with a marginal likelihood to efficiently rank optimal rationales and answers. We empirically demonstrate that the proposed method enhances the state-of-the-art LVLMs on four reasoning benchmarks, in terms of effectiveness, generalization, and interpretability.

---

## 论文详细总结（自动生成）

# 论文总结：Latent Chain-of-Thought for Visual Reasoning

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：大型视觉-语言模型（LVLMs）在视觉推理任务中依赖思维链（CoT）来提升可解释性和可靠性。然而，现有训练算法（如监督微调 SFT、近端策略优化 PPO、组相对策略优化 GRPO）存在泛化性差、依赖有偏奖励模型、探索不足、易导致奖励黑客（reward hacking）等问题。
- **核心问题**：如何让 LVLMs 在不依赖直接监督和偏置奖励模型的前提下，学习到多样且高似然的潜在 CoT，从而提升跨任务泛化能力和推理可靠性。
- **整体含义**：本文将视觉推理重新定义为后验推断问题，利用摊销变分推理（Amortized Variational Inference）框架，使模型能够从真实后验分布中采样潜在推理链，替代传统方法中受限的确定性采样。

## 2. 方法论：核心思想、关键技术细节

### 2.1 核心思想
- 将推理链 Z 视为给定视觉指令 X 和答案 Y 条件下的潜在变量，目标是近似后验分布 P(Z|X,Y)。
- 使用生成流网络（GFlowNets）作为摊销变分推断工具，通过优化证据下界（ELBO）实现 token 级学习，鼓励多样化的推理轨迹。

### 2.2 关键技术细节
- **Token 级边际奖励近似**：针对长序列（~1k tokens）中逐 token 奖励计算不可行的问题，提出线性插值策略。设置分块长度 λ，仅每隔 λ 步计算真实奖励，中间步骤通过线性插值估计，大幅降低计算开销。对应损失函数为插值子轨迹平衡（L_ISubTB）。
- **参考引导的 GFlowNet 微调（RGFN）**：为了解决无约束探索导致的灾难性遗忘，引入参考推理链（由教师模型生成），通过指示函数过滤掉低奖励候选（R(Z_i) < δ_s R(Z_ref)），仅对“优于参考”的样本进行反向传播。δ_s 使用退火系数，初期允许更多探索，后期逐渐收紧。
- **贝叶斯推理扩展（BiN）**：推理时，从策略模型 qθ(Z|X) 采样 N 个潜在推理链，分别采样答案，然后计算每个答案的边际似然（长度归一化的联合似然均值），选择最高似然的答案作为最终输出。该方法无需外部奖励模型，具有概率可解释性。

## 3. 实验设计

### 3.1 数据集与基准
- 数学推理：MathVista、MathVision、MathVerse（Vision-only 子集）
- 通用多模态推理：MMMU、MMMU-pro、MMVet、MME
- 训练数据：混合 LLaVA-CoT 和 R1-Onevision 数据集中的 CoT 示例

### 3.2 对比方法
- 闭源模型：GPT-4o、Gemini-1.5-Pro、Claude-3.5-Sonnet
- 开源 LVLMs：Qwen2.5-VL（3B/7B）、InternVL2（4B/8B）、LLaVA-CoT-11B、LLaVA-OV-7B、MiniCPM-V2.6
- 训练基线：零样本、SFT、GRPO（R1-Onevision 方案）

## 4. 资源与算力
- **计算资源**：8 块 80GB GPU 节点，使用 DeepSpeed ZeRO-3 和梯度检查点。
- **训练时长**：
  - SFT：约 30 小时（250k 推理数据样本）
  - GRPO / RGFN：约 120 小时（3k 样本）
- 模型规模：Qwen2.5-VL 3B 和 7B，使用 LoRA（r=64, alpha=128）微调策略模型。

## 5. 实验数量与充分性
- **实验组数**：覆盖 7 个推理基准（表1），对比 10 余个模型；消融实验包括：
  - 训练算法对比（SFT vs GRPO vs RGFN，表3）
  - 推理扩展方法对比（BiN vs BoN，表2）
  - 超参数分析：分块长度 λ（表 T5）、候选数 N 和温度 T 的影响（图7）
  - BiN 在 SFT 模型上的通用性验证（表4）
- **充分性与公平性**：
  - 实验设计系统，包含多数据集、多模型尺度、多基线对比。
  - 消融实验覆盖方法核心组件（RGFN、BiN、插值策略）。
  - 推理扩展对比时确保无外部奖励模型，保证公平。
  - 未报告误差棒（因计算成本过高），可能影响结果确定性判断。

## 6. 主要结论与发现
- **LaCoT 显著提升性能**：7B 模型在 MathVista 上达 68.4%（提升 6.6%），优于 GRPO（10.6%）；3B 模型在 MathVerse Vision-only 上提升 14 个百分点，超越 LLaVA-CoT-11B 等更大模型。
- **RGFN 优于 SFT 与 GRPO**：表3显示 RGFN 在三项基准上均显著优于 SFT 和 GRPO，说明变分推断框架能有效避免奖励黑客和探索不足。
- **BiN 比 BoN 更优**：表2显示 BiN 在所有基准上一致优于 Best-of-N，且计算开销更低（无需额外奖励模型）。
- **多样性有助于性能**：图6显示 LaCoT 采样的推理链兼具高似然和高多样性，这是传统方法难以实现的。
- **推理扩展有效**：增加候选数 N 和温度 T 可系统提升准确率（图7），符合贝叶斯采样原理。

## 7. 优点
- **方法创新**：首次将 GFlowNets 的摊销变分推断引入视觉 CoT 训练，为推理提供概率建模框架。
- **实用性强**：提出的线性插值奖励近似解决了长序列训练效率问题，参考引导策略避免了灾难性遗忘。
- **推理扩展新颖**：BiN 替代 BoN，无需外部奖励模型，具有理论依据（边际似然），且计算开销更低。
- **实验充分**：覆盖数学/通用多模态推理，对比多个主流模型，消融实验完整。
- **代码开源**：提供 GitHub 仓库促进复现。

## 8. 不足与局限
- **模型规模限制**：实验仅到 7B 参数，未验证更大模型（如 70B）的泛化，但作者认为结论可推广。
- **计算资源需求**：RGFN 训练需要约 120 小时（8×80GB GPU），对一般研究者门槛较高。
- **未解决幻觉问题**：尽管推理性能提升，但模型仍可能产生错误推理（幻觉），与内部知识相关。
- **探索效率挑战**：作为 on-policy 方法，在复杂任务中探索仍受序列长度和内存限制。
- **未报告误差棒**：实验结果缺少统计显著性分析，可能影响结果稳健性判断。

（完）
