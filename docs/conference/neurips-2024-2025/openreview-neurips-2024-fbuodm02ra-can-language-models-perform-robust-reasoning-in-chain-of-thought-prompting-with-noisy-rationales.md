---
title: Can Language Models Perform Robust Reasoning in Chain-of-thought Prompting with Noisy Rationales?
title_zh: 语言模型能在带噪声理由的思维链提示中进行鲁棒推理吗?
authors: "Zhanke Zhou, Rong Tao, Jianing Zhu, Yiwen Luo, Zengmao Wang, Bo Han"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=FbuODM02ra"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 直接研究带噪声理由的不忠实思维链推理
tldr: 噪声理由严重损害思维链推理的忠实性。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 思维链提示中的噪声理由会导致不忠实推理，需评估鲁棒性。
method: 构建NoRa数据集，测试模型在含不相关或错误理由时的表现。
result: 所有模型对噪声理由脆弱，准确率大幅下降。
conclusion: 现有鲁棒方法对噪声理由效果有限，需新策略。
---

## Abstract
This paper investigates an under-explored challenge in large language models (LLMs): chain-of-thought prompting with noisy rationales, which include irrelevant or inaccurate reasoning thoughts within examples used for in-context learning. We construct NoRa dataset that is tailored to evaluate the robustness of reasoning in the presence of noisy rationales. Our findings on NoRa dataset reveal a prevalent vulnerability to such noise among current LLMs, with existing robust methods like self-correction and self-consistency showing limited efficacy. Notably, compared to prompting with clean rationales, base LLM drops by 1.4%-19.8% in accuracy with irrelevant thoughts and more drastically by 2.2%-40.4% with inaccurate thoughts.

Addressing this challenge necessitates external supervision that should be accessible in practice. Here, we propose the method of contrastive denoising with noisy chain-of-thought (CD-CoT). It enhances LLMs' denoising-reasoning capabilities by contrasting noisy rationales with only one clean rationale, which can be the minimal requirement for denoising-purpose prompting. This method follows a principle of exploration and exploitation: (1) rephrasing and selecting rationales in the input space to achieve explicit denoising and (2) exploring diverse reasoning paths and voting on answers in the output space. Empirically, CD-CoT demonstrates an average improvement of 17.8% in accuracy over the base model and shows significantly stronger denoising capabilities than baseline methods. The source code is publicly available at: https://github.com/tmlr-group/NoisyRationales.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：大型语言模型（LLMs）在使用思维链（Chain-of-Thought, CoT）提示时，通常依赖高质量的示例（demonstrations）进行上下文学习。然而，在实际应用中（如众包平台、对话系统或机器生成数据），CoT示例中的推理步骤（rationales）可能包含噪声——即**不相关（irrelevant）** 或**不准确（inaccurate）** 的思考步骤。这被称为**噪声理性（Noisy Rationales, Noisy-R）** 问题，与以往研究的噪声问题（如噪声问题Noisy-Q）不同。
- **核心问题**：当前LLMs在面对噪声理性时，其推理鲁棒性究竟如何？现有鲁棒方法（如自我纠正、自我一致性）能否有效应对？
- **整体含义**：该问题对CoT提示的忠实性和可靠性构成严重威胁，亟需系统性的基准评估和有效的解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：提出一种对比去噪方法**CD-CoT (Contrastive Denoising with Noisy Chain-of-Thought)**，通过**仅需一个干净CoT示例**作为外部监督，利用对比机制帮助LLMs识别并去除噪声理性。
- **关键技术细节**：
  - **步骤1：理性重述（Rephrasing via Supervised Contrasting, 1→N）**：对每个带噪声的示例，基于一个干净示例构建对比提示，让LLM生成N个重述后的理性（去除或修正噪声）。
  - **步骤2：理性选择（Rationale Selection, N→M）**：通过答案匹配筛选出重述后答案仍不变的示例，随机选出M个合格的重述理性。
  - **步骤3：理性探索（Rationale Exploration, M→D）**：将M个重述理性分配给M个不同的上下文，每个上下文包含重述的示例、干净示例和测试问题。对每个上下文进行多次（共D次）推理，探索多样化的推理路径。
  - **步骤4：答案投票（Answer Voting, D→1）**：对D个答案进行等权投票，选出最终答案。
- **算法流程**：算法1（paper中给出）详细描述了上述四步，包括超参数N、M、D的设定。整体遵循“探索-利用”原则：先对输入空间进行显式去噪（重述+选择），再在输出空间进行多样化推理与投票。

## 3. 实验设计
- **数据集**：构建了**NoRa (Noisy Rationales)** 数据集，包含26,391个问题，覆盖三种推理任务：
  - **数学推理（NoRa-Math）**：基于Base Calculation，包括base-9和base-11加法，难度分为Easy、Medium、Hard（噪声比0.3、0.5、0.8）。
  - **符号推理（NoRa-Symbolic）**：基于SCAN，包括等长和超长两个子任务。
  - **常识推理（NoRa-Commonsense）**：基于CLUTRR，关系路径推理。
