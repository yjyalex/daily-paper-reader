---
title: "DLoFT: Gradient-Decoupled Fine-Tuning for Generalizable Long Chain-of-Thought Reasoning"
title_zh: DLoFT：用于泛化长思维链推理的梯度解耦微调
authors: "Sitong Wu, Haoru Tan, Jingyao Li, Shaofeng Zhang, XIAOJUAN QI, Bei Yu, Jiaya Jia"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=5ZDT1dxojA"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 提升长思维链推理的泛化能力
tldr: DLoFT通过梯度解耦防止长CoT微调中的过拟合。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1560, \"height\": 686}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1926, \"height\": 1070}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-003.webp\", \"caption\": \"\", \"page\": 10, \"index\": 3, \"width\": 1982, \"height\": 698}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-5zdt1dxoja/fig-004.webp\", \"caption\": \"\", \"page\": 18, \"index\": 4, \"width\": 1448, \"height\": 2010}]"
motivation: 长CoT微调容易过拟合问题特定的知识和启发式。
method: 提出梯度解耦算法，分离推理内容与问题特定信息的学习。
result: 提升了长CoT推理的分布外泛化性能。
conclusion: 梯度解耦有效促进长CoT推理的泛化性。
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

### 论文总结：DLoFT（用于泛化长思维链推理的梯度解耦微调）

#### 1. 核心问题与整体含义（研究动机和背景）
- 长思维链（LongCoT）通过系统的思考阶段（反思、纠错、探索等）提升了大型语言模型（LLM）在复杂推理任务上的能力。
- 当前主流方法是利用高质量 LongCoT 轨迹进行监督微调（SFT），但实验发现 SFT 容易过拟合到训练数据中的**问题特定知识和启发式**，导致在未见过的领域（分布外）性能显著下降，而泛化性的丢失限制了 LongCoT 在数据稀疏或隐私受限领域的应用。
- 论文旨在解决该泛化问题，希望模型能学习**通用的 LongCoT 推理范式**，而非记忆特定问题内容。

#### 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：将完整响应的梯度解耦为两个正交分量：**范式相关梯度（\( g_{par} \)）**（对应通用推理范式）和**内容相关梯度（\( g_{con} \)）**（对应问题特定知识和启发式），仅用 \( g_{par} \) 更新模型参数。
- **关键技术细节**：
  - 利用 LongCoT 响应的两阶段结构：思考阶段（包含范式+内容）和解决方案阶段（仅包含内容）。
  - 计算完整响应（思考+解决方案）的梯度 \( g_{full} \)。
  - 仅计算解决方案部分的梯度作为**参考梯度 \( g_{ref} \)**，代表问题特定信息。
  - 通过**投影操作**：\( g_{con} = \frac{\langle g_{full}, g_{ref} \rangle}{\|g_{ref}\|^2} g_{ref} \)，从 \( g_{full} \) 中提取与 \( g_{ref} \) 对齐的内容相关分量。
  - 通过**正交化操作**：\( g_{par} = g_{full} - g_{con} \)，得到与内容正交的范式相关梯度。
- **算法流程**（每轮迭代）：
  1. 使用完整 LongCoT 响应（\( T_i \oplus S_i \)）计算负对数似然损失及其梯度 \( g_{full} \)。
  2. 仅使用解决方案 \( S_i \) 计算参考梯度 \( g_{ref} \)。
  3. 梯度解耦：计算 \( g_{con} \)（投影）和 \( g_{par} \)（正交化）。
  4. 模型更新：\( \theta \leftarrow \theta - \eta \cdot g_{par} \)。

#### 3. 实验设计：数据集、基准、对比方法
- **数据集**：
  - 混合域：`s1K`（1000条数据，覆盖数学、编程、科学、谜题，由 Gemini Flash Thinking 生成）。
  - 单域：`OpenR1-Math-5K`（数学）、`OpenThoughts-Code-5K`（代码）、`Medical-o1-5K`（医学）。
- **评估基准**：
  - 域内：AIME24、MATH-500、OlympiadBench（数学），LiveCodeBench（代码），MedQA（医学）。
  - 域外：SuperGPQA 中涵盖 17 个域（物理、化学、生物、计算机、机械、电子、通信、天文、地理、土木、农业、经济、历史、法律、文学、哲学、社会学）。
