---
title: "Let Me Think! A Long Chain of Thought Can Be Worth Exponentially Many Short Ones"
title_zh: 让我想想！长思考链可能价值指数倍的多条短思考链。
authors: "Parsa Mirtaheri, Ezra Edelman, Samy Jelassi, Eran Malach, Enric Boix-Adserà"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=GuvQJGgbLm"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 比较顺序与并行思考链扩展
tldr: 证明长思考链在图形推理中比多条短思考链有指数优势。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1651, \"height\": 1316}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 551, \"height\": 439}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 1920, \"height\": 1440}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 2400, \"height\": 1800}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 2400, \"height\": 1800}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-006.webp\", \"caption\": \"\", \"page\": 30, \"index\": 6, \"width\": 551, \"height\": 439}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-007.webp\", \"caption\": \"\", \"page\": 30, \"index\": 7, \"width\": 551, \"height\": 439}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-008.webp\", \"caption\": \"\", \"page\": 30, \"index\": 8, \"width\": 551, \"height\": 439}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-guvqjggblm/fig-009.webp\", \"caption\": \"\", \"page\": 31, \"index\": 9, \"width\": 1698, \"height\": 979}]"
motivation: 推理时计算分配不明确，需理解顺序与并行扩展的优势。
method: 理论分析并实验验证顺序扩展在图形连接问题中的指数优势。
result: 发现顺序扩展在特定推理场景中优于并行扩展。
conclusion: 长思考链能提供指数级收益。
---

## Abstract
Inference-time computation has emerged as a promising scaling axis for improving large language model reasoning. However, despite yielding impressive performance, the optimal allocation of inference-time computation remains poorly understood. A central question is whether to prioritize sequential scaling (e.g., longer chains of thought) or parallel scaling (e.g., majority voting across multiple short chains of thought). In this work, we seek to illuminate the landscape of test-time scaling by demonstrating the existence of reasoning settings where sequential scaling offers an exponential advantage over parallel scaling. These settings are based on graph connectivity problems in challenging distributions of graphs. We validate our theoretical findings with comprehensive experiments across a range of language models, including models trained from scratch for graph connectivity with different chain of thought strategies as well as large reasoning models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：在大语言模型（LLM）的推理时计算分配中，应当优先采用**顺序扩展**（如更长的思维链，Chain-of-Thought）还是**并行扩展**（如对多条短思维链进行多数投票）？哪种方式在计算效率上更优？
- **研究动机**：虽然推理时计算（test-time compute）被证明能显著提升LLM推理能力，但其最优分配策略仍不明确。现有方法多样（如CoT、best-of-n、多数投票等），且可组合使用，使得分析复杂化。
- **背景**：顺序扩展（长CoT）已在数学、编程等困难任务中展现潜力，但Transformer的注意力机制导致其计算量随序列长度平方增长，成本更高。因此需要量化两者的权衡。
- **整体含义**：论文通过理论和实验证明，在特定推理任务（图连通性问题）中，顺序扩展可以提供**指数级**优势，即少量减少顺序规模需要大量增加并行规模才能维持同等准确率。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将推理时方法分为两类：①并行扩展（生成多条独立响应并聚合，如多数投票、best-of-n）；②顺序扩展（生成连贯推理步骤，如CoT）。通过构造具有挑战性的图分布，展示顺序扩展的不可替代性。
- **关键技术细节**：
  - **任务设计**：使用 **(s, t1, t2)-连通性**问题：给定图G和三个顶点s, t1, t2，且保证s与t1或t2恰好之一连通，判断与哪个连通。该对称任务避免了证书缺失问题。
  - **理论分析**：
    - 基于Transformer表达性限制（TC0复杂度类），证明**定理4**：假设TC0⊉L，则：
      - 顺序扩展成功：多项式长度CoT可解决任意(s,t1,t2)-连通性；
      - 并行扩展失败：对常数长度CoT，多数投票需超多项式条才能达到非平凡准确率。
    - 基于**顶点查询模型（VQM）**：抽象Transformer在图推理中的链式思维行为，证明在“两路径图”和“桥图”上，顺序查询次数与路径长度线性相关，而并行扩展需指数级独立运行才能成功（定理2、3）。
  - **训练策略**：
    - 从零开始训练小Transformer（Mistral架构，4层），使用不同CoT策略生成训练数据：**Shortest-Path**（最短路径）、**Path**（DFS树中的路径）、**DFS**（DFS遍历顺序）、**Walk-L**（随机游走限制步数）。
    - 通过**强化学习（STaR）** 对模型进行迭代微调，自举验证过的CoT。
- **算法流程**（文字说明）：
  1. 生成图任务（桥图或两路径图），随机排列边顺序和顶点标签。
  2. 根据CoT策略构造训练样本：输入包含边列表、s、t1、t2，输出为推理序列加最终决策。
  3. 使用自回归变换器训练，预测下一个token。
  4. 在推理时，通过控制最大生成长度实现顺序扩展，通过对多次独立采样进行多数投票或best-of-n实现并行扩展。

## 3. 实验设计：使用的数据集/场景、benchmark、对比方法
- **数据集/场景**：
  - 主要场景：**桥图（Bridge Graph）**，参数为 short=3, long=5, deadend=3，深度 d=1~5。
  - 辅助场景：**两路径图（Two-Path Graph）**，路径长度 L 可变。
  - LLM实验：额外使用 **AIME2024** 数学竞赛数据集。
