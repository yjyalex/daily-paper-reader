---
title: "SELF-DISCOVER: Large Language Models Self-Compose Reasoning Structures"
title_zh: "SELF-DISCOVER: 大语言模型自组合推理结构"
authors: "Pei Zhou, Jay Pujara, Xiang Ren, Xinyun Chen, Heng-Tze Cheng, Quoc V Le, Ed H. Chi, Denny Zhou, Swaroop Mishra, Steven Zheng"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=BROvXhmzYK"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 通过自组合推理结构增强LLM推理能力
tldr: 自发现推理结构在困难基准上超越思维链。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1640, \"height\": 970}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 1640, \"height\": 960}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 872, \"height\": 431}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-004.webp\", \"caption\": \"\", \"page\": 3, \"index\": 4, \"width\": 1000, \"height\": 492}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-005.webp\", \"caption\": \"\", \"page\": 3, \"index\": 5, \"width\": 583, \"height\": 261}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-006.webp\", \"caption\": \"\", \"page\": 3, \"index\": 6, \"width\": 830, \"height\": 231}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 596, \"height\": 1000}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 533, \"height\": 275}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 533, \"height\": 293}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-010.webp\", \"caption\": \"\", \"page\": 4, \"index\": 10, \"width\": 921, \"height\": 233}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-brovxhmzyk/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 1000, \"height\": 357}]"
motivation: 复杂推理问题对典型提示方法具有挑战。
method: 让LLM自发现并组合多个原子推理模块形成显式推理结构。
result: "在多个推理基准上超过思维链等方法达32%。"
conclusion: 自发现推理结构能有效提升LLM的复杂推理能力。
---

## Abstract
We introduce SELF-DISCOVER, a general framework for LLMs to self-discover the task-intrinsic reasoning structures to tackle complex reasoning problems that are challenging for typical prompting methods. Core to the framework is a self-discovery process where LLMs select multiple atomic reasoning modules such as critical thinking and step-by-step thinking, and compose them into an explicit reasoning structure for LLMs to follow during decoding. SELF-DISCOVER substantially improves GPT-4 and PaLM 2’s performance on challenging reasoning benchmarks such as BigBench-Hard, grounded agent reasoning, and MATH, by as much as 32% compared to Chain of Thought (CoT). Furthermore, SELF-DISCOVER outperforms inference-intensive methods such as CoT-Self-Consistency by more than 20%, while requiring 10-40x fewer inference compute. Finally, we show that the self-discovered reasoning structures are universally applicable across model families: from PaLM 2-L to GPT-4, and from GPT-4 to Llama2, and share commonalities with human reasoning patterns.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型（LLM）在复杂推理任务上仍面临挑战，现有提示方法如思维链（Chain-of-Thought, CoT）虽然引入逐步推理，但都是预设单一原子推理模块（如“逐步思考”），未考虑不同任务具有内在独特的推理结构。例如，符号操作任务更适合分解式提示，而科学问题可能需要反思式思考。因此，需要一种方法让模型自动发现并组合适合特定任务的推理结构。
- **整体含义**：本文提出的 **SELF-DISCOVER** 框架，让 LLM 通过元推理自行发现任务的推理结构，从而显著提升推理能力。这项工作展示了“结构化推理”的潜力，使 LLM 在复杂问题上超越传统提示方法，且推理结构可跨模型迁移，具有通用性和可解释性。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：受人类问题解决中“制定推理程序”启发，SELF-DISCOVER 让 LLM 从一组原子推理模块（如“批判性思考”、“分解为子问题”等）中挑选、调整并组合成一个显式的、任务特定的推理结构（以 JSON 键值对形式），然后在解码时引导模型按该结构逐步输出答案。
- **关键技术细节**：
  - **原子推理模块集合**：包含 39 个高层次启发式模块（如“逐步思考”、“创造性思考”、“风险分析”等），来源于 Fernando et al. (2023)。
  - **Stage 1：发现推理结构（任务级）**，包含三个动作：
    - **SELECT**：根据任务示例，从模块集合中选出相关模块。
    - **ADAPT**：将选出的模块描述重新表述为任务特定版本。
    - **IMPLEMENT**：将调整后的模块组合成一个可执行的推理结构（JSON 格式），并给出结构各键的填充指令。
  - **Stage 2：用发现的结构解决问题（实例级）**：对每个任务实例，将结构拼接到输入中，指导 LLM 按键顺序生成答案。
- **无需标签与训练**：Stage 1 仅需几个无标签任务示例，通过元提示（meta-prompt）实现；Stage 2 直接使用该结构推理。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - **BIG-Bench Hard (BBH)**：23 个具有挑战性的推理任务，涵盖算法、自然语言理解、世界知识、多语言等四类。
  - **Thinking for Doing (T4D)**：基于社会智能的智能体推理任务，需心理状态推理来决定行动。
  - **MATH**：数学竞赛级问题，实验中随机抽取 200 个示例。
  - 额外在 MMLU 子集（10 个学科，每科 50 题）上进行了补充实验。
