---
title: Enhancing the Outcome Reward-based RL Training of MLLMs with Self-Consistency Sampling
title_zh: 通过自一致性采样增强多模态大语言模型的结果奖励强化学习训练
authors: "Jiahao Wang, Weiye Xu, Aijun Yang, Wengang Zhou, Lewei Lu, Houqiang Li, Xiaohua Wang, Jinguo Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=cGkfMGQdCy"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 直接解决MLLM强化学习中思维链轨迹不忠实问题
tldr: 提出自一致性采样，减轻MLLM结果奖励强化学习中不忠实的思维链轨迹
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 325, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3960, \"height\": 2356}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 2400, \"height\": 1500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-005.webp\", \"caption\": \"\", \"page\": 24, \"index\": 5, \"width\": 430, \"height\": 321}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-006.webp\", \"caption\": \"\", \"page\": 24, \"index\": 6, \"width\": 430, \"height\": 321}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-007.webp\", \"caption\": \"\", \"page\": 24, \"index\": 7, \"width\": 471, \"height\": 352}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-008.webp\", \"caption\": \"\", \"page\": 24, \"index\": 8, \"width\": 421, \"height\": 314}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-009.webp\", \"caption\": \"\", \"page\": 24, \"index\": 9, \"width\": 430, \"height\": 321}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-010.webp\", \"caption\": \"\", \"page\": 25, \"index\": 10, \"width\": 733, \"height\": 399}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-011.webp\", \"caption\": \"\", \"page\": 27, \"index\": 11, \"width\": 776, \"height\": 462}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-012.webp\", \"caption\": \"\", \"page\": 27, \"index\": 12, \"width\": 1848, \"height\": 338}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-013.webp\", \"caption\": \"\", \"page\": 28, \"index\": 13, \"width\": 1606, \"height\": 740}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-014.webp\", \"caption\": \"\", \"page\": 28, \"index\": 14, \"width\": 787, \"height\": 682}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-015.webp\", \"caption\": \"\", \"page\": 30, \"index\": 15, \"width\": 531, \"height\": 367}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-016.webp\", \"caption\": \"\", \"page\": 30, \"index\": 16, \"width\": 750, \"height\": 625}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-017.webp\", \"caption\": \"\", \"page\": 31, \"index\": 17, \"width\": 489, \"height\": 252}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-cgkfmgqdcy/fig-018.webp\", \"caption\": \"\", \"page\": 32, \"index\": 18, \"width\": 800, \"height\": 300}]"
motivation: 在结果奖励强化学习中，不忠实的思维链（错误推理后猜对答案）获得与真实推理相同的奖励，导致训练偏差。
method: 提出自一致性采样（SCS），通过引入微小视觉扰动和重复截断重采样参考轨迹，利用轨迹间一致性过滤不忠实轨迹。
result: 在多个多模态推理基准上显著提高了模型推理的忠实性和最终性能。
conclusion: SCS有效矫正了结果奖励强化学习中的不忠实问题，提升了MLLM的推理质量。
---

## Abstract
Outcome‑reward reinforcement learning (RL) is a common—and increasingly significant—way to refine the step‑by‑step reasoning of multimodal large language models (MLLMs). In the multiple‑choice setting—a dominant format for multimodal reasoning benchmarks—the paradigm faces a significant yet often overlooked obstacle: unfaithful trajectories that guess the correct option after a faulty chain of thought receive the same reward as genuine reasoning, which is a flaw that cannot be ignored. We propose Self‑Consistency Sampling (SCS) to correct this issue. For each question, SCS (i) introduces small visual perturbations and (ii) performs repeated truncation‑and‑resampling of a reference trajectory; agreement among the resulting trajectories yields a differentiable consistency score that down‑weights unreliable traces during policy updates. Plugging SCS into RLOO, GRPO, REINFORCE++ series improves accuracy by up to 7.7 percentage points on six multimodal benchmarks with negligible extra computation, offering a simple, general remedy for outcome‑reward RL in MLLMs.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在多模态大语言模型（MLLM）的结果奖励强化学习（outcome‑reward RL）训练中，对于多选形式的推理任务，存在一个严重但常被忽视的缺陷：模型可以通过错误的思维链（例如胡乱猜测）却猜对正确选项，这种“不忠实”的轨迹与真实推理轨迹获得完全相同的奖励（即答案正确即得满分），导致强化学习信号被污染，模型倾向于学习投机取巧的推理路径，而非真正可靠的逻辑推演。
- **研究背景**：当前强化学习（如 RLOO、GRPO、REINFORCE++ 等）在提升 MLLM 推理能力方面取得了显著进展，但在多选设置下，由于只关注最终答案的正确性而忽略推理过程的忠实性，训练效率大打折扣。作者通过初步实验（图 2）证实了这种“不忠实推理”现象的普遍性：多选格式下模型精度提升远低于开放式问答格式，且大量正确答案背后隐藏着错误的推理过程。
- **整体意义**：提出一种轻量级、即插即用的方法——自一致性采样（Self‑Consistency Sampling, SCS），用于修正结果奖励 RL 中的不忠实问题，从而提升推理的可信度和模型最终性能，为多模态/语言模型的强化学习训练提供了一种高效且泛化性强的解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

