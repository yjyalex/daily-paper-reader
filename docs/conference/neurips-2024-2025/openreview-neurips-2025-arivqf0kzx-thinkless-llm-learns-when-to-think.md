---
title: "Thinkless: LLM Learns When to Think"
title_zh: Thinkless：大语言模型学会何时思考
authors: "Gongfan Fang, Xinyin Ma, Xinchao Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ariVQf0KZx"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 基于强化学习学习何时进行思维链推理
tldr: Thinkless使用强化学习自适应选择短或长推理，提高效率。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-arivqf0kzx/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 943, \"height\": 531}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-arivqf0kzx/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 281, \"height\": 578}]"
motivation: 现有推理模型对所有查询都进行繁重推理，造成计算浪费。
method: 使用强化学习训练两个控制标记，让模型根据任务复杂度和自身能力选择短或长推理。
result: 模型能自适应选择推理长度，提升效率而不牺牲准确性。
conclusion: 大语言模型可以学会何时需要深入思考，何时可以快速回答。
---

## Abstract
Reasoning Language Models, capable of extended chain-of-thought reasoning, have demonstrated remarkable performance on tasks requiring complex logical inference. However, applying elaborate reasoning for all queries often results in substantial computational inefficiencies, particularly when many problems admit straightforward solutions. This motivates an open question: Can LLMs learn when to think? To answer this, we propose Thinkless, a learnable framework that empowers an LLM to adaptively select between short-form and long-form reasoning, based on both task complexity and the model's ability. Thinkless is trained under a reinforcement learning paradigm and employs two control tokens, \<short\> for concise responses and \<think\> for detailed reasoning. At the core of our method is a Decoupled Group Relative Policy Optimization (DeGRPO) algorithm, which decomposes the learning objective of hybrid reasoning into two components: (1) a control token loss that governs the selection of the reasoning mode, and (2) a response loss that improves the accuracy of the generated answers. This decoupled formulation enables fine-grained control over the contributions of each objective, stabilizing training and effectively preventing the collapse observed in vanilla GRPO. Empirically, on several benchmarks such as Minerva Algebra, MATH-500, and GSM8K, Thinkless is able to reduce the usage of long-chain thinking by 50% - 90%, significantly improving the efficiency of Reasoning Language Models. The code is available at \url{https://github.com/VainF/Thinkless}

---

## 论文详细总结（自动生成）

# 论文总结：Thinkless: LLM Learns When to Think

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：当前推理语言模型（如DeepSeek-R1）对所有查询都采用长链思维（chain-of-thought）推理，即使对简单问题也生成大量中间步骤，导致显著的计算浪费。
- **核心动机**：探索“大语言模型能否学会何时需要深入思考、何时可以快速回答”——即根据任务复杂度和模型自身能力，自适应地选择短响应（short-form）或长响应（long-form）推理模式。
- **背景**：已有混合推理方法依赖人工设计的启发式规则（如固定计算预算、提示控制信号），但无法根据输入动态调整，且未考虑模型自身能力。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程
- **整体框架（Thinkless）**：两阶段训练。
  - **第一阶段：蒸馏预热（Distillation for Warm-up）**  
    - 使用两个专家模型（一个推理模型π_think、一个指令跟随模型π_short）为每个查询生成配对的长短响应。  
    - 用控制标记`<think>`和`<short>`作为输出首token，通过监督微调（SFT）使目标模型同时学会两种响应风格。
  - **第二阶段：强化学习（Decoupled GRPO）**  
    - 核心算法：**解耦组相对策略优化（Decoupled GRPO, DeGRPO）**。  
    - 将标准GRPO的目标分解为两部分：
      - **控制标记损失**（mode selection）：优化首token（`<think>`或`<short>`）的选择策略。  
      - **响应标记损失**（accuracy improvement）：优化后续响应token的生成质量以提高答案正确性。  
    - 引入权重系数α平衡两种损失，避免标准GRPO中因响应长度差异导致的梯度不平衡和模式崩溃。  
    - 奖励设计：正确短响应得1.0，正确长响应得1.0-γ（γ>0倾向短响应），错误响应得-1.0。
