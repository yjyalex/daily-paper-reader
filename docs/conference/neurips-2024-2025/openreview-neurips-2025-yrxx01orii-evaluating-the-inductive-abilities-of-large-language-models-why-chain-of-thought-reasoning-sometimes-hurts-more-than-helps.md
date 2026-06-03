---
title: "Evaluating the Inductive Abilities of Large Language Models: Why Chain-of-Thought Reasoning Sometimes Hurts More Than Helps"
title_zh: 评估大语言模型的归纳能力：为什么思维链推理有时弊大于利
authors: "Haibo Jin, Peiyan Zhang, Man Luo, Haohan Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=yRxX01oRIi"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 评估思维链推理在归纳任务上的表现，显示其可能有害
tldr: 发现思维链推理可能降低归纳推理性能，挑战了现有假设。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1600, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 1600, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 1600, \"height\": 600}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-005.webp\", \"caption\": \"\", \"page\": 9, \"index\": 5, \"width\": 1879, \"height\": 263}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-yrxx01orii/fig-006.webp\", \"caption\": \"\", \"page\": 9, \"index\": 6, \"width\": 1600, \"height\": 600}]"
motivation: 大语言模型归纳推理能力有限，但思维链提示通常被认为能增强推理。
method: 创建四个受控博弈任务，比较推理模型与非推理模型的表现。
result: 思维链推理导致归纳性能下降，推理模型往往不如非推理模型。
conclusion: 推理步骤可能放大偏差，从而损害归纳推理。
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

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大语言模型（LLM）在归纳推理（从稀疏示例中推断潜在规则）方面能力有限。主流观点认为，思维链（Chain-of-Thought, CoT）提示，尤其是在大型推理模型（LRM）中的应用，能增强此类推理。该论文则质疑这一假设。
- **研究动机**：近期有研究指出，较长的推理轨迹可能降低准确性，暗示推理深度与性能之间存在非单调关系。论文旨在系统评估CoT是否总能提升归纳性能，并揭示其失败机制。
- **整体意义**：论文挑战了“CoT = 更好推理”的普遍认知，表明在多步骤推理中，若无结构约束，中间步骤可能引入并放大错误，反而损害归纳能力。这为设计更可靠的推理策略提供了理论依据和实践指导。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将CoT推理建模为一系列离散操作：提出子任务、求解子任务、总结最终答案。每个步骤存在潜在错误来源，错误会沿推理链传播。
- **关键技术细节**：
  - **理论框架**：定义推理状态 \(x_k = (m_k, s_k)\)，其中 \(m_k\) 为当前信念，\(s_k\) 为推理模式（需要提问、需要回答、完成）。模型通过证据向量 \(g_k = \alpha_k (y^\star - m_{k-1}) + \epsilon_k\) 更新信念，得到误差递归 \(e_k = (1 - \gamma_k \alpha_k)e_{k-1} - \gamma_k \epsilon_k\)。由此推导出预期误差 \(E(N)\) 呈U形曲线，存在最优推理长度 \(N^\star\)（详见定理4.1）。
  - **三种失败模式**：
    1. **错误子任务分解（Breakdown Error）**：子问题与目标不对齐（\(\alpha_k\) 接近0或负值），导致误差放大或发散。
    2. **错误子任务求解（Solving Error）**：即使分解正确，求解阶段的随机噪声（\(\epsilon_k\)）也会造成偏差，实证中表现为数学过度使用、过度泛化、幻觉规则等。
    3. **错误最终答案总结（Summarization Error）**：在非最优步数停止，导致欠推理或过推理。
  - **干预措施**：针对三种失败模式设计结构化干预：
    - **分解阶段**：使用固定模板，强制将推理分为“识别实体 → 归纳候选规则 → 验证”三阶段。
    - **求解阶段**：提供非数值型工作示例，引导模型避免数学过度使用，减少随机噪声。
    - **总结阶段**：施加1000词token预算，抑制过度推理，促使模型尽早收敛。

## 3. 实验设计：使用的数据集/场景、benchmark、对比方法

- **数据集/场景**：构建四类受控博弈任务：
  1. **国际象棋**：8种棋子，每个棋子分配一个正常规则或特殊规则（共6个正常+6个特殊，每局选4+4，225种不同对局记录）。
  2. **德州扑克**：5个正常规则（如一对、三条等）和5个特殊规则（如“五张连续质数”），每局选2+2，100种组合。
  3. **骰子游戏**（改编自Sic Bo）：4个正常规则（小/大总和、对子、三条）和4个特殊规则（质数总和、全质数、交替奇偶、差一对），36种组合。
  4. **二十一点**：4个正常规则（21点、爆牌、点数比较、Ace弹性）和4个特殊规则（质数总和、三张同花顺、异色对子、等差序列），36种组合。
  每个任务提供简短的对战记录（10-12步），模型需仅从观察中推断规则，无显式规则描述。