### 核心思想
利用“如果推理正确，则对同一问题的多次采样结果应该高度一致”这一假设，引入一个额外的**一致性奖励**（consistency reward），与原有的答案正确性奖励（accuracy reward）和格式奖励（format reward）一起指导策略更新。通过降低多样性高的采样（即多次结果不一致）所对应的轨迹权重，惩罚那些不稳定的、可能依赖于猜测的推理路径。

### 关键技术细节
SCS 包含两个主要模块：

1. **截断-重采样（Truncation–Resampling, TR）**  
   - 对每个初始推理轨迹 \(\tau\)，按照预定义的截断比例 \(k\)（例如 0.8）在中间某位置截断，保留前缀 \(\tau_{<}\)。  
   - 将 \(\tau_{<}\) 作为 prefix，多次（\(m\) 次）继续生成完整的答案，得到一组新答案 \(\{a_1, a_2, \dots, a_m\}\)。  
   - 统计这 \(m\) 个答案中不同选项的数量 \(|A|\)（A 为答案集合），多样性越大，一致性越低。

2. **视觉扰动（Visual Perturbation, VP）**  
   - 在每次重采样时，对输入图像叠加微弱的高斯噪声，噪声强度从均匀分布 \(U(\sigma_{\min}, \sigma_{\max})\) 中独立采样。  
   - 迫使模型在视觉轻微变化的条件下依然保持推理一致性，进一步抑制对噪声敏感的虚假推理。

### 一致性奖励计算
\[
r_{\text{con}} = \frac{1}{N} \left( N - |A| \right) \times c
\]
其中 \(N\) 为总选项数（一般等于题目中选项个数），\(c\) 为缩放系数。当 \(|A| = 1\)（全部采样结果一致）时得到最大奖励；当 \(|A| > 1\)（结果多样）时奖励降低。

### 算法流程（伪代码级文字描述）
1. 对每个问题 \(x\)，采样初始轨迹 \(\tau\) 和答案 \(a\)，获得基础奖励 \(r_{\text{acc}}\)（正确性奖励）。
2. 初始化答案集合 \(A = \emptyset\)。
3. 循环 \(m\) 次：
   - 以截断比例 \(k\) 截断 \(\tau\) 得到前缀；
   - 对图像添加随机高斯噪声；
   - 从前缀继续生成新答案 \(a_t\)，加入集合 \(A\)。
4. 计算一致性奖励 \(r_{\text{con}} = (N - |A|) \times c\)。
5. 总奖励 \(r = r_{\text{acc}} + r_{\text{format}} + r_{\text{con}}\)。
6. 使用任意策略梯度算法（如 RLOO、GRPO、REINFORCE++）更新策略，其中 \(r - b\) 作为优势（\(b\) 为基线）。

该方法不依赖额外的奖励模型或人工标注，仅通过多次采样的一致性来引入惩罚，计算开销主要来自多次采样（可并行），总体额外成本可控。

## 3. 实验设计：数据集、基准、对比方法

### 训练数据集
- 来源：M3CoT（通用，7.8k）、Geometry3K（几何，2.1k）、ScienceQA（科学，12.7k，过滤后保留6.2k）。
- 过滤条件：仅保留有多模态输入的多选题。

### 评测基准（6个）
| 基准名称 | 领域/任务 | 多选题数量 |
|---------|-----------|-----------|
| M3CoT | 通用多步多模态推理 | 2.3k |
| ScienceQA | 科学 | 4.241k |
| MathVision | 数学 | 1.5k |
| We-Math | 数学 | 5.0k |
| MMMU (val) | 多学科 | 851 |
| MathVerse | 数学 | 2.1k |

### 对比方法
- **基线**：基础预训练模型（Qwen2.5‑VL‑7B‑Instruct, Qwen2.5‑VL‑3B‑Instruct, InternVL3‑8B）
- **SFT**：在相同训练数据上进行有监督微调
- **普通 RL 算法**：GRPO、REINFORCE++‑baseline、REINFORCE++、RLOO（均使用纯结果奖励：正确性+格式）
- **SCS 增强版**：上述四种 RL 算法分别集成 SCS 后得到的变体