- **对比方法**：标准监督微调（SFT）。此外还对比了 SFT + RL 与 DLoFT + RL 作为冷启动的差异。
- **消融实验**：投影操作的必要性（完整 vs 直接赋值 \( g_{con}=g_{ref} \)）、梯度解耦的有效性（仅用 \( g_{con} \) 更新 vs 仅用 \( g_{par} \) 更新）、权重衰减正则化效果。

#### 4. 资源与算力
- 论文在附录 A.4 给出了训练资源信息（使用 Qwen2.5-7B-Instruct 和 s1K 数据集）：
  - 单步运行时间：SFT 约 6.2 分钟，DLoFT 约 6.8 分钟（增加约 10%）。
  - GPU 内存：SFT 约 69986 MB，DLoFT 约 70684 MB（基本持平）。
- **未明确说明的部分**：使用的 GPU 型号、数量、总训练时长等具体算力细节未提及。作者在 Limitations 中承认因资源限制未进行数百亿参数级别的大规模实验。

#### 5. 实验数量与充分性
- **实验数量**：
  - 通用 LLM 实验：Qwen2.5-7B/32B-Instruct，在 17 个域上评估（图 3）。
  - 领域专用 LLM 实验：Qwen2.5-Math-7B、Qwen2.5-Coder-7B、Meditron-7B，分别进行域内和域外数据训练（图 4）。
  - RL 冷启动实验：对比 SFT+RL 和 DLoFT+RL 在三个领域的行为。
  - 消融实验：3 个（投影操作、梯度解耦有效性、权重衰减）。
- **充分性与公平性**：
  - 覆盖了多种模型尺寸（7B、32B）、多种领域（通用及三个专用领域）、多种数据源（混合域、单域）。
  - 实验设置一致（学习率、批量大小、优化器等），对比客观。
  - 泛化性评估使用了 17 个域，覆盖面广；域内和域外表现均有报告。
  - 消融实验验证了关键组件的必要性，结论有说服力。
- **局限性**：仅与 SFT 进行了主对比，未与其它正则化方法（如 dropout、标签平滑等）系统比较，也未在更大参数规模（>70B）或更复杂任务上验证。

#### 6. 主要结论与发现
- **DLoFT 显著提升泛化性**：相比 SFT，在域外基准上平均提升 +16.4 点，在域内基准上平均提升 +11.5 点（图 3、图 4）。
- **LongCoT 行为更好**：DLoFT 训练中模型更快展现出反思、纠错等 LongCoT 特征（图 1b），且不会因过拟合而丢失。
- **作为 RL 冷启动更优**：
  - 摆脱了对域内 LongCoT 数据的依赖：使用域外数据冷启动后，DLoFT 性能与使用域内数据相当，而 SFT 会显著下降（图 4）。
  - 增强了后续 RL 效果：DLoFT 冷启动后 RL 增益更大（+5.0 vs +3.7）。
- **梯度解耦机制有效**：只使用 \( g_{par} \) 更新的模型表现了 LongCoT 行为，而只使用 \( g_{con} \) 更新的模型无此行为（消融实验表 2）。

#### 7. 优点
- **方法创新性强**：巧妙利用 LongCoT 响应的两阶段结构实现梯度解耦，原理清晰且计算开销极小（仅额外轻量向量运算）。
- **实用价值高**：大幅降低了数据收集成本（无需覆盖所有域），尤其适用于数据难以获取或隐私受限的领域。
- **通用性强**：在多个通用和领域专用 LLM 上均验证有效，且可直接作为 RL 的冷启动阶段。
- **实验设计全面**：多模型、多数据集、多评价域，消融实验支撑充分。

#### 8. 不足与局限
- **实验规模有限**：未在数百亿参数的超大规模模型（如 671B DeepSeek-V3）上进行验证，通用性仍需进一步确认。
- **对比基线单一**：仅与 SFT 对比，未系统考察其它通用正则化方法（如权重衰减、对抗训练等）在 LongCoT 场景的表现（虽然消融了权重衰减，但未见完整比较）。
- **缺乏理论分析**：未给出梯度解耦为何有效的形式化推导或理论保证（如证明 \( g_{par} \) 确实只包含范式信息）。
- **未讨论多任务或跨领域迁移**：实验聚焦于单一任务的微调，未探索同时学习多个域的情境。
- **应用限制**：论文在 Impact Statement 中指出可能存在被误用于不人道的监控或控制的潜在风险，需立法关注。

（完）
