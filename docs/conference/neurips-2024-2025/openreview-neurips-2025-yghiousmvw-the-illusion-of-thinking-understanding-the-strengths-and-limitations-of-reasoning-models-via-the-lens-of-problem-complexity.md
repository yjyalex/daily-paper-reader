---
title: "The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity"
title_zh: 思维的幻觉：通过问题复杂性理解推理模型的优势与局限
authors: "Parshin Shojaee, Seyed Iman Mirzadeh, Keivan Alizadeh, Maxwell Horton, Samy Bengio, Mehrdad Farajtabar"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=YghiOusmvw"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 系统研究推理痕迹的结构和质量
tldr: 超越最终准确率评估推理模型的局限性
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 1287, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 2573, \"height\": 1791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2573, \"height\": 1791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 1288, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 1288, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 1288, \"height\": 898}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-007.webp\", \"caption\": \"\", \"page\": 6, \"index\": 7, \"width\": 2573, \"height\": 1791}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1287, \"height\": 900}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 2575, \"height\": 1788}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-010.webp\", \"caption\": \"\", \"page\": 7, \"index\": 10, \"width\": 1287, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-011.webp\", \"caption\": \"\", \"page\": 7, \"index\": 11, \"width\": 2573, \"height\": 1789}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-012.webp\", \"caption\": \"\", \"page\": 7, \"index\": 12, \"width\": 1287, \"height\": 900}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-013.webp\", \"caption\": \"\", \"page\": 7, \"index\": 13, \"width\": 2574, \"height\": 1795}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-014.webp\", \"caption\": \"\", \"page\": 7, \"index\": 14, \"width\": 2573, \"height\": 1793}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-015.webp\", \"caption\": \"\", \"page\": 7, \"index\": 15, \"width\": 1288, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-016.webp\", \"caption\": \"\", \"page\": 7, \"index\": 16, \"width\": 1288, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-017.webp\", \"caption\": \"\", \"page\": 7, \"index\": 17, \"width\": 2575, \"height\": 1789}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-018.webp\", \"caption\": \"\", \"page\": 7, \"index\": 18, \"width\": 1288, \"height\": 897}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-019.webp\", \"caption\": \"\", \"page\": 7, \"index\": 19, \"width\": 2575, \"height\": 1787}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-020.webp\", \"caption\": \"\", \"page\": 8, \"index\": 20, \"width\": 10562, \"height\": 2982}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-021.webp\", \"caption\": \"\", \"page\": 8, \"index\": 21, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-022.webp\", \"caption\": \"\", \"page\": 8, \"index\": 22, \"width\": 2636, \"height\": 2332}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-023.webp\", \"caption\": \"\", \"page\": 8, \"index\": 23, \"width\": 2636, \"height\": 2334}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-024.webp\", \"caption\": \"\", \"page\": 8, \"index\": 24, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-025.webp\", \"caption\": \"\", \"page\": 8, \"index\": 25, \"width\": 2636, \"height\": 2332}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-026.webp\", \"caption\": \"\", \"page\": 8, \"index\": 26, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-027.webp\", \"caption\": \"\", \"page\": 8, \"index\": 27, \"width\": 2636, \"height\": 2332}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-028.webp\", \"caption\": \"\", \"page\": 8, \"index\": 28, \"width\": 2635, \"height\": 2335}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-029.webp\", \"caption\": \"\", \"page\": 8, \"index\": 29, \"width\": 2338, \"height\": 1359}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-030.webp\", \"caption\": \"\", \"page\": 8, \"index\": 30, \"width\": 2338, \"height\": 1358}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-031.webp\", \"caption\": \"\", \"page\": 8, \"index\": 31, \"width\": 3538, \"height\": 1359}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-032.webp\", \"caption\": \"\", \"page\": 8, \"index\": 32, \"width\": 3538, \"height\": 1358}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-033.webp\", \"caption\": \"\", \"page\": 35, \"index\": 33, \"width\": 5940, \"height\": 1440}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-034.webp\", \"caption\": \"\", \"page\": 35, \"index\": 34, \"width\": 5940, \"height\": 1440}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-035.webp\", \"caption\": \"\", \"page\": 36, \"index\": 35, \"width\": 5913, \"height\": 1139}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-036.webp\", \"caption\": \"\", \"page\": 36, \"index\": 36, \"width\": 5940, \"height\": 1139}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-037.webp\", \"caption\": \"\", \"page\": 38, \"index\": 37, \"width\": 3085, \"height\": 761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-038.webp\", \"caption\": \"\", \"page\": 38, \"index\": 38, \"width\": 3084, \"height\": 878}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-039.webp\", \"caption\": \"\", \"page\": 38, \"index\": 39, \"width\": 3084, \"height\": 761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yghiousmvw/fig-040.webp\", \"caption\": \"\", \"page\": 38, \"index\": 40, \"width\": 3085, \"height\": 761}]"
motivation: 当前评估仅关注最终答案，忽视推理痕迹质量。
method: 使用可控拼图任务分析推理痕迹的结构和缩放性质。
result: 揭示了推理模型在复杂问题上的根本能力限制。
conclusion: 需要更全面的推理质量评估范式。
---

