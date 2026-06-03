---
title: "Evaluating the Inductive Abilities of Large Language Models: Why Chain-of-Thought Reasoning Sometimes Hurts More Than Helps"
title_zh: 评估大语言模型的归纳能力：为什么思维链推理有时弊大于利
authors: "Haibo Jin, Peiyan Zhang, Man Luo, Haohan Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=yRxX01oRIi"
tags: ["query:cot-unfaith"]
score: 10.0
evidence: 直接研究不忠实的思维链推理
tldr: 表明思维链推理会降低归纳性能，显示不忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1600, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 1600, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 1600, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 1879, \"height\": 263}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 1600, \"height\": 600}]"
motivation: 探究CoT推理是否真的能提升归纳推理能力。
method: 设计四个诊断性游戏任务，比较CoT推理与非推理模型的表现。
result: 发现CoT推理会降低归纳性能，特别是在大推理模型中。
conclusion: CoT推理可能放大错误，需要谨慎使用。
---

## Abstract
Large Language Models (LLMs) have shown remarkable progress across domains, yet their ability to perform inductive reasoning—inferring latent rules from sparse examples—remains limited. 
It is often assumed that chain-of-thought (CoT) prompting, as used in Large Reasoning Models (LRMs), enhances such reasoning. 
We investigate this assumption with creating four controlled, diagnostic game-based tasks—chess, Texas Hold’em, dice games, and blackjack—with hidden human-defined rules. 
We find that CoT reasoning can degrade inductive performance, with LRMs often underperforming their non-reasoning counterparts.

To explain this, we present a theoretical framework that reveals how reasoning steps can amplify error through three failure modes: incorrect sub-task decomposition, incorrect sub-task solving, and incorrect final answer summarization. 
Based on our theoretical and empirical analysis, we introduce structured interventions that adapt CoT generation according to our identified failure types. These interventions improve inductive accuracy without retraining. Our findings suggest that effective (CoT) reasoning depends not only on taking more steps but also on ensuring those steps are well-structured.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型（LLMs）在归纳推理（从稀疏样例推断隐藏规则）方面能力有限。通常认为链式思维（CoT）提示能增强这种推理能力，但论文质疑这一假设，试图探究CoT是否真能提升归纳性能。
- **背景**：现有研究多假设推理深度有益，但近期工作发现过长推理轨迹可能降低准确性。论文通过设计受控游戏任务，系统比较推理模型（LRMs）与非推理模型的归纳表现，发现**CoT可能引入噪声并放大错误，有时反而损害性能**。

## 2. 方法论：核心思想、关键技术细节、公式与算法流程

- **核心思想**：将CoT推理建模为序列过程，每一步包含子任务提问、求解和总结。通过理论分析识别三种失败模式，并设计针对性干预。
- **关键技术细节**：
  - **任务框架**：构建四个游戏（国际象棋、德州扑克、骰子、二十一点），每个包含正常规则（NR）和隐藏特殊规则（SR）。模型仅基于简短游戏记录（10–12步）推断规则。
  - **理论公式**：定义信念状态 \(x_k = (m_k, s_k)\)，证据模型 \(g_k = \alpha_k (y^* - m_{k-1}) + \varepsilon_k\)，其中 \(\alpha_k\) 为任务对齐标量，\(\varepsilon_k\) 为高斯噪声。信念更新：\(m_k = m_{k-1} + \gamma_k g_k\)，误差递归：\(e_k = (1 - \gamma_k \alpha_k) e_{k-1} - \gamma_k \varepsilon_k\)。
  - **三种失败模式**：
    1. 错误子任务分解（\(\alpha_k \approx 0\) 或负）
    2. 错误子任务求解（\(\varepsilon_k\) 噪声，常见于数学滥用、过度概括、虚构规则）
    3. 错误最终答案总结（停止点 \(N\) 选择不当，导致U形误差曲线）
  - **干预方法**（无需重训练）：
    - 分解干预：用结构化模板替代自由推理，强制分三阶段（实体识别→规则归纳→验证）
    - 求解干预：提供非数值化示例，引导避免数学滥用
    - 总结干预：限制输出token数 ≤ 1000，避免过度推理

