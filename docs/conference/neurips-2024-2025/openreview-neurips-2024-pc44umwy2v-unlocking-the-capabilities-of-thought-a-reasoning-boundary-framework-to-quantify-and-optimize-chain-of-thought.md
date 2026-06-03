---
title: "Unlocking the Capabilities of Thought: A Reasoning Boundary Framework to Quantify and Optimize Chain-of-Thought"
title_zh: 解锁思维的能力：量化与优化思维链的推理边界框架
authors: "Qiguang Chen, Libo Qin, Jiaqi WANG, Jingxuan Zhou, Wanxiang Che"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=pC44UMwy2v"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 引入推理边界框架来量化和优化思维链
tldr: 为思维链推理提供定量指标和优化指导
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 457, \"height\": 430}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 472, \"height\": 401}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 1005, \"height\": 836}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 609, \"height\": 497}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 883, \"height\": 698}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 323, \"height\": 856}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 945, \"height\": 784}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-008.webp\", \"caption\": \"\", \"page\": 5, \"index\": 8, \"width\": 982, \"height\": 682}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-009.webp\", \"caption\": \"\", \"page\": 5, \"index\": 9, \"width\": 822, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-010.webp\", \"caption\": \"\", \"page\": 10, \"index\": 10, \"width\": 562, \"height\": 483}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-011.webp\", \"caption\": \"\", \"page\": 10, \"index\": 11, \"width\": 954, \"height\": 858}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-012.webp\", \"caption\": \"\", \"page\": 10, \"index\": 12, \"width\": 982, \"height\": 682}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-013.webp\", \"caption\": \"\", \"page\": 10, \"index\": 13, \"width\": 822, \"height\": 701}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-014.webp\", \"caption\": \"\", \"page\": 10, \"index\": 14, \"width\": 864, \"height\": 780}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-015.webp\", \"caption\": \"\", \"page\": 17, \"index\": 15, \"width\": 620, \"height\": 413}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-016.webp\", \"caption\": \"\", \"page\": 17, \"index\": 16, \"width\": 461, \"height\": 445}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-017.webp\", \"caption\": \"\", \"page\": 17, \"index\": 17, \"width\": 365, \"height\": 360}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-018.webp\", \"caption\": \"\", \"page\": 19, \"index\": 18, \"width\": 480, \"height\": 410}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-019.webp\", \"caption\": \"\", \"page\": 19, \"index\": 19, \"width\": 477, \"height\": 416}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-020.webp\", \"caption\": \"\", \"page\": 19, \"index\": 20, \"width\": 904, \"height\": 794}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-021.webp\", \"caption\": \"\", \"page\": 19, \"index\": 21, \"width\": 890, \"height\": 405}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-022.webp\", \"caption\": \"\", \"page\": 19, \"index\": 22, \"width\": 986, \"height\": 446}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-023.webp\", \"caption\": \"\", \"page\": 19, \"index\": 23, \"width\": 463, \"height\": 388}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-pc44umwy2v/fig-024.webp\", \"caption\": \"\", \"page\": 19, \"index\": 24, \"width\": 505, \"height\": 868}]"
motivation: 缺乏量化指标评估思维链能力以及优化指导。
method: 定义推理边界来量化思维链的上限，并建立框架进行优化。
result: 提出了有效的量化方法和优化策略。
conclusion: 推理边界框架为评估和提升思维链忠实性提供了工具。
---

## Abstract
Chain-of-Thought (CoT) reasoning has emerged as a promising approach for enhancing the performance of large language models (LLMs) on complex reasoning tasks. Recently, a series of studies attempt to explain the mechanisms underlying CoT, aiming to deepen the understanding of its efficacy. Nevertheless, the existing research faces two major challenges: (1) a lack of quantitative metrics to assess CoT capabilities and (2) a dearth of guidance on optimizing CoT performance. Motivated by this, in this work, we introduce a novel reasoning boundary framework (RBF) to address these challenges. To solve the lack of quantification, we first define a reasoning boundary (RB) to quantify the upper-bound of CoT and establish a combination law for RB, enabling a practical quantitative approach applicable to various real-world CoT tasks. To address the lack of optimization, we propose three categories of RBs. We further optimize these categories with combination laws focused on RB promotion and reasoning path optimization for CoT improvement. Through extensive experiments on 27 models and 5 tasks, the study validates the existence and rationality of the proposed framework. Furthermore, it explains the effectiveness of 10 CoT strategies and guides optimization from two perspectives. We hope this work can provide a comprehensive understanding of the boundaries and optimization strategies for reasoning in LLMs. Our code and data are available at https://github.com/LightChen233/reasoning-boundary.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：Chain-of-Thought (CoT) 推理虽能提升大语言模型在复杂推理任务上的表现，但现有研究存在两大缺陷：
  - 缺乏量化指标来评估 CoT 的上限能力（仅依赖定性分析）；
  - 缺乏针对 CoT 性能优化的具体指导（多数研究仅限于理解机制，未提供可操作的优化策略）。
