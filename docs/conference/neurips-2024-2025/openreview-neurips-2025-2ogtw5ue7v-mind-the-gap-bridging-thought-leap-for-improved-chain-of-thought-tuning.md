---
title: "Mind the Gap: Bridging Thought Leap for Improved Chain-of-Thought Tuning"
title_zh: 弥合思维跳跃：改进思维链调优
authors: "Haolei Xu, Yuchen Yan, Yongliang Shen, Wenqi Zhang, Guiyang Hou, Shengpei Jiang, Kaitao Song, Weiming Lu, Jun Xiao, Yueting Zhuang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=2ogTw5ue7v"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 检测并填补思维链中缺失的步骤（思维跳跃）以提高忠实性
tldr: 提出任务来弥合思维链数据中的思维跳跃，提高完整性和连贯性
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 325, \"height\": 492}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 1164, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 1240, \"height\": 1754}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1968, \"height\": 495}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 1968, \"height\": 495}]"
motivation: 现有数学思维链数据集常因专家省略中间步骤而存在思维跳跃，影响模型学习和泛化。
method: 构建CoT-Bridge模型，自动检测跳跃并生成缺失的推理步骤。
result: 在数学推理基准上显著提升了思维链的完整性和模型性能。
conclusion: 填补思维跳跃是提高思维链忠实性的有效方法。
---

## Abstract
Large language models (LLMs) have achieved remarkable progress on mathematical tasks through Chain-of-Thought (CoT) reasoning. However, existing mathematical CoT datasets often suffer from **Thought Leaps** due to experts omitting intermediate steps, which negatively impacts model learning and generalization. We propose the CoT Thought Leap Bridge Task, which aims to automatically detect leaps and generate missing intermediate reasoning steps to restore the completeness and coherence of CoT. To facilitate this, we constructed a specialized training dataset called **ScaleQM+**, based on the structured ScaleQuestMath dataset, and trained **CoT-Bridge** to bridge thought leaps. Through comprehensive experiments on mathematical reasoning benchmarks, we demonstrate that models fine-tuned on bridged datasets consistently outperform those trained on original datasets, with improvements of up to +5.87\% on NuminaMath. Our approach effectively enhances distilled data (+3.02\%) and provides better starting points for reinforcement learning (+3.1\%), functioning as a plug-and-play module compatible with existing optimization techniques. Furthermore, CoT-Bridge demonstrates improved generalization to out-of-domain logical reasoning tasks, confirming that enhancing reasoning completeness yields broadly applicable benefits.

---

## 论文详细总结（自动生成）

## 论文详细总结

### 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型（LLMs）通过思维链（Chain-of-Thought, CoT）在数学推理任务上取得了显著进展。然而，现有数学 CoT 数据集普遍存在**思维跳跃（Thought Leap）** 现象——即专家在编写推理步骤时，常因自身背景知识省略中间步骤，导致相邻推理步骤之间存在认知鸿沟。这种不完整性会严重影响模型的学习效果和泛化能力。
- **背景与问题**：与事实性错误或答案不准确不同，思维跳跃专门针对推理结构的**完整性**。初步实验表明，在 MetaMathQA 数据集中系统性引入不同严重程度的步骤省略后，模型性能下降高达 27.83%（见附录 C）。因此，自动识别并填补这些跳跃，对提升 CoT 质量至关重要。

### 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **CoT 思维跳跃桥接任务（CoT Thought Leap Bridge Task）**，包括两个子任务：  
  1. **检测跳跃位置**：定位 CoT 中哪些相邻步骤之间存在缺失的中间步骤。  
  2. **生成缺失步骤**：为每个跳跃位置生成恰当的中间推理步骤序列，使推理链恢复连贯与完整。
