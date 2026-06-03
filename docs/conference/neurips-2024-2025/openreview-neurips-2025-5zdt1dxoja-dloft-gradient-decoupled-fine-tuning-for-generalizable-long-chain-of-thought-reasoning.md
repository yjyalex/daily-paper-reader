---
title: "DLoFT: Gradient-Decoupled Fine-Tuning for Generalizable Long Chain-of-Thought Reasoning"
title_zh: DLoFT：梯度解耦微调实现可泛化的长思维链推理
authors: "Sitong Wu, Haoru Tan, Jingyao Li, Shaofeng Zhang, XIAOJUAN QI, Bei Yu, Jiaya Jia"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=5ZDT1dxojA"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 长思维链推理的泛化能力提升
tldr: 通过梯度解耦防止长思维链微调中的过拟合。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1560, \"height\": 686}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1926, \"height\": 1070}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-003.webp\", \"caption\": \"\", \"page\": 10, \"index\": 3, \"width\": 1982, \"height\": 698}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-004.webp\", \"caption\": \"\", \"page\": 18, \"index\": 4, \"width\": 1448, \"height\": 2010}]"
motivation: 监督微调长思维链轨迹会导致模型过拟合问题特定知识，降低泛化能力。
method: 提出解耦微调算法，分离推理内容与问题特定信息的学习。
result: 模型在分布外场景下推理性能显著提升。
conclusion: 梯度解耦有效提升长思维链推理的泛化性。
---

## Abstract
Long chain-of-thought (LongCoT) has emerged as a powerful reasoning paradigm for enabling large language models (LLMs) to solve complex tasks through a systematic and thorough thinking phase.
Although supervised fine-tuning (SFT) on high-quality LongCoT traces has proven effective to activate LongCoT abilities, we find that models trained in this way tend to overfit problem-specific knowledge and heuristics, leading to degraded out-of-distribution performance.
To address this issue, we propose a Decoupled LongCoT Fine-Tuning (DLoFT) algorithm, which enables the model to learn generalizable LongCoT reasoning abilities while preventing overfitting to the reasoning content with problem-specific information.
The key idea is to decouple the gradient into two orthogonal components: 1) a paradigm-relevant gradient corresponding to the general LongCoT paradigm and 2) a content-relevant gradient reflecting the problem-specific information, where only the former gradient is used to update model parameters.
Specifically, by leveraging the unique two-phase composition (thinking and solution) of the LongCoT response, our gradient decoupling mechanism isolates the content-relevant gradient via a projection operation and separates the paradigm-relevant gradient through orthogonalization.
Our DLoFT ensures the model concentrate on internalizing the LongCoT paradigm rather than memorizing problem-specific knowledge and heuristics.
Extensive experiments demonstrate that our DLoFT significantly improves the generalization behavior of LongCoT abilities compared to SFT while maintaining strong in-distribution performance.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：长思维链（Long Chain-of-Thought, LongCoT）推理是一种有效的复杂任务求解范式，但现有的监督微调（SFT）方法在高品质LongCoT轨迹上训练时，容易过拟合问题特定的知识和启发式策略，导致在分布外（out-of-distribution）场景下泛化能力显著下降。例如，在数学竞赛数据上训练的模型在医学、工程等未见领域上性能大幅退步。
- **研究动机**：构建覆盖所有领域的大规模LongCoT数据集成本极高且不现实，因此需要一种方法，使模型在少量代表性数据上学习到可泛化的LongCoT推理能力，避免对问题特定内容的记忆。
- **整体含义**：提出了一种梯度解耦微调算法（DLoFT），通过分离与推理范式相关的梯度和与问题特定内容相关的梯度，仅用前者更新模型，从而让模型专注于内化通用的LongCoT推理模式，提高泛化性。

## 2. 论文提出的方法论

- **核心思想**：利用LongCoT响应固有的两阶段结构（思考阶段 + 解决方案阶段），将完整响应的梯度分解为两个正交分量：
  - **范式相关梯度**（g_par）：对应通用的LongCoT推理范式。
  - **内容相关梯度**（g_con）：对应问题特定的知识和启发式。
- **关键技术细节**：
  1. **计算完整梯度**：对问题P_i和完整LongCoT响应（思考过程T_i与解决方案S_i拼接）计算负对数似然损失，得到梯度g_full。
  2. **计算参考梯度**：仅对解决方案S_i（不包含思考过程）计算损失，得到梯度g_ref，该梯度只反映问题特定内容。
  3. **投影与正交化**：
     - 通过投影操作提取内容相关梯度：g_con = (⟨g_full, g_ref⟩ / ‖g_ref‖²) · g_ref。
     - 通过正交化操作得到范式相关梯度：g_par = g_full - g_con。
  4. **参数更新**：仅使用g_par更新模型参数，丢弃g_con。
- **算法流程**：每个训练步骤中，先从批次数据采样，依次计算g_full、g_ref，再执行投影和正交化解耦，最后用g_par更新参数（详细见论文Algorithm 1）。

## 3. 实验设计

- **使用数据集**：
  - **混合领域数据集**：s1K（1000条高质量LongCoT数据，涵盖数学、编程、科学、谜题等）。
  - **单领域数据集**：OpenR1-Math-5K（数学）、OpenThoughts-Code-5K（编程）、Medical-o1-5K（医学），各从原始大样本中随机抽取5000条。
