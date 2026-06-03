---
title: "A*-Thought: Efficient Reasoning via Bidirectional Compression for Low-Resource Settings"
title_zh: "A*-思想：通过双向压缩实现低资源场景下的高效推理"
authors: "Xiaoang Xu, Shuo Wang, Xu Han, Zhenghao Liu, Huijia Wu, Pei Pei Li, Zhiyuan Liu, Maosong Sun, Zhaofeng He"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=uvyr9bYwL6"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 提出高效树搜索来压缩推理链以提高效率
tldr: "引入结合A*搜索的统一框架，从长推理链中识别关键思想"
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 长思维链导致效率低下，现有压缩方法会降低性能。
method: "将推理过程建模为搜索树，使用A*算法选择最有用的推理跨度。"
result: 在保持性能的同时显著减少了推理步骤。
conclusion: "A*-思想在低资源环境下实现了高效的推理，平衡了性能和效率。"
---

## Abstract
Large Reasoning Models (LRMs) achieve superior performance by extending the thought length. However, a lengthy thinking trajectory leads to reduced efficiency. Most of the existing methods are stuck in the assumption of overthinking and attempt to reason efficiently by compressing the Chain-of-Thought, but this often leads to performance degradation. To address this problem, we introduce A*-Thought, an efficient tree search-based unified framework designed to identify and isolate the most essential thoughts from the extensive reasoning chains produced by these models. It formulates the reasoning process of LRMs as a search tree, where each node represents a reasoning span in the giant reasoning space. By combining the A* search algorithm with a cost function specific to the reasoning path, it can efficiently compress the chain of thought and determine a reasoning path with high information density and low cost. In addition, we also propose a bidirectional importance estimation mechanism, which further refines this search process and enhances its efficiency beyond uniform sampling. Extensive experiments on several advanced math tasks show that A*-Thought effectively balances performance and efficiency over a huge search space. Specifically, A*-Thought can improve the performance of QwQ-32B by 2.39$\times$ with low-budget and reduce the length of the output token by nearly 50\% with high-budget. The proposed method is also compatible with several other LRMs, demonstrating its generalization capability. The code can be accessed at: https://github.com/AI9Stars/AStar-Thought.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：大型推理模型（LRM，如 OpenAI o1、DeepSeek-R1、QwQ-32B）通过生成长链思维序列（Chain-of-Thought, CoT）实现了强大的复杂推理能力，但这带来了高昂的计算开销和推理延迟，尤其限制了在资源受限环境（如端侧设备）中的部署。
- **核心问题**：现有 CoT 压缩方法（如 Chain-of-Draft、Break-the-Chain、TokenSkip）要么假设 LRM 存在“过度思考”行为，要么采用简单剪枝或提示压缩，往往导致推理性能明显下降。如何在不牺牲性能的前提下高效压缩长 CoT，是论文要解决的关键挑战。
- **本文目标**：提出一个统一的压缩框架，能够自动识别并保留长推理链中真正关键的思维步骤，以形成信息密度高、路径短的推理轨迹，在低资源预算下维持甚至提升模型的推理性能。