- **关键技术细节**：
  - **形式化定义**：给定一个完整的 CoT \( C^* = (Q, s^*_1, \dots, s^*_m) \)，若相邻步骤对 \((s_k, s_{k+1})\) 不满足完整性函数 \( V \)（即跳跃），则存在缺失步骤序列 \( S'_{\text{miss}} \)。
  - **数据集构建**：基于结构完整的 ScaleQuestMath 数据集，通过系统删除中间步骤生成不完整链，形成训练数据集 **ScaleQM+**。删除策略：保留答案步骤；初期步骤可被删除以训练宏观规划；删除数量随链长变化（\( m \le 10 \) 时删 1-2 步，\( m > 10 \) 时删 1-3 步）；20% 概率保留完整链，以训练模型识别无需桥接的情况。
  - **模型训练**：以 **Qwen2.5-Math-7B** 为基座，通过标准指令微调训练 **CoT-Bridge** 模型，学习从输入的不完整链到跳跃位置和缺失步骤的映射。共 588k 训练样本，10k 测试样本。训练 1 个 epoch，全局 batch size 1024。
  - **数据增强应用**：将训练好的 CoT-Bridge 应用于现有数学推理数据集（如 MetaMathQA、NuminaMath-CoT），生成桥接版本（MetaMath-Bridge、NuminaMath-Bridge）。桥接时，对每个检测到的跳跃位置，插入生成的中间步骤。
  - **对照变体**：**CoT-Bridge-Random**——在随机位置生成插补步骤（不依赖定位），用于评估精确检测的重要性。

### 3. 实验设计

- **Benchmark 与数据集**：
  - **数学推理**：GSM8K、MATH500、GaoKao2023EN（基础级）；MathOdyssey、OlympiadBenchEN、AMC23（竞赛级）。
  - **逻辑推理（OOD）**：FOLIO、LogicQA、ProofWriter、ReClor、RuleTaker。
  - **训练数据**：MetaMathQA (395k)、NuminaMath-CoT (859k) 及其桥接版本；另包含蒸馏数据（Distill）和拒绝采样数据（Reject Sampling）。
- **对比方法**：
  - **直接 SFT**（基线）。
  - **零样本桥接**：使用 Qwen2.5-Instruct-7B/72B 直接提示桥接（不经过专门训练）。
  - **CoT-Bridge-Random**：在随机位置插入步骤。
  - **CoT-Bridge**：本文方法。
  - 额外参考：4-shot、GSM8K+MATH、MathInstruct。
- **基座模型**：Meta-Llama3.1-8B（通用）、Qwen2.5-Math-1.5B（数学专用）。
- **评估设置**：vLLM 推理，贪婪解码（温度 0），零样本；每个问题采样 4 次取平均准确率；答案提取使用 Math-Verify 及 DeepSeek-R1 辅助验证。

### 4. 资源与算力

- **模型训练（SFT）**：使用 8 块 **Ascend H910B-64G**（华为910B）。
- **模型评估**：使用 4 块 **NVIDIA A100-40G**。
- **CoT-Bridge 训练**：1 个 epoch，global batch size 1024，同样在 8 块 Ascend H910B-64G 上完成。
- **RL 训练**：使用 **veRL** 框架，4 个样本 / 输入，全局 batch size 512，最大响应长度 4096。
- **未提及具体训练时长**（如 GPU 小时数），但提供了足够硬件配置信息。

### 5. 实验数量与充分性

- **主实验**表 1 覆盖了 2 个基座模型 × 4 种数据集 × 6 个 benchmark × 5 种方法（含对照），共多组对比。
- **消融与分析**：
  - 桥接位置消融（begin / middle / end）——表 5。
  - 噪声分析（PRM 分数阈值过滤）——表 6。
  - 蒸馏与拒绝采样增强实验——表 2。
  - 强化学习冷启动效果——图 3、表 9。
  - 桥接任务自身评估（位置精度、冗余度）——表 3。
  - OOD 泛化实验——表 4。
- **充分性**：实验覆盖了数学推理、逻辑推理、蒸馏、RL 等多个场景，控制了训练设置一致（学习率、epoch 数、序列长度等），并报告了随机性的影响（取 4 次平均）。实验设计较为全面、客观、公平。

### 6. 主要结论与发现

1. **桥接思维跳跃显著提升推理性能**：CoT-Bridge 在所有基准上一致优于直接 SFT，最高提升 +5.87%（LLaMA + NuminaMath），尤其对竞赛级题目效果更明显（AMC23 提升 +15.63%）。
2. **精确的跳跃定位至关重要**：CoT-Bridge-Random 表现不稳定，甚至在某些 benchmark 上退步；而 CoT-Bridge 通过准确定位实现稳定提升。
3. **零样本桥接有一定效果但缺乏一致性**：72B 零样本模型在多数场景下优于基线，但弱于特定训练的 CoT-Bridge，且有时引入噪声（如 AMC23 下降 15%）。
4. **即插即用增强**：CoT-Bridge 可提升蒸馏数据（+3.02%）和拒绝采样数据（+1.37%）的质量，并为 RL 提供更好的冷启动模型（最终 RL 准确率 63.98% vs 60.88%）。
5. **OOD 泛化能力**：在逻辑推理 benchmark 上，桥接后模型准确率提升（LLaMA +2.99%，Qwen +0.99%），且无效输出减少。
6. **桥接位置贡献**：开始、中间、结尾的桥接内容均有正面作用，其中中间桥接占比最高（55%~66%），缺失任一类型都会导致性能下降。

### 7. 优点

- **问题新颖且重要**：首次系统定义并形式化 CoT 中的思维跳跃现象，并提出了对应的桥接任务。
- **方法即插即用**：无需修改现有训练流程，可直接应用于已有数据集或与其他技术（蒸馏、RL）兼容。
- **实验充分且结果显著**：多个 benchmark、两种基座模型、多种训练范式，均验证了方法的有效性。
- **泛化验证**：不仅限于数学，还在逻辑推理任务上展示了跨领域迁移能力。
- **结构清晰、开源承诺**：提供了详细的数据构建过程、训练参数和模板，承诺开源代码。

### 8. 不足与局限

- **潜在噪声**：CoT-Bridge 生成的中间步骤无法保证完全正确，可能引入一定噪声。不过实验表明噪声对最终性能影响有限（表 6）。
- **模型规模限制**：仅在小到中等规模模型（8B / 1.5B）上验证，未在 32B / 72B 等更大模型上测试，泛化性到大规模模型有待确认。
- **领域局限**：CoT-Bridge 仅基于数学数据集训练，虽然展示了向逻辑推理的泛化，但尚未在更广泛的多步推理任务（如法律、医学、科学 QA）中验证。
- **计算资源**：虽然提供了硬件配置，但未给出具体训练时长，可复现性描述可进一步完善。

（完）
