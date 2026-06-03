---
title: "Think Silently, Think Fast: Dynamic Latent Compression of LLM Reasoning Chains"
title_zh: 静默思考，快速推理：大语言模型推理链的动态潜在压缩
authors: "Wenhui Tan, Jiaze Li, Jianzhong Ju, Zhenbo Luo, Ruihua Song, Jian Luan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=AQsko3PPUe"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 推理链的潜在空间压缩以提高效率
tldr: 提出压缩潜在推理（CoLaR），动态压缩思维链推理以降低计算开销。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-aqsko3ppue/fig-001.webp\", \"caption\": \"\", \"page\": 22, \"index\": 1, \"width\": 789, \"height\": 286}]"
motivation: 基于token的思维链计算开销大，需要更高效的推理方式。
method: 在监督微调中引入辅助的下一个压缩嵌入预测目标，将连续token嵌入合并并预测压缩嵌入分布。
result: 在保持性能的同时显著减少了推理计算量。
conclusion: 潜在空间压缩可提升思维链推理效率。
---

## Abstract
Large Language Models (LLMs) achieve superior performance through Chain-of-Thought (CoT) reasoning, but these token-level reasoning chains are computationally expensive and inefficient.
In this paper, we introduce Compressed Latent Reasoning (CoLaR), a novel framework that dynamically compresses reasoning processes in latent space through a two-stage training approach.
First, during supervised fine-tuning, CoLaR extends beyond next-token prediction by incorporating an auxiliary next compressed embedding prediction objective. This process merges embeddings of consecutive tokens using a compression factor $c$ randomly sampled from a predefined range, and trains a specialized latent head to predict distributions of subsequent compressed embeddings. Second, we enhance CoLaR through reinforcement learning (RL) that leverages the latent head's non-deterministic nature to explore diverse reasoning paths and exploit more compact ones.
This approach enables CoLaR to: i) **perform reasoning at a dense latent level** (i.e., silently), substantially reducing reasoning chain length, and ii) **dynamically adjust reasoning speed** at inference time by simply prompting the desired compression factor.
Extensive experiments across four mathematical reasoning datasets demonstrate that CoLaR achieves 14.1% higher accuracy than latent-based baseline methods at comparable compression ratios, and reduces reasoning chain length by 53.3% with only 4.8% performance degradation compared to explicit CoT method. Moreover, when applied to more challenging mathematical reasoning tasks, our RL-enhanced CoLaR demonstrates performance gains of up to 5.4% while dramatically reducing latent reasoning chain length by 82.8%.
The code and models will be released upon acceptance.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大语言模型（LLM）通过思维链（CoT）推理取得了优异性能，但生成冗长的 token 级推理链计算开销巨大，在真实场景中（尤其高并发下）造成严重服务器负载，阻碍效率与可扩展性。
- **动机**：现有方法多聚焦于 token 级优化（如跳过冗余 token、生成更简洁步骤），仍受限于稀疏的 token 表示。潜在空间推理（如 Coconut、CODI）虽尝试将推理压缩到连续空间，但局限于固定长度推理链且采用确定性生成，缺乏探索-利用能力。
- **整体含义**：提出 **CoLaR（Compressed Latent Reasoning）**，实现动态、可调速的潜在空间推理，显著降低推理链长度，同时保持甚至提升准确性。

## 2. 方法论
### 核心思想
- 两阶段训练：**监督微调（SFT）** + **强化学习（RL）**。
- 在 SFT 阶段，引入**辅助的下一个压缩嵌入预测目标**：随机采样压缩因子 $c \in [1, c_{\text{max}}]$，将 $c$ 个连续推理 token 的嵌入合并为一个压缩嵌入，训练专门的**潜在头（Latent Head）** 预测后续压缩嵌入的分布。
- 在 RL 阶段，利用潜在头的概率性质（输出均值和标准差）采样多样推理路径，通过 **GRPO 算法** 探索正确路径并利用更短的路径。

### 关键技术细节
- **嵌入压缩模块（Embedding Compress）**：采用 $\frac{1}{\sqrt{c}}$ 缩放求和，避免均值池化带来的分布扭曲。
- **语言模型损失 $L_{\text{comp}}$**：从每组 $c$ 个 token 中随机采样一个作为标签，近似多标签分类。
- **潜在头损失 $L_{\text{latent}}$**：提供两种形式——NLL 损失（式 2）和 soft-MSE 损失（式 3，含熵正则项），后者对简单数据集更稳定。
- **推理阶段**：自回归预测压缩嵌入，由语言头决定何时终止推理。压缩因子可动态调整（测试时通过提示指定）。
- **RL 阶段**：GRPO 采用组级奖励归一化和逐 token 平均奖励，鼓励探索并压缩推理长度（如正确但更短的路径获得更高强化）。

