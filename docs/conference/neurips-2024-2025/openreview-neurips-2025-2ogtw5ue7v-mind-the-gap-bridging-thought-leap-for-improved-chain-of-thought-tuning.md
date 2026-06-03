---
title: "Mind the Gap: Bridging Thought Leap for Improved Chain-of-Thought Tuning"
title_zh: 弥合差距：弥合思维跳跃以改进思维链调优
authors: "Haolei Xu, Yuchen Yan, Yongliang Shen, Wenqi Zhang, Guiyang Hou, Shengpei Jiang, Kaitao Song, Weiming Lu, Jun Xiao, Yueting Zhuang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=2ogTw5ue7v"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 检测并弥合思维跳跃以提高CoT忠实度
tldr: 提出CoT-Bridge检测并填补CoT推理中的缺失步骤以提升忠实度。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 325, \"height\": 492}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 1164, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 1240, \"height\": 1754}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1968, \"height\": 495}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-2ogtw5ue7v/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 1968, \"height\": 495}]"
motivation: 现有数学CoT数据集因专家省略中间步骤而存在思维跳跃，影响模型学习。
method: 构建ScaleQM+数据集并训练CoT-Bridge模型自动检测并生成缺失推理步骤。
result: 在数学推理基准上验证了方法能有效提升CoT的完整性和连贯性。
conclusion: 弥合思维跳跃可以显著改善CoT推理的质量和忠实度。
---

## Abstract
Large language models (LLMs) have achieved remarkable progress on mathematical tasks through Chain-of-Thought (CoT) reasoning. However, existing mathematical CoT datasets often suffer from **Thought Leaps** due to experts omitting intermediate steps, which negatively impacts model learning and generalization. We propose the CoT Thought Leap Bridge Task, which aims to automatically detect leaps and generate missing intermediate reasoning steps to restore the completeness and coherence of CoT. To facilitate this, we constructed a specialized training dataset called **ScaleQM+**, based on the structured ScaleQuestMath dataset, and trained **CoT-Bridge** to bridge thought leaps. Through comprehensive experiments on mathematical reasoning benchmarks, we demonstrate that models fine-tuned on bridged datasets consistently outperform those trained on original datasets, with improvements of up to +5.87\% on NuminaMath. Our approach effectively enhances distilled data (+3.02\%) and provides better starting points for reinforcement learning (+3.1\%), functioning as a plug-and-play module compatible with existing optimization techniques. Furthermore, CoT-Bridge demonstrates improved generalization to out-of-domain logical reasoning tasks, confirming that enhancing reasoning completeness yields broadly applicable benefits.

---

## 论文详细总结（自动生成）

# 论文总结：Mind the Gap: Bridging Thought Leap for Improved Chain-of-Thought Tuning

## 1. 核心问题与整体含义
- **研究动机**：大语言模型（LLM）在数学推理任务中依赖思维链（CoT）逐步推导。然而，现有数学CoT数据集普遍存在“**Thought Leap（思维跳跃）**”现象——专家在编写推理链时省略了他们认为“显然”的中间步骤，导致推理链不完整。这种跳跃会严重阻碍模型的学习效率和泛化能力，且其危害性甚至超过事实错误。
- **整体含义**：本文旨在**自动检测并弥合CoT中的思维跳跃**，恢复推理链的完整性与连贯性，从而提升下游模型在数学及逻辑推理任务上的性能。

## 2. 方法论
- **核心思想**：构建一个**CoT思维跳跃桥接任务**，分为两步：①识别跳跃位置；②生成缺失的中间推理步骤。通过微调一个专门的桥接模型（CoT-Bridge），将其应用于现有CoT数据集以达到数据增强。
- **关键技术细节**：
  - **任务形式化**：定义完整性函数V，若相邻步骤间存在逻辑跳转（V=False）则视为跳跃。缺失的步骤序列需满足局部完整性条件。
  - **训练数据构建（ScaleQM+）**：基于结构完整的ScaleQuestMath数据集，按照一定策略移除中间步骤（保留最终答案，移除1-3步），产生带跳跃的不完整链和对应的缺失步骤作为监督信号。共获得588k训练样本。
  - **桥接模型训练**：以Qwen2.5-Math-7B为基座，通过指令微调学习从跳跃链到完整链的映射（同时预测跳跃位置和生成缺失步骤）。另设变体CoT-Bridge-Random（给定位置仅生成步骤）。
  - **数据增强流程**：将训练好的CoT-Bridge应用于MetaMathQA和NuminaMath-CoT数据集，对每条推理链进行跳跃检测与缺失步骤插入，生成桥接版本。

## 3. 实验设计
- **数据集与场景**：
  - **数学推理基准**（6个）：GSM8K（基础）、MATH500（基础）、GaoKao2023EN（基础）、MathOdyssey（竞赛级）、OlympiadBenchEN（竞赛级）、AMC23（竞赛级）。
  - **逻辑推理基准**（5个，用于OOD泛化）：FOLIO、LogicQA、ProofWriter、ReClor、RuleTaker。
  - **数据增强场景**：知识蒸馏（Distill）、拒绝采样（Reject Sampling）、强化学习（GRPO，使用DAPO-Math-17K）。