## Abstract
Recent generations of frontier language models have introduced Large Reasoning Models (LRMs) that generate detailed thinking processes before providing answers. While these models demonstrate improved performance on reasoning benchmarks, their fundamental capabilities, scaling properties, and limitations remain insufficiently understood.
Current evaluations primarily focus on established mathematical and coding benchmarks, emphasizing final answer accuracy. However, this evaluation paradigm often suffers from data contamination and does not provide insights into the reasoning traces' structure and quality. In this work, we systematically investigate these gaps with the help of controllable puzzle environments that allow precise manipulation of compositional complexity while maintaining consistent logical structures.
This setup enables the analysis of not only final answers but also the internal reasoning traces, offering insights into how LRMs ``think''.
Through extensive experimentation across diverse puzzles, we show that frontier LRMs face a complete accuracy collapse beyond certain complexities. Moreover, they exhibit a counterintuitive scaling limit: their reasoning effort increases with problem complexity up to a point, then declines despite having an adequate token budget. By comparing LRMs with their standard LLM counterparts under equivalent inference compute, we identify three performance regimes: (1) low-complexity tasks where standard models surprisingly outperform LRMs, (2) medium-complexity tasks where additional thinking in LRMs demonstrates advantage, and (3) high-complexity tasks where both models experience complete collapse.
We found that LRMs have limitations in exact computation: they fail to use explicit algorithms and reason inconsistently across scales and problems. We also investigate the reasoning traces in more depth, studying the patterns of explored solutions and analyzing the models' computational behavior, shedding light on their strengths, limitations, and ultimately raising questions about the nature for their reasoning capabilities.

---

## 论文详细总结（自动生成）

# 论文《The Illusion of Thinking》中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前大型推理模型（LRMs，如o1、DeepSeek-R1、Claude Sonnet Thinking）虽然在地理基准上表现提升，但评估仍主要依赖数学和编程题目的最终答案准确性。这种范式存在两个问题：一是**数据污染**（模型在训练中可能见过类似题目）；二是**忽视推理痕迹的质量**（无法洞察模型内部的思维过程）。
- **整体含义**：论文旨在通过**可控难度的拼图环境**，系统研究LRMs的**根本能力、缩放特性和局限**，从而回答“这些模型是否具备可泛化的推理能力？其推理过程本质上是模式匹配还是算法推理？”
- **核心问题**：LRMs的性能如何随问题复杂性变化？与标准LLMs相比有何本质差异？推理痕迹中是否存在结构性的低效或崩溃模式？

## 2. 方法论

- **核心思想**：使用**四个经典拼图**（Tower of Hanoi、Checker Jumping、River Crossing、Blocks World），通过调整问题规模（如圆盘数、棋子数、方块数）精确控制**组成复杂性**，同时保持逻辑结构一致。
- **关键技术细节**：
  - 对每个拼图，模拟器对最终答案**逐步骤验证**（而非仅检查最终状态），可定位首次失败步骤。
  - 分析推理痕迹时，提取模型思考过程中产生的**中间候选解**，记录其在思维流中的位置（归一化令牌位置）和正确性。
  - 对比LRMs与标准LLMs（同一基座）在**等效推理计算预算**下的表现，通过pass@k评估上限能力。
  - 额外实验：**提供显式算法**（如汉诺塔递归伪代码）给模型，观察其执行能力；**温度消融**（temperature=0）以消除采样影响。
- **算法/流程**（文字描述）：
  - 为每个拼图实例给定初始状态和目标状态，模型需输出合法移动序列。
  - 模拟器逐条检查移动是否违反规则（如大小顺序、跳跃条件、船容量等）。
  - 分析推理痕迹时，使用正则表达式提取所有候选解，并记录每个解在思考过程中的令牌位置。
  - 比较thinking与非thinking模型时，固定推理计算预算（令牌数），计算pass@k曲线。

## 3. 实验设计

- **数据集/场景**：**四个可控拼图环境**：
  - Tower of Hanoi（N=1~12，移动次数 2^N-1）
  - Checker Jumping（N=1~15，移动次数 (N+1)^2-1）
  - River Crossing（N=2~5，移动次数随N线性增长）
  - Blocks World（N=4~40，移动次数随N线性增长）
- **基准方法**：在数学基准（MATH-500、AIME24、AIME25）上先做初步对比，然后转向拼图环境进行系统实验。
- **对比模型**：
  - **Thinking vs Non-Thinking配对**：Claude 3.7 Sonnet（with/without extended thinking）、DeepSeek R1 vs V3、QwQ-32B vs Qwen2.5-32B
  - **额外Thinking模型**：o3-mini（medium/high配置）、DeepSeek-R1-Distill-Qwen-32B