### 公式/算法流程（文字说明）
1. SFT：输入 = [问题嵌入, 压缩推理嵌入, 答案嵌入]；随机选择 $c$，压缩 $c$ 个 token 嵌入；联合优化 $L_{\text{comp}}$ + $L_{\text{latent}}$。
2. RL：对同一问题采样 $G$ 个输出（潜在链 + 答案）；GRPO 基于准确率计算奖励并更新策略；去掉 KL 正则化项以加速训练。

## 3. 实验设计
### 数据集
- **主要训练集**：GSM8k-Aug（约 38.5 万训练样本，1k 测试）。
- **域内评估**：GSM8k（原始）。
- **域外泛化**：GSM-Hard、SVAMP、MultiArith（均为小学级数学推理）。
- **挑战性任务**：MATH（7.5k 训练，5k 测试，含代数、微积分等）。
- **扩展实验**：GPQA（化学、生物、物理）。

### 基准方法
- **CoT**（显式 CoT 微调）、**iCoT**（逐步消除推理步骤）、**Coconut**（6 步固定长度潜在推理）、**Distill**（复现 CODI，自蒸馏固定长度潜在推理）。
- **TokenSkip**（仅在 8B 模型对比）。

### 评估指标
- **准确率（Acc.）**、**推理链平均长度（#L）**。

## 4. 资源与算力
- **SFT 阶段**：8 张 A100 GPU，总 batch size 256。
- **RL 阶段**：单张 A100 GPU，rollout batch size 8，优化器 step batch size 4，组大小 $G=8$。
- **训练限制**：最多 50 个 epoch 或 12 小时（先到者为准）。
- **未明确说明**：具体总训练时长、单次实验 GPU 小时数。

## 5. 实验数量与充分性
- **充分性**：实验覆盖 4 个小学级数据集 + 1 个挑战级数据集 + 1 个域外数据集；对比 4 种基线（含显式 CoT、潜在方法）及 TokenSkip（8B 规模）。
- **消融实验**：4 种设置（确定性潜在头、移除压缩推理链监督、均值池化、NLL 损失），均报告了统计差异。
- **附加分析**：压缩因子动态训练 vs 固定训练、跨未见过压缩因子泛化、层激活差异、RL 训练曲线、模型规模扩展（1B/3B/8B）。
- **客观性**：所有结果以 5 次独立运行的平均值 ± 95% 置信区间报告，统计严谨。

## 6. 主要结论与发现
- **主要结果**：CoLaR 在可比压缩率下准确率比现有潜在基线（Coconut）高 **14.1%**，推理链长度减少 **53.3%**（性能仅下降 4.8%）。
- **挑战任务强化**：在 MATH 数据集上，RL 后准确率提升 **5.36%**，推理链长度减少 **82.8%**。
- **缩放性**：从 1B 到 8B 模型，CoLaR 保持压缩增益且 RL 效果持续（如 GPQA 超越 CoT 教师）。
- **动态压缩有效性**：训练时使用随机 $c$ 优于固定 $c$；对未见过压缩因子具备插值泛化能力。
- **RL 设计关键**：逐 token 平均奖励能有效促使模型探索正确路径并利用更短的路径。

## 7. 优点
- **创新性**：首次在潜在空间实现动态压缩因子支持，且结合概率潜在头与 RL 进行探索-利用。
- **效率显著**：推理链长度大幅下降，计算开销降低明显。
- **可解释性**：通过余弦相似度检索压缩嵌入对应的 top-5 token，可直观解释潜在推理过程（如案例中正确揭示计算步骤）。
- **鲁棒性**：跨域泛化能力强（MultiArith 上几乎无性能损失），RL 训练曲线展现清晰的三阶段模式（探索→利用→过拟合）。

## 8. 不足与局限
- **性能上限**：除 GPQA 外，CoLaR 整体准确率接近但未超越显式 CoT 教师（性能差距约 4.8%）。
- **压缩因子限制**：无法泛化到非整数压缩因子（如 c=1.5）或超出训练最大值的因子，归因于 LLM 离散 token 化约束。
- **潜在偏差风险**：可能放大已有偏见或被用于生成更令人信服的错误信息，建议对下游应用谨慎监控。
- **实验覆盖**：目前仅聚焦数学推理，未扩展到更通用的常识推理或 NLP 任务；RL 阶段仅在 MATH 和 GPQA 上验证，泛化性待进一步考查。

（完）
