---
title: "Reasoning Is Not a Race: When Stopping Early Beats Going Deeper"
title_zh: 推理不是比赛：何时提前停止胜过深入
authors: "Mohan Zhang, Jiaxuan Gao, Shusheng Xu, Yi Wu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=xoL5zo1O86"
tags: ["query:rl-nlplr"]
score: 8.0
evidence: 用于长思维链推理的过程奖励模型
tldr: 使用PRM与提前停止改进长思维链推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 3002, \"height\": 2381}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 4553, \"height\": 2581}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 2342, \"height\": 2158}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 2345, \"height\": 2158}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-005.webp\", \"caption\": \"\", \"page\": 4, \"index\": 5, \"width\": 2345, \"height\": 2158}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 912, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-009.webp\", \"caption\": \"\", \"page\": 8, \"index\": 9, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-010.webp\", \"caption\": \"\", \"page\": 8, \"index\": 10, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-011.webp\", \"caption\": \"\", \"page\": 8, \"index\": 11, \"width\": 873, \"height\": 807}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-012.webp\", \"caption\": \"\", \"page\": 16, \"index\": 12, \"width\": 1500, \"height\": 1050}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xol5zo1o86/fig-013.webp\", \"caption\": \"\", \"page\": 16, \"index\": 13, \"width\": 1500, \"height\": 1050}]"
motivation: 过程奖励模型在长CoT推理中表现不一致。
method: 提出基于PRM z分数的提前停止策略ZGES。
result: ZGES在多个数学基准上优于标准PRM和无PRM方法。
conclusion: 提前停止可缓解步骤质量退化，提升推理效果。
---

## Abstract
We study the use of Process Reward Models (PRMs) for guiding Long Chain-of-Thought (CoT) reasoning in large language models. Although PRMs deliver fine-grained feedback in standard tasks, PRM-guided beam search does not consistently outperform PRM-free approaches in long CoT reasoning. We trace this shortfall to a "step quality degradation''—the expected step quality shows concave behavior, yielding unimodal or monotonically declining trends. To counteract this, we propose Z-Score Guided Early Stopping (ZGES), which halts search at the detected quality peak using local PRM-reward z-scores. Across multiple math benchmarks and model scales, ZGES outperforms both standard PRM-guided beam search and the PRM-free methods. Ablation studies further highlight the advantages and robustness of ZGES’s adaptive stopping mechanism.

---

## 论文详细总结（自动生成）

好的，遵照您的要求，以下是对该论文《Reasoning Is Not a Race: When Stopping Early Beats Going Deeper》的详细中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：过程奖励模型（Process Reward Model, PRM）在短思维链（Short CoT）推理中能提供细粒度反馈，有效指导 LLM 进行 beam search 或重新排序。然而，在长思维链（Long CoT）推理任务中（如 OpenAI o1、DeepSeek R1），PRM 引导的 beam search 并未像预期那样一致优于无 PRM 的方法（如 Majority@N 多数投票）。
- **核心问题**：
    1. PRM 在长 CoT 模型的测试时搜索中表现如何？（表现不佳）
    2. 如何增强 PRM 在长 CoT 设置下的有效性？
- **整体含义**：本文揭示了长 CoT 推理中一个关键现象——“步骤质量退化”（step quality degradation），即 beam search 过程中候选推理步骤的期望质量呈单峰或单调递减趋势，这导致 PRM 的效用随搜索深入而下降。因此，**“深度”比“广度”更重要时，提前停止可能更好**。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：在 beam search 过程中，利用 PRM 奖励的局部 z-score 作为信号，动态检测步骤质量的峰值。一旦检测到步骤质量出现下降趋势（z-score 低于阈值），立即停止 beam search，并从峰值步骤的前一步骤直接开始解码生成最终的答案轨迹。
- **关键技术细节**：
    1. **步骤质量退化分析与理论证明**：通过实验观察到 beam search 中步骤质量的期望值呈凹性（concave），并理论证明了这是因为 PRM 的重新排名能力随搜索深入而显著下降（Corollary 3.3），导致期望步骤质量的二阶差分为负（Proposition 3.8）。
    2. **Z-score 一致性发现**：实验发现，PRM 奖励的平均值与真实步骤质量的均值之间存在强线性相关（Pearson 相关系数 > 0.91）。因此，二者的 z-score（标准化后的值）在搜索过程中一致（Lemma 4.1），使得可以通过 PRM 奖励的 z-score 间接反映步骤质量。
    3. **Z-score 引导的提前停止（ZGES）**：
        - 设置阈值 λ 和 beam search 配置 B×E。
        - 执行 beam search，每一步记录所有候选的 PRM 平均奖励 xt。
        - 计算 xt 在历史序列 {x1,...,xt} 中的局部 z-score x't。
        - 如果 x't < λ，则停止 beam search，并从 t-1 步的候选开始直接解码；否则继续下一步。
        - 最终的答案通过加权最佳 N（Weighted Best of N, WBoN）从收集到的轨迹中选择。

### 3. 实验设计：数据集、Benchmark 和对比方法
- **数据集/Benchmark**：
    - 数学推理基准：AMC2023、AIME2024、AIME2025。
