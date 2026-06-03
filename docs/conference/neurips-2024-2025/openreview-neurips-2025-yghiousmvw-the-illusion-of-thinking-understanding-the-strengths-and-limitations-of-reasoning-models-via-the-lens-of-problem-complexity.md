---
title: "The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity"
title_zh: 思维的错觉：通过问题复杂性理解推理模型的优势与局限
authors: "Parshin Shojaee, Seyed Iman Mirzadeh, Keivan Alizadeh, Maxwell Horton, Samy Bengio, Mehrdad Farajtabar"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=YghiOusmvw"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 通过问题复杂性系统研究推理模型的优势与局限
tldr: 分析推理轨迹的结构和质量，以理解推理模型的能力边界
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 1287, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 2573, \"height\": 1791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2573, \"height\": 1791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 1288, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 1288, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 1288, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-007.webp\", \"caption\": \"\", \"page\": 6, \"index\": 7, \"width\": 2573, \"height\": 1791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1287, \"height\": 900}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 2575, \"height\": 1788}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-010.webp\", \"caption\": \"\", \"page\": 7, \"index\": 10, \"width\": 1287, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-011.webp\", \"caption\": \"\", \"page\": 7, \"index\": 11, \"width\": 2573, \"height\": 1789}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-012.webp\", \"caption\": \"\", \"page\": 7, \"index\": 12, \"width\": 1287, \"height\": 900}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-013.webp\", \"caption\": \"\", \"page\": 7, \"index\": 13, \"width\": 2574, \"height\": 1795}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-014.webp\", \"caption\": \"\", \"page\": 7, \"index\": 14, \"width\": 2573, \"height\": 1793}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-015.webp\", \"caption\": \"\", \"page\": 7, \"index\": 15, \"width\": 1288, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-016.webp\", \"caption\": \"\", \"page\": 7, \"index\": 16, \"width\": 1288, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-017.webp\", \"caption\": \"\", \"page\": 7, \"index\": 17, \"width\": 2575, \"height\": 1789}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-018.webp\", \"caption\": \"\", \"page\": 7, \"index\": 18, \"width\": 1288, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-019.webp\", \"caption\": \"\", \"page\": 7, \"index\": 19, \"width\": 2575, \"height\": 1787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-020.webp\", \"caption\": \"\", \"page\": 8, \"index\": 20, \"width\": 10562, \"height\": 2982}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-021.webp\", \"caption\": \"\", \"page\": 8, \"index\": 21, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-022.webp\", \"caption\": \"\", \"page\": 8, \"index\": 22, \"width\": 2636, \"height\": 2332}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-023.webp\", \"caption\": \"\", \"page\": 8, \"index\": 23, \"width\": 2636, \"height\": 2334}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-024.webp\", \"caption\": \"\", \"page\": 8, \"index\": 24, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-025.webp\", \"caption\": \"\", \"page\": 8, \"index\": 25, \"width\": 2636, \"height\": 2332}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-026.webp\", \"caption\": \"\", \"page\": 8, \"index\": 26, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-027.webp\", \"caption\": \"\", \"page\": 8, \"index\": 27, \"width\": 2636, \"height\": 2332}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-028.webp\", \"caption\": \"\", \"page\": 8, \"index\": 28, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-029.webp\", \"caption\": \"\", \"page\": 8, \"index\": 29, \"width\": 2338, \"height\": 1359}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-030.webp\", \"caption\": \"\", \"page\": 8, \"index\": 30, \"width\": 2338, \"height\": 1358}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-031.webp\", \"caption\": \"\", \"page\": 8, \"index\": 31, \"width\": 3538, \"height\": 1359}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-032.webp\", \"caption\": \"\", \"page\": 8, \"index\": 32, \"width\": 3538, \"height\": 1358}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-033.webp\", \"caption\": \"\", \"page\": 35, \"index\": 33, \"width\": 5940, \"height\": 1440}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-034.webp\", \"caption\": \"\", \"page\": 35, \"index\": 34, \"width\": 5940, \"height\": 1440}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-035.webp\", \"caption\": \"\", \"page\": 36, \"index\": 35, \"width\": 5913, \"height\": 1139}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-036.webp\", \"caption\": \"\", \"page\": 36, \"index\": 36, \"width\": 5940, \"height\": 1139}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-037.webp\", \"caption\": \"\", \"page\": 38, \"index\": 37, \"width\": 3085, \"height\": 761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-038.webp\", \"caption\": \"\", \"page\": 38, \"index\": 38, \"width\": 3084, \"height\": 878}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-039.webp\", \"caption\": \"\", \"page\": 38, \"index\": 39, \"width\": 3084, \"height\": 761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-040.webp\", \"caption\": \"\", \"page\": 38, \"index\": 40, \"width\": 3085, \"height\": 761}]"
motivation: 当前评估主要关注最终答案准确性，忽略了推理轨迹的质量和结构。
method: 使用可控谜题任务系统分析推理轨迹的结构和缩放特性。
result: 揭示了推理模型在复杂问题上的局限性以及推理轨迹的质量差异。
conclusion: 推理轨迹的质量和结构是理解模型推理能力的关键。
---