- **评估基准（17个领域）**：
  - **数学**：AIME24、MATH-500、OlympiadBench。
  - **编程**：LiveCodeBench (v2)。
  - **医学**：MedQA。
  - **其他领域**：来自SuperGPQA的物理、化学、生物学、计算机、机械、电子、通信、天文、地理、土木、农业、经济、历史、法律、文学、哲学、社会学等子集。
- **对比方法**：
  - 标准SFT（直接模仿完整LongCoT响应）。
  - 权重衰减正则化（A.3节）。
  - 仅使用g_con或g_par更新的对比（A.2节）。
  - 作为强化学习（RL）冷启动阶段与SFT+RL对比，RL使用GRPO算法。
- **模型**：
  - 通用LLM：Qwen2.5-7B/32B-Instruct。
  - 领域专用LLM：Qwen2.5-Math-7B-Instruct（数学）、Qwen2.5-Coder-7B-Instruct（编程）、Meditron-7B（医学）。

## 4. 资源与算力

- **文中信息**：未明确说明使用的GPU型号、数量或总训练时长。附录A.4给出了效率对比：每个训练步SFT约6.2分钟，DLoFT约6.8分钟；GPU内存分别为69986MB和70684MB（模型为Qwen2.5-7B-Instruct，数据集s1K）。但未提供具体硬件配置。
- **作者说明**：在“Limitations”部分指出，受限于计算资源，未能在更大规模（如数百亿参数模型、超大模型）上验证。
- **总结**：论文未披露完整的算力开销，现有信息仅能反映相对计算开销较小（DLoFT额外步骤很少）。

## 5. 实验数量与充分性

- **实验数量**：涉及多组实验：
  - 通用模型在17个领域上的主实验（图3，对比SFT与DLoFT的相对性能变化）。
  - 领域专用模型在三种领域上，分别使用in-domain和out-domain数据训练（图4），并包含后续RL阶段。
  - 消融实验（附录A.1–A.3）：投影操作的影响、梯度解耦有效性（比较g_con与g_par）、权重衰减正则化。
  - 效率分析（A.4）。
  - 作为RL冷启动的额外实验（图4标注）。
- **充分性与公平性**：实验设计较为充分。所有对比均使用相同训练设置（学习率、优化器、epoch等）；评估涵盖大量分布内和分布外领域，减少了偶然性。消融实验逐一验证了各个组件的必要性。对比基线包括标准SFT、常见正则化方法等，结论客观。
- **潜在不足**：未在更大模型（如70B以上）或更多样化的数据集上验证；部分结果（如图3）未报告误差棒，但论文提到“error bars”已在回答中说明（实际正文中未显示误差棒，可能缺失）。

## 6. 论文的主要结论与发现

- DLoFT显著优于SFT，在分布外领域平均提升+16.4点（7B模型），在分布内领域也提升+11.5点。
- DLoFT有效缓解了SFT中出现的过拟合问题，模型能在训练过程中持续提升分布外性能，而SFT则出现性能退化（图1）。
- DLoFT作为RL冷启动阶段时，可以摆脱对领域内数据的依赖（仅用公共跨领域数据即可达到甚至超过SFT+in-domain数据的效果），并且能放大RL的增益（RL后性能提升更大）。
- 梯度解耦机制成功分离了范式相关与内容相关信号：仅用g_par更新的模型展现出100%的LongCoT行为，而仅用g_con更新的模型则完全不出现LongCoT行为（表2），验证了解耦的有效性。
- 权重衰减等常规正则化方法无法有效防止此类过拟合（表3），凸显了DLoFT的独特价值。

## 7. 优点

- **创新性强**：首次利用LongCoT响应的两阶段结构设计梯度解耦方法，思路新颖且直观。
- **效果显著**：在多种模型和广泛领域上验证，泛化能力大幅超越SFT，且在分布内也有增益。
- **实用高效**：额外计算开销极小（每步仅增加约0.6分钟，内存增加约700MB），易于替代现有SFT流程。
- **对RL友好**：作为冷启动能降低对领域内数据的依赖，并提升RL最终性能，具有实际部署价值。
- **实验系统**：覆盖通用模型和三种领域专用模型，消融实验充分，对比公平。

## 8. 不足与局限

- **实验规模受限**：仅验证了7B和32B参数规模的模型，未在更大模型（如70B、300B+）或更大数据集（如百万级）上测试，泛化性结论可能受限于模型规模。
- **资源信息不透明**：未提供GPU型号、数量、总训练时长等关键算力信息，不利于复现和成本评估。
- **基准覆盖仍有盲区**：尽管涉及17个领域，但大多来自SuperGPQA的子集，某些领域可能数据量少或代表性不足；此外，未评估多语言或动态任务。
- **潜在的负作用**：论文未讨论该方法是否可能带来新的偏差（如过于侧重推理范式而忽视领域知识），也未分析对模型安全性的影响。
- **缺少统计不确定性**：主要结果未给出方差或置信区间，难以判断性能提升的显著性。
- **代码与数据未公开**（投稿时匿名，但声称将公开），目前无法独立验证。

（完）
