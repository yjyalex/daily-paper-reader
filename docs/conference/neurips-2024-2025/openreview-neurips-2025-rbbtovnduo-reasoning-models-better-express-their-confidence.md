---
title: Reasoning Models Better Express Their Confidence
title_zh: 推理模型能更好地表达其置信度
authors: "Dongkeun Yoon, Seungone Kim, Sohee Yang, Sunkyoung Kim, Soyeon Kim, Yongil Kim, Eunbi Choi, Yireun Kim, Minjoon Seo"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rbBtoVnduo"
tags: ["query:cot-unfaith"]
score: 4.0
evidence: 思维链推理模型展现更好的置信度校准
tldr: 表明使用扩展CoT的推理模型比非推理模型有更好的置信度校准。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM常常不能准确表达置信度，限制了可靠性，需要改进。
method: 在六个数据集上对六个推理模型进行了置信度校准基准测试。
result: 推理模型在33/36个设定中置信度校准更优。
conclusion: CoT推理的慢思考行为提升了置信度表达。
---

## Abstract
Despite their strengths, large language models (LLMs) often fail to communicate their confidence accurately, making it difficult to assess when they might be wrong and limiting their reliability. In this work, we demonstrate that reasoning models that engage in extended chain-of-thought (CoT) reasoning exhibit superior performance not only in problem-solving but also in accurately expressing their confidence.
Specifically, we benchmark six reasoning models across six datasets and find that they achieve strictly better confidence calibration than their non-reasoning counterparts in 33 out of the 36 settings. Our detailed analysis reveals that these gains in calibration stem from the slow thinking behaviors of reasoning models (e.g., exploring alternative approaches and backtracking) which enable them to adjust their confidence dynamically throughout their CoT, making it progressively more accurate. In particular, we find that reasoning models become increasingly better calibrated as their CoT unfolds, a trend not observed in non-reasoning models. Moreover, removing slow thinking behaviors from the CoT leads to a significant drop in calibration. Lastly, we show that non-reasoning models also demonstrate enhanced calibration when simply guided to slow think via in-context learning, fully isolating slow thinking as the source of the calibration gains.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：大型语言模型（LLM）在输出时常常表现出过度自信（即使错误时也显得很确定），这严重限制了它们在风险敏感场景中的可靠性。用户难以判断模型何时可能出错。
- **整体含义**：本文发现，采用扩展思维链（CoT）的推理模型（如OpenAI o1、DeepSeek-R1）不仅问题求解能力更优，还能更准确地表达其置信度（即置信度校准更好）。这种能力源于“慢思考”行为（如探索替代方案、回溯验证），使模型能在推理过程中动态调整置信度，从而“知道自己知道什么”，提升信任度与可靠性。

## 2. 方法论

- **核心思想**：通过让模型在CoT中主动进行置信度推理并输出数值化/语言化的置信度，利用慢思考过程中的自我校验实现更精确的校准。
- **关键技术细节**：
  - 采用**言语化置信度估计**（verbalized confidence estimation），直接要求模型在回答中同时输出预测答案和置信度（分10个区间，如“Almost certain (0.9–1.0)”）。
  - 推理流程分三步：① 解决方案推理（SOLUTION REASONING）；② 置信度推理（CONFIDENCE REASONING）；③ 置信度言语化（CONFIDENCE VERBALIZATION）。
  - 对于部分推理模型（如R1-Distill）不会自动做置信度推理，采用强制方式：在`</think>`前插入提示要求模型评估自己的推理过程。
  - 最终格式化为 `Answer: $ANSWER Confidence: $CLASS`，通过正则提取，并用GPT-4o mini进行答案匹配。
  - 所有模型（推理和非推理）使用相同的提示模板，以公平比较。非推理模型也进行CoT，但不使用慢思考结构。

## 3. 实验设计

- **数据集**（共6个/子集）：
  - 知识密集型：TriviaQA（事实问答）、NonambigQA（歧义性问答）。
  - 推理密集型：SuperGPQA（数学与非数学子集）、MMLU-Pro（数学与非数学子集）。
  - 每个数据集/子集均匀采样1000个样本以控制计算开销。