## Abstract
Recent generations of frontier language models have introduced Large Reasoning Models (LRMs) that generate detailed thinking processes before providing answers. While these models demonstrate improved performance on reasoning benchmarks, their fundamental capabilities, scaling properties, and limitations remain insufficiently understood.
Current evaluations primarily focus on established mathematical and coding benchmarks, emphasizing final answer accuracy. However, this evaluation paradigm often suffers from data contamination and does not provide insights into the reasoning traces' structure and quality. In this work, we systematically investigate these gaps with the help of controllable puzzle environments that allow precise manipulation of compositional complexity while maintaining consistent logical structures.
This setup enables the analysis of not only final answers but also the internal reasoning traces, offering insights into how LRMs ``think''.
Through extensive experimentation across diverse puzzles, we show that frontier LRMs face a complete accuracy collapse beyond certain complexities. Moreover, they exhibit a counterintuitive scaling limit: their reasoning effort increases with problem complexity up to a point, then declines despite having an adequate token budget. By comparing LRMs with their standard LLM counterparts under equivalent inference compute, we identify three performance regimes: (1) low-complexity tasks where standard models surprisingly outperform LRMs, (2) medium-complexity tasks where additional thinking in LRMs demonstrates advantage, and (3) high-complexity tasks where both models experience complete collapse.
We found that LRMs have limitations in exact computation: they fail to use explicit algorithms and reason inconsistently across scales and problems. We also investigate the reasoning traces in more depth, studying the patterns of explored solutions and analyzing the models' computational behavior, shedding light on their strengths, limitations, and ultimately raising questions about the nature for their reasoning capabilities.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前大型推理模型（LRMs，如 OpenAI o1/o3、DeepSeek-R1、Claude Sonnet Thinking 等）在数学和编程基准上表现出色，但其真正推理能力、缩放特性及局限性仍未被充分理解。现有评估主要依赖最终答案准确率，忽视了推理轨迹（推理过程中的中间步骤）的结构与质量，且常受数据污染影响。
- **整体含义**：本文旨在通过可控谜题环境，系统剖析 LRMs 的推理机制。研究不仅关注最终答案，更深入分析其内部推理过程，揭示模型在面对不同复杂度问题时的行为模式、缩放限制及根本性弱点，从而质疑当前“推理模型”的本质及其通用推理能力。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程

### 核心思想
- 采用**可控谜题环境**，允许精确操纵问题的**组合复杂度**（如增加元素数量），同时保持核心逻辑一致。通过这些环境，可同时验证最终答案和中间推理步骤的正确性，实现对推理轨迹的细粒度分析。
- 比较**推理模型**（具有强化学习驱动的长思维链）与其**标准基座模型**（相同骨干网络但不启用思维模式），在等量推理计算预算下的表现差异。

### 关键技术细节
1. **谜题环境**：
   - **河内塔**：n个盘子的递归转移，遵循大小约束。复杂度控制：盘子数 n（所需最小移动次数为 2ⁿ − 1）。
   - **跳棋（Checker Jumping）**：线性排列的红蓝棋子和一个空格，通过滑动或跳跃交换位置。复杂度控制：单色棋子数 n（最小移动次数 (n+1)² − 1）。
   - **过河问题（River Crossing）**：n对演员与代理人需在约束下乘船过河。复杂度控制：对数 n。
   - **积木世界（Blocks World）**：块状堆叠规划问题。复杂度控制：积木数 n。

2. **推理轨迹分析**：
   - 提取模型思维过程中的所有中间解（候选方案）。
   - 记录每个解的**位置**（在思维流中的令牌位置）、**正确性**（通过谜题模拟器验证）及对应问题复杂度。
   - 分析思维过程中正确与错误解的分布演变。

3. **对比实验设计**：
   - 对每个谜题，逐渐增加复杂度参数 N，观察模型准确率和推理令牌消耗的变化。
   - 对比推理模型（如 Claude 3.7 Sonnet + Thinking）与非推理模型（相同模型关闭思维模式）在相同推理计算预算下的 pass@k 表现。

### 公式与算法（文字说明）
- 论文未引入新公式，但利用了谜题固有的复杂度可量化特性（如移动次数随 N 呈指数、二次或线性增长）。
- 在算法执行实验中，向模型提供明确的递归算法伪代码（如河内塔的标准递归解法），要求模型仅需按步骤执行，以测试其算法遵循能力。

## 3. 实验设计：数据集/场景、基准、对比方法

