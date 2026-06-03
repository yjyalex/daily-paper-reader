---
title: Reasoning Planning for Language Models
title_zh: 语言模型的推理规划
authors: "Bao Nguyen, Hieu Trung Nguyen, Ruifeng She, Xiaojin Fu, Viet Anh Nguyen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QFjssnKdBI"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 提出EPIC框架用于选择大语言模型的推理方法
tldr: 提出EPIC，一个对比学习框架，为每个查询选择最优推理方法
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 1189, \"height\": 790}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-007.webp\", \"caption\": \"\", \"page\": 27, \"index\": 7, \"width\": 857, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-008.webp\", \"caption\": \"\", \"page\": 27, \"index\": 8, \"width\": 860, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-qfjssnkdbi/fig-009.webp\", \"caption\": \"\", \"page\": 27, \"index\": 9, \"width\": 857, \"height\": 701}]"
motivation: 现有方法假设更多候选答案带来更高准确性，但缺乏理论分析，且未考虑查询与方法之间的匹配。
method: 提出EPIC框架，通过对比学习学习推理能力和查询-方法兼容性的共享表示空间，并融入概率界作为正则化项。
result: 在多个推理基准上，EPIC优于固定推理方法或简单聚合策略。
conclusion: 选择合适推理方法可以显著提升语言模型推理的准确性和鲁棒性。
---

## Abstract
Selecting an appropriate reasoning method for a given query remains a key challenge in language model generation. Existing approaches typically generate multiple candidate responses and use an aggregation strategy to select the output answer, often assuming that more candidate answers yield higher accuracy. We revisit this assumption through a rigorous theoretical analysis, deriving accuracy bounds for standard aggregation methods under fixed generation distributions and candidate sizes. Building on these insights, we introduce EPIC, an Ensemble Planning with Contrastive learning framework to learn a shared representation space that captures both model reasoning abilities and query-method compatibility. EPIC incorporates our probability bounds as a regularizer in a utility-driven optimization that balances accuracy and computational cost. Experiments on diverse mathematical reasoning tasks show that EPIC consistently selects optimal reasoning methods, improving accuracy while reducing computational overhead. Our code can be found at https://github.com/nguyenngocbaocmt02/EPIC.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：为给定查询选择最合适的推理方法（reasoning method），以在准确率和计算成本之间取得最优权衡。现有方法通常假设增加候选答案数量（如N个样本）能直接提升准确性，但缺乏理论支撑，且未考虑不同查询与不同推理方法之间的适配性。
- **整体含义**：本文通过理论分析揭示了聚合方法（如多数投票、得分求和、得分最大值）的准确率界，并基于此提出EPIC框架，在推理前动态匹配查询与方法，显著提升效率与准确性。

### 2. 论文提出的方法论

- **核心思想**：构建一个共享表示空间，同时学习问题嵌入和推理方法嵌入，通过对比学习使每个问题嵌入靠近其最优推理方法的嵌入，并利用概率界作为正则化提高样本效率。
- **关键技术细节**：
  - **效用函数**：`u = λ * accuracy + (1-λ) * (1 - normalized_cost)`，λ ∈ [0,1] 控制精度-成本权衡。
  - **特征提取**：使用轻量级句子嵌入模型 `all-MiniLM-L6-v2`（22.6M参数）将问题转为384维特征，再通过MLP映射到d维嵌入（d << 384）。
  - **学习过程**：联合优化问题嵌入网络参数θ和方法嵌入向量v_i，损失函数包括：
    - **对比损失**（InfoNCE）：拉近问题嵌入与正方法嵌入（最高效用方法），推远其他方法。
    - **正则化损失**：基于概率界（定理3.1-3.3）约束仅N不同的方法对，使相似度之比接近目标概率比。β用于平衡两者。
  - **推理**：对新问题，通过训练好的网络得到嵌入，选择与各方法嵌入相似度最高的方法执行生成。
- **公式与流程**：见论文公式(6)(7)(8)；核心训练问题为 `min_{θ, v_i} L_contrastive + τ L_reg`。

### 3. 实验设计

- **数据集**：
  - 主要：MATH（训练7,500题，测试MATH500）。
  - 迁移性：GSM8K（算术推理），LiveCodeBench（代码生成）。
- **基准方法**：
  - 个体推理方法：CoT-G, Best-of-N (N=2,4,8,16), MCTS, Beam Search，以及多种聚合变体（MV, PVM, PVL, PMM, PML）。
  - 强模型参考：DeepSeek-V3, OpenAI-o1-mini（不在M中）。
  - 选择方法基线：RA（随机分配）、Offline Ada-BoK、DRA-λ（分布式随机）、CL-λ（分类）。
- **评估指标**：准确率（自动评分）、平均生成token数。

### 4. 资源与算力

- **硬件**：单机8×NVIDIA RTX A5000 GPU + Intel Xeon Gold 6148 CPU @ 2.40GHz。
- **训练时长**：论文未明确给出总训练时间。

### 5. 实验数量与充分性

- **实验数量**：包括主实验（图2、表1）、迁移性实验（表2a/2b）、消融实验（维度d、λ、效用函数、代码生成）、可视化分析（PCA）、案例研究。
- **充分性**：覆盖了多种推理策略、聚合方式、模型规模；对比了多个强基线和选择方法；进行了跨数据集和跨模型迁移验证。但论文声明因计算资源限制未提供误差棒，统计显著性未报告。

### 6. 论文的主要结论与发现

- EPIC在多个λ设置下均取得接近理论上限（91.2%）的准确率，同时大幅降低计算成本。例如λ=0.25时，准确率86.4%，与best-of-16（86.8%）相近，但token数减少约5倍（1859 vs 10036）。
- 迁移实验显示EPIC在GSM8K上同样优于Baselines，并在混合不同规模模型（1.5B+7B）时保持有效性。
- 可视化表明EPIC能根据成本-精度偏好有效组织方法和问题嵌入空间。

### 7. 优点

- **理论驱动**：首次推导了多数投票、PRM得分和与最大值聚合的概率界，并用于正则化，提升样本效率。
- **灵活可扩展**：通过对比学习+双塔结构，新增推理方法只需添加新嵌入向量，无需重训网络。
- **用户可控**：效用函数中λ系数允许用户显式调节成本-精度偏好。
- **实验验证充分**：在数学推理和代码生成任务上均取得领先，迁移性良好。

### 8. 不足与局限

- **统计显著性缺失**：未提供误差棒或置信区间，无法评估结果稳定性。
- **方法覆盖有限**：仅测试了M中81种推理配置，未涵盖所有可能推理方法（如基于干预、自适应计算等）。
- **正则化依赖假设**：概率界适用于大N，小N时需用经验值替代，可能引入偏差。
- **训练成本**：需要预先对每个方法在训练集上执行推理以获取准确率和成本，计算开销较大。
- **代码实验简化**：代码生成中未使用PRM等复杂聚合，仅基于pass@k，与数学实验不对齐。

（完）