- **Benchmark**：任务为 (s, t1, t2)-连通性判断，评价指标包括：
  - **决策准确率**：最终输出是否指向正确目标节点。
  - **证据准确率**：输出的CoT是否构成有效证明（从s出发，经过邻接关系到达正确目标，且不违反图结构）。
  - 并行扩展时使用**多数决策**和**best-of-n**聚合。
- **对比方法**：
  - CoT策略：Shortest-Path、Path、DFS、Walk-L（不同步数）、Longest-Path。
  - 模型：从零训练的Mistral模型（4层和2层）、DeepSeek-R1-Distill-Qwen-32B、Qwen3-32B、s1-32B。
  - 扩展方式：顺序扩展（控制最大CoT长度） vs 并行扩展（不同采样次数 n=1~128）。

## 4. 资源与算力
- **训练小模型**：使用NVIDIA A100 GPU（40GB），每个预训练实验<12 GPU小时，RL微调<3 GPU小时，总调试<72 GPU小时。
- **LLM实验**：
  - AIME2024实验：使用H200 GPU，每个run约1.5小时，总计约24 H200 GPU小时。
  - 图连通性实验：使用vllm + 2张A100（80GB），每个图<4小时，所有图<32 GPU小时。调试<120 GPU小时。
- **说明**：论文提供了较详细的算力开销，结果表明实验是可行的。

## 5. 实验数量与充分性
- **实验组数**：
  - 不同桥图深度（d=1~5）×多种CoT策略（4种以上）×不同顺序/并行规模×RL迭代（0~4轮）。
  - 额外消融：小模型（2层）对比、Walk-L策略、LLM跨模型对比。
  - 每个条件通常有多次独立采样（如并行扩展时n=1~128），误差条表示95%置信区间。
- **充分性**：
  - 实验覆盖了理论预测的主要场景，验证了顺序扩展的优势。
  - 对比了多种CoT策略，展示了短CoT模型失败的原因（陷入OOD情况）。
  - 使用了多种模型族（自训练+开源LLM），增强了结论的泛化性。
- **客观公平性**：论文报告了误差条和统计显著性（二项式置信区间），且采样使用相同温度（如0.6或1.0），fairness较好。

## 6. 论文的主要结论与发现
- **核心发现**：在图形推理任务中，顺序扩展（长CoT）相比并行扩展（多条短CoT）具有**指数级优势**：小幅减少顺序规模需要指数级增加的并行规模才能弥补。
- **理论结论**：
  - 基于复杂度假设（TC0⊉L），常数长度CoT的并行聚合无法解决连通性问题，而多项式长度CoT可解决。
  - 在顶点查询模型下，顺序查询次数与路径长度线性相关，而并行扩展需要指数次独立运行。
- **实证结论**：
  - 短CoT模型（Shortest-Path, Path）在深图下准确率极低（如Bridge(5)证据准确率0%~11%），且行为符合概率分析（如随机选择路径）。
  - 长CoT模型（DFS）可实现接近100%准确率。
  - 对于固定总token预算，顺序扩展始终优于并行扩展。
  - RL训练（STaR）自然促使模型产生更长的CoT，并提升所有顺序规模下的准确率。
  - 大型推理模型（DeepSeek-R1, Qwen3, s1）也呈现相同趋势：顺序扩展是获得非平凡准确率的必要条件。

## 7. 优点：方法或实验设计上的亮点
- **理论深度**：结合复杂度理论（TC0）和抽象模型（VQM）严格证明顺序扩展的指数优势，而非仅靠实验。
- **任务设计精巧**：选择的图连通性问题难度可控，可调节参数（桥深、路径长度），便于量化分析。
- **全面对比**：既训练小型模型进行受控实验，又验证前沿LLM，结论具有一致性。
- **实验分析细致**：不仅报告准确率，还分析“证据准确率”，并解释短CoT模型失败机制（无法区分路径、陷入OOD）。
- **RL融合**：通过STaR自举，展示了模型如何自动增长CoT长度，解释现实中R1等现象。
- **可视化清晰**：图1、图4、图10等直观展示顺序与并行扩展的权衡曲线，支持指数级差距。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **任务局限性**：仅研究图连通性任务，未覆盖更广泛的推理类型（如数学、代码、常识）。结论可能不直接适用于所有场景。
- **抽象模型的启发性**：顶点查询模型（VQM）是对Transformer能力的启发式抽象，缺乏严格理论保证，可能高估或低估真实模型的能力。
- **理论假设强**：定理4依赖于复杂度假设“TC0⊉L”，该假设在密码学中常见但未证明；若被否定，则理论分离可能不成立。
- **实验覆盖有限**：
  - 小模型仅训练Mistral架构（4层），未探索更深/更宽模型。
  - 桥图参数固定（short=3, long=5），未系统变化路径长度比例。
  - 并行扩展方式仅用了多数投票和best-of-n，未考虑更先进的聚合方法（如加权投票、验证器）。
- **偏差风险**：训练短CoT时模型可能过拟合训练分布（如只见过最短路径），导致在测试时易陷入OOD，这人为放大了顺序扩展的优势。Real-world任务中模型可能更鲁棒。
- **应用限制**：论文未给出如何混合顺序与并行扩展以获得最优计算效率的通用准则，仅指出两者可结合。
- **对大型模型的开销**：LLM实验中，很多token被用于描述策略而非实际推理，可能浪费预算；更好的提示或微调可减轻。

（完）