- **模型**：
    - LLM 策略（Policy）：DeepSeek-R1-Distill-Qwen-1.5B 和 DeepSeek-R1-Distill-Qwen-7B 两种规模。
    - PRM 训练：基于这两个模型的变体，训练了 Hard PRM 和 Soft PRM 两种类型。
- **对比方法**：
    - **无 PRM 方法**：Majority@N（多数投票）。
    - **标准 PRM 引导搜索**：PRM-guided Beam Search (Hard PRM 和 Soft PRM)。
    - **消融实验对比**：固定步停止（Fixed-step stopping）、不同 λ 值的 ZGES、以及标准 Beam Search 进行 PRM 调用次数和 token 使用效率对比。

### 4. 资源与算力
- **未明确说明**：论文在实验设置部分提供了 PRM 训练数据的生成细节（9K 道难题，每道题生成 16 个响应，每个步骤生成 8 个补全），但**未明确说明使用的 GPU 型号、数量或训练时长**。作者在局限性部分提到“由于计算资源有限”，但未给出具体指标。

### 5. 实验数量与充分性
- **实验数量**：实验设计较为全面，主要包含：
    - **主实验**：在 3 个基准上，对 2 个模型，比较了 ZGES 与 Majority@N、Hard/Soft PRM Beam Search 在不同 beam size（4、8、16、32、64）下的表现（图 4，表格 1）。共约 3 × 2 × 5 = 30 组（不含扩展因子变化）。
    - **超参数敏感性分析**：λ 取 -0.4、-0.6、-0.8、-1.0 四个值，在多个配置下评估，用箱线图展示（图 5）。
    - **动态 vs 固定步停止比较**：将 ZGES 与在预定步骤停止的 beam search 进行对比（图 6）。
    - **效率分析**：比较 ZGES 与标准 Beam Search 的 PRM 调用次数和 token 比率（表 3）。
- **充分性与公平性**：
    - **充分性**：实验覆盖了不同模型规模、不同基准、不同 beam size，并进行了充分的消融和效率分析，验证了方法的有效性和鲁棒性。
    - **客观与公平**：对比方法包括 PRM-free 和 PRM-based 的主流方法，设置统一的总预算（N = B × E）作为公平比较的基数。消融实验也合理控制了变量。但未报告方差/统计显著性检验（作者在 checklist 中注明未报告误差条），但总体是客观的。

### 6. 论文的主要结论与发现
- **步骤质量退化**：在长 CoT 的 beam search 中，候选步骤的期望质量呈单峰或单调递减，这是 PRM 引导搜索失效的核心原因。
- **ZGES 有效**：提出的 ZGES 方法在所有模型规模和基准上**一致优于**标准 PRM-guided beam search 和 PRM-free 方法。
- **效率提升**：ZGES 在提升性能的同时，显著降低了 PRM 调用次数（减少 50%以上）和 token 消耗，实现了计算成本与效果的更优权衡。
- **鲁棒性**：ZGES 对超参数 λ 不敏感，在较宽范围内表现稳定；动态停止优于固定的提前停止。

### 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
    - **问题发现新颖**：首次系统性地识别并验证了长 CoT 推理中“步骤质量退化”这一现象，并给出了理论解释。
    - **方法简单有效**：ZGES 仅需在标准 beam search 中加入一个基于 z-score 的动态停止判断，无需额外训练或复杂模块，实用性强。
    - **理论支撑充分**：不仅通过实验观察，还给出了理论证明（凹性定理），解释了退化原因，并基于 PRM 奖励与步骤质量的线性关系推导出 z-score 一致性，为方法提供了扎实的理论基础。
- **实验设计亮点**：
    - **诊断分析深入**：不仅报告最终结果，还深入分析了 PRM 的重新排名精度随时间下降（附录 C.2），并通过蒙特卡洛 rollout 估计了步骤质量，验证了退化现象。
    - **消融实验全面**：包含超参数鲁棒性、与固定步停止的比较、以及计算效率的量化分析，增强了结论的可信度。

### 8. 不足与局限
- **实验覆盖局限**：
    - **算力限制**：作者承认由于计算资源有限，未探索更激进的 λ 值（如 λ ∈ [0, 1]），可能限制了方法潜力的完全发挥。
    - **任务类型单一**：仅在数学推理（AMC、AIME）上验证，未在代码、科学推理等其他长 CoT 任务上测试，方法的泛化性有待进一步检验。
    - **模型特定性**：实验仅基于 DeepSeek-R1-Distill 系列模型，对其它架构（如 Llama、DeepSeek-R1 本身）的表现未知。
- **偏差风险**：
    - **步骤分割依赖**：论文使用混合启发式方法进行步骤分割（基于符号和 token 长度）。这种分割方式可能引入噪声，且对不同模型需调整，影响了结果的稳定性和可复现性。
    - **PRM 训练偏差**：PRM 基于自动标注训练，其质量受限于标注策略（Hard/Soft），自动标注本身的噪声可能传播到搜索过程中。
- **应用限制**：
    - 方法依赖于 PRM 奖励与步骤质量的线性相关性假设（Lemma 4.1），如果 PRM 与步骤质量的关系偏离线性，z-score 一致性可能不成立，方法可能失效。
    - ZGES 的核心是“在质量峰值停止”，这在步骤质量呈单调递减时适用，但如果出现非凹性的复杂模式（如多峰），提前停止的时机可能不理想。

（完）