对比严格按照相同的超参数（除 SCS 开关外）进行，并报告各方法的平均精度。

## 4. 资源与算力

- **硬件**：8 张 A800 GPU（每卡约80 GB显存）。
- **训练时长**：每次实验约 24 小时。采用 vLLM 等高效推理引擎并行采样，额外计算开销可接受。
- **额外成本量化**（附录表14）：在 RLOO 设置下，基线训练约 12.5 小时，加入 SCS 后约 17.2 小时（增加约 38%），但精度提升 +7.7pp。

## 5. 实验数量与充分性

- **主实验**：3 种基础模型 × 4 种 RL 算法 × 有/无 SCS，共 24 个模型/算法组合，在 6 个基准上评测，结果见表 2。
- **推理忠实性定量分析**（表 3）：使用人类和两个闭源大模型（o3‑mini, Gemini 2.5 Flash）评估 100 个正确回答的案例，统计不忠实推理的比例。
- **消融实验**：表 4 分别测试截断-重采样（TR）和视觉扰动（VP）的独立贡献。
- **超参数敏感性**：图 5 展示了截断比例（k）和重采样次数（m）的影响；附录表 11-13 对其他 RL 算法也进行了类似分析。
- **统计稳健性**：表 5 报告了 RLOO、GRPO 等算法下 SCS 结果的 95% 置信区间（3 次独立运行），区间较小，证明效果稳定。
- **定性案例**：图 4、15-22 对比了有无 SCS 时模型的输出，直观展示推理质量提升。

**充分性评价**：实验覆盖了多种模型规模（3B/7B/8B）、多种 RL 算法、多种基准，并进行了组件消融、超参数分析和统计验证，结论可信度高。但也存在局限（见第8点）。

## 6. 论文的主要结论与发现

1. **标准结果奖励 RL 在多选任务中存在严重的不忠实推理问题**：正确答案背后往往是错误推理或猜测，且占比可达 15%–25%（图 14）。
2. **SCS 能显著改善推理忠实性**：在不依赖额外奖励模型的前提下，通过一致性奖励有效抑制了不忠实轨迹。
3. **在所有 RL 算法上均带来一致提升**：在 Qwen2.5‑VL‑7B 上，RLOO 提升 7.7pp（57.8→65.5），GRPO 提升 0.9pp，REINFORCE++ 提升 2.0pp，REINFORCE++‑baseline 提升 1.7pp。
4. **跨模型泛化能力强**：在 Qwen2.5‑VL‑3B 上 RLOO 提升 3.2pp，InternVL3‑8B 提升 1.6pp。
5. **两个组件（TR 和 VP）均不可或缺**：单独使用时分别带来 5.2pp 和 5.0pp 提升，联合使用时达到 7.7pp。
6. **超参数鲁棒性较好**：不同截断比例和重采样次数下性能波动在 4pp 以内。

## 7. 优点

- **方法简单高效**：仅需在采样阶段增加少量并行计算，无需训练额外的奖励模型或验证器，成本低、易集成。
- **针对性强**：直接解决了结果奖励 RL 在多选任务中的固有缺陷，具有很强的实际价值。
- **实验设计严谨**：涵盖了多种模型、多种算法、多角度评估（精度、忠实性、消融、超参数），并报告了置信区间。
- **定性分析丰富**：大量案例展示前后对比，直观证明方法有效性。
- **公开代码**：代码已在 GitHub 上发布，便于复现和扩展。

## 8. 不足与局限

- **实验覆盖范围有限**：仅测试了三种 MLLM（Qwen2.5‑VL 两个版本和 InternVL3‑8B），未在纯文本 LLM 或其他更多 MLLM 上验证，泛化性待进一步检验（作者在局限性中也提到了这一点）。
- **仅关注多选场景**：方法假设答案选项已知且有限，对于开放式生成任务无法直接应用。
- **计算开销虽可控但存在**：约 38% 的训练时间增加，在大规模训练时可能仍需优化。
- **超参数敏感度分析不够深入**：虽然展示了变化趋势，但最优参数可能因任务和模型而异，需要人工调参。
- **理论假设的局限性**：假设正确轨迹唯一且必然导致唯一正确答案，但在某些情况下（如多解问题）可能不完全成立。
- **未讨论对公平性、偏见等社会维度的影响**：作为方法论研究，缺少对模型安全性的评估。

（完）
