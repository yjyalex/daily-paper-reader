---
title: "System-1.5 Reasoning: Traversal in Language and Latent Spaces with Dynamic Shortcuts"
title_zh: System-1.5推理：语言和潜在空间中的动态捷径遍历
authors: "Xiaoqiang Wang, Suyuchen Wang, Yun Zhu, Bang Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=MNduv07wAu"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 自适应思维链推理框架
tldr: 提出通过潜在空间捷径动态分配计算的自适应思维链推理以提升效率。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-mnduv07wau/fig-001.webp\", \"caption\": \"\", \"page\": 8, \"index\": 1, \"width\": 677, \"height\": 471}]"
motivation: 现有思维链和潜在空间推理缺乏自适应计算分配，导致效率低下。
method: 设计自适应推理框架，在潜在空间中为推理步骤选择捷径路径。
result: 在多个推理任务上实现了更高的效率和相当的准确性。
conclusion: 动态计算分配是提升思维链推理效率的关键。
---

## Abstract
Chain-of-thought (CoT) reasoning enables large language models (LLMs) to move beyond fast System-1 responses and engage in deliberative System-2 reasoning. However, this comes at the cost of significant inefficiency due to verbose intermediate output. Recent latent-space reasoning methods improve efficiency by operating on hidden states without decoding into language, yet they treat all steps uniformly, failing to distinguish critical deductions from auxiliary steps and resulting in suboptimal use of computational resources. In this paper, we propose System-1.5 Reasoning, an adaptive reasoning framework that dynamically allocates computation across reasoning steps through shortcut paths in latent space.Specifically, System-1.5 Reasoning introduces two types of dynamic shortcuts. The model depth shortcut (DS) adaptively reasons along the vertical depth by early exiting non-critical tokens through lightweight adapter branches, while allowing critical tokens to continue through deeper Transformer layers. The step shortcut (SS) reuses hidden states across the decoding steps to skip trivial steps and reason horizontally in latent space. Training System-1.5 Reasoning involves a two-stage self-distillation process: first distilling natural language CoT into latent-space continuous thought, and then distilling full-path System-2 latent reasoning into adaptive shortcut paths (System-1.5 Reasoning).Experiments on reasoning tasks demonstrate the superior performance of our method.
For example, on GSM8K, System-1.5 Reasoning achieves reasoning performance comparable to traditional CoT fine-tuning methods while accelerating inference by over 20× and reducing token generation by 91.0\% on average.

---

## 论文详细总结（自动生成）

# 论文总结：System-1.5 Reasoning: Traversal in Language and Latent Spaces with Dynamic Shortcuts

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：大型语言模型（LLMs）通过链式思维（Chain-of-Thought，CoT）实现从快速启发式的 System-1 推理到深思熟虑的 System-2 推理的转变，显著提升了复杂推理能力。然而，CoT 需要生成冗长的中间步骤，导致推理效率低下，尤其对于简单问题也存在“过度思考”现象。
- **现有方法局限**：近期提出的潜在空间推理方法（如 Coconut、CCoT、pause token 等）通过压缩或延迟语言输出，在一定程度上提升了效率，但它们对所有推理步骤一视同仁，无法区分关键推理步骤与辅助步骤，导致计算资源分配不优。
- **研究动机**：能否动态地根据推理步骤的复杂度分配计算资源——简单步骤快速处理（System-1），复杂步骤细致推理（System-2），而无关步骤直接跳过？从而实现效率与性能的最优平衡。

## 2. 方法论：核心思想、关键技术细节

### 2.1 核心思想
提出 **System-1.5 Reasoning**，一种在潜在空间中通过动态捷径（Dynamic Shortcuts）自适应分配计算资源的推理框架。框架引入两类动态捷径：
- **深度捷径（Depth Shortcut, DS）**：在 Transformer 层维度上自适应调整计算深度。非关键令牌（非关键推理步骤）通过轻量适配器分支提前退出，关键令牌则继续通过深层 Transformer 层。
- **步长捷径（Step Shortcut, SS）**：在解码步长维度上重用隐藏状态，允许跳过无关紧要的步骤，实现横向潜在空间推理。

### 2.2 关键技术细节
- **动态捷径架构**：
  - 每个 Transformer 层增加一个路由器-适配器模块（Router-Adapter）。路由器输出权重 \(w = R_l(h_{l-1,t})\)，训练时输出为适配器输出与 Transformer 层输出的加权组合（公式1-2）；推理时若 \(w\) 超过阈值 \(\lambda_{\text{depth}}\) 则提前退出（公式3）。
  - 步长捷径：将前一步的隐藏状态直接复制到当前步同一层，训练时也采用加权组合（公式4）。结合两者得到最终隐藏状态（公式5）。
