---
title: "Buffer of Thoughts: Thought-Augmented Reasoning with Large Language Models"
title_zh: 思维缓冲区：用大模型进行思维增强推理
authors: "Ling Yang, Zhaochen Yu, Tianjun Zhang, Shiyi Cao, Minkai Xu, Wentao Zhang, Joseph E. Gonzalez, Bin CUI"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=ANO1i9JPtb"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 通过元缓冲区提升推理准确率和效率
tldr: BoT存储高层思考模板以改进大模型推理。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-ano1i9jptb/fig-001.webp\", \"caption\": \"\", \"page\": 9, \"index\": 1, \"width\": 1462, \"height\": 1104}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-ano1i9jptb/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1462, \"height\": 1104}]"
motivation: 现有推理方法缺乏通用性和鲁棒性。
method: 构建元缓冲区存储问题解决的高层思维模板，动态检索实例化。
result: 在10个推理基准上取得更好准确率和效率。
conclusion: 思维增强方法可显著提升推理性能。
---

## Abstract
We introduce Buffer of Thoughts (BoT), a novel and versatile thought-augmented reasoning approach for enhancing accuracy, efficiency and robustness of large language models (LLMs). Specifically, we propose meta-buffer to store a series of informative high-level thoughts, namely thought-template, distilled from the problem-solving processes across various tasks. Then for each problem, we retrieve a relevant thought-template and adaptively instantiate it with specific reasoning structures to conduct efficient reasoning. To guarantee the scalability and stability, we further propose buffer-manager to dynamically update the meta-buffer, thus enhancing the capacity of meta-buffer as more tasks are solved. We conduct extensive experiments on 10 challenging reasoning-intensive tasks, and achieve significant performance improvements over previous SOTA methods: 11\% on Game of 24, 20\% on Geometric Shapes and 51\% on Checkmate-in-One. Further analysis demonstrate the superior generalization ability and model robustness of our BoT, while requiring only 12\% of the cost of multi-query prompting methods (e.g., tree/graph of thoughts) on average. Code is available at: https://github.com/YangLing0818/buffer-of-thought-llm

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

现有的大语言模型（LLM）推理方法主要分为两类：
- **单查询推理**（如CoT、Few-shot）：依赖人工设计的特定任务提示或示例，缺乏通用性和泛化能力。
- **多查询推理**（如ToT、GoT）：需要递归扩展推理路径，计算成本高，且忽视了从先前任务中总结通用高层指导（high-level guidelines）的可能性。

论文指出，这两种方法都未能利用历史问题解决过程中产生的通用高层思维，导致推理效率低、准确性受限、鲁棒性不足。为此，论文提出**Buffer of Thoughts (BoT)**，一种新颖的、通用的思维增强推理框架，旨在同时提升LLM推理的准确性、效率和鲁棒性。

### 2. 论文提出的方法论：核心思想、关键技术细节

**核心思想**：构建一个轻量级的元缓冲区（meta-buffer），存储从多种任务问题解决过程中蒸馏出的通用高层思维模板（thought-template）。对于每个新问题，检索最相关的模板，并自适应地实例化为具体的推理结构进行高效推理。同时，通过缓冲管理器（buffer-manager）动态更新元缓冲区，使其随任务处理而持续扩展。

**关键技术细节**：

1. **问题蒸馏器（Problem Distiller）**：在推理之前，先对输入问题进行关键信息提取和形式化，包括：关键参数/变量、目标与约束。将复杂问题转化为高层概念和结构，便于后续模板检索。使用元提示（meta prompt）实现。

2. **思维模板（Thought Template）**：元缓冲区中存储的高层思维指导。BoT将模板分为6大类别：文本理解、创造性语言生成、常识推理、数学推理、代码编程、应用调度。每个模板包含描述和对应类别。

3. **模板检索**：对于每个问题，计算蒸馏后问题描述与各模板描述之间的嵌入相似度，选择相似度超过阈值 δ（推荐0.5~0.7）的最高分模板。若低于阈值，则认为是新任务，使用预设的通用模板。

4. **实例化推理（Instantiated Reasoning）**：
   - 若检索到模板：结合蒸馏信息和模板，通过实例化推理器（LLM）生成解决方案。
   - 若为新任务：使用三个预定义粗粒度模板之一进行推理。

5. **缓冲管理器（Buffer Manager）**：
   - **模板蒸馏**：从每个问题解决过程中提取通用模板，包括三步：核心任务总结、解决方案步骤描述、通用回答模板生成。使用精心设计的上下文示例（包括任务内和跨任务示例）增强泛化能力。
   - **动态更新**：计算新蒸馏模板与现有模板描述的相似度，仅当相似度低于阈值时更新到元缓冲区，避免冗余，保持轻量。