## 3. 实验设计

- **数据集/场景**：四个游戏任务：
  - 国际象棋：8种棋子，各配一个规则（6个NR + 6个SR，每次选4+4），225个独立记录。
  - 德州扑克：5个NR + 5个SR，每次选2+2，100种组合，每轮12手牌。
  - 骰子游戏：4个NR + 4个SR，36种组合，每轮12次。
  - 二十一点：4个NR + 4个SR，36种组合，每手5张牌。
- **Benchmark**：以规则级准确率为指标，使用GPT-4o作为裁判（三次投票），人工验证100例显示完全一致（Cohen’s κ = 1.0）。
- **对比方法**：
  - 模型：4个非推理LLM（GPT-4o, DeepSeek-V3, Qwen2.5-Max, Grok-2）与4个推理LRM（GPT-o3, DeepSeek-R1, QwQ, Grok-3-Mini）
  - 干预策略：分解干预、求解干预、总结干预、联合干预 vs. 无干预基线
  - 额外：对比同一模型（GPT-4o）有/无CoT指令；不同版本（GPT-o1, DeepSeek-R1-0528）

## 4. 资源与算力

- 论文仅提及部分模型本地运行（DeepSeek-R1、QwQ），其余通过API访问。**未明确说明所使用的GPU型号、数量、训练时长或推理计算量**。因此算力细节未知，无法量化。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：8个模型在4个游戏上的规则级准确率，每游戏包含4–6个NR和4–6个SR，总计约12–24个规则子任务。
  - 错误分析：对DeepSeek-R1、QwQ、Grok-3在四个游戏上的失败轨迹进行手动标注（超过100条），统计三种错误类型占比。
  - 干预实验：在四个游戏上测试分解、求解、总结及联合干预，对比基线，并额外在AIME 2024数学题上验证（20题）。
  - 消融：比较不同干预成分的单独与联合效果。
- **充分性与公平性**：
  - 覆盖多种模型家族（OpenAI、DeepSeek、Qwen、xAI）和规模（API版与本地版）。
  - 使用标准API且重复实验取平均，人工验证裁判可靠性。
  - 存在潜在偏差：裁判为GPT-4o，尽管人工验证完全一致，但样本仅100例；游戏任务高度人工设计，泛化性有限；未对比其他推理方法（如Tree-of-Thought）等。

## 6. 主要结论与发现

1. **非推理模型普遍优于推理模型**：在特殊规则（SR）上，GPT-4o等非推理模型准确率显著高于GPT-o3等LRM（例如国际象棋SR1: 56.67% vs. 21.33%）。
2. **CoT推理可能放大错误**：理论证明推理步数存在最优值（U形误差曲线），过深或过浅均有害；实际中解决错误占比超80%，尤其数学滥用最突出。
3. **结构化干预有效**：联合干预将SR准确率提升20–40%（如国际象棋GPT-o3 SR3从4.67%升至54%），且不降低NR性能。
4. **干预可迁移至数学推理**：在AIME 2024上从45%提升至65%。

## 7. 优点

- **新颖的诊断性任务设计**：通过四类游戏模拟真实归纳场景，比静态基准更贴近人类假设形成过程。
- **严格的理论模型**：基于错误传播的数学推导，清晰解释了CoT失效的原因，具有解释性。
- **实用干预方法**：无需重训练，直接修改提示即可见效，易于实际部署。
- **多维度验证**：结合人工标注、消融实验和跨模型比较，增强了结论可靠性。

## 8. 不足与局限

- **任务局限性**：游戏规则高度人为构建，可能无法完全代表真实世界的归纳推理（如科学发现、法律推理）。
- **评判偏差风险**：尽管人工验证一致，但裁判GPT-4o本身可能存在偏见，且样本量有限。
- **泛化性不足**：干预模板针对特定失败模式设计，在其他领域（如视觉推理、多跳问答）效果未知。
- **未量化计算成本**：缺乏对推理模型的token消耗和延迟的详细分析，可能影响实用性。
- **对比基线有限**：未与最新推理策略（如Tree-of-Thought、Self-Refine）比较，仅对比有无CoT。

（完）