### 数据集/场景
- **谜题环境**：四个控制严密的规划与约束满足任务（河内塔、跳棋、过河、积木世界），每个谜题通过调整元素数量（N）生成不同复杂度的实例。
- **数学基准**（用于初步对比）：MATH-500、AIME24、AIME25，以展示传统基准的局限性（如潜在数据污染、无法控制复杂度）。

### 基准
- 无预设“基准”榜单，而是通过对比推理模型与其同系列标准模型的性能差异，以及各模型在不同复杂度下的行为模式，作为评估依据。

### 对比方法
- **模型对**：
  - Claude 3.7 Sonnet（开启 vs 关闭 Extended Thinking）
  - DeepSeek-R1 vs DeepSeek-V3
  - QwQ-32B vs Qwen2.5-32B（附录）
  - o3-mini（medium/high 配置）
- **消融实验**：
  - 温度设为 0（消除采样随机性）重复实验。
  - 在提示中提供完整求解算法，测试算法执行能力。
- **评估指标**：
  - 最终答案准确率（完全正确才计分）。
  - 推理努力（思维令牌数）。
  - 首次失败移动位置。
  - 思维轨迹内解的时序分布。

## 4. 资源与算力

- **未明确说明**具体 GPU 型号、数量或训练时长。论文提到实验主要依赖封闭模型的 API（Claude、o3-mini），以及本地服务器运行 DeepSeek 等开源模型（最大生成长度 64k tokens，温度 1.0）。
- 附录中声明“detailed computer resources and GPU hours will be reported in Appendix”，但提供的文本中未包含这些细节。因此只能指出**信息缺失**。

## 5. 实验数量与充分性

- **实验数量**：大量系统实验覆盖四个谜题，每个复杂度级别、每个模型均采样 25 次；包括多组模型对对比、消融实验（温度控制、算法提供）、轨迹分析等。总实验样本数以百计。
- **充分性与客观性**：
  - **优点**：实验设计系统，控制变量（相同骨干网络的推理/非推理对），使用可重复的模拟器验证，避免主观评分。
  - **潜在不足**：
    - 仅四种谜题，未能覆盖真实世界中复杂、开放的知识推理。
    - 黑盒 API 模型无法进行内部机制分析。
    - 模拟器假设完美验证，在非确定性领域可能不适用。
    - 论文自身也指出这些局限。

## 6. 主要结论与发现

### 关键发现
1. **三个表现区间**：
   - **低复杂度**：标准 LLM 准确率更高且更节省令牌（推理模型存在“过度思考”）。
   - **中等复杂度**：推理模型优势显现，准确性更高。
   - **高复杂度**：两类模型均完全崩溃（准确率逼近零），推理模型仅延迟崩溃点，无法突破根本限制。

2. **反直觉的推理努力下降**：随着复杂度接近崩溃临界点，推理模型的思维令牌量不增反降，尽管远未达到上下文长度限制。表明存在一种**推理计算缩放极限**。

3. **思维轨迹模式**：
   - 简单问题：模型过早找到正确解后继续探索错误解（“过度思考”）。
   - 中等复杂度：正确解通常出现在思维后期，经过大量错误探索。
   - 高复杂度：模型早期即锚定错误解，无法恢复，浪费后续令牌预算。

4. **精确计算局限性**：
   - 提供明确求解算法后，模型性能无明显提升（河内塔、跳棋），表明其在*执行算法步骤*上存在根本困难。
   - 首次失败移动位置与复杂度不成单调关系（有时在更高复杂度下更早失败），反映策略应用的不一致性。
   - 不同谜题间模型的表现差异巨大（如河内塔 100 步正确，过河只能 4 步），更多反映训练数据分布而非问题固有计算复杂度。

## 7. 优点：方法或实验设计的亮点

- **创新方法论**：使用可控谜题环境代替标准基准，避免数据污染，允许精细控制复杂度并完全验证推理过程。
- **深入轨迹分析**：首次系统量化推理轨迹中正确/错误解的时间分布，揭示过度思考、早期锚定等行为模式。
- **公平比较**：严格对比相同骨干网络的推理/非推理模型，控制推理计算预算，排除架构或参数量干扰。
- **多角度验证**：结合最终准确率、推理努力、失败位置、算法执行测试等多维度证据，增强结论可靠性。

## 8. 不足与局限

- **谜题覆盖不足**：仅四种规划/约束满足任务，未涉及知识密集型、常识推理或开放式生成任务。
- **黑盒依赖**：主要依托封闭模型的 API 输出，无法分析内部状态或架构组件。
- **完美验证假设**：模拟器严格逐步骤验证，但真实世界问题往往缺乏此类精确验证，限制结论泛化。
- **算力信息缺失**：未提供 GPU 型号、数量、训练时长等详细信息，影响可复现性。
- **统计误差未充分报告**：论文提到“reasoning models are compute heavy”，未给出误差棒（error bars），仅呈现点估计，可能影响对结果统计显著性的判断。

（完）