- **对比方法**：
  - **基于固有能力的去噪方法**：ISC（固有自我纠正）、SP（自我打磨）、SM（平滑LLM）、SD（自我去噪）、SC（自我一致性）。
  - **需要额外信息的方法**：SCO（带oracle反馈的自我纠正）、BT（回溯，需噪声位置）、CC（对比思维链，需一个干净示例）。
- **评估设置**：主实验使用GPT-3.5-turbo-0613，温度τ=1，每个任务采样300个问题，每个问题重复5次。此外还评估了Gemini-Pro、Llama2-70B、Mixtral-8x7B等模型。

## 4. 资源与算力
- 文中未明确说明使用的GPU型号、数量及训练时长。但提到使用GPT-3.5-turbo-0613 API进行实验，总计消耗**2.03B tokens**（输入1.21B，输出0.82B）。对于其他模型（如Llama2-70B、Mixtral-8x7B）的推理计算资源未详细说明。

## 5. 实验数量与充分性
- **实验数量**：论文进行了大量对比实验：
  - 主实验（Tab.3）：在NoRa的5个数据集上，对比了Base模型与5种去噪方法在干净、不相关、不准确理性下的准确率。
  - 额外监督方法实验（Tab.8）：对比了CD-CoT与SCO、BT、CC在三种噪声类型上的表现。
  - 消融实验（Tab.9、Tab.34等）：分析了CD-CoT中重述、投票、是否使用干净示例等组件的影响。
  - 参数分析（温度、示例数量、噪声数量等）：Tab.4、Tab.5、Tab.39等。
  - 不同LLM的泛化实验（Tab.11）。
  - 新数据集实验（GSM-8K、Blocksworld、BIG-Bench Hard Dyck Languages）和MT-Bench多轮对话实验（Tab.46-49）。
  - 多种噪声语义难度实验（Appendix F.4）。
- **充分性与公平性**：
  - 实验设计较为全面，涵盖了多种任务、多种噪声类型和比率、多种LLM。
  - 对比方法均为已发表或基线方法，且设置保持一致（如温度、推理次数等）。
  - 消融实验验证了CD-CoT各组件的有效性。
  - 但未在所有设置下报告标准差（仅部分提供），且未说明是否进行多次独立重复以评估统计显著性（仅在5次重复上报告均值）。

## 6. 论文的主要结论与发现
- 当前LLMs对噪声理性普遍脆弱，不相关噪声导致准确率下降1.4%-19.8%，不准确噪声下降2.2%-40.4%。
- 现有鲁棒方法（如ISC、SP、SM、SD、SC）效果有限，甚至在某些任务上表现更差。
- 自我纠正方法在无外部反馈时效果不佳；自我一致性方法可提升鲁棒性但无显式去噪。
- **CD-CoT**方法借助一个干净示例，通过对比去噪和多样性推理投票，平均提升准确率17.8%，在所有设置下优于大多数基线，且对噪声程度具有较强抵抗力。
- 理论分析（Appendix D）表明噪声理性会降低ICL的可区分性，而CD-CoT通过重述降低误差项，提高可区分性。

## 7. 优点
- **问题新颖性**：首次系统研究噪声理性问题，区别于已有的噪声问题、噪声答案等。
- **方法简洁有效**：CD-CoT仅需一个干净CoT示例，易于获取，且实现简单（无需微调、无需额外工具）。
- **实验充分全面**：覆盖多种推理任务、多种噪声类型与强度、多种LLM，并进行了广泛的消融和参数分析。
- **理论和实证结合**：提供了基于可区分性的理论分析，支撑方法有效性。
- **资源公开**：代码和数据集已开源，可复现。

## 8. 不足与局限
- **依赖外部干净示例**：当前方法仍需一个手动标注的干净CoT示例，实际场景中可能难以保证质量或数量；自监督变体效果有限（Appendix F.7）。
- **未探索其他噪声类型**：仅考虑了不相关和不准确两种噪声，未覆盖混合噪声、对抗性噪声等更复杂情况。
- **计算成本较高**：CD-CoT需要多次重述和推理（N=5, M=2, D=5），token消耗较大（Tab.10），实际部署可能有延迟和成本限制。
- **实验中的潜在偏差**：主要在GPT-3.5上进行主要实验，其他模型仅部分测试；NoRa数据集为人工构造噪声，可能与真实噪声分布有差距。
- **理论分析较浅**：理论部分基于现有可区分性框架，未深入分析CD-CoT的泛化界或鲁棒性保证。
- **应用限制**：方法侧重于输入去噪，未考虑模型内部噪声或输出空间噪声的应对；对超参数N、M、D的选择较为敏感，需调优。

（完）