- **Benchmark**：以规则级准确率（rule-level accuracy）为指标，使用GPT-4o作为自动评判器，判断模型推断规则是否与真实规则语义一致（采用多数投票）。并通过人类评估（100个样本，Cohen’s κ=0.86）验证评判可靠性。
- **对比方法**：
  - 非推理模型：GPT-4o, DeepSeek-V3, Qwen2.5-Max, Grok-2。
  - 推理模型（LRM）：GPT-o3, DeepSeek-R1, QwQ, Grok-3-Mini。
  - 干预条件：基线（无干预）、仅分解干预、仅求解干预、仅总结干预、组合干预。

## 4. 资源与算力

- 论文中未明确报告使用的GPU型号、数量或训练时长。
- 提及部分模型（DeepSeek-R1, QwQ）在本地运行以便收集中间推理轨迹，其他模型通过API访问。
- 研究获得国家人工智能研究资源（NAIRR）试点的支持（NAIRR240283），但无具体算力量化细节。

## 5. 实验数量与充分性

- **实验数量**：涵盖四个游戏，每个游戏包含多个规则（正常+特殊，共数十个规则），每个规则有多个实例（如国际象棋每规则225个实例，德州扑克每规则100个组合）。总共生成大量样本（文中提到“over 100 failed reasoning traces”用于人工分析）。
- **充分性**：
  - **多模型对比**：包含8个主流LLM，横跨不同家族和规模（非推理 vs 推理）。
  - **消融实验**：对三种干预策略进行分离和组合测试，验证各成分贡献。
  - **人类评估**：对100个规则推断进行人工验证，显示Cohen’s κ=0.86，与GPT-4o评判一致。
  - **额外实验**：控制模型版本和尺寸（DeepSeek-V3 vs R1-0528, GPT-4o vs o1 vs o3），确认推理模型始终低于非推理模型；在GPT-4o上测试CoT提示，无改善。
  - **泛化测试**：在AIME 2024数学题上测试组合干预，准确率从45%提升至65%。
- **客观与公平**：实验设计控制了规则类型、游戏结构、模型家族，并采用独立人类评估验证自动化评判，结果可靠。但所有任务均为博弈类，可能限制结论的泛化范围。

## 6. 论文的主要结论与发现

1. **归纳推理中，推理模型反而更差**：在特殊规则（隐藏、非表面规则）上，LRM（如GPT-o3, DeepSeek-R1）准确率显著低于非推理模型（如GPT-4o, DeepSeek-V3），平均低20-40%。正常规则上两者均接近饱和。
2. **CoT通过传播错误损害性能**：理论分析表明，错误子任务分解（\(\alpha_k\)不佳）和求解噪声（\(\epsilon_k\)）会在迭代中放大。实证中，求解错误占失败案例的80%以上，其中“数学过度使用”最突出（如将符号输入进行算术运算）。
3. **错误类型的分布**：求解错误 > 分解错误 > 总结错误。求解错误中，数学过度使用在骰子和二十一点中达60%以上。
4. **结构化干预有效提升归纳能力**：组合干预（三阶段约束）在所有游戏中提升特殊规则准确率20-40%，且不降低正常规则性能。求解阶段干预贡献最大。在AIME 2024上，组合干预也带来20%的提升。

## 7. 优点

- **任务设计创新**：采用四类博弈任务，内嵌隐藏规则，精准测量归纳推理能力，避免了静态基准的局限性。
- **理论框架系统**：将推理失败形式化为三种误差源（分解、求解、总结），推导出U形误差曲线，为分析提供了数学基础。
- **实证充分**：涵盖多模型、多游戏、多规则，并辅以人类验证和消融实验，结果稳健。
- **干预可操作**：提出的干预不依赖重训练，仅通过提示工程实现，具有实际应用价值。
- **反驳常见假设**：实证表明“更多推理不一定更好”，为LLM评估提供了新视角。

## 8. 不足与局限

- **任务泛化性有限**：所有任务均为博弈场景，规则抽象程度高，结论能否推广到真实世界推理（如法律、科学发现）尚未验证。
- **干预依赖领域知识**：当前干预（如分解模板、非数值示例）需针对任务人工设计，可能难以自动化迁移到其他领域。
- **模型版本控制不完全**：虽然控制了模型家族，但不同模型间训练数据、参数量差异可能引入混杂因素。论文未深入分析这些因素对归纳能力的影响。
- **自动化评判的潜在偏差**：使用GPT-4o作为评委，尽管人类验证表明一致，但仍存在同源模型偏好风险。
- **算力资源未报告**：缺少具体计算资源消耗细节，影响可复现性。
- **未探索其他推理范式**：仅关注CoT，未涉及思维树（ToT）或思维图（GoT）等其他结构，可能无法捕捉更广泛推理模式的失败机制。

（完）
