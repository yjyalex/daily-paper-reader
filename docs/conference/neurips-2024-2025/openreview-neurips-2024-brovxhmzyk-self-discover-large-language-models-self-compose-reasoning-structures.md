---
title: "SELF-DISCOVER: Large Language Models Self-Compose Reasoning Structures"
title_zh: SELF-DISCOVER：大语言模型自组合推理结构
authors: "Pei Zhou, Jay Pujara, Xiang Ren, Xinyun Chen, Heng-Tze Cheng, Quoc V Le, Ed H. Chi, Denny Zhou, Swaroop Mishra, Steven Zheng"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=BROvXhmzYK"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 引入自发现推理结构来改进LLM推理
tldr: 提出框架让LLM自组合原子推理模块为显式结构，提升性能
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1640, \"height\": 970}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 1640, \"height\": 960}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 872, \"height\": 431}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-004.webp\", \"caption\": \"\", \"page\": 3, \"index\": 4, \"width\": 1000, \"height\": 492}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-005.webp\", \"caption\": \"\", \"page\": 3, \"index\": 5, \"width\": 583, \"height\": 261}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-006.webp\", \"caption\": \"\", \"page\": 3, \"index\": 6, \"width\": 830, \"height\": 231}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 596, \"height\": 1000}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 533, \"height\": 275}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 533, \"height\": 293}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-010.webp\", \"caption\": \"\", \"page\": 4, \"index\": 10, \"width\": 921, \"height\": 233}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 1000, \"height\": 357}]"
motivation: 典型提示方法对复杂推理问题效果有限。
method: LLM自主选择并组合多个推理模块（如批判性思维、逐步思考）形成显式推理结构。
result: "在多个推理基准上显著优于思维链，提升高达32%。"
conclusion: 自发现推理结构能有效提高LLM的推理能力。
---

## Abstract
We introduce SELF-DISCOVER, a general framework for LLMs to self-discover the task-intrinsic reasoning structures to tackle complex reasoning problems that are challenging for typical prompting methods. Core to the framework is a self-discovery process where LLMs select multiple atomic reasoning modules such as critical thinking and step-by-step thinking, and compose them into an explicit reasoning structure for LLMs to follow during decoding. SELF-DISCOVER substantially improves GPT-4 and PaLM 2’s performance on challenging reasoning benchmarks such as BigBench-Hard, grounded agent reasoning, and MATH, by as much as 32% compared to Chain of Thought (CoT). Furthermore, SELF-DISCOVER outperforms inference-intensive methods such as CoT-Self-Consistency by more than 20%, while requiring 10-40x fewer inference compute. Finally, we show that the self-discovered reasoning structures are universally applicable across model families: from PaLM 2-L to GPT-4, and from GPT-4 to Llama2, and share commonalities with human reasoning patterns.

---

## 论文详细总结（自动生成）

# SELF-DISCOVER: 大语言模型自组合推理结构 —— 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题背景**：尽管 Chain-of-Thought (CoT)、分解式提示等已有方法提升了 LLM 的推理能力，但每种方法本质上都是一种**原子化推理模块**，隐含了对任务处理过程的先验假设（例如 CoT 假设逐步推理适用于所有任务）。  
- **核心动机**：每个复杂推理任务都有其**内在的、独特的推理结构**（例如符号操作任务更适合分解式推理，而世界知识任务需要多角度思考）。现有方法无法自动适配这种结构。  
- **整体含义**：本文提出 **SELF-DISCOVER**，让 LLM 自主**选择、适配并组合多个原子推理模块**，形成显式的、任务特定的推理结构，从而更高效、更准确地解决复杂推理问题。

## 2. 方法论：核心思想与关键技术细节

### 2.1 核心思想
- 受人类解决问题时先**内部检索技能、适配、组合**的启发，SELF-DISCOVER 将推理结构发现分为两个阶段：**Stage 1（任务级发现）** 和 **Stage 2（实例级推理）**。  
- 一组预定义的 **39 个原子推理模块描述**（如“批判性思考”、“分步思考”、“简化问题”、“反思”等）作为种子，LLM 通过元提示自主执行三项动作：**SELECT → ADAPT → IMPLEMENT**。

### 2.2 关键技术细节

#### Stage 1：任务级自发现推理结构
1. **SELECT（选择）**：给定任务实例（无标签）和所有模块描述，LLM 选出对当前任务有用的子集 \(D_S\)。  
2. **ADAPT（适配）**：将每个选中的模块描述重新表述为更贴近任务的具体指令（例如从“分解问题”改写为“计算每个颜色的物品数量”）。  
3. **IMPLEMENT（实现）**：将适配后的模块整合成一个**可执行的推理计划**，以 JSON 键值对格式输出。结构要求模型在解码时依次填充每个键对应的值，并最终输出答案。

#### Stage 2：使用发现的结构求解实例
- 每个测试实例直接拼接任务级得到推理结构，然后模型按结构中的键顺序逐步推理并填入具体内容，最后给出答案。

### 2.3 算法流程（文字描述）
```
输入：任务 T，一组推理模块描述 D，任务实例样例 t_i（无标签）
Stage 1：
  1. SELECT: D_S = M(选择提示 || D || t_i)
  2. ADAPT: D_A = M(适配提示 || D_S || t_i)
  3. IMPLEMENT: D_I = M(实现提示 || 人类示例结构 || D_A || t_i)
Stage 2：
  对每个实例 t ∈ T：
    A = M(结构 D_I || t)   // 模型按结构填充 JSON 并得出答案
```

## 3. 实验设计

