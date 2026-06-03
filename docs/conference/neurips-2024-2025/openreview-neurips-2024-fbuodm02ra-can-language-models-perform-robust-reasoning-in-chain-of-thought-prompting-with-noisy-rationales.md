---
title: Can Language Models Perform Robust Reasoning in Chain-of-thought Prompting with Noisy Rationales?
title_zh: 语言模型能否在包含噪声理性的思维链提示中进行稳健推理？
authors: "Zhanke Zhou, Rong Tao, Jianing Zhu, Yiwen Luo, Zengmao Wang, Bo Han"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=FbuODM02ra"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 评估思维链对噪声理性的鲁棒性
tldr: 研究并基准测试大语言模型在思维链中对无关/不准确理性的鲁棒性。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 现有评估忽略提示中理性步骤可能包含噪声，影响推理可靠性。
method: 构建NoRa数据集，评估模型在含噪声理性示例下的推理准确性。
result: 目前大模型对噪声理性普遍脆弱，自校正等方法效果有限。
conclusion: 需要更鲁棒的思维链推理方法来应对噪声理性。
---

## Abstract
This paper investigates an under-explored challenge in large language models (LLMs): chain-of-thought prompting with noisy rationales, which include irrelevant or inaccurate reasoning thoughts within examples used for in-context learning. We construct NoRa dataset that is tailored to evaluate the robustness of reasoning in the presence of noisy rationales. Our findings on NoRa dataset reveal a prevalent vulnerability to such noise among current LLMs, with existing robust methods like self-correction and self-consistency showing limited efficacy. Notably, compared to prompting with clean rationales, base LLM drops by 1.4%-19.8% in accuracy with irrelevant thoughts and more drastically by 2.2%-40.4% with inaccurate thoughts.

Addressing this challenge necessitates external supervision that should be accessible in practice. Here, we propose the method of contrastive denoising with noisy chain-of-thought (CD-CoT). It enhances LLMs' denoising-reasoning capabilities by contrasting noisy rationales with only one clean rationale, which can be the minimal requirement for denoising-purpose prompting. This method follows a principle of exploration and exploitation: (1) rephrasing and selecting rationales in the input space to achieve explicit denoising and (2) exploring diverse reasoning paths and voting on answers in the output space. Empirically, CD-CoT demonstrates an average improvement of 17.8% in accuracy over the base model and shows significantly stronger denoising capabilities than baseline methods. The source code is publicly available at: https://github.com/tmlr-group/NoisyRationales.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有大语言模型（LLM）在思维链（Chain-of-Thought, CoT）提示中，常使用包含推理步骤的示例（rationales）。然而，实际应用中这些示例可能包含**噪声理性（noisy rationales）**——即与问题无关（irrelevant）或事实不准确（inaccurate）的推理步骤。此前研究多关注噪声问题（noisy questions），但忽略了噪声理性这一核心挑战。
- **整体含义**：论文旨在系统评估LLM在面对噪声理性时的推理鲁棒性，并设计有效的去噪方法。作者认为，现有LLM对此问题极为脆弱，而现有自校正、自一致性等方法作用有限，需要引入外部监督（一个干净理性示例）来引导模型进行对比去噪。

## 2. 论文提出的方法论

- **核心思想**：通过**对比去噪**（contrastive denoising）让模型识别并修复噪声理性。仅需一个干净理性示例作为“锚点”，与噪声示例进行对比，使模型学会分辨并移除噪声。
- **关键技术细节**：提出**CD-CoT**（Contrastive Denoising with noisy Chain-of-Thought），包含四个步骤：
  1. **理性重述（Rephrasing, 1→N）**：对每个噪声示例，使用对比提示（包含干净示例和噪声示例）让LLM独立生成N个重述后的理性（尝试去除噪声）。
  2. **理性选择（Selection, N→M）**：通过答案匹配（检查重述后的答案是否与原始正确答案一致）筛选出M个候选重述（只保留答案正确的重述）。
  3. **理性探索（Exploration, M→D）**：构建M个不同的上下文（每个上下文包含K个重述后的示例 + 干净示例 + 测试问题），对每个上下文进行多次推理（共D次），生成多样化推理路径。
  4. **答案投票（Voting, D→1）**：对所有D个答案进行等权多数投票，得到最终答案。
- **算法流程说明**：如Algorithm 1所示，先对K个噪声示例各N次重述，筛选后随机选M个，组成M个K-shot上下文，分别推理Bj次（总和=D），最后投票。