- **对比方法**：
  - 无桥接的直接SFT
  - 零样本桥接（Qwen2.5-Instruct-7B/72B）
  - CoT-Bridge-Random（随机位置桥接）
  - CoT-Bridge（本文方法）
  - 另与4-shot、GSM8K+MATH、MathInstruct等基线对比。
- **评估指标**：在数学基准上使用vLLM推理、greedy解码、采样4次取平均精度；逻辑推理使用XFinder提取答案；桥接任务自身评估使用精确率、召回率、冗余率和位置感知BERTScore。

## 4. 资源与算力
- **训练**：
  - SFT实验：8块Ascend H910B-64G，批量大小128。
  - CoT-Bridge训练：8块H910B-64G，批量大小1024，训练1个epoch。具体训练时长未报告。
- **评估**：4块NVIDIA A100-40G。
- **RL实验**：使用veRL框架，8块H910B-64G（推测）。批大小512，采样4个回答。
- 文中未给出精确的GPU小时数，但算力规模为中等（7B模型为主，数据量百万级）。

## 5. 实验数量与充分性
- **实验组数**：超过**30组**，涵盖：
  - 主实验（表1）：2个基座模型 × 2个原始数据集 × 5种对比方法，在6个基准上评测。
  - 即插即用实验（表2）：蒸馏数据、拒绝采样数据桥接对比。
  - RL实验（表9/图3）：2个模型（原始vs桥接）GRPO训练曲线与最终精度。
  - 桥接任务自身评测（表3）：4种方法在ScaleQM+测试集上。
  - OOD泛化（表4）：2个模型 × 5个数据集。
  - 位置消融（表5）：删除开始/中间/结尾桥接成分。
  - 噪声分析（表6）：基于PRM阈值去噪后性能。
  - PRM分布分析（图4/表11）：三大桥接方法在MetaMath和NuminaMath上的评分区间。
- **充分性评估**：实验设计**非常充分且客观公平**。对比基线全面（零样本、随机、无桥接），消融实验覆盖结构位置、噪声影响、泛化能力，且所有SFT与RL设置统一。评估时采用多次采样取平均以降低方差，验证方法使用Math-Verify+DeepSeek-R1双重校验。

## 6. 主要结论与发现
1. **桥接思维跳跃持续提升推理性能**：CoT-Bridge在几乎所有配置下优于直接SFT，最大提升达+5.87%（LLaMA3.1-8B+NuminaMath），尤其在竞赛级基准（AMC23提升+15.63%）上效果显著。
2. **准确识别跳跃位置至关重要**：CoT-Bridge-Random（随机位置）性能不稳定甚至下降，而CoT-Bridge通过正确识别跳跃位置带来稳定提升。
3. **零样本桥接能力有限**：72B模型零样本桥接有潜力但噪声大（冗余率高），CoT-Bridge在质量和鲁棒性上明显更优。
4. **即插即用兼容性好**：桥接可改善蒸馏数据（+3.02%）和拒绝采样数据（+1.37%），并为RL提供更好的冷启动（+3.1%），最终超过官方GRPO训练版。
5. **OOD泛化能力**：在逻辑推理基准上桥接后精度提升（LLaMA：+2.99%，Qwen：+0.99%），无效输出下降。
6. **桥接内容位置均有贡献**：开始、中间、结尾位置的桥接都正向影响性能，中间位置（计算与逻辑推进）占比最高。
7. **噪声影响可控**：CoT-Bridge生成步骤在PRM评分中高质量占比极高（>83%），低分部分对训练影响甚微。

## 7. 优点
- **问题新颖性**：首次系统定义并形式化了CoT中的“思维跳跃”现象，填补了领域空白。
- **方法简洁有效**：CoT-Bridge作为即插即用模块，可无缝集成到SFT、蒸馏、强化学习等现有流程中，无需改动训练范式。
- **实验全面深入**：涵盖多模型、多数据集、多基准，并进行了位置消融、噪声分析、OOD泛化等细致实验，结论可信度强。
- **开源友好**：项目代码和数据将开源，便于复现。

## 8. 不足与局限
- **潜在噪声**：桥接步骤不能保证完全正确，可能引入少量推理噪声。尽管分析表明影响有限，但仍不可忽略。
- **未验证大规模模型**：实验限于1.5B和8B模型，未在32B/72B等大模型上评测泛化性。
- **领域局限性**：CoT-Bridge训练仅基于数学领域（ScaleQuestMath）。虽然OOD逻辑推理有效，但在法律、医学等非符号推理领域效果未知。理论上可迁移，但需验证。
- **计算开销**：尽管方法轻量，但生成桥接数据需要在原始数据集上逐条运行模型，对大规模数据集有一定成本。
- **自动评估依赖**：RL实验使用Math-Verify和DeepSeek-R1辅助验证，可能存在误判，但文中通过多次采样降低影响。

（完）