### 3.1 数据集与场景
| 基准 | 任务数 | 说明 |
|------|--------|------|
| **Big-Bench Hard (BBH)** | 23 个 | 涵盖算法/算术、自然语言理解、世界知识、多语言知识四类 |
| **Thinking for Doing (T4D)** | 1 个（但包含多样场景） | 基于心理状态推理的社会智能体任务 |
| **MATH** | 200 个子样本 | 数学竞赛题，测试复杂计算推理 |

### 3.2 对比方法
- **直接提示（Direct）**：不生成中间推理步骤  
- **CoT（Chain-of-Thought）**  
- **Plan-and-Solve (PS)**  
- **CoT + Self-Consistency**（采样 10 次）  
- **Majority Voting of each RM**（分别使用每个推理模块+多数投票）  
- **Best of each RM**（假设有标签，选择最高分模块）  
- **OPRO**（提示优化方法，需训练集）  
- 额外对比：**Tree-of-Thought (ToT)**、**Graph-of-Thought (GoT)**（附录 D 补充）

### 3.3 模型
- **PaLM 2-L**（指令微调版）  
- **GPT-4**（gpt-4-turbo-preview）  
- **GPT-3.5-turbo**  
- **Llama-2-70B**

## 4. 资源与算力

- 文中**未明确说明**使用的 GPU 型号、数量或训练时长，因为该方法**仅需推理调用**，无需额外训练。  
- 在推理效率方面：SELF-DISCOVER 在任务级仅需要 **3 次额外推理调用**（SELECT/ADAPT/IMPLEMENT），实例级仅需 **1 次推理**。对比 CoT-Self-Consistency 需要 10 次采样，多数投票各模块需 40 次调用，因此 SELF-DISCOVER 在保证高性能的同时，推理计算量减少 **10–40 倍**。

## 5. 实验数量与充分性

- **实验量**：共在 25 个任务（23 BBH + T4D + MATH）上进行了主实验；同时在 BBH 的 4 个类别上进行了分类性能分析；在 4 个任务上进行了消融实验（SELECT、ADAPT、IMPLEMENT 三个动作）；进行了跨模型迁移性测试（PaLM 2-L → GPT-4，GPT-4 → Llama-2-70B，GPT-4 → GPT-3.5-turbo）；在 MATH 上进行了错误分析（200 样本人工标注）。  
- **充分性与公平性**：对比了多种基线方法（包括零样本、少样本、基于投票的、优化的提示等），覆盖了不同复杂度的推理场景。消融实验直接证明了每个动作的必要性。误差分析揭示了方法的主要瓶颈。整体实验设计**较为充分且客观**。

## 6. 主要结论与发现

1. **显著提升推理性能**：
   - 在 BBH 上，PaLM 2-L + SELF-DISCOVER 比 CoT 提升 **7%**（绝对值），GPT-4 提升 **6%**。
   - 在 T4D 上，提升幅度最大：PaLM 2-L 提升 **29%**，GPT-4 提升 **32%**（达到 85% 准确率）。
   - 在 MATH 上，提升相对温和（1.5%–2%），错误分析表明主要瓶颈在于计算执行错误（占失败案例 74.7%），推理结构本身正确率高达 87.5%。

2. **效率优势**：在仅需 1 次实例级推理的情况下，效果超过需要 10–40 次推理的 CoT-Self-Consistency 和多数投票方法。

3. **通用性与可迁移性**：由 PaLM 2-L 发现的结构直接用于 GPT-4 仍能保持良好性能，GPT-4 发现的结构也能提升 Llama-2-70B 和 GPT-3.5-turbo 的推理能力；结构特征与人类推理模式具有共性。

4. **消融实验验证**：SELECT、ADAPT、IMPLEMENT 三个步骤缺一不可，每一步都带来持续改进。

## 7. 优点

- **创新性**：首次提出让 LLM 自主组合多个原子推理模块形成结构化推理方案，突破了单一模块（如 CoT）的先验限制。
- **高效性**：相比需要多次采样的方法，推理成本降低 10–40 倍，且无需额外训练或人工标注标签。
- **可解释性**：自发现的结构以 JSON 形式输出，清晰展示了模型认为解决问题应遵循的步骤，比优化后的提示词更易理解。
- **通用性**：结构可在不同模型家族之间迁移，表明其捕获了任务的内在规律而非特定模型的偏好。
- **实验充分**：覆盖多类推理任务，进行了消融、迁移、误差分析等多种验证，结论可靠。

## 8. 不足与局限

- **对数学计算的局限性**：在 MATH 上提升较小，主要失败来自计算执行错误（74.7%），即使推理结构正确，模型仍无法准确完成算术运算。这表明方法依赖于 LLM 的计算能力，未来可能需要结合外部工具（如代码执行器）来克服。
- **任务级 vs 实例级**：对于非常开放的任务（如 MMLU），任务级结构可能不够精细，而实例级发现会增加成本。文中在附录中展示了实例级 SELF-DISCOVER 在 MMLU 上比 CoT 提升 7.2%，但主实验仍以任务级为主，可能未充分探索实例级的收益。
- **模块依赖**：方法依赖一组预定义的 39 个推理模块（来自 Fernando et al. 2023），这些模块可能不能覆盖所有推理类型或存在文化/领域偏差。不同模块的选择和描述方式也可能影响最终结构质量。
- **计算与 token 开销**：尽管推理调用次数少，但 Stage 1 的三次对话生成较长（任务级），且 Stage 2 的 JSON 结构可能增加输入 token 数，对于资源受限环境仍需注意。
- **未报告统计显著性**：文中虽然报告了平均提升，但未给出误差条或置信区间，尤其对于随机性较大的 LLM 任务，结果可能存在一定波动。

（完）
