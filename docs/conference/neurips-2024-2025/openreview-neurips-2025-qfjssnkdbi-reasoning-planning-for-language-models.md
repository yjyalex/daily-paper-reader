---
title: Reasoning Planning for Language Models
title_zh: 语言模型的推理规划
authors: "Bao Nguyen, Hieu Trung Nguyen, Ruifeng She, Xiaojin Fu, Viet Anh Nguyen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QFjssnKdBI"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 推理方法选择框架
tldr: 提出EPIC，结合对比学习的集成规划框架，为查询选择合适推理方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 1189, \"height\": 790}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-007.webp\", \"caption\": \"\", \"page\": 27, \"index\": 7, \"width\": 857, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-008.webp\", \"caption\": \"\", \"page\": 27, \"index\": 8, \"width\": 860, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-009.webp\", \"caption\": \"\", \"page\": 27, \"index\": 9, \"width\": 857, \"height\": 701}]"
motivation: 现有方法假设更多候选答案提高准确性，但缺乏理论支撑。
method: 通过理论推导准确率边界，并用对比学习学习查询与推理方法的共享表示。
result: EPIC在多个基准上优于固定推理方法。
conclusion: 规划推理方法可提升LLM性能。
---

## Abstract
Selecting an appropriate reasoning method for a given query remains a key challenge in language model generation. Existing approaches typically generate multiple candidate responses and use an aggregation strategy to select the output answer, often assuming that more candidate answers yield higher accuracy. We revisit this assumption through a rigorous theoretical analysis, deriving accuracy bounds for standard aggregation methods under fixed generation distributions and candidate sizes. Building on these insights, we introduce EPIC, an Ensemble Planning with Contrastive learning framework to learn a shared representation space that captures both model reasoning abilities and query-method compatibility. EPIC incorporates our probability bounds as a regularizer in a utility-driven optimization that balances accuracy and computational cost. Experiments on diverse mathematical reasoning tasks show that EPIC consistently selects optimal reasoning methods, improving accuracy while reducing computational overhead. Our code can be found at https://github.com/nguyenngocbaocmt02/EPIC.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：为语言模型生成的查询（query）选择合适的推理方法（reasoning method）是一个关键挑战。现有方法通常生成多个候选答案并通过聚合策略（如多数投票、评分求和）选取最终输出，隐含假设“更多候选答案会带来更高准确率”。论文质疑这一假设，通过理论分析揭示其局限性。
- **整体含义**：不同推理方法对不同复杂度的查询效率与效果差异显著，统一使用同一静态策略会导致计算浪费或性能不足。因此，需要动态地为每个查询匹配最优推理方法，以平衡准确性与计算成本。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出 **EPIC（Ensemble Planning with Contrastive learning）** 框架，学习一个共享的表示空间，同时捕获模型推理能力与查询-方法兼容性。通过对比学习将查询嵌入与最优推理方法的嵌入拉近，并融入理论推导的概率界作为正则化项，引导训练样本更高效。
- **关键技术细节**：
  - **准确率-成本效用函数**：定义效用 \( u(a_{i,j}, c_{i,j}) = \lambda a_{i,j} + (1-\lambda)(1-c_{i,j}) \)，其中 \( a_{i,j} \) 为准确率，\( c_{i,j} \) 为归一化token成本，\( \lambda \in [0,1] \) 控制权衡。
  - **对比损失**：使用InfoNCE损失，将查询 \( x_j \) 的特征向量 \( f_j \) 通过轻量网络 \( g_\theta \) 映射为嵌入 \( g_\theta(f_j) \)，与正方法嵌入 \( v_{m^+} \) 相似度高，与负方法嵌入相似度低。
  - **概率正则项**：对仅候选数量 \( N \) 不同的方法对（共享相同生成策略），约束其嵌入相似度之比与理论概率界之比一致。使用定理3.1/3.2/3.3推导的概率界（如 \( e^{-N(\sqrt{p_1}-\sqrt{p_k})^2} \)）作为目标值，提升小样本下的学习效率。
  - **总损失**：\( \ell = \ell_{\text{contrastive}} + \tau \ell_{\text{reg}} \)，\( \tau \) 为超参数。
  - **推理时**：对新查询，计算嵌入与所有方法嵌入的相似度，选最高者作为推荐方法。

## 3. 实验设计
- **数据集与场景**：
  - **数学推理**：MATH 数据集（7,500 训练 / 500 测试），GSM8K 测试集（迁移评估）。
  - **代码生成**：LiveCodeBench。