- **DeGRPO目标函数**（公式4）：  
  \[
  J_{\text{DeGRPO}}(\theta) = \mathbb{E}_{x, a^i} \left[ \frac{1}{G}\sum_{i=1}^G \left( \alpha L_{i,0}(\theta) + \frac{1}{T_i}\sum_{t=1}^{T_i} L_{i,t}(\theta) \right) - \beta D_{\text{KL}} \right]
  \]
  其中L_{i,0}为控制标记的surrogate loss，L_{i,t}为响应token的loss，α为平衡系数（文中取0.001）。

## 3. 实验设计
- **数据集和benchmark**：
  - 预热阶段：OpenR1-97K、OpenThoughts-114K、OpenThoughts-1M（开源长链数据），短响应由Qwen2.5-Math-1.5B-Instruct生成。
  - RL阶段：DeepScaleR数据集（约40K个标注数学问题）。
  - 评估：AIME 2024、Minerva Algebra、MATH-500、GSM8K。
- **对比方法**：
  - 基线：DeepSeek-R1-Distill-Qwen-1.5B（完全长链）、Qwen2.5-Math-1.5B-Instruct（短链）、模型合并（Merging）、CoT-Valve、L1控制长度。
  - 混合推理基线：随机路由、基于Qwen-7B的路由器（Router）。

## 4. 资源与算力
- **明确说明**：所有实验在单节点4块H100 GPU上完成。
  - 预热阶段：使用Megatron框架在24K上下文下训练1 epoch，约17,000步。
  - RL阶段：使用VeRL框架训练600步，批量大小128，每查询采样8条轨迹（共1024条/步），学习率1e-6。
- 未提供总训练时间或能耗具体数字。

## 5. 实验数量与充分性
- **实验组数**：主要包括：
  - 4个数学基准上的全模型对比（表1）。
  - 消融实验：标准GRPO vs DeGRPO的崩溃对比（图3a、3b）。
  - 控制标记权重α的影响（图4）。
  - 不同预热数据集规模的效果对比（表2）。
  - 模式统计与案例分析（表3、图5）。
- **充分性评估**：
  - **优点**：覆盖了多个难度等级的数据集，对比了多种现有技术（合并、长度控制、路由器），消融设计清晰。
  - **局限性**：仅在1.5B模型上验证（DeepSeek-R1-Distill-Qwen-1.5B），未在更大模型（如7B、13B）上测试；评估仅限数学领域，未涵盖科学、编程等任务；未提供误差棒或统计显著性检验。

## 6. 主要结论与发现
- **核心结论**：大语言模型可以学会“何时思考”，通过强化学习自适应选择推理模式，可在保持准确性的前提下减少50%-90%的长链推理使用。
- **发现1**：标准GRPO导致模式崩溃（所有样本倾向短响应或长响应），DeGRPO通过解耦控制标记和响应标记的学习实现平衡训练。
- **发现2**：训练呈现U形曲线——起初长响应比例升高（因长链准确率高），随后短响应比例回升（短链质量提升且简单任务被分配给短模式）。
- **发现3**：模型能根据问题难度平滑调整思考概率（简单算术接近0，复杂逻辑接近1）。
- **发现4**：简单预热蒸馏即可使模型学会短响应，但较大蒸馏数据集带来更优性能。

## 7. 优点
- **方法创新性**：首次将强化学习用于自适应混合推理，提出DeGRPO解决模式崩溃问题，设计巧妙且有效。
- **实验扎实**：在四个基准上全面对比多种基线，并深入分析训练动态（U形曲线、模式平衡）。
- **实用价值高**：显著降低推理成本，同时性能损失极小，适合实际部署。
- **代码开源**：提供GitHub仓库，可复现。

## 8. 不足与局限
- **模型规模单一**：仅验证1.5B参数，更大模型（如7B/70B）的效果未知。
- **领域局限**：仅测试数学推理，未涉及科学、编程、逻辑等更广泛领域。
- **预热阶段非最优**：简单SFT造成性能轻微下降，未探索更优混合模型构建方法（如合并、LoRA）。
- **训练稳定性**：α值（0.001）对训练动态敏感，对超参数的选择需要一定调优。
- **实验统计**：未报告误差棒或多次重复结果，可能影响结论可靠性。
- **奖励信号简单**：仅基于正确性，未考虑推理质量或用户偏好细化。

（完）