## 3. 实验设计

- **数据集/场景**：
  - 构建了**NoRa**（Noisy Rationales）数据集，包含26,391个问题，覆盖三种推理任务：
    - **数学推理**：Base-9和Base-11加法（NoRa-Math）
    - **符号推理**：SCAN数据集的等长和更长子任务（NoRa-Symbolic Equal/Longer）
    - **常识推理**：CLUTRR家族关系路径推理（NoRa-Commonsense）
  - 噪声类型：**无关理性**（irrelevant）和**不准确理性**（inaccurate），每种设置三种难度（噪声比0.3/0.5/0.8）。
- **基准（Benchmark）**：以3-shot CoT作为基本设置，报告准确率（%）。
- **对比方法**：
  - **自校正**：ISC（内在自校正）、SP（自抛光）
  - **自一致性**：SM（SmoothLLM）、SD（自去噪）、SC（自一致性）
  - **需要外部监督**：SCO（带Oracle反馈的自校正）、BT（回溯，需噪声位置）、CC（对比思维链，需干净示例）
  - **Ours**：CD-CoT
- **LLM主体**：主要使用GPT-3.5-turbo-0613，另外使用Gemini-Pro、Llama2-70B、Mixtral-8x7B验证泛化性。

## 4. 资源与算力

- 论文未明确说明具体的GPU型号、数量或训练时长，因为实验主要涉及LLM推理调用API。
- 论文附录提到：在GPT-3.5-turbo-0613上共消耗约**2.03B tokens**（输入1.21B，输出0.82B），用于完成全部实验。

## 5. 实验数量与充分性

- **实验数量**：非常充分。包括：
  - 主结果表3（5种基线方法在5个子集上，各3种难度、2种噪声类型，共大量实验）
  - 表4-7（温度、示例数量、不同LLM、shuffle等消融）
  - 表8（外部监督方法对比）
  - 表9-11（CD-CoT的超参数、token消耗、不同LLM）
  - 附录中额外实验：不同噪声语义难度、噪声数量、自监督变体、新数据集（GSM-8K, Blocksworld, Dyck Languages）等。
- **充分性**：实验设计系统，覆盖了多种任务、噪声类型、难度、LLM，并进行了全面的消融和对比，结果客观、公平（使用相同温度、重复5次取准确率）。

## 6. 论文的主要结论与发现

- **主要结论**：
  1. 当前LLM对噪声理性**普遍脆弱**。相比干净理性，GPT-3.5在无关理性下准确率下降1.4%-19.8%，在不准确理性下下降2.2%-40.4%。
  2. **自校正方法（ISC, SP）几乎无效**，甚至会降低性能。
  3. **自一致性方法（SM, SD, SC）仅部分改善，但未实现真正去噪**。SC投票可提高准确率但未去除噪声。
  4. 需要**外部监督**才能有效去噪。CD-CoT仅需一个干净理性示例，便能在多数设置下显著优于所有基线，平均提高17.8%的准确率。
  5. CD-CoT对不准确噪声的鲁棒性更强（下降幅度小于其他方法）。

## 7. 优点

- **问题新颖**：首次系统研究CoT中的噪声理性问题，构建了专用基准NoRa。
- **方法简洁有效**：CD-CoT仅需一个干净示例，利用对比学习思想，四步流程清晰，易于实现。
- **实验全面**：覆盖多种推理任务、噪声类型、难度，对比多种现有方法，并在多个LLM上验证泛化性。
- **理论支撑**：在附录D中基于distinguishability理论分析了噪声对ICL性能的影响，并解释了CD-CoT的有效性。
- **可复现**：开源代码和数据集。

## 8. 不足与局限

- **依赖外部干净示例**：需要人工标注的干净理性示例，这在实际应用中可能不易获取。自监督变体效果有限。
- **计算成本**：CD-CoT需多次重述和推理，token消耗较高（但仍在可接受范围内）。
- **理论深度有限**：理论分析主要基于已有distinguishability框架，未提出全新定理。
- **覆盖面有限**：仅包含文本推理任务，未涉及多模态或更复杂的现实场景（如对话系统）。噪声类型仅为插入式，未考虑其他噪声形式（如替换、删除）。
- **公平性**：与SCO比较时，SCO需要测试问题真实答案，实用性低，而CD-CoT仅需一个干净示例，更实际。但CD-CoT在某些任务（如Symbolic Longer）上略逊于SCO。

（完）
