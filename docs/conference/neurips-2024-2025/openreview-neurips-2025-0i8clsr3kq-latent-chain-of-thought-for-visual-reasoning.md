---
title: Latent Chain-of-Thought for Visual Reasoning
title_zh: 视觉推理中的潜在思维链
authors: "Guohao Sun, Hang Hua, Jian Wang, Jiebo Luo, Sohail Dianat, MAJID RABBANI, Raghuveer Rao, Zhiqiang Tao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=0i8ClSr3kQ"
tags: ["query:token"]
score: 8.0
evidence: token级强化学习与稀疏奖励用于潜在思维链
tldr: 提出token级稀疏奖励的强化学习方法用于视觉推理中的潜在思维链
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 320, \"height\": 562}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-003.webp\", \"caption\": \"\", \"page\": 10, \"index\": 3, \"width\": 721, \"height\": 458}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-004.webp\", \"caption\": \"\", \"page\": 15, \"index\": 4, \"width\": 424, \"height\": 591}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-0i8clsr3kq/fig-005.webp\", \"caption\": \"\", \"page\": 17, \"index\": 5, \"width\": 800, \"height\": 557}]"
motivation: 现有训练算法在未见推理任务上泛化差且依赖有偏奖励模型。
method: 将推理视为后验推断，使用变分推断和多样性追求强化学习，设计稀疏奖励函数提供token级学习信号。
result: 该方法有效提升视觉推理性能，避免奖励黑客。
conclusion: 潜在思维链结合token级强化学习可提升视觉推理的泛化和可靠性。
---

## Abstract
Chain-of-thought (CoT) reasoning is critical for improving the interpretability and reliability of Large Vision-Language Models (LVLMs). However, existing training algorithms such as SFT, PPO, and GRPO may not generalize well across unseen reasoning tasks and heavily rely on a biased reward model. To address this challenge, we reformulate reasoning in LVLMs as posterior inference and propose a scalable training algorithm based on amortized variational inference. By leveraging diversity-seeking reinforcement learning algorithms, we introduce a novel sparse reward function for token-level learning signals that encourage diverse, high-likelihood latent CoT, overcoming deterministic sampling limitations and avoiding reward hacking. Additionally, we implement a Bayesian inference-scaling strategy that replaces costly Best-of-N and Beam Search with a marginal likelihood to efficiently rank optimal rationales and answers. We empirically demonstrate that the proposed method enhances the state-of-the-art LVLMs on four reasoning benchmarks, in terms of effectiveness, generalization, and interpretability.

---

## 论文详细总结（自动生成）

# 论文《Latent Chain-of-Thought for Visual Reasoning》中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：大型视觉-语言模型（LVLMs）在视觉推理任务中，需要生成显式、逐步的思维链（Chain-of-Thought, CoT）以提升可解释性和可靠性。然而，现有的训练算法（如监督微调 SFT、PPO、GRPO）在未见推理任务上泛化能力差，且依赖有偏的奖励模型，容易导致reward hacking（奖励黑客）。
- **动机**：将视觉推理重新形式化为后验推断问题，通过变分推断（Variational Inference）来学习潜在CoT，从而更好地捕捉不确定性、生成多样化推理轨迹，并避免对离散监督或偏置奖励的依赖。
- **意义**：提出一种可扩展的、具有理论依据的算法，使LVLMs能够学习采样多样且高似然的潜在思维链，提升推理的有效性、泛化性和可解释性。

## 2. 提出的方法论

### 核心思想
- 将视觉推理视为**潜在变量模型**，将CoT序列 \( Z \) 视为从后验分布 \( P(Z|X,Y) \) 中采样的潜在变量（\( X \) 为视觉指令，\( Y \) 为答案）。通过**变分推断**（AVI）训练一个策略模型 \( q_\theta(Z|X) \) 来近似该后验。
- 使用**生成流网络（GFlowNets）** 的目标函数（如子轨迹平衡 subTB）进行训练，确保生成轨迹的概率与奖励成正比。

### 关键技术细节
1. **Token级边际奖励近似**：
   - 由于长CoT序列（约1000 token）无法直接计算每个token的奖励，提出**线性插值**策略：每 \( \lambda \) 步计算一次真实奖励，中间步骤通过线性插值估计（见图中分段线性近似），大幅降低计算开销。
   - 理论上证明当 \( \lambda \) 足够小时，插值误差可控（Proposition 1）。

2. **参考引导的GFlowNet微调（RGFN）**：
   - 为解决无约束探索导致的灾难性遗忘问题，引入**参考引导**机制：探索 \( m \) 个候选轨迹，与参考轨迹 \( Z_{\text{ref}} \)（来自老师模型如GPT-4o或Deepseek-R1）比较，通过指示函数 \( I(Z_i) \) 过滤掉低奖励（劣于参考）的样本，只对“优于参考”的轨迹进行梯度更新。
   - 使用退火系数 \( \delta_s \)，前期允许更多探索，后期提高标准。
   - 最终损失为 \( L_{\text{RGFN}}(Z_i;\theta) = \sum_i I(Z_i) \cdot L_{\text{ISubTB}}(Z_i;\theta) \)。

3. **贝叶斯推理缩放（BiN）**：
   - 在推理时替代昂贵的Best-of-N或Beam Search，采用概率化的边际似然估计。
   - 流程：从 \( q_\theta(Z|X) \) 采样 \( N \) 个潜在CoT → 对每个CoT用答案模型 \( \pi_\Phi \) 生成答案 → 计算每个答案的**长度归一化边际似然** \( P(Y_i|X) \propto \frac{1}{N}\sum_j \frac{1}{|Z_iY_i|} \pi_\Phi(Z_iY_i|X) \) → 选择边际似然最高的答案作为输出。
   - 优点：无需额外奖励模型，具有贝叶斯统计依据，且能平滑波动、扩大候选集。