- **Benchmark**：对比三大类方法：
  - **单个推理方法**：CoT-Greedy、Best-of-2/4/8/16、MCTS、Beam search。
  - **强模型参考**：DeepSeek-V3、OpenAI-o1-mini（不在方法池内）。
  - **替代选择方法**：Random Allocation (RA)、Offline Ada-BoK、Distributional Random Allocation (DRA-λ)、分类基线 (CL-λ)。
- **基础模型**：主实验使用 Qwen2.5-Math-7B-Instruct 生成，math-shepherd-mistral-7b-prm 作为奖励模型；迁移实验增加 Qwen2.5-Math-1.5B；代码实验使用 Qwen2.5-Coder-3B/7B。
- **方法池**：81 种推理方法，涵盖不同策略、温度、聚合方式（MV、PVM、PVL、PMM、PML）、候选数 \( N \in \{1,2,4,8,16\} \)。

## 4. 资源与算力
- **硬件**：单台机器，配备 8× NVIDIA RTX A5000 GPU（24 GB 显存）和 Intel Xeon Gold 6148 CPU @ 2.40GHz。
- **训练时长**：论文未明确说明具体训练时长（如运行小时数），仅提及在单台机器上完成所有实验。

## 5. 实验数量与充分性
- **实验组数**：
  - 主要结果：表1（对比多个 λ 下的 EPIC、基线），表2（迁移到 GSM8K、多模型规模）。
  - 消融实验：表示维度 d（16/32/64/128）、λ 变化（0.0~1.0）、不同效用函数（PMU）。
  - 额外实验：代码生成任务（表8）、嵌入空间可视化（图3）、定性案例分析（表9）。
  - 附录还给出完整结果表（表4）和方法池构建细节。
- **充分性与公平性**：
  - 对比基线丰富，涵盖同类及跨领域方法。
  - 使用相同方法池和训练/测试集，确保公平。
  - 实验覆盖多个维度（精度、成本、泛化、模型尺寸），较全面。
  - 但未提供误差条或多次运行统计显著性，受限于计算资源。

## 6. 论文的主要结论与发现
- **EPIC 显著优于单个推理方法**：例如 λ=0.25 时，达到 86.4% 准确率（与 Best-of-16 相同），但 token 消耗仅 1859.2（Best-of-16 为 10036.2），降低约 75%。
- **EPIC 支持灵活精度-成本权衡**：随着 λ 从 0 升至 1，精度从 85.8% 升到 89.4%，成本从 892.9 升至 6921.7，逼近方法池上限（91.2%）。
- **跨数据集/模型有效**：MATH 训练后在 GSM8K 上达 95.0% 准确率且 token 数低于高成本基线；多模型混合时同样高效。
- **理论概率界切实提升样本效率**：正则化项有助于小样本学习。

## 7. 优点
- **理论严谨**：首次推导多数投票、评分求和、评分最大三种聚合方法的准确率概率界，并用于指导正则化设计。
- **方法创新**：将推理方法选择建模为对比学习问题，结合效用函数和概率正则，实现动态、可扩展的规划。
- **成本-精度可控**：λ 参数可灵活适应不同部署场景。
- **轻量化**：问题嵌入使用仅 22.6M 参数的 sentence transformer，无需额外大模型。
- **泛化性好**：实验证明在数学和代码任务、不同模型规模上均有效。

## 8. 不足与局限
- **实验统计性**：未提供多次运行的标准差或置信区间，结果可能受随机性影响。
- **方法池覆盖有限**：仅包含 81 种代表性子集，未涵盖所有可能推理方法（如树搜索变体、流程奖励模型变种等）。
- **正则化依赖近似**：概率界基于i.i.d.假设和正态分布假设，实际生成分布可能偏离，影响正则化准确性。
- **跨域验证不足**：仅在数学和代码上测试，未涉及常识推理、问答等其他领域。
- **新方法扩展性**：文末提及需要额外网络 \( h_\vartheta \) 来编码新方法描述，但未实现，实际添加新方法需重新训练嵌入。
- **成本度量简化**：仅用 token 数量代表成本，未考虑实际推理延迟、显存占用等因素。

（完）

---

**注**：本文总结基于论文提供的全文内容，所有数据、图表引用均来自原文。若需更详细原始数据，请查阅原文及附录。
