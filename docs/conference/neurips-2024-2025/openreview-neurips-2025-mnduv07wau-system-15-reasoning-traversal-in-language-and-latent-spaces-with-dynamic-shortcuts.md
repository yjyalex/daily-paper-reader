---
title: "System-1.5 Reasoning: Traversal in Language and Latent Spaces with Dynamic Shortcuts"
title_zh: System-1.5推理：在语言和潜在空间中使用动态捷径遍历
authors: "Xiaoqiang Wang, Suyuchen Wang, Yun Zhu, Bang Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=MNduv07wAu"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 自适应推理框架用于思维链中动态计算分配
tldr: System-1.5通过潜在空间捷径动态分配计算
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-mnduv07wau/fig-001.webp\", \"caption\": \"\", \"page\": 8, \"index\": 1, \"width\": 677, \"height\": 471}]"
motivation: CoT推理虽强但冗余，现有潜在空间方法未能区分关键步骤。
method: 提出自适应框架，在潜在空间中通过捷径动态分配计算。
result: 在保持准确性的同时显著减少推理开销。
conclusion: 混合语言和潜在空间推理可提升效率。
---

## Abstract
Chain-of-thought (CoT) reasoning enables large language models (LLMs) to move beyond fast System-1 responses and engage in deliberative System-2 reasoning. However, this comes at the cost of significant inefficiency due to verbose intermediate output. Recent latent-space reasoning methods improve efficiency by operating on hidden states without decoding into language, yet they treat all steps uniformly, failing to distinguish critical deductions from auxiliary steps and resulting in suboptimal use of computational resources. In this paper, we propose System-1.5 Reasoning, an adaptive reasoning framework that dynamically allocates computation across reasoning steps through shortcut paths in latent space.Specifically, System-1.5 Reasoning introduces two types of dynamic shortcuts. The model depth shortcut (DS) adaptively reasons along the vertical depth by early exiting non-critical tokens through lightweight adapter branches, while allowing critical tokens to continue through deeper Transformer layers. The step shortcut (SS) reuses hidden states across the decoding steps to skip trivial steps and reason horizontally in latent space. Training System-1.5 Reasoning involves a two-stage self-distillation process: first distilling natural language CoT into latent-space continuous thought, and then distilling full-path System-2 latent reasoning into adaptive shortcut paths (System-1.5 Reasoning).Experiments on reasoning tasks demonstrate the superior performance of our method.
For example, on GSM8K, System-1.5 Reasoning achieves reasoning performance comparable to traditional CoT fine-tuning methods while accelerating inference by over 20× and reducing token generation by 91.0\% on average.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：大型语言模型（LLMs）通过链式思维（Chain-of-Thought, CoT）实现了从快速启发式（System-1）到深思熟虑（System-2）的推理能力提升。然而，CoT推理需要生成长篇幅的中间步骤，导致严重的计算效率低下，即“过度思考”现象。
- **问题**：现有的潜在空间推理方法（如Coconut、CCoT）虽然通过隐藏状态避免了语言解码的开销，但它们对所有推理步骤一视同仁，无法区分关键推导步骤与辅助步骤，导致计算资源分配次优。
- **整体含义**：本文提出 **System-1.5 Reasoning**，一种自适应推理框架，在潜在空间中通过动态捷径为不同推理步骤分配不同的计算量，从而在保持推理性能的同时大幅提升效率。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 结合System-1（快速）和System-2（慢速）的推理，在潜在空间中自适应地分配计算：非关键步骤使用浅层计算（System-1风格），关键步骤使用深层计算（System-2风格），而琐碎步骤可直接跳过（通过步骤捷径）。

### 关键技术细节
1. **深度捷径（Depth Shortcut, DS）**：
   - 在每个Transformer层中插入一个轻量级的路由器-适配器模块（Router-Adapter）。
   - 路由器（`R_l`）动态决定当前token是继续通过标准Transformer层（`f_l`）还是通过适配器分支（`g_l`）早期退出。
   - 训练时输出为加权组合：`h_{l,t} = (g_{l-1}(h_{l-1,t}) + g_l(h_{l,t-1})) * w + f_l(h_{l-1,t}) * (1-w)`，其中`w = R_l(h_{l-1,t})`。
   - 推理时根据阈值`λ_depth`决定是否早退：若`R_l > λ_depth`则退出，否则继续。

2. **步骤捷径（Step Shortcut, SS）**：
   - 允许将早期退出的隐藏状态直接复制到下一个解码步骤的同一层，而无需从第一层重新计算。
   - 这使得模型能够水平地跳过琐碎的解码步骤，实现潜在空间中的横向推理。

