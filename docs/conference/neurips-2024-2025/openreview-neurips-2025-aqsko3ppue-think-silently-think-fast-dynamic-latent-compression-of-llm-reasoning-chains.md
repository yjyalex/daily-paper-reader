---
title: "Think Silently, Think Fast: Dynamic Latent Compression of LLM Reasoning Chains"
title_zh: 无声思考，快速思考：LLM推理链的动态潜在压缩
authors: "Wenhui Tan, Jiaze Li, Jianzhong Ju, Zhenbo Luo, Ruihua Song, Jian Luan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=AQsko3PPUe"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 压缩思维链推理链以提高效率
tldr: CoLaR在潜在空间中动态压缩LLM推理链以减少计算。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-aqsko3ppue/fig-001.webp\", \"caption\": \"\", \"page\": 22, \"index\": 1, \"width\": 789, \"height\": 286}]"
motivation: 思维链推理的计算成本高昂，需要更高效的推理方法。
method: 提出CoLaR框架，通过监督微调中的压缩嵌入预测目标动态合并词元嵌入。
result: 实现了推理效率的提升，同时保持性能。
conclusion: 动态潜在压缩是一种有效的CoT推理加速方法。
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

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLM）通过思维链（Chain-of-Thought, CoT）推理显著提升了数学等任务的性能，但CoT在词元（token）级别生成冗长的推理链，导致计算成本高昂、效率低下。尤其在真实部署中，高并发场景下推理链的延长会严重增加服务器负载。
- **整体含义**：针对此问题，论文提出一种动态潜在压缩推理框架**CoLaR**（Compressed Latent Reasoning），旨在将显式的、逐词元的推理过程压缩到稠密的潜在空间中，以“无声思考”的方式在保持或接近原有效果的同时大幅缩短推理链长度，并允许在推理时动态调整推理速度。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过两阶段训练（监督微调SFT + 强化学习RL），让LLM学会在潜在空间中自动回归地预测压缩后的嵌入向量（latent variables），每个潜在变量编码多个连续推理词元的语义信息，从而实现推理链的压缩。
- **关键技术细节**：
  1. **嵌入压缩模块（Embedding Compress）**：在每个训练步，从范围[1, c_max]中随机采样压缩因子$c$，将连续$c$个推理词元的嵌入向量合并为一个压缩嵌入。合并方式采用$\frac{1}{\sqrt{c}}$缩放的和（而非平均池化），以避免分布偏移。
  2. **辅助下一个压缩嵌入预测目标**：除了通常的语言建模损失（预测压缩后的推理词元和答案），还训练一个**潜在头（Latent Head）**——一个两层的MLP——来预测下一个压缩嵌入的概率分布（均值和标准差）。训练损失为负对数似然（NLL）或软MSE损失（结合MSE与熵正则化项）。
  3. **动态推理**：在推理时，通过提示压缩因子$c$（例如“Let's think 2×faster”），CoLaR可自动回归地产生不同长度的潜在推理链，并通过语言头自动决定何时终止。
  4. **强化学习增强（GRPO）**：利用潜在头的非确定性（概率输出）以采样多样化的推理路径，再通过组相对策略优化（GRPO）算法，对正确且更短的推理链给予更高奖励（奖励按词元平均，激励压缩），从而在探索正确路径的同时开发更紧凑的推理链。

- **公式与算法流程**（文字说明）：
  - SFT阶段总损失：$\mathcal{L}_{\text{comp}} + \mathcal{L}_{\text{latent}}$。$\mathcal{L}_{\text{comp}}$为交叉熵，用于预测压缩后推理词元及答案；$\mathcal{L}_{\text{latent}}$为潜在嵌入预测的NLL或软MSE。
  - RL阶段采用GRPO：对同一问题采样$G$个输出（包含潜在推理链和预测答案），计算组内归一化优势$A_i$，用clip后的策略梯度优化，无KL正则化（遵循DAPO）。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - 主要训练和评估：**GSM8k-Aug**（约385k训练样本，1k测试样本）。
  - 分布外泛化评估：**GSM-Hard**（数字更大）、**SVAMP**（1k测试）、**MultiArith**（600测试）。
  - 挑战性评估：**MATH**（7.5k训练，5k测试，含代数、几何等）。
  - 额外扩展：**MATH-500**和**GPQA**（科学问答）。

- **评价指标**：推理准确率（Acc.）和平均推理链长度（#L，词元/潜在数量）。