- **训练过程**：两阶段自蒸馏：
  1. **语言到潜在空间对齐**：教师模型（System-2 教师，CoT 微调）的最后层隐藏状态作为特征，学生模型（System-2 学生）学习在潜在空间中推理，通过均方误差（MSE）损失对齐。同时教师模型用 NLL 损失监督 CoT 和学生模型用 NLL 损失监督最终答案。
  2. **捷径学习（Shortcut Learning）**：冻结学生模型参数，仅训练路由器-适配器模块。利用原子思维分解（Atom-of-Thought）将 CoT 分解为有向无环图（DAG），标记每个步骤为关键（critical）或非关键（non-critical）。应用早期退出损失（公式10-11），鼓励非关键步骤在浅层退出，关键步骤继续深层处理，同时保持与完整路径隐藏状态的一致性。

## 3. 实验设计

### 3.1 数据集与场景
- **数学推理**：GSM8K（增强版，约40万条数据）用于训练和测试；GSM-HARD（数值更复杂）用于域外泛化测试。
- **常识推理**：StrategyQA（约2780个样例，需多跳推理，答案为 Yes/No）。

### 3.2 Benchmark 与对比方法
- **基准**：标准 CoT 微调（CoT fine-tuning）。
- **对比方法**（六种）：
  - 语言空间条件计算：LITE、LayerSkip（早期退出机制）
  - 潜在空间压缩推理：iCoT、Coconut、CODI
  - 潜在空间扩展推理：pause token

### 3.3 评估指标
- 准确率（Exact match）
- 解码步数（平均 token 生成数量）
- 计算成本（每步 FLOPs 减少率）
- 整体推理加速比（实际推理时间）

## 4. 资源与算力
- **硬件**：单张 NVIDIA RTX A5000（24 GB）GPU。
- **训练时间**：对于 LLaMA 3.2 1B 模型，训练 8 个 epoch 约需 26 小时；对于 GPT-2（124M）约需 5 小时。
- **超参数**：优化器 AdamW（lr=2e-5, β1=0.9, β2=0.99），批量大小 2，学习率预热 6% 步数。
- 论文说明了硬件资源，但未给出多 GPU 扩展或更大型模型的资源需求。

## 5. 实验数量与充分性
- **主要结果**（表1）：在 GSM8K、GSM-HARD、StrategyQA 三个数据集上比较了七种方法（含自身），报告了准确率、解码步数、FLOPs 减少率和推理加速比。
- **消融实验**（图3）：
  - 替换不同 System-2 学生（Coconut 或 CODI 蒸馏的潜在推理模型）对 System-1.5 性能的影响。
  - 比较联合学习（两阶段同时进行）和全参数微调（不冻结 Transformer 参数）的效果。
- **测试时缩放分析**（图4）：调节深度退出阈值 \(\lambda_{\text{depth}}\) 和潜在推理步数常数 \(\lambda_{\text{step}}\) 对性能的影响，展示了可控的测试时计算缩放能力。
- **公平性**：在相同骨干模型（GPT-2 124M 或 LLaMA 3.2 1B）下与各基线公平对比；每个实验运行4次取平均，保证统计稳定性。
- 实验覆盖了效率与准确率的多个维度，消融实验验证了设计选择，总体充分且客观。

## 6. 主要结论与发现
- **效率大幅提升**：在 GSM8K 上，System-1.5 Reasoning 相比 CoT 实现 **20.27× 推理加速**，中间 token 减少 **91.0%**；在 StrategyQA 上加速比达 **55.65×**。
- **性能接近或超越 CoT**：在数学推理任务上准确率与 CoT 持平（GSM8K 46.94% vs 46.94%），在常识推理任务上超越 CoT（48.61% vs 47.62%）。
- **动态捷径优于统一计算**：模型能够根据步骤关键性自适应分配计算，训练策略上采用两阶段蒸馏优于联合学习或全参数微调。
- **可控制的测试时计算缩放**：通过调节深度阈值和潜在推理步数，可以灵活地在性能和计算之间权衡。

## 7. 优点
- **创新性**：首次在潜在空间中同时引入垂直（层深度）和水平（解码步长）的动态计算分配，更贴近人类“快慢思考”模式。
- **高效性**：在保持相当准确率的同时，实现数量级的推理加速，对实际部署具有重大意义。
- **系统性**：两阶段训练策略清晰，从语言 CoT 蒸馏到潜在空间再到捷径学习，过程可复现。
- **可控性**：支持测试时计算缩放，适应不同资源约束场景。

## 8. 不足与局限
- **可解释性不足**：潜在空间推理缺乏显式中间步骤，难以理解、分析或验证模型的内部逻辑，在高风险场景存在安全隐患。
- **评估范围有限**：仅在 GSM8K、GSM-HARD、StrategyQA 三个中等规模基准上测试，未在更大模型（如 7B/13B）或更广泛任务（如代码、科学推理）上验证。
- **计算资源单一**：仅使用单张 GPU，未报告多卡或更高效能下的可扩展性。
- **偏差风险**：依赖原子思维分解标记步骤关键性，若标注不准确可能影响训练；模型仅基于 supervised fine-tuning，未与强化学习等方法对比。

（完）
