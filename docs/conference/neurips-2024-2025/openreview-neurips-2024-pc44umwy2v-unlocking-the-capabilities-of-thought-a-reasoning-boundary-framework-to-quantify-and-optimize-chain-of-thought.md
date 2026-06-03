---
title: "Unlocking the Capabilities of Thought: A Reasoning Boundary Framework to Quantify and Optimize Chain-of-Thought"
title_zh: 解锁思想的能力：量化与优化思维链的推理边界框架
authors: "Qiguang Chen, Libo Qin, Jiaqi WANG, Jingxuan Zhou, Wanxiang Che"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=pC44UMwy2v"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 量化评估CoT能力的框架
tldr: 推理边界框架量化CoT上限并指导优化。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 457, \"height\": 430}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 472, \"height\": 401}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 1005, \"height\": 836}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 609, \"height\": 497}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 883, \"height\": 698}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 323, \"height\": 856}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 945, \"height\": 784}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-008.webp\", \"caption\": \"\", \"page\": 5, \"index\": 8, \"width\": 982, \"height\": 682}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-009.webp\", \"caption\": \"\", \"page\": 5, \"index\": 9, \"width\": 822, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-010.webp\", \"caption\": \"\", \"page\": 10, \"index\": 10, \"width\": 562, \"height\": 483}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-011.webp\", \"caption\": \"\", \"page\": 10, \"index\": 11, \"width\": 954, \"height\": 858}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-012.webp\", \"caption\": \"\", \"page\": 10, \"index\": 12, \"width\": 982, \"height\": 682}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-013.webp\", \"caption\": \"\", \"page\": 10, \"index\": 13, \"width\": 822, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-014.webp\", \"caption\": \"\", \"page\": 10, \"index\": 14, \"width\": 864, \"height\": 780}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-015.webp\", \"caption\": \"\", \"page\": 17, \"index\": 15, \"width\": 620, \"height\": 413}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-016.webp\", \"caption\": \"\", \"page\": 17, \"index\": 16, \"width\": 461, \"height\": 445}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-017.webp\", \"caption\": \"\", \"page\": 17, \"index\": 17, \"width\": 365, \"height\": 360}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-018.webp\", \"caption\": \"\", \"page\": 19, \"index\": 18, \"width\": 480, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-019.webp\", \"caption\": \"\", \"page\": 19, \"index\": 19, \"width\": 477, \"height\": 416}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-020.webp\", \"caption\": \"\", \"page\": 19, \"index\": 20, \"width\": 904, \"height\": 794}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-021.webp\", \"caption\": \"\", \"page\": 19, \"index\": 21, \"width\": 890, \"height\": 405}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-022.webp\", \"caption\": \"\", \"page\": 19, \"index\": 22, \"width\": 986, \"height\": 446}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-023.webp\", \"caption\": \"\", \"page\": 19, \"index\": 23, \"width\": 463, \"height\": 388}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-024.webp\", \"caption\": \"\", \"page\": 19, \"index\": 24, \"width\": 505, \"height\": 868}]"
motivation: 缺乏量化指标评估CoT能力以及优化指导。
method: 定义推理边界（RB）量化CoT上限，并建立框架优化。
result: 实现了对CoT能力的量化和优化指导。
conclusion: 推理边界框架为CoT评估和提升提供了理论工具。
---

## Abstract
Chain-of-Thought (CoT) reasoning has emerged as a promising approach for enhancing the performance of large language models (LLMs) on complex reasoning tasks. Recently, a series of studies attempt to explain the mechanisms underlying CoT, aiming to deepen the understanding of its efficacy. Nevertheless, the existing research faces two major challenges: (1) a lack of quantitative metrics to assess CoT capabilities and (2) a dearth of guidance on optimizing CoT performance. Motivated by this, in this work, we introduce a novel reasoning boundary framework (RBF) to address these challenges. To solve the lack of quantification, we first define a reasoning boundary (RB) to quantify the upper-bound of CoT and establish a combination law for RB, enabling a practical quantitative approach applicable to various real-world CoT tasks. To address the lack of optimization, we propose three categories of RBs. We further optimize these categories with combination laws focused on RB promotion and reasoning path optimization for CoT improvement. Through extensive experiments on 27 models and 5 tasks, the study validates the existence and rationality of the proposed framework. Furthermore, it explains the effectiveness of 10 CoT strategies and guides optimization from two perspectives. We hope this work can provide a comprehensive understanding of the boundaries and optimization strategies for reasoning in LLMs. Our code and data are available at https://github.com/LightChen233/reasoning-boundary.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：Chain-of-Thought (CoT) 推理能提升大语言模型 (LLM) 在复杂推理任务上的性能，但现有研究面临两大挑战：
  1. **缺乏量化指标**：已有工作多停留在定性分析，无法客观比较不同 CoT 方法或定义 CoT 能力的明确上界。
  2. **缺乏优化指导**：对 CoT 机制的理解未能有效转化为可操作的性能提升策略。
