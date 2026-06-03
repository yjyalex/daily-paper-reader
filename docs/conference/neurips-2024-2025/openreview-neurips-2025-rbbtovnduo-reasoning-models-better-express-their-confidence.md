---
title: Reasoning Models Better Express Their Confidence
title_zh: 推理模型能更好地表达其置信度
authors: "Dongkeun Yoon, Seungone Kim, Sohee Yang, Sunkyoung Kim, Soyeon Kim, Yongil Kim, Eunbi Choi, Yireun Kim, Minjoon Seo"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rbBtoVnduo"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 通过置信度校准研究思维链推理的忠实度
tldr: 扩展思维链的推理模型展现更好的置信度校准，表明忠实性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 大模型常不准确表达置信度，需要评估推理模型的置信度校准。
method: 在六个数据集上基准测试六个推理模型的置信度校准。
result: 推理模型在33/36设置中置信度校准优于非推理模型。
conclusion: 扩展的思维链推理提高了置信度表达的准确性。
---

## Abstract
Despite their strengths, large language models (LLMs) often fail to communicate their confidence accurately, making it difficult to assess when they might be wrong and limiting their reliability. In this work, we demonstrate that reasoning models that engage in extended chain-of-thought (CoT) reasoning exhibit superior performance not only in problem-solving but also in accurately expressing their confidence.
Specifically, we benchmark six reasoning models across six datasets and find that they achieve strictly better confidence calibration than their non-reasoning counterparts in 33 out of the 36 settings. Our detailed analysis reveals that these gains in calibration stem from the slow thinking behaviors of reasoning models (e.g., exploring alternative approaches and backtracking) which enable them to adjust their confidence dynamically throughout their CoT, making it progressively more accurate. In particular, we find that reasoning models become increasingly better calibrated as their CoT unfolds, a trend not observed in non-reasoning models. Moreover, removing slow thinking behaviors from the CoT leads to a significant drop in calibration. Lastly, we show that non-reasoning models also demonstrate enhanced calibration when simply guided to slow think via in-context learning, fully isolating slow thinking as the source of the calibration gains.

---

## 论文详细总结（自动生成）

## 论文结构化总结

### 1. 核心问题与整体含义（研究动机与背景）

大语言模型（LLMs）常存在过度自信问题，即使犯错也表现出高置信度，这严重制约了其在高风险场景下的可靠性。近期，推理模型（如OpenAI o1、Deepseek-R1）通过生成扩展的思维链（Chain-of-Thought, CoT）展现出强大的问题解决能力，这些模型表现出“慢思考”行为（如探索替代方案、回溯验证、自我修正）。然而，这种慢思考行为是否也有助于模型准确评估并表达自身置信度（即“知道自身所知”）仍未被充分研究。本文旨在系统探究推理模型在置信度表达（verbalized confidence estimation）方面的能力，并揭示慢思考如何带来校准增益。

### 2. 方法论

- **核心思想**：推理模型通过扩展的CoT与慢思考动态调整置信度，从而比非推理模型更准确地表达置信度。校准增益主要源于慢思考行为，而非简单的更长推理或显式置信度推理。
- **关键技术细节**：
  - **三步推理流程**：在单轮对话中，模型依次进行：① 解决方案推理（Solution Reasoning）→ ② 置信度推理（Confidence Reasoning）→ ③ 置信度言语化（Confidence Verbalization）。置信度划分为十个区间（如0~0.1, 0.1~0.2, ...），每个区间附带语言描述和数值范围。
  - **推理模型强制置信度推理**：部分推理模型（如R1-Distill, OR1-Preview, GLM-Z1）在CoT中较少主动进行置信度推理，故采用两阶段推理：先生成至`</think>`前，替换为“Okay, now let's assess my overall thinking process...”并再次推理，确保置信度推理包含在思考过程中。
  - **消融分析**：移除CoT中的特定成分（置信度推理、认知标记、非线性推理）以识别关键贡献因素。
  - **上下文学习诱导慢思考**：对非推理模型使用R1-Distill-Qwen-32B的慢思考示例进行少量提示，验证慢思考本身的有效性。
- **公式/算法流程**（文字说明）：
  1. 输入问题，模型生成CoT，包含非线性慢思考行为。
  2. 在思考过程结束后，模型给出答案和置信度（从十类中选择）。
  3. 通过正则表达式提取答案和置信度，使用GPT-4o mini匹配答案正确性。
  4. 计算校准指标：ECE、Brier Score、AUROC。

### 3. 实验设计

- **数据集**：
  - **知识型**（Knowledge-focused）：TriviaQA（事实问答）、NonambigQA（模糊问答）。用于控制任务难度，确保推理与非推理模型准确率相近，隔离校准增益来源。
  - **推理密集型**（Reasoning-intensive）：MMLU-Pro 和 SuperGPQA 的数学（Math）和非数学（Non-Math）子集，验证泛化性。