- **整体目标**：提出一个 **推理边界框架（Reasoning Boundary Framework, RBF）**，用于量化 CoT 的上限（即推理边界），并基于该框架指导 CoT 的优化，从而提升 LLM 的推理能力。

## 2. 论文提出的方法论

### 核心思想
- **推理边界（Reasoning Boundary, RB）**：定义为一个模型在给定任务上，当准确率达到某个阈值（例如 90%）时所能处理的最大问题难度。数学上表示为：
  \[
  B_{\text{Acc}=K_1}(t|m) = \sup\{d \mid \text{Acc}(t|d,m) \le K_1\}
  \]
  其中 \(d\) 是问题难度（如推理步数、计算复杂度），\(K_1\) 是预设准确率阈值。
- **RB 的组合定律**：当模型需要组合多种能力（如算术计算与规划）时，联合 RB 近似为各基础 RB 的**加权调和平均**：
  \[
  B_{\text{Acc}=K_1}(t_1,\dots,t_n|m) \approx \frac{1}{\sum_{i=1}^n \frac{N_i}{B_{\text{Acc}=K_1}(t_i|m) - b_i}}
  \]
  其中 \(N_i, b_i\) 为仅与任务相关的缩放因子。
- **三类推理边界**：
  - **完全可行推理边界（CFRB）**：准确率 ≥90%，模型能够完全掌握；
  - **部分可行推理边界（PFRB）**：10% < 准确率 < 90%，需要多次共识或更清晰的信息；
  - **完全不可行推理边界（CIRB）**：准确率 ≤10%，模型几乎无法解决。

### 关键技术细节
- 通过二元搜索在标准设置下计算具体 RB 值。
- 结合组合定律，可解释 Tool-Usage、Program-of-Thought (PoT) 等方法的有效性：Tool-Usage 通过将计算 RB 推向无穷大来提升联合 RB；PoT 通过更清晰的代码规划提升规划 RB，从而进一步优于 Tool-Usage。
- 提出 **最小可接受推理路径（Minimum Acceptable Reasoning Paths, MARP）** 策略：在给定 RB 内，通过限制单步计算量、最大乘积上限，并尽量增加每步操作数、减少全局规划步数，从而同时减轻计算和规划压力。

## 3. 实验设计

### 数据集与场景
- **主要基准**：为评估 RB，作者构建了 **BigGSM** 数据集，包含 610 个测试样本，涉及多步推理（1~16 步）和大范围计算量（6~3e5 乘积）。
- **其他任务**：
  - 算术计算（加、减、乘、除）；
  - 自然语言规划（数学推理中的步骤规划）；
  - 代码规划（使用 PAL 生成代码格式的计划）；
  - 多跳问答（HotpotQA）；
  - 多语言数学推理（MGSM）。
- **模型**：27 个模型，包括 GPT-3.5/4/4o/o1-preview、Claude 3 系列、Gemini-1.0-Pro、LLaMA 系列、Mistral、MAmmoTH、OpenMath 等。

### 对比方法
- **基线**：标准 CoT、Zero-shot CoT。
- **RB 优化方法**：Tool-Usage、PoT。
- **推理路径优化方法**：Least-to-Most、Complex-CoT、CoT+MARP、PoT+MARP。

## 4. 资源与算力

- 论文未明确给出总的训练/推理耗时，但提到：
  - 所有开源模型实验在 **两块 A100 80G** 上使用 **vLLM** 框架部署。
  - 闭源模型（GPT 系列、Claude 等）通过 API 调用。
  - 未说明具体训练或推理的时间量，但可以推断实验规模较大（27 个模型 × 5 个任务 × 多种提示策略）。

