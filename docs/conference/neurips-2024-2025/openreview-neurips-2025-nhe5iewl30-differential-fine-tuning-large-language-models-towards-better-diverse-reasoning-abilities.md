---
title: Differential Fine-Tuning Large Language Models Towards Better Diverse Reasoning Abilities
title_zh: 差分微调大语言模型以提升多样化推理能力
authors: "Xiaosong Yuan, Chen Shen, Shaotian Yan, kaiyuan liu, Xiaofeng Zhang, Sinan Fan, Qingyi Meng, Liang Xie, Wenxiao Wang, Renchu Guan, Ying Wang, Jieping Ye"
date: 2025-05-10
pdf: "https://openreview.net/pdf?id=NhE5IeWl30"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 差分微调以增强多样化推理能力
tldr: 提出差分微调减轻冲突并提升LLM多种推理能力。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 联合或持续微调不同推理数据集会引发性能冲突。
method: 通过分析任务差异进行差分微调，协同多种推理能力。
result: 该方法在多个推理任务上超越单独微调。
conclusion: 差分微调有效协调了不同推理能力间的冲突。
---

## Abstract
Reasoning abilities of large language models (LLMs) require explicit derivations compared to general question-answering, supervised fine-tuning (SFT) can empower multiple reasoning abilities in LLMs via learning from various datasets. However, neither training the datasets jointly (mix-up) nor continually can maintain the performance of single-dataset SFT, sometimes better while sometimes even worse, illustrating vanilla SFT can not only facilitate reasoning abilities but also introduce conflicts. In this paper, we propose a novel framework to mitigate the conflicts and preserve benefits among different reasoning tasks, and even surpass each task's single dataset SFT performance. We start by exploring the differences between reasoning fine-tuned and base LLMs by analyzing their parameter variations during model inference, and we discover that each reasoning capability has exclusive parameters that benefit itself more evidently than others. In contrast, the overlapped parameters of tasks can bring benefits or conflicts. Inspired by the findings, we propose to update the exclusive and overlapped parameters according to specific reasoning task combinations differentially, thereby avoiding unnecessary conflicts while maintaining benefits. Consistent improvements in mix-up and continual SFT experiments demonstrate that the proposed SFT strategy can achieve better performance on various LLMs (Llama3-8B, Mistral-7B, and Qwen2.5-14B) and diverse reasoning tasks with fewer conflicts, showing the superiority and generality of our analysis findings and the proposed approach.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：大型语言模型（LLM）通过监督微调（SFT）可以获得多种推理能力（如数学、代码、逻辑、常识推理）。然而，将多个推理数据集混合训练（mix-up）或顺序训练（continual）时，性能往往不如单独使用单个数据集进行SFT，有时甚至更差。这表明不同推理任务之间存在**收益与冲突**——某些任务组合能相互促进，另一些则相互干扰。
- **关键挑战**：现有方法（如随机冻结参数、任务向量提取等）未能深入分析任务间参数层面的关系，无法区分哪些参数对特定任务至关重要，从而难以精准地保留收益、避免冲突。
- **研究目标**：通过分析微调前后模型参数的变化，识别不同推理任务对应的关键参数（delta-scale rows），并据此设计一种**差分微调（DiFT）** 策略，在混合微调和连续微调场景下协调多个推理能力，实现性能提升。

## 2. 方法论：核心思想、技术细节与算法流程
- **核心思想**：每个推理任务在LLM参数空间中拥有部分“专属”参数（对当前任务贡献大），以及与其他任务重叠的参数（可能带来收益或冲突）。DiFT通过选择性更新这些参数来平衡收益与冲突。
- **关键技术细节**：
  1. **Delta-scale Row 分析**：对每个任务 \(k\)，用该任务的数据分别通过基础模型 \(M_{\text{base}}\) 和该任务微调后的模型 \(M_{\text{ft}}^k\)，计算每层线性层权重矩阵中每一行（对应输出维度）的激活差异平方均值：
     \[
     s_k = \frac{1}{N}\sum_{t=1}^N \| \Delta Y_t^k \|_2^2
     \]
     其中 \(\Delta Y_t^k\) 是第 \(t\) 个token在基础模型和微调模型输出上的差异。高分值行即为该任务的关键参数。
  2. **混合微调（Mix-up）**：对所有参与任务的delta-scale行取并集（\(DSR_{\text{union}}\)），仅更新这些行对应的参数，冻结其余参数。这样所有任务的关键参数都能被训练，而非关键参数不受干扰。
  3. **连续微调（Continual）**：对当前任务 \(k\)，取其在历史任务并集上的差集（\(DSR_{\text{diff}} = DSR_k - \cup_{j<k} DSR_j\)），仅更新这些独占参数，冻结历史和当前任务的重叠参数，以保留旧知识。