## 3. 实验设计

### 使用的数据集/基准（Benchmark）
- **MathVista**：融合数学与视觉任务，需细粒度视觉理解和组合推理。
- **MathVision**：3040道来自真实数学竞赛的高质量数学问题，包含复杂视觉上下文。
- **MathVerse**：视觉数学基准，报告Vision-Only结果（788题）。
- **MMMU**：多学科大学级知识推理。
- **MMMU-Pro**：更鲁棒的MMMU版本。
- **MMVet / MME**：额外视觉任务通用能力评估。

### 对比方法
- **基线**：Qwen2.5-VL 3B/7B（零样本、SFT、GRPO）、LLaVA-CoT-11B、LLaVA-OV-7B、InternVL2-4B/8B、MiniCPM-V2.6、R1-Onevision等。
- **推理缩放对比**：Best-of-N（BoN） vs. 提出的BiN。
- **消融实验**：零样本 vs. SFT vs. GRPO vs. RGFN，以及不同候选数 \( N \) 和温度 \( T \) 的影响。

### 实验数量与充分性
- **主实验**：在6个基准（MathVista、MathVision、MathVerse、MMMU、MMMU-Pro、MMVet、MME）上报告了3B和7B模型的准确率（表1）。
- **推理缩放对比**：表2比较了BoN与BiN在MathVerse、MathVista、MMMU、MMVet上的表现。
- **消融实验**：
  - 表3：RGFN vs. SFT vs. GRPO vs. Zero-shot。
  - 图7：不同候选数 \( N \)（1,5,10）与温度 \( T \)（0.5, 0.7, greedy）的影响。
  - 图6：SFT与LaCoT在多样性-似然权衡上的对比。
  - 表4：BiN在SFT模型上的泛化能力。
  - 表T5：插值步长 \( \lambda \) 的敏感性分析。
- **定性结果**：图8、F9、F10、F11展示具体推理案例，对比GRPO、SFT与LaCoT的推理质量。
- **结论**：实验设计全面，覆盖多个基准、多种基线和消融变量，数据充分且客观。

## 4. 资源与算力

- 论文明确说明：
  - 使用 **8×80GB GPU-node** 进行训练。
  - 设置 **Deepspeed Zero-3 stage** 和 **gradient-checkpointing** 以降低显存。
  - 训练时间：SFT约30小时（250k数据），GRPO和RGFN约120小时（3k数据）。
  - 模型规模：Qwen2.5-VL 3B和7B，使用LoRA（r=64, alpha=128）进行RGFN微调。
- 计算资源描述清晰，可复现。

## 5. 实验数量与充分性评价

- **充分性**：实验覆盖了数学推理、通用视觉推理等多个维度；对比了主流的SFT、RL（GRPO）和更大模型（11B）；进行了消融、敏感性、推理缩放、定性分析。
- **公平性**：基线与提出方法均基于相同的骨干模型（Qwen2.5-VL）进行训练；推理缩放对比中不使用外部奖励模型，公平比较。
- **潜在的不足**：未给出多次运行的误差棒（因计算成本），但通过不同温度/候选数展示了变异性。总体实验设计客观、系统。

## 6. 主要结论与发现

1. **LaCoT模型（3B/7B）在所有七个推理基准上均显著优于SFT和GRPO基线**，且7B模型平均提升6.6%，3B模型提升13.9%，甚至超越更大的模型（如LLaVA-CoT-11B）。
2. **RGFN相比GRPO避免了reward hacking**，通过变分推断和参考引导探索覆盖了更广的后验分布，生成更高质量的推理轨迹。
3. **BiN推理缩放策略在效率与性能上均优于BoN**，且无需额外奖励模型；边际似然选择机制提高了统计鲁棒性。
4. **增加推理时候选数 \( N \) 和温度 \( T \) 能持续提升准确率**，验证了更广泛的覆盖有助于找到正确答案，且减少幻觉。
5. **BiN可通用应用于任意SFT模型**，提升其推理性能（表4）。

## 7. 优点

- **理论扎实**：将推理建模为潜在变量后验推断，使用GFlowNets进行变分推断，具有严格的概率学基础。
- **创新贡献**：
  - 提出token级稀疏奖励近似（线性插值），解决长序列下的计算问题。
  - 参考引导的GFlowNet（RGFN）避免灾难性遗忘并提升探索效率。
  - 贝叶斯推理缩放（BiN）提供统计稳健的答案选择，无需外部奖励模型。
- **实验充分**：多个基准、多种对比、消融实验、定性分析，证明方法有效性、泛化性和可解释性。
- **开源代码**：提供代码仓库，利于复现。
- **模型大小灵活**：3B模型即能超越更大模型，证明方法的轻量高效。

## 8. 不足与局限

- **模型规模限制**：仅测试到7B参数，更大型号（如70B+）的效果未知（论文提及受限于资源，但预期结论可推广）。
- **计算成本**：训练需多卡长时间（120小时 on-policy），且推理时多候选采样会增加延迟，虽可通过批处理缓解但未深入优化。
- **泛化性边界**：MathVision数据集上的部分子项表现不如某些基线，表明在极端复杂推理场景（如竞赛级别）仍有挑战。
- **幻觉问题**：论文指出未解决幻觉问题，可能与内部知识相关，是未来工作方向。
- **实验变异性**：未报告多次运行的标准差或误差棒，可能影响对结果稳定性的判断（但论文通过不同温度与候选数展示了趋势）。
- **可复现性细节**：虽然提供了超参数，但训练数据构造中使用了外部老师模型（GPT-4o/Deepseek-R1）生成参考CoT，若这些模型关闭可能影响复现。

（完）