## 5. 实验数量与充分性

- **实验组数量**：覆盖 27 个模型、5 个任务，涉及三类 RB 验证、组合定律验证、自然性分析（自我一致性、合成 CoT 生成等）、优化策略对比。主要表格（Table 1）报告了 BigGSM 上的准确率及 token 消耗。此外还有扩展实验（HotpotQA、Medical Probing、StrategyQA）和多语言 MGSM 验证。
- **消融与分析**：
  - 验证 RB 存在性（图 2）；
  - 验证组合定律（图 3、图 12）；
  - 分析不同 RB 的自然性（图 4）：CFRB 无需演示即可高精度；PFRB 通过自我一致性可提升；CIRB 即使自我一致性也无改善；模型对自身 RB 有自我认知（图 4c）。
  - 对 Tool-Usage、PoT、Complex-CoT、Least-to-Most 进行了理论分析和实验对比（图 5-7）。
- **充分性**：实验设计较系统，从基本算术到复杂多步推理、多语言场景均有覆盖，且包含了多个模型族，结论具有较强泛化性。但消融实验如调整 MARP 中的参数（每步最大操作数、乘积上限）对性能的影响未报告，可能不够全面。

## 6. 论文的主要结论与发现

1. **RB 的存在性与普遍性**：在不同任务（算术、规划、代码、多跳问答、多语言推理）中均观察到明显的三类 RB，且组合定律成立（加权调和平均）。
2. **RB 可解释现有 CoT 策略**：
   - Tool-Usage 和 PoT 通过提升计算/规划 RB 而优于标准 CoT；
   - Complex-CoT 和 Least-to-Most 在特定 RB 内可改善性能，但各有局限（步数与计算压力的平衡；全局规划压力未缓解）。
3. **MARP 策略有效**：在 BigGSM 上 CoT+MARP 达到 64.37% 准确率，PoT+MARP 达到 80.55%，均优于各自基线，且 token 消耗更低。
4. **RB 与模型规模/能力正相关**：对 25 个开源/闭源模型分析显示，CIRB/CFRB 值与真实基准（GSM8k、MATH、BigGSM）上的准确率呈正相关，并且存在缩放定律（参数越大 RB 越大）。
5. **o1-preview 的 CFRB 提升显著**：相比 GPT-3.5/4，o1-preview 的 CFRB 提升幅度远超 CIRB，暗示强化学习和推理缩放策略对掌握完全可行区域贡献巨大。

## 7. 优点

- **首次提出量化框架**：将 CoT 上限化为可计算的推理边界，并提供组合定律进行多能力整合，填补了定性分析的空白。
- **理论指导优化**：不仅解释现有方法（Tool-Usage、PoT、Complex-CoT 等）为什么有效，还基于 RB 分析提出新策略 MARP，实验显示有效。
- **广泛的实验覆盖**：27 个模型、5 种任务类型，涵盖通用/数学/闭源/开源，结论具有较强推广性。
- **辅助分析深刻**：如对自我一致性效果在 PFRB 中的提升、模型对自身 RB 的自我认知等，揭示了 RB 的内在特性。
- **应用指导明确**：给出了如何使用 RB 组合定律进行新任务边界测量的教程（附录 B），实用性强。

## 8. 不足与局限

- **基础 RB 间复杂关系未探讨**：论文假设各基础 RB 相互独立，但实际可能存在耦合或因果依赖，作者在 Limitations 部分承认了这一点。
- **动态场景鲁棒性未评估**：未在持续变化或交互式环境中验证 RB 的稳定性。
- **MARP 参数调优不系统**：提示中硬编码了“每步最多5个基本操作”和“乘法上限1.5e5”，未探索这些参数的最优设置及泛化到其他任务的表现。
- **数据集的局限性**：BigGSM 为人工合成，虽然涵盖多步复杂计算，但真实世界多样性可能不足；且测试集仅 610 样本，统计效度可能受限。
- **部分实验缺乏误差线**：如多跳问答、多语言任务上的扩展结果（表 2、图 10）未报告误差范围，复现时可参考性稍弱。
- **资源消耗度量不全**：未给出所有实验的总 GPU 小时或 API 调用成本，不利于他人评估可复现性。

（完）