- **背景**：已有工作从自然语言规划、代码规划、单步计算等角度定性探讨了 CoT 的边界，但缺少统一量化和系统优化框架。
- **整体含义**：本文提出 **推理边界框架 (Reasoning Boundary Framework, RBF)**，旨在量化 CoT 的上界，并据此指导优化，为 LLM 推理能力的理解和提升提供理论基础。

#### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：定义“推理边界 (Reasoning Boundary, RB)”为模型在特定任务上达到某一准确率阈值所能处理的最大问题难度；并基于加权调和平均建立“组合律”，将复杂任务的 RB 分解为多个基本任务 RB 的融合。
- **关键技术细节**：
  - **推理边界 (RB)**：对于模型 \(m\) 和任务 \(t\)，RB 定义为 \(B^{Acc=K_1}(t|m) = \sup\{d \mid Acc(t|d,m) \leq K_1\}\)，其中 \(d\) 是问题难度（如推理步数、计算复杂度），\(K_1\) 是预设准确率阈值（常用 90% 或 10%）。
  - **组合律**：当任务需多种能力协同（如算术计算与规划），联合 RB 近似为各基本 RB 的加权调和平均：
    \[
    B(t_1,\dots,t_n|m) \approx \frac{1}{\sum_{i=1}^n \frac{N_i}{B(t_i|m)-b_i}}
    \]
    其中 \(N_i, b_i\) 为任务相关的缩放因子。该公式通过泰勒展开和独立难度假设推导（详见附录 A）。
  - **三类推理边界**：根据经验准确率划分：
    - **完全可行 (CFRB)**：准确率 ≥ 90%，模型能有效掌握。
    - **完全不可行 (CIRB)**：准确率 ≤ 10%，模型几乎无法解决。
    - **部分可行 (PFRB)**：介于两者之间，需多次推理或更清晰信息。
  - **优化策略**：
    - **提升 RB**：通过工具使用 (Tool-Usage) 或程序思维 (PoT) 扩展计算或规划边界。例如，工具使用将计算 RB 推向无穷，联合 RB 仅受规划 RB 限制；PoT 使用代码提升规划清晰度，进一步提升联合 RB。
    - **推理路径优化**：在固定 RB 下调整问题分解方式。例如 **Complex CoT** 通过增加步数降低单步计算压力但增加规划压力；**Least-to-Most** 分解子问题降低局部规划压力但增加全局规划压力；本文提出的 **最小可接受推理路径 (MARP)** 通过指令限制单步计算量并最大化每步操作，平衡计算与规划压力，减少 token 消耗。

#### 3. 实验设计：数据集 / 场景、benchmark、对比方法

- **数据集与场景**：
  - **BigGSM**：新构建的数学推理数据集，包含 610 个测试样本，计算复杂度（乘法值 6~3e5）和推理步数（1~16 步）范围广泛，用于评估 RB。
  - 其他任务：算术计算（四则运算）、数学推理 (GSM8K, SingleEq, MultiArith, AQuA, SVAMP)、多跳问答 (HotpotQA)、医学知识探测 (Medical Probing)、多语言数学推理 (MGSM)。
- **Benchmark**：主要使用 GPT-3.5-Turbo 进行实验（温度 0~1，top-p 0.95/1），并在 27 个模型（开源/闭源通用 LLM 及数学 LLM，如 LLaMA 系列、Code-LLaMA、Mistral、GPT-3.5/4、Claude 3、Gemini、MAmmoTH 等）上验证扩展性。
- **对比方法**：
  - 基线：标准 CoT (3-shot)
  - RB 提升方法：Tool-Usage、PoT
  - 路径优化方法：Least-to-Most、Complex CoT、MARP（自然语言版及 PoT 版）
  - 分析实验：Zero-shot CoT、Self-Consistency、Synthetic-CoT 等。

