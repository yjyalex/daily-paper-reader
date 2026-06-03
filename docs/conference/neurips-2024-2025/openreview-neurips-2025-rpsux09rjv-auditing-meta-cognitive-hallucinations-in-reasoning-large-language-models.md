---
title: Auditing Meta-Cognitive Hallucinations in Reasoning Large Language Models
title_zh: 审计推理大语言模型中的元认知幻觉
authors: "Haolang Lu, Yilian Liu, Jingxin Xu, Guoshun Nan, Yuanlong Yu, Zhican Chen, Kun Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rPsUx09RJV"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 审计思维链轨迹中的幻觉
tldr: 审计思维链轨迹以研究推理大模型中幻觉的产生和演变
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法无法深入揭示幻觉在思维链中的产生和演变机制。
method: 通过审计思维链轨迹并评估模型对错误或偏见声明的认知置信度，分析幻觉因果关系。
result: 发现长思维链设置下模型更容易出现幻觉。
conclusion: 审计方法可有效揭示思维链中幻觉的因果路径，为提升忠实性提供指导。
---

## Abstract
The development of Reasoning Large Language Models (RLLMs) has significantly improved multi-step reasoning capabilities, but it has also made hallucination problems more frequent and harder to eliminate. While existing approaches address hallucination through external knowledge integration, model parameter analysis, or self-verification mechanisms, they fail to provide a comprehensive insight into how hallucinations **emerge** and **evolve** throughout the reasoning chain. In this work, we investigate hallucination causality under constrained knowledge domains by auditing the Chain-of-Thought (CoT) trajectory and assessing the model's cognitive confidence in potentially erroneous or biased claims.
Analysis reveals that in long-CoT settings, RLLMs may iteratively reinforce biases and errors through flawed reflective processes, ultimately inducing hallucinated reasoning paths.
Counterintuitively, even with interventions at hallucination origins, reasoning chains display pronounced ''chain disloyalty'', resisting correction and sustaining flawed trajectories.
We further point out that existing hallucination detection methods are *less reliable and interpretable than previously assumed*, especially in complex multi-step reasoning contexts.
Unlike circuit tracing that requires access to model parameters, our auditing **enables more interpretable long-chain hallucination attribution in black-box settings**, demonstrating stronger generalizability and practical utility.
Our code is available at [this link](https://github.com/Winnie-Lian/AHa_Meta_Cognitive).

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

推理大语言模型（RLLMs）在多步推理能力上取得显著进步，但同时也使得幻觉问题更加频繁且难以消除。现有方法主要依赖外部知识整合、模型参数分析或自我验证机制，它们虽然能缓解幻觉，但无法深入揭示幻觉在思维链（Chain-of-Thought, CoT）中如何产生（emerge）和演变（evolve）。本文旨在对CoT轨迹进行审计，评估模型对潜在错误或偏见声明的认知置信度，从而从根本上理解幻觉的因果路径，为提升推理忠实性提供指导。

## 2. 方法论

- **核心思想**：通过审计CoT轨迹，分析模型在推理过程中对每一步声明的内部置信度（认知置信度），以区分哪些步骤是模型“确信”但实际错误的，哪些是模型“不确定”而导致的漂移。进而通过对幻觉起源点进行干预，观察后续推理路径的变化。
- **关键技术细节**：
  - 在受约束的知识领域（constrained knowledge domains）下进行审计，确保外部知识已知，从而准确定位模型是否偏离事实。
  - 评估模型对不同CoT步骤的认知置信度，重点分析模型对那些可能包含错误或偏见声明的判断强度。
  - 引入“链不忠诚”（chain disloyalty）概念：即使在幻觉起源处进行纠正干预，推理链仍表现出强烈的抗纠正性，持续维持错误轨迹。
- **算法/流程（文字说明）**：首先收集RLLMs在long-CoT设置下的完整推理轨迹；然后对每个推理步骤进行置信度评分；接着定位可能产生幻觉的起始步骤；最后施加外部纠正干预（如替换或删除该步骤），观察后续推理是否沿正确路径恢复。通过对比干预前后的轨迹变化，量化幻觉的因果路径。
- **与现有方法的区别**：不同于电路追踪（circuit tracing）需要访问模型参数，本文方法完全基于黑盒（仅需输入输出及模型置信度输出），因而更具泛化性和实际可用性。

## 3. 实验设计

- **数据集/场景**：文档未明确指定具体数据集名称，仅在摘要中提到“在受约束的知识领域下”进行实验。可能使用已知事实的问答或推理任务（如常识推理、数学推理）进行约束领域测试。
- **Benchmark**：未明确列出标准基准。作为对比的可能是传统幻觉检测方法（如外部知识检索、自我一致性检验、参数内分析等）。
- **对比方法**：摘要指出“现有幻觉检测方法在复杂多步推理中比以前假设的更不可靠和更不可解释”，但未列举具体对比方法名称。推测与常见的基于概率、基于不确定性或基于外部验证的检测方法进行了比较。

## 4. 资源与算力

- 文中未提及使用的GPU型号、数量、训练时长或推理资源。因此无法评估计算开销。

## 5. 实验数量与充分性

- 元数据仅给出“结果：发现长思维链设置下模型更容易出现幻觉”以及“审计方法可有效揭示思维链中幻觉的因果路径”。未报告具体的实验组数（如不同模型规模、不同领域、不同干预策略的消融实验数量）。
- 从摘要可知，至少进行了两类关键实验：
  - 分析long-CoT下幻觉的迭代强化现象。
  - 干预幻觉起源后观察链不忠诚现象。
- 充分性评价：实验设计意图清晰，但缺乏具体数据集名称、模型参数设置、统计显著性等细节，使得复现和客观性评估受限。不过作为概念验证性研究，其核心发现具有启发性。

## 6. 主要结论与发现

- **幻觉的自我强化**：在长思维链设置下，RLLMs可能通过有缺陷的反思过程（flawed reflective processes）迭代地强化自身的偏见和错误，最终诱导出幻觉化的推理路径。
- **链不忠诚现象**：即使在幻觉起源处进行外部干预（纠正错误），推理链表现出明显的“链不忠诚”，抗拒纠正并持续沿着错误的轨迹前进。这表明幻觉一旦形成便具有很强的鲁棒性。
- **现有检测方法不可靠**：现有幻觉检测方法在复杂多步推理上下文中的可靠性和可解释性低于先前假设，本文的审计方法提供了更可解释的长链幻觉归因。
- **黑盒可操作性**：本文方法不需要访问模型参数，仅需黑盒API即可实现有效的幻觉因果归因，因而更具实用价值。

## 7. 优点

- **可解释性强**：审计CoT轨迹与认知置信度，直观地展示了幻觉步骤的因果关系，而非仅给出一个二元判断。
- **黑盒友好**：无需模型内部参数，适用于商业闭源模型，泛化性更好。
- **新颖的发现**：提出“链不忠诚”概念，揭示干预失效的机制，为提升推理忠实性提供了新视角。

## 8. 不足与局限

- **实验细节缺失**：未提供具体使用的数据集、模型名称、实验次数、统计指标等，使得结果难以被独立复现与验证。
- **知识领域约束**：实验在受约束的知识领域下进行，可能无法自然推广到开放域或知识边界模糊的场景。
- **干预手段的可行性**：自动检测幻觉起源并施加干预在实际系统中可能难以实时实现，限制了直接应用。
- **算力消耗未评估**：未说明审计过程本身的计算开销，可能对于长CoT轨迹而言计算成本较高。
- **偏差风险**：仅凭认知置信度判断幻觉起源可能存在偏差，因为模型可能对错误答案同样保持高置信度。

（完）