### 2. 论文提出的方法论
- **核心思想**：将 LRM 的推理过程建模为一棵搜索树，每个节点对应原始长 CoT 中的一个推理跨度（span）。采用 A* 搜索算法在指数级候选路径空间中进行高效探索，通过双向重要性评估和定制代价函数，快速筛选出最具信息价值的推理路径。
- **关键技术细节**：
  - **步骤级双向重要性评分（Bidirectional Importance Score, BIS）**：对每个思维步骤 `t^(n)`，同时考虑它对 **问题 q** 和 **最终答案 s** 的重要性。利用 GPT-2 小模型计算注意力分数（ATTN）和负对数似然（NLL），加权融合得到 BIS。公式为：
    `BIS(t^(n)) = [(1-α) ATTN(q|t^(n)) + α ATTN(s|t^(n))] / [(1-α) NLL(q|t^(n)) + α NLL(s|t^(n))]`
    其中 α 控制问题与答案的重要性比重（默认 0.5）。
  - **路径级 A* 搜索**：
    1. **初始化**：将原始 CoT 的思维步骤按 BIS 降序排列，构建队列 Q。从 Q 中弹出第一个步骤作为根节点，形成初始路径 `t'_k`。
    2. **验证（Verification）**：当路径深度 `k ≥ k_min`（默认 5）时，用验证模型 V（实验中固定为 s1.1-32B）判断当前路径能否得出正确答案。若能则直接返回该路径。
    3. **探索（Exploration）**：若验证未通过，则从队列 Q 中取出前 W 个步骤（默认 W=2），分别扩展为候选路径。对每个候选路径计算总成本函数 `f = g + h`：
       - **当前成本 `g`**：评价当前已选路径的质量，计算公式为 `g = - (β / |t'_k|) * log P_V(t'_k | q)`，其中 β=0.1 控制权重。
       - **未来成本 `h`**：评价从当前节点到目标解所需的预期代价，采用条件自信息：`h = - (1/|s|) * log P_V(s | q, t'_k)`。
    4. 选择成本最小的候选路径作为新的当前路径，迭代直到验证通过或达到最大深度 `k_max`（默认 20）。
  - **阈值处理**：设置 `k_min` 防止短暂路径无法验证；设置 `k_max` 防止无休止探索，超限则直接输出原始 CoT。

### 3. 实验设计
| 维度 | 内容 |
|------|------|
| **数据集** | MATH500、AMC23、OlympiadBench、GSM8K（均为数学推理任务）；另外在 LiveCodeBench 和 MMLU 上做了跨领域验证。 |
| **骨干模型** | QwQ-32B（主要）、DeepSeek-R1-Distill-Qwen-32B、s1.1-32B；以及 7B、14B 规模的 Qwen2.5 系列。 |
| **对比方法** | 基线包括：原始 QwQ-32B、QwQ-32B 在 s1K-1.1 上直接微调、+Chain-of-Draft (CoD)、+Break-the-Chain (BtC) 两种变体、+TokenSkip。 |
| **评估指标** | **Accuracy**（准确率）、**Length**（输出 token 数）、**ACU**（Accuracy per Computation Unit = Accuracy / Length）。 |
| **预算设置** | 分别限制最大输出长度 512、1024、2048、4096 tokens，模拟不同计算资源场景。 |

### 4. 资源与算力
- **训练硬件**：8 张 NVIDIA A100 80GB GPU。
- **训练配置**：每张 GPU batch size 为 1，梯度累积步数 8（有效 batch size = 8×8=64？但文本未直接说明总 batch size），训练 3 个 epoch，峰值学习率 1×10⁻⁵，warm-up 比例 0.1。
- **训练时间**：以 QwQ-32B 为例，A*-Thought 处理后的训练数据（压缩率为 31.31%）训练耗时约 10,468 秒（约 2.9 小时），而原始 s1K-1.1 数据训练耗时约 13,820 秒（约 3.8 小时），TokenSkip 耗时约 12,846 秒。
- **说明**：论文明确给出了训练硬件、时长和压缩后的数据量，算力信息充分。

### 5. 实验数量与充分性
- **主实验**：在 4 个数学数据集、4 种预算、6 种方法（含 A*-Thought）下进行对比，覆盖 **4×4×6=96** 个条件组合（Table 1）。
- **多骨干实验**：在 R1-Distill-32B 和 s1.1-32B 上重复相似对比（Table 7、Table 8），并在多个骨干上呈现 ACU 曲线（Figure 4）和准确率曲线（Figure 5）。
- **规模泛化**：在 Qwen2.5-7B/14B 上验证（Table 6）。
- **消融实验**：
  - BIS 设计中 ATTN 与 NLL 的单独 vs. 联合效果（Figure 8）。
  - 超参数 α（0~1）对性能的影响（Figure 9）。
  - 搜索深度限值 `k_max`（10/15/20）的影响（Figure 10）。
  - 验证起始深度 `k_min`（5 vs. 15）的比较（Table 3）。
  - 当前成本权重 β（0.1/0.5/0.9）在 ARC 和 LiveCodeBench 上的测试（Table 9）。