- **实验设计特点**：每个复杂度水平取**25个样本**，针对每个拼图、每个模型运行多次，确保统计意义。实验包括：
  - 准确性 vs 复杂性的完整扫描
  - 推理努力（思考令牌数） vs 复杂性
  - 中间解的位置与正确性分析
  - 首次失败步骤分析
  - 算法提供 vs 默认提示对比
  - 温度=0消融（去除采样影响）

## 4. 资源与算力

- **文中未明确说明**具体的GPU型号、数量或训练时长。
- 提到实验使用**Claude 3.7 Sonnet和o3-mini的API**（黑盒），**DeepSeek R1/V3本地服务器**（最大长度64k tokens）。
- **成本**方面仅提及“推理模型计算开销大”，但未量化具体资源消耗。因此无法评估实验的可复现性成本。

## 5. 实验数量与充分性

- **实验规模**：涵盖4个拼图 × 多个复杂度等级 × 多个模型对（至少5个thinking模型 + 3个非thinking基线） × 每条件25个样本；还包含消融（温度=0、算法提供、数学基准对比）。
- **充分性**：
  - **覆盖范围广**：问题复杂性从极低到完全崩溃点均有采样。
  - **客观性**：使用确定性模拟器逐步骤验证，结果可严格复现；对比thinking与非thinking模型时为同一基座，公平。
  - **不足之处**：
    - 每个条件仅25个样本，对于较复杂问题可能不足以稳定估计精确准确率。
    - 某些实验（如QwQ-32B对）较晚添加，详细分析较少。
    - 未在更多真实世界推理任务（如长链条规划、知识密集型推理）上验证。

## 6. 主要结论与发现

1. **完全崩溃**：所有LRMs在超过特定复杂度阈值后准确率降至接近0%，无法泛化解决规划问题。
2. **三种表现体制**：
   - 低复杂性：标准LLMs优于LRMs（更高效、更准确）。
   - 中等复杂性：LRMs的思考过程带来优势。
   - 高复杂性：两者均完全崩溃。
3. **反直觉的缩放极限**：推理努力（思考令牌数）随复杂度先增后减，尽管有充足的令牌预算。在崩溃点附近模型“放弃思考”。
4. **推理痕迹模式**：
   - 简单问题：早期找到正确解但持续探索错误解（“过度思考”）。
   - 中等问题：先探索错误解，后期才找到正确解。
   - 复杂问题：早期错误固定，无法恢复，浪费令牌预算。
5. **精确计算局限**：即使提供显式算法，模型执行时仍会在相同复杂度点崩溃，说明其无法可靠跟随逻辑步骤。
6. **跨问题不一致性**：失败步数随复杂度非单调变化（如对N=10失败于100步，对N=12可能更早失败于50步）。不同拼图间初始失败步数差异巨大（汉诺塔可达100步，河内拼图仅4步），暗示训练数据分布影响。

## 7. 优点

- **挑战现有评估范式**：突破仅看最终准确率的局限，通过**可控拼图环境**允许操纵复杂性，并**深入分析推理痕迹**。
- **系统性的对比设计**：使用同一基座的thinking vs non-thinking模型对，在等效推理预算下比较，排除了模型架构和参数量的混淆因素。
- **揭示反直觉现象**：推理努力先增后减、过度思考、算法提供无效等发现对LRM的设计和部署有重要启示。
- **方法严谨**：每个实验均进行多步验证，包括模拟器校验、提取去重、位置归一化等，确保内部一致性。
- **补充材料丰富**：附录包含完整的实验细节、定性思考链示例、以及针对批评点的详细回应，增强了可信度。

## 8. 不足与局限

- **任务覆盖狭窄**：仅使用四个经典拼图，未涉及更广泛的推理场景（如数学证明、科学推理、开放域规划）。
- **黑盒依赖**：主要依赖API（Claude、o3-mini）和本地服务器（DeepSeek），无法分析模型内部机制或架构组件。
- **样本量有限**：每个条件仅25个样本，高复杂度时可能采样不足，尤其对于随机性强的模型。
- **温度消融不充分**：虽然做了温度=0实验，但未系统讨论采样温度对结果稳定性的影响。
- **数据污染风险未完全消除**：虽然拼图环境新颖，但经典拼图可能也在训练数据中出现过（如汉诺塔递归算法），论文仅通过操作复杂性来缓解，未明确证明零污染。
- **评估假设**：假设推理可被步骤级模拟器完全验证，但现实推理往往更结构化、模糊，结果可能不直接迁移。
- **计算资源未公开**：缺乏GPU型号、运行时间、总成本等详细信息，影响实验的可复现性和经济性评估。
- **模型版本时效性**：论文中的模型版本（如Claude 3.7 Sonnet、DeepSeek-R1）可能后续更新，结论可能不适用于最新版本。

（完）