- **Benchmark**：六个推理模型 vs. 非推理对手，涵盖四类骨干模型。
- **对比方法**：
  - **骨干1**：Qwen2.5-32B-Instruct vs. R1-Distill-Qwen-32B, OR1-Preview, QwQ
  - **骨干2**：GLM-4-0414 vs. GLM-Z1-0414
  - **骨干3**：EXAONE-3.5-Instruct vs. EXAONE-Deep
  - **骨干4**：Qwen3 Non-thinking mode vs. Qwen3 Thinking mode（同一检查点，仅切换模式）
  - 另评估非推理模型使用高级提示（Top-K、Multi-step、Two-turn）以及抽样解码等变体。
- **评估指标**：ECE（期望校准误差）、Brier Score（均方概率差）、AUROC（区分能力）。所有指标均考虑。

### 4. 资源与算力

文中在Appendix B.1提及：
- 使用Nvidia A6000 48GB 或 A100 80GB GPU，每个32B模型评估需2张GPU。
- 借助vLLM进行高效推理，单个32B推理模型在单数据集上约需1小时（2张A6000）。
- 未明确说明总训练时间或消耗的精确GPU小时数，但指出由于实验范围广，未对所有组合进行bootstrap（仅对代表性模型做标准差分析）。

### 5. 实验数量与充分性

- **主要基准实验**：6个推理模型 × 6个数据集/子集 = 36个设置，其中33/36推理模型胜出（主要表格 Table 1, Table 2）。每个设置使用1000个均匀采样样本，并做了bootstrap（5次重采样）评估变异性（Appendix A.2.4）。
- **消融实验**：在R1-Distill-Qwen-32B上对TriviaQA和NonambigQA进行三种消融（移除置信度推理、移除认知标记、移除非线性推理），并对比No CoT基线。
- **额外稳健性实验**（Appendix A.2）：
  - 不同置信度表达风格（仅语言描述、直接数值概率无分箱）
  - 非推理模型高级提示（Top-K, Multi-step, Two-turn）
  - 抽样解码（Temperature=0.6）与贪心解码对比
- **动态校准分析**（Section 4.1）：将CoT分为11个累积段，测量校准指标变化，对每个模型/数据集拟合线性趋势并报告显著性。
- **上下文学习诱导慢思考实验**（Section 4.3）：三个非推理模型分别用慢思考示例提示，比较校准改善。
- **预算强制实验**（Section 5, Appendix A.1）：附加“Wait,”强制延长CoT观察效果。
- **模型规模实验**（Section 5, Appendix A.1）：测试不同大小模型（7B, 14B, 32B?）的校准增益相对变化。
- 总体而言，实验覆盖面广，考虑了多种变体和控制条件，设计较为充分公平，但存在一定偏差（如仅聚焦32B规模，少数AUROC异常因多选格式导致等，文中已解释）。

### 6. 主要结论与发现

1. **推理模型在校准上显著优于非推理模型**：在33/36个设置中全面领先，即使知识型任务上准确率相近甚至略低时仍表现更优（如GLM-Z1-0414在NonambigQA上准确率低7%但校准更好）。
2. **校准增益源于慢思考**：推理模型的校准随CoT进展逐步改善（线性趋势显著），非推理模型无此趋势甚至变差；移除非线性推理导致校准大幅下降；通过上下文学习诱导非推理模型慢思考也能提升校准。
3. **非线性慢思考行为是关键**：探索替代方案、回溯验证等行为比显式置信度推理或认知标记更重要。
4. **预算强制不必然提升校准**：延长CoT不一定继续改善，质量比长度更关键。
5. **模型规模越大，慢思考的校准增益越显著**。

### 7. 优点

- **全面性**：覆盖4类骨干模型、6个数据集、多种提示策略和解码方式，提供了大量对比和消融，结论稳健。
- **分析深入**：不仅报告整体指标，还分析了CoT动态变化、消融各成分、上下文学习模拟，有力证明因果关系。
- **控制严谨**：知识型数据集的设计隔离了准确率的影响，确保校准差异非性能所致；Qwen3同一模型不同模式的对比直接凸显慢思考作用。
- **实用价值**：展示慢思考可提升模型可信度，对部署高可靠性LLM具有指导意义；且无需修改模型参数即可通过上下文学习受益。

### 8. 不足与局限

- **模型规模局限**：主要实验聚焦32B规模，仅少量扩展到其他规模（见Figure 4 right），缺乏对不同规模（如7B、70B）的全面比较，泛化性存疑。
- **数据集偏向**：知识型数据集部分只包含问答任务，缺乏更多样化的知识型任务（如多选、生成）；推理型数据集仅含MMLU-Pro和SuperGPQA的子集，可能不覆盖所有推理类型。
- **AUROC异常**：在多选格式任务中，非推理模型因仅使用两个置信度区间获得ADVANTAGE，导致AUROC偏高（文中已解释但未完全排除混淆）。
- **评估成本**：依赖GPT-4o mini进行答案匹配，存在一定偏差；且手动检查消融样本（各100例）主观性较强。
- **未讨论计算开销**：慢思考显著增加生成token数，但论文未量化推理延迟或成本与校准增益的平衡。
- **安全与公平性**：未探讨模型在不同群体或敏感话题上的校准表现是否一致。

（完）