- **对比方法**：
  - 直接提示（Direct Prompting）
  - 零样本 CoT（Kojima et al., 2022）
  - Plan-and-Solve (PS)（Wang et al., 2023）
  - CoT + Self-Consistency（采样 10 次）
  - 每个原子模块的多数投票（Majority voting of each RM）
  - 每个原子模块的最佳结果（Best of each RM，需 oracle 标签）
  - Tree-of-Thought (ToT) 和 Graph-of-Thought (GoT)（零样本版本）
  - 提示优化方法 OPRO（Yang et al., 2023）
- **模型**：GPT-4、GPT-3.5-turbo、PaLM 2-L、Llama2-70B。

### 4. 资源与算力

- **论文未明确说明使用的 GPU 型号、数量或训练时长**。方法仅涉及 LLM 推理调用（API 或部署），无需额外训练。作者提到 SELF-DISCOVER 在 Stage 1 每个任务仅需 3 次额外推理调用（SELECT、ADAPT、IMPLEMENT），Stage 2 每个实例仅需 1 次推理，总计算量远低于 CoT-Self-Consistency 等方法。可认为该方法算力需求较低。

### 5. 实验数量与充分性

- **实验数量**：
  - 主实验：3 个主要基准（BBH 23 任务 + T4D + MATH），且使用了两种模型（PaLM 2-L 和 GPT-4），共约 25×2 = 50 组结果。
  - 消融实验：在 4 个任务（Snarks、Movie、T4D、Geometry）上测试 SELECT、ADAPT、IMPLEMENT 各步骤的必要性。
  - 迁移实验：将 PaLM 2-L 发现的结构用到 GPT-4，以及将 GPT-4 发现的结构用到 Llama2-70B 和 GPT-3.5。
  - 效率对比：在 2 个任务上比较推理调用次数与性能。
  - 附加实验：MMLU 子集（10 任务）、ToT/GoT 对比、实例级 SELF-DISCOVER 等。
- **充分性与公平性**：实验设计较为全面，覆盖了多种推理类型、多种模型、多种基线。消融实验验证了各步骤有效性。与 CoT-Self-Consistency 等相比公平（采用相同采样次数或 oracle 知识）。但 MATH 仅用 200 个子样本，可能存在抽样偏差；BBH 和 T4D 上均使用完整数据集。总体较为充分。

### 6. 论文的主要结论与发现

- SELF-DISCOVER 在 BBH、T4D、MATH 上均显著优于 CoT、Plan-and-Solve 等基线，性能提升最高达 32%（T4D 上 PaLM 2-L 从 40%→69%）。
- 相比需要多次采样的 CoT-Self-Consistency 和多数投票方法，SELF-DISCOVER 以 **10-40 倍更少的推理调用** 取得更好或相当的性能。
- 自发现推理结构在模型间具有**跨模型可迁移性**：PaLM 2-L 发现的结构在 GPT-4 上依然有效，GPT-4 发现的结构在 Llama2-70B 上也优于 CoT。
- 与人类推理模式存在**共通性**：结构包含逐步分析、反思、分解等元素。
- 错误分析表明，MATH 上 74.7% 的错误来自计算执行，而非推理结构本身（87.5% 的结构是正确的）。

### 7. 优点：方法或实验设计上的亮点

- **零样本高效性**：无需任何标注数据或训练，仅凭任务描述和示例即可发现结构。
- **可解释性**：推理结构以 JSON 形式呈现，清晰展示了模型认为的任务关键步骤，比优化后的黑盒提示更易理解。
- **通用性与可迁移性**：结构可在不同模型、不同任务间迁移，且与人类推理模式相似，说明其捕捉了任务内在逻辑。
- **性能与效率双赢**：仅需少量额外调用即可大幅提升性能，尤其适合大规模部署。
- **消融实验设计合理**：逐步验证了 SELECT、ADAPT、IMPLEMENT 三步的必要性。

### 8. 不足与局限

- **计算错误瓶颈**：在 MATH 上，即使推理结构正确，模型仍频繁出现算术执行错误，需借助工具（如代码生成）来改进。
- **结构质量依赖 LLM 能力**：较小模型（如 Llama2-70B）难以自行生成有效结构（论文注释提到，尝试零样本元提示 Llama2 但输出质量低），需借助 GPT-4 生成后迁移。
- **实验覆盖有限**：MATH 仅用 200 个子样本，可能不能完全代表全测试集；未在更多开放领域任务（如常识推理）上验证。
- **评估偏差**：答案提取采用启发式规则（如“Thus, the final answer is [X]”），存在少量遗漏或错误，尤其是 MATH 中需人工校验（论文已说明手动检查和标注）。
- **缺乏与搜索式方法（如 ToT/GoT）的深度比较**：虽然附录补充了 ToT/GoT 结果，但未采用与 SELF-DISCOVER 等量的推理预算（如相同采样次数）进行对比，可能不够公平。
- **未讨论负面社会影响**：作者认为此为基础研究，无直接社会影响，但改进的推理能力可能被滥用（如生成更逼真的虚假信息）。

（完）