#### 4. 资源与算力

- **明确说明**：开源模型实验在 **两个 A100 80G** GPU 上运行，使用 vLLM 框架部署。商业模型（GPT-3.5-Turbo 等）通过 API 调用。未提供训练 GPU 总时长或具体推理耗时，但提到人工标注约 60 小时。

#### 5. 实验数量与充分性

- **数量**：涵盖 27 个模型、5 类任务（算术计算、数学推理、多跳问答、医学探测、多语言数学），进行了以下实验：
  - RB 存在性验证（算术、自然语言规划、代码规划）
  - 组合律验证（复杂算术、数学推理、多跳问答、多语言数学）
  - RB 类别本质分析（CFRB 零样本表现、PFRB 自一致性增益、CIRB 低准确率、模型自我认知）
  - 优化效果对比（表 1、表 2），包含 5 种优化方法在 BigGSM、HotpotQA、Medical Probing、StrategyQA 上的结果
  - 扩展性验证（25 种开源/闭源模型上 RB 与基准准确率的相关性）
  - Scaling law 分析（模型参数与 CIRB 关系）
  - 元分析（Complex CoT 步数与性能关系）
- **充分性**：实验设计较为系统，从定性存在性到定量组合律，再到优化指导，覆盖多个维度。消融实验（如 MARP vs. 其他方法）和鲁棒性（不同温度/ top-p）均有考虑。但部分实验仅报告 1-3 次结果，且未对所有任务提供误差线。

#### 6. 论文的主要结论与发现

- **RB 普遍存在**：在算术计算、自然语言规划、代码规划等任务中均观察到清晰的 CFRB/PFRB/CIRB 分区。
- **组合律成立**：在复杂算术、数学推理、多跳问答、多语言数学中，实际性能分布与加权调和平均预测一致。
- **RB 类别内禀性**：CFRB 中模型零样本即可高准确率；PFRB 中自一致性显著提升；CIRB 中模型几乎无效且无法通过重复提升。
- **模型有自我感知**：Synthetic-CoT 生成的数据 65% 位于 CFRB。
- **优化有效**：
  - Tool-Usage 和 PoT 通过提升 RB 显著优于标准 CoT。
  - MARP 在固定 RB 下平衡计算与规划压力，在 BigGSM 上达到最高准确率（PoT+MARP: 80.55%），且输入/输出 token 更少。
- **扩展性**：不同模型（开源/闭源/数学 LLM）的 RB 与基准准确率正相关；参数规模增大或数据质量提升会提高 RB（Scaling law）。

#### 7. 优点：方法或实验设计上的亮点

- **首次提出量化框架**：RBF 是第一个系统量化 CoT 上界的工作，填补了定性分析的空白。
- **组合律简洁有效**：加权调和平均形式直观，理论推导与实验验证一致，可推广到多种任务。
- **三类 RB 划分实用**：为模型能力提供了可操作的分类，并自然引出优化方向（提升 RB vs. 调整路径）。
- **MARP 策略创新**：通过简单指令（限制单步计算量、最大化每步操作）同时降低计算与规划压力，显著提升性能并减少 token 消耗。
- **实验广泛**：27 个模型、5 类任务、多种 CoT 变体，验证了框架的普适性和鲁棒性。

#### 8. 不足与局限

- **理论假设限制**：组合律推导假设各基本 RB 相互独立，实际中可能存在交叉影响（如因果条件），文中未充分讨论。
- **动态场景缺失**：未评估 RB 在实时交互、多轮对话或环境变化下的鲁棒性。
- **实验细节不完整**：部分实验（如 GPT-4 对比）未详细说明超参数；BigGSM 仅 610 样本，规模较小；误差线仅在部分结果报告。
- **优化策略局限性**：Complex CoT 和 Least-to-Most 存在平衡点问题，MARP 虽优但需人工设定上限（如“最多 5 步基本操作”），自动化程度不足。
- **应用限制**：框架依赖人工定义任务难度度量（如步数、乘积值），对非结构化或开放域任务适用性未验证。

（完）