- **跨领域测试**：在 LiveCodeBench（代码生成）和 MMLU（知识问答）上评估（Table 2），虽然模型仅由数学数据训练，仍取得正向迁移。
- **测试时缩放**：使用 best-of-N 策略（N=1,2,4,8）计算 pass@k（Table 5），验证与缩放策略的组合效益。
- **总体评价**：实验覆盖了多个数据集、模型规模、预算限制、消融组件和跨领域任务，设计系统且充分。但论文未报告多次重复实验的误差棒（如 variance），统计显著性仅通过数值差值体现，未提供置信区间。

### 6. 论文的主要结论与发现
- **低预算下显著提升**：在 512 token 预算时，A*-Thought 使 QwQ-32B 的平均准确率从 12.3% 提升至 **29.4%**（2.39×提升），ACU 从 2.41 提升至 **5.99**。
- **高预算下大幅缩短长度**：在 4096 token 预算时，平均输出长度从 2826 降至 **1877 个 token**（减少 33.6%），准确率仅轻微下降（70.6% → 69.3%），ACU 仍然最高。
- **跨模型、跨领域通用**：在 R1-Distill-32B、s1.1-32B 以及 7B/14B 模型上均取得最佳效果，在非数学任务（代码、知识问答）上也表现优于基线。
- **消融验证有效性**：BIS 中同时使用注意力与似然、合理设置 α 和 β、搜索深度选择合适的 `k_max` 等设计均对最终性能有正向贡献。

### 7. 优点
- **方法创新**：首次将 A* 搜索与双向重要性估计结合到 CoT 压缩中，同时捕捉步骤对问题和答案的贡献，克服了单向重要性估计的片面性。
- **两级协同优化**：步骤级（BIS）指导搜索起点，路径级（A* + f 函数）评估全局收益，理论框架清晰。
- **实验设计全面**：涵盖多数学数据集、多模型骨干、多预算设置、详细消融，并延伸到跨领域任务和测试时缩放，验证了方法的鲁棒性和泛化性。
- **结果显著**：在极低预算（512 tokens）下获得数倍准确率提升，高预算下大幅降低推理成本，且性能不被牺牲。
- **代码开源**：提供了 GitHub 代码仓库，利于复现和后续研究。

### 8. 不足与局限
- **当前限于 SFT**：论文在 Appendix H 中明确说明目前仅应用于监督微调（SFT），未扩展到强化学习（RL）范式。而 RL 在提升 LRM 推理能力方面已展现潜力，是本方法的一个自然扩展方向。
- **依赖外部验证模型**：A* 搜索中的验证步骤需要一个能评判中间路径正确性的验证模型（实验中固定为 s1.1-32B），这引入了额外的依赖和可能的误差。验证模型的质量直接影响搜索效果。
- **未报告多次运行的统计误差**：所有结果均为单次实验，缺少误差棒或置信区间，难以判断结果的可信度波动（论文在 Checklist 中对此回答“No”）。
- **计算开销未详细分析**：虽然 BIS 使用 GPT-2 小模型，且搜索仅用于离线压缩训练数据，但文中未量化 A* 搜索本身的耗时和资源消耗，也未与其他压缩方法（如 TokenSkip 的压缩时间）进行全面对比。
- **跨领域任务有限**：仅测试了 LiveCodeBench 和 MMLU 两个非数学任务，且模型本身在数学数据上训练，对其他领域（如常识推理、开放域问答）的泛化性尚待验证。
- **超参数选择可能欠泛化**：搜索深度、验证起始深度、W 等超参数默认值（k_min=5, k_max=20, W=2）仅在数学任务上调优，在其他场景可能需要重新调整。

（完）