- **对比方法**：
  - **CoT**：基于完整词元链的监督微调。
  - **iCoT**：逐步移除推理步骤的内部化方法。
  - **Coconut**：渐进替换词元推理为固定6步潜在推理。
  - **Distill**（重现CODI）：自蒸馏固定长度潜在推理。
  - **TokenSkip**（仅在1B/8B对比中）：词元跳过压缩。
  - **CoLaR变种**：CoLaR-DL（确定性头）、CoLaR-OC（移除压缩推理词元监督）、CoLaR-MP（平均池化）、CoLaR-NLL（NLL损失）、CoLaR-RL（强化学习后训练）。

- **基础模型**：Llama-3.2-1B-Instruct（冻结骨干+可训练LoRA），DeepSeek-R1-Distill-Qwen-1.5B，以及Llama-3-8B（扩展实验）。

### 4. 资源与算力

- 文中明确提到：
  - SFT阶段使用**8块A100 GPU**，总批次大小256。
  - RL阶段使用**1块A100 GPU**，批次大小8，组大小$G=8$。
  - 训练轮次：最多50个epoch或12小时（取先到者）。
  - 优化器：AdamW（SFT lr=1e-4，RL lr=1e-6）。
- 未说明总GPU小时数，但提供了硬件配置和训练约束。

### 5. 实验数量与充分性

- **实验组数**（按主要结果统计）：
  - 表1：在4个数据集上与5个基线对比，含5次随机种子取平均和95%置信区间（共约20+组实验）。
  - 表1灰色部分：4种消融实验（DL、OC、MP、NLL）在2个压缩因子下的结果。
  - 表2：在2个基础模型上对比CoT、CoLaR-DL、CoLaR-NLL、CoLaR-RL，含RL消融（w/o average）。
  - 表3：在1B/8B规模上对比TokenSkip。
  - 图4、图5：动态压缩因子分析（不同训练/测试c组合）。
  - 附录：曲线、层分析等。
- **充分性评价**：实验设计较全面，覆盖了性能、效率、消融、泛化、可扩展性、RL效果等多个维度，统计报告了置信区间，对比公平（初始化一致、Checkpoint选择方式相同）。消融实验证明了各组件必要性。

### 6. 论文的主要结论与发现

- **主要结论**：
  1. CoLaR在压缩比相近时比Coconut等潜在方法准确率高**14.1%**。
  2. 相比显式CoT，CoLaR可缩短推理链**53.3%**，仅损失**4.8%**精度。
  3. 强化学习后在MATH上进一步提升准确率**5.36%**，同时压缩链长**82.8%**。
  4. 动态压缩训练使模型能泛化到未见过的压缩因子，并展示出插值能力。
- **关键发现**：
  - 潜在头的概率输出对探索正确路径至关重要，确定性头在困难任务上效果差。
  - 训练时随机采样压缩因子提供互补增益，优于单一因子训练。
  - RL过程中初期探索（链变长-精度上升），后期压缩（链变短-精度稳定）。
  - 基础模型质量影响RL收益（Qwen-1.5B优于Llama-1B）。
  - 在GPQA上，CoLaR-8B-RL甚至超越CoT教师模型（37.5% vs 35.7%）。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  1. 提出用概率潜在头预测压缩嵌入，同时支持动态推理速度和RL探索-利用。
  2. 嵌入压缩采用$\frac{1}{\sqrt{c}}$缩放和，避免分布偏移，并搭配随机token采样监督，实现密集监督。
  3. 在RL中引入每token平均奖励，促进模型压缩推理链。
- **实验设计亮点**：
  1. 在5种不同规模数据集上测试，涵盖简单、中等、挑战性和领域外任务。
  2. 消融实验全面（确定性头、有无压缩监督、池化方式、不同损失函数）。
  3. 对压缩因子动态范围进行系统分析（训练/测试c的不同组合）。
  4. 提供案例研究（图3）展示潜在变量与词元的可解释性。
  5. 扩展至1B/8B模型验证可扩展性。

### 8. 不足与局限

- **性能未全面超越CoT**：在大多数基准上CoLaR精度仍低于显式CoT（虽链长大减），仅GPQA上超越。
- **压缩因子限制**：模型无法泛化到非整数压缩因子（如c=1.5）或大于训练最大c_max的值，受离散词元表示约束。
- **分布外泛化仍有差距**：在SVAMP等简单数据集上虽优于其他潜在方法，但与CoT差距明显（平均约5%）。
- **计算开销**：RL训练阶段需要多次采样，可能增加前期训练成本，且文中未比较训练总时间。
- **偏差与安全风险**：文中提及可能放大已有偏见或被用于生成虚假信息，但未给出具体评测或缓解方案。
- **实验覆盖**：仅限数学推理，未验证通用NLP或代码生成等场景。强化学习部分仅基于简单的二元奖励（正确/错误）。

（完）