3. **两阶段蒸馏训练**：
   - **第一阶段：语言到潜在对齐（Language-to-Latent Alignment）**：
     - 学生模型（`M_θ_student`）学习直接在潜在空间中推理，通过教师强制（teacher-forcing）将教师模型（CoT微调）的最后一层隐藏状态作为目标，最小化MSE损失。
     - 教师模型优化标准NLL损失（中间步骤+答案），学生模型只优化答案的NLL损失，同时通过一致性损失对齐隐藏状态。
   - **第二阶段：System-2到System-1.5蒸馏（Shortcut Learning）**：
     - 冻结原始Transformer参数，仅训练路由器-适配器模块。
     - 利用“原子思想分解”（Atom-of-Thought）将CoT分解为有向无环图（DAG），标注独立节点为“非关键”，衍生节点为“关键”。
     - 定义早退损失`L_early-exit`，鼓励非关键步骤在浅层退出，关键步骤深入计算，同时保持与完整路径隐藏状态的一致性。

## 3. 实验设计

### 数据集与场景
- **数学推理**：训练集为扩展后的GSM8K（~40万题），测试集为GSM8K原始测试集；域外评估使用GSM-HARD（难度提升的变体）。
- **常识推理**：StrategyQA（包含多跳推理的是/否问题，约2780例）。

### Benchmark与对比方法
- **主基准**：标准CoT微调。
- **对比方法**：
  - 语言空间条件计算：LITE、LayerSkip。
  - 潜在空间压缩推理：iCoT、Coconut、CODI。
  - 潜在空间扩展推理：pause token。
- **评估指标**：准确率（Acc.）、解码步骤数（#Steps）、每步平均FLOPs降低率（FLOPs r.）、相对于CoT的推理加速比（Speedup）。

## 4. 资源与算力

- **硬件**：单块NVIDIA RTX A5000（24 GB GPU）。
- **训练时间**：
  - LLaMA 3.2 1B模型：约26小时（8 epochs）。
  - GPT-2 124M模型：约5小时（8 epochs）。
- **其他**：使用AdamW优化器，学习率2e-5，batch size 2，warmup 6%步数。论文未明确说明使用多少块GPU，但报告为单GPU实验。

## 5. 实验数量与充分性

- **主实验**：表1展示了在GSM8K、GSM-HARD、StrategyQA三个数据集上，与7种基线方法的对比结果，覆盖准确率、效率指标。
- **消融实验**（图3）：
  - 替换System-2学生模型（Coconut、CODI）对比蒸馏效果。
  - 对比联合学习 vs. 两阶段学习、全参数微调 vs. 仅路由器微调。
- **测试时缩放分析**（图4）：调整深度阈值`λ_depth`和解码步数常数`λ_step`，观察性能变化，展示可控制的预算缩放。
- **充分性**：实验覆盖了多个数据集、多种方法、多维度消融，且在所有实验中报告了4次独立运行的平均结果，具有统计稳定性。对比方法涵盖语言空间和潜在空间的主要技术，实验设计较为充分、客观、公平。

## 6. 主要结论与发现

- **性能与效率**：System-1.5 Reasoning在GSM8K上实现了与CoT微调相当的准确率（46.94% vs 46.67%），同时推理加速超过20倍，平均token生成减少91.0%。在StrategyQA上准确率超过CoT（48.61% vs 47.36%），加速达55倍以上。
- **蒸馏策略**：直接语言到潜在蒸馏（两阶段）优于基于课程学习（如Coconut）的复杂调度；联合学习或全参数微调会降低性能。
- **可控性**：通过调整深度阈值和解码步数，可以实现灵活的预算可控的测试时缩放，且深度和步数两个维度同等重要。

## 7. 优点

- **创新性**：首次提出同时沿模型深度和解码步数两个维度进行自适应动态捷径推理，结合了System-1的快速和System-2的深度。
- **高效性**：在保持或超越CoT准确率的前提下，实现了数量级的推理加速，显著降低计算成本。
- **训练方法**：两阶段蒸馏有效解决了潜在空间推理的适应性问题，并利用步骤关键性标引导自适应学习。
- **可控性**：推理时可通过简单阈值调整计算预算，提供了灵活的test-time scaling能力。

## 8. 不足与局限

- **可解释性不足**：潜在空间推理缺乏显式中间步骤，难以理解、分析和验证模型内部逻辑，在高风险场景下存在安全风险。
- **评估规模有限**：实验仅基于中等规模基准（GSM8K、StrategyQA）和中等模型（GPT-2 124M、LLaMA 3.2 1B），尚未验证在更大模型（如70B+）和更复杂任务（如代码生成、多模态推理）上的泛化性。
- **偏差风险**：关键/非关键步骤的标注依赖原子思想分解，标注质量可能影响训练；未讨论数据偏差或公平性问题。
- **应用限制**：由于缺乏语言解释，不适合需要可解释推理的领域（如医疗、法律）。

（完）