- **基准对比方法**：
  - 6组推理模型 vs 非推理模型（32B规模）：
    - R1-Distill-Qwen、QwQ、OR1-Preview vs Qwen2.5-Instruct
    - GLM-Z1-0414 vs GLM-4-0414
    - EXAONE-Deep vs EXAONE-3.5-Instruct
    - Qwen3 Thinking vs Qwen3 Non-thinking（同一模型不同模式）
  - 评估指标：Expected Calibration Error (ECE)、Brier Score、AUROC（分别衡量绝对校准、个体级校准与判别能力）。
- **鲁棒性检验**：额外测试了多种置信表达风格（纯语言描述、直接数值输出）、不同解码策略（sampling vs 贪婪）、替代提示策略（Two-turn、Top-K、Multi-step）、以及通过bootstrap评估结果稳定性。

## 4. 资源与算力

- 实验使用Nvidia A6000 48GB或A100 80GB GPU，评估32B模型需2块GPU。
- 采用vLLM进行高效推理，单个数据集评估约需1小时（双A6000）。
- 论文未给出总计算量估计，但考虑到涉及多模型、多数据集、多消融实验，总计算资源消耗较大。

## 5. 实验数量与充分性

- **主实验**：6个数据集 × 6组模型对（推理vs非推理）共36个设置，所有指标均报告，33/36设置中推理模型全面优于非推理。
- **消融实验**（Section 4.2）：在R1-Distill上移除置信推理、认知标记、非线性推理三类组件，以及No CoT基线，各在2个知识数据集上评测。
- **渐进校准分析**（Section 4.1）：在TriviaQA和NonambigQA上，对每个CoT的11个累积片段测量校准指标变化，并做线性趋势显著性检验。
- **额外实验**：预算强制（延长CoT）、模型规模扩展（部分小规模）、非推理模型通过ICL进行慢思考的验证。
- **充分性评价**：实验设计全面，对比公平（同一提示模板、多种替代策略、统计显著性检验）。消融实验明确分量贡献，鲁棒性检验覆盖多种设置。唯一不足是模型规模主要集中在32B，对其他规模验证较少（仅有小规模附件实验）。

## 6. 主要结论与发现

- **核心结论**：推理模型在33/36设置中严格优于非推理模型，更好的置信度校准不仅体现在推理密集型任务上，也体现在知识密集型任务上（即使任务准确率相近或更低）。
- **校准改进源于慢思考**：
  - 随CoT展开，推理模型的校准持续改善（统计显著），而非推理模型无此趋势甚至恶化。
  - 消融实验表明，非线性推理（探索替代、回溯）影响最大，其次是认知标记；而明确的置信推理作用有限。
  - 非推理模型通过ICL模仿慢思考也能提升校准，进一步证明慢思考本身是校准提升的根源。
- **局限性**：推理模型仍倾向于高置信度（很少使用55%以下区间）；在NonambigQA上校准较差（因任务难度高）；预算强制（强制延长CoT）并不能进一步提升校准，暗示校准改善来自思考质量而非长度。

## 7. 优点

- **方法简洁实用**：采用言语化置信度估计，无需访问模型内部状态或训练外部探针，兼容闭源模型和纯黑盒场景。
- **分析深入透彻**：通过渐进校准分析、组件消融、ICL迁移等多种手段，因果证据充分，将校准提升归因于慢思考而非模型其他差异。
- **实验覆盖面广**：涵盖多种模型家族（Qwen、GLM、EXAONE、Qwen3）和任务类型，且进行多维度鲁棒性检验，结果可靠。
- **实践价值高**：指出即使推理模型在简单任务上准确率不提升，也能通过更可靠的置信度表达提高可用性，为部署提供了新视角。

## 8. 不足与局限

- **计算成本**：推理模型生成的长CoT导致推理代价显著高于非推理模型，文中未讨论成本与收益的权衡。
- **校准仍有提升空间**：推理模型仍较少输出低置信度（<55%），在较难数据集（NonambigQA）上校准仍不理想。
- **模型规模局限**：主要实验限于32B规模，虽有小规模附件实验，但对更大或更小模型的泛化性验证不足。
- **依赖外部模型**：答案匹配依赖GPT-4o mini，消融数据依赖GPT-4.1，可能引入噪声或偏差。
- **任务类型局限**：知识密集型数据集（TriviaQA）容易模型记忆，可能无法完全反映置信度真实分布；推理密集型数据集为多选题，可能提供选项线索影响校准测量。
- **静态评估**：所有实验采用单轮问答，未考虑多轮对话或交互场景中的置信度演化。

（完）