整个推理流程：输入问题 → 问题蒸馏 → 模板检索 → 实例化推理 → 输出答案 → 缓冲管理器更新元缓冲区。

### 3. 实验设计：数据集、基准、对比方法

**数据集/任务（共10个）**：
- Game of 24
- BIG-Bench Hard (BBH)：Geometric Shapes、Multi-Step Arithmetic Two、Word Sorting
- BIG-Bench：Checkmate-in-One、Penguins、Date Understanding
- Python Programming Puzzles (P3)
- Multilingual Grade School Math (MGSM)
- Shakespearean Sonnet Writing

**基准方法**：
- Standard Prompting（无引导）
- 单查询方法：Zero-shot CoT、PAL、Expert Prompting
- 多查询方法：ToT、GoT、Meta Prompting

**主要实验**：使用GPT-4作为基座模型，对比各方法在10个任务上的准确率（表1）。此外，还进行了推理时间比较（图3）和鲁棒性比较（成功率为评估指标，随机采样1000个样本重复10次，图4）。

### 4. 资源与算力

论文明确提到：实验使用NVIDIA A100-PCIE-40GB GPU。但未具体说明GPU的数量、训练或推理的总时长。主要实验基于GPT-4（API调用），部分分析实验使用了Llama3-8B和Llama3-70B模型。因此算力信息不够详细。

### 5. 实验数量与充分性

- **主要性能实验**：在10个不同难度的推理任务上进行了系统比较，覆盖数学、算法、常识、创造性写作等多种类型。
- **消融实验**：对buffer-manager（图7、图10）、problem-distiller（图8）、meta-buffer（图9）分别进行了消融，使用Llama3-70B和GPT-4作为基座模型，验证各组件贡献。
- **鲁棒性实验**：采用成功率为指标，随机采样1000样本重复10次，评估稳定性。
- **效率分析**：比较了各方法的推理时间（图3），并分析了BoT各组件的时间分布（图5右）。
- **模板质量分析**：在MATH-500上对比自动模板与手动模板的准确率（表2），显示自动模板更优且随积累提升。
- **模型规模与性能权衡**：使用Llama3-8B和Llama3-70B在三个任务上验证BoT的效果（图6）。
- **模板分布分析**：展示了不同任务生成模板数量的差异（图5左）。

总体而言，实验覆盖全面，消融设计合理，对比基线方法具有代表性，结果客观。但缺乏在更多开源模型上的验证，且未报告标准误差或置信区间。

### 6. 论文的主要结论与发现

- BoT在10个推理任务上均取得最先进（SOTA）性能，显著超越以往方法：Game of 24提高11%，Geometric Shapes提高20%，Checkmate-in-One提高51%。
- 推理效率极高：平均仅需多查询方法（如ToT、Meta-Prompting）成本的12%。
- 鲁棒性强：成功率平均比第二名高10%。
- 泛化能力好：Llama3-8B + BoT有潜力超越Llama3-70B单独表现，展示了小模型通过BoT接近甚至超越大模型的可能性。
- 缓冲管理器的动态更新机制使准确率随任务处理轮次增加而持续提升，同时推理时间下降。
- 问题蒸馏器在复杂任务中作用显著，而简单任务中影响较小。

### 7. 优点：方法或实验设计上的亮点

- **创新性思维增强范式**：提出元缓冲区存储高层思维模板，而非具体实例，兼具通用性与灵活性，类似人类认知中的归纳与类比。
- **高效**：通过检索和实例化一次查询即可完成推理，避免了多查询方法的复杂搜索。
- **可扩展**：缓冲管理器支持增量学习，元缓冲区不断丰富，性能随时间提升。
- **鲁棒性**：高层思维模板减少了任务间的差异，使模型输出更稳定。
- **实验设计全面**：覆盖10种多样化任务，多种消融实验，以及模型规模与性能的权衡分析，验证充分。
- **开源代码**：提供项目仓库，可复现。

### 8. 不足与局限

- **算力信息不完整**：未报告GPT-4 API调用的具体次数、成本或GPU训练细节，影响可复现性。
- **基座模型单一**：主要实验基于GPT-4，虽在分析中使用了Llama3系列，但整体泛化性对其他模型（如开源模型的中文版本）未验证。
- **模板分类固定**：6类模板是否能覆盖所有复杂任务？可能在新颖或跨领域任务上检索匹配效果不佳。
- **阈值依赖**：检索阈值δ由人工设定（0.5~0.7），缺乏自适应机制，可能影响新任务的识别和模板更新。
- **未考虑任务关联性**：模板检索仅基于文本相似度，未深入利用任务间结构关系。
- **潜在偏差风险**：模板蒸馏过程依赖LLM自身输出，可能继承或放大模型偏见。
- **应用限制**：BoT需要维护元缓冲区并管理模板更新，在持续学习和部署中可能带来额外维护成本。

（完）