- **算法流程**：见论文Algorithm 1（附录B）：先对每个任务采样少量数据，通过前向钩子计算delta-scale行并提取top-C行；然后在微调阶段根据设置（混合/连续）选择性冻结参数。

## 3. 实验设计
- **数据集与任务**：
  - 数学：MathInstruct（20k条）
  - 代码：Code Bagel Hermes（20k条）
  - 逻辑：LogiCoT（20k条）
  - 常识：CommonsenseQA、CoS-e、OpenBookQA等混合（20k条）
- **基准测试**：
  - GSM8k（数学0-shot准确率）
  - xGLUE（代码通过率）
  - LogiQA2（逻辑0-shot准确率）
  - CSQA（常识0-shot准确率）
  - MMLU（通用知识）
- **对比方法**：
  - 混合微调：vanilla mix-up、DMT（双阶段混合）、CoBa（收敛平衡器）
  - 连续微调：vanilla continual、HFT（随机冻结一半参数）、LoTA（任务向量+稀疏适应）、CoBa
- **实验设置**：分别使用Llama3-8B、Mistral-7B、Qwen2.5-14B作为基础模型，所有SFT采用相同超参数（lr=2e-5, batch size=256, max length=2048等），保存并评估3个checkpoint取最佳。

## 4. 资源与算力
- **分析阶段**：在1块NVIDIA A100 GPU上运行，7B/8B模型约需30GB显存、~900秒；14B模型约需62GB显存、~1200秒。
- **微调阶段**：使用8块A100组成的集群（一个服务器即可完成所有实验），但未给出每轮训练的具体时长。
- **备注**：文中未详细说明总GPU小时数，仅指出计算成本相对于常规LLM推理可忽略。

## 5. 实验数量与充分性
- **实验数量**：涵盖了多种任务组合（两两组合如Math-Code、Code-Logic、Logic-CSQA，以及多任务组合如Math-Code-CSQA、Math-Code-Logic-CSQA），并在3种不同规模/架构的LLM上验证。消融实验包括：
  - 不同delta-scale行数量（20/50/100/200）
  - 逆DiFT（交换冻结位置）
  - 不同连续学习顺序（如Code→Math, Logic→Math）
- **充分性与公平性**：
  - 对比了多个SOTA基线，超参数一致，随机种子固定（42）。
  - 评估指标采用标准准确率/通过率，且对代码结果乘以常数以对齐尺度。
  - 在14B模型上报告了置信区间，表明结果稳定。
  - 总体设计较为充分，能支撑主要结论。

## 6. 主要结论与发现
- **关键发现**：不同推理任务拥有专属参数，且任务间参数重叠程度与收益/冲突相关。例如数学和代码共享较多关键参数，混合微调可互相促进；逻辑与常识共享较少，混合易产生冲突。
- **方法有效性**：DiFT在几乎所有混合和连续微调设置下，平均目标准确率（ATA）均优于或持平于其他基线，能够保留收益同时减轻冲突。
- **重要性验证**：逆DiFT（冻结关键参数、训练其他参数）性能大幅下降，证明识别出的delta-scale行对推理任务不可或缺。
- **泛化性**：在Llama3、Mistral、Qwen2.5三个模型上均取得一致改进。

## 7. 优点
- **方法论创新**：首次从参数敏感度（delta-scale row）角度量化推理任务的参数重要性，并据此设计差分更新策略，思路清晰且具有解释性。
- **实用性强**：DiFT不改变模型架构或训练数据，仅需额外一次前向分析即可获得关键参数，适用于现有任何LLM。
- **实验设计全面**：覆盖混合与连续两大场景，包含多任务、多模型、多基线和消融，验证充分。
- **可视化支撑**：热力图展示了不同任务在特定层上的delta-scale行分布差异，直观证明了参数专属性和重叠性。

## 8. 不足与局限
- **理论欠缺**：缺乏严格的数学证明解释为何delta-scale行能充分表征任务重要性，仅凭经验观察。
- **模型规模局限**：受硬件限制，仅在7B/8B和14B模型上实验，未在30B/70B等更大模型上验证，无法确认方法在更大规模下的适用性。
- **灾难性遗忘未完全解决**：在连续微调中，DiFT能缓解冲突但仍无法彻底防止遗忘，尤其在历史任务与新任务冲突严重时（如Math→Logic）性能仍有大幅下降。
- **计算代价**：DiFT需要为每个任务额外执行一次前向分析，且微调时仍需全参数（除冻结部分）更新，计算成本与全微调相当，高于LoRA等PEFT方法（文中也指出LoRA在部分设置下遗忘更少但学习更差）。

（完）
