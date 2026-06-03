---
title: "MR-Ben: A Meta-Reasoning Benchmark for Evaluating System-2 Thinking in LLMs"
title_zh: MR-Ben：评估LLMs系统2思维的元推理基准
authors: "Zhongshen Zeng, Yinhong Liu, Yingjia Wan, Jingyao Li, Pengguang Chen, Jianbo Dai, Yuxuan Yao, Rongwu Xu, Zehan Qi, Wanru Zhao, Linling Shen, Jianqiao Lu, Haochen Tan, Yukang Chen, Hao Zhang, Zhan Shi, Bailin Wang, Zhijiang Guo, Jiaya Jia"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=GN2qbxZlni"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 基于过程的基准MR-Ben，用于定位COT推理步骤中的错误
tldr: 提出MR-Ben，一个基于过程的元推理基准，用于评估COT推理的忠实性。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-004.webp\", \"caption\": \"\", \"page\": 3, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-005.webp\", \"caption\": \"\", \"page\": 3, \"index\": 5, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-006.webp\", \"caption\": \"\", \"page\": 22, \"index\": 6, \"width\": 2491, \"height\": 900}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-gn2qbxzlni/fig-007.webp\", \"caption\": \"\", \"page\": 24, \"index\": 7, \"width\": 568, \"height\": 384}]"
motivation: 现有结果导向基准饱和，无法有效跟踪推理能力进步。
method: 构建过程基准，要求模型定位和分析自动生成推理步骤中的潜在错误。
result: 该基准特别适合评估系统2缓慢思维，模拟人类检查假设、条件等认知过程。
conclusion: MR-Ben提供了更精细的推理评估方式，有助于追踪LLM推理能力的进展。
---

## Abstract
Large language models (LLMs) have shown increasing capability in problem-solving and decision-making, largely based on the step-by-step chain-of-thought reasoning processes. However, evaluating these reasoning abilities has become increasingly challenging. Existing outcome-based benchmarks are beginning to saturate, becoming less effective in tracking meaningful progress. To address this, we present a process-based benchmark MR-Ben that demands a meta-reasoning skill, where LMs are asked to locate and analyse potential errors in automatically generated reasoning steps. Our meta-reasoning paradigm is especially suited for system-2 slow thinking, mirroring the human cognitive process of carefully examining assumptions, conditions, calculations, and logic to identify mistakes. MR-Ben comprises 5,975 questions curated by human experts across a wide range of subjects, including physics, chemistry, logic, coding, and more. Through our designed metrics for assessing meta-reasoning on this benchmark, we identify interesting limitations and weaknesses of current LLMs (open-source and closed-source models). For example, with models like the o1 series from OpenAI demonstrating strong performance by effectively scrutinizing the solution space, many other state-of-the-art models fall significantly behind on MR-Ben, exposing potential shortcomings in their training strategies and inference methodologies.

---

## 论文详细总结（自动生成）

# MR-Ben: 论文详细中文总结

## 1. 核心问题与整体含义

**研究动机：** 现有的LLM推理能力评估主要基于最终结果（outcome-based），但此类基准（如GSM8K、MATH）正面临饱和，难以有效跟踪模型的真实进步。同时，模型可能通过错误推理获得正确答案，因此需要更细粒度的过程导向评估。

**整体含义：** 论文提出**MR-Ben**（Meta-Reasoning Benchmark），要求模型定位并分析自动生成的CoT推理步骤中的潜在错误，从而评估其**系统2思维**（慢思考、审慎分析）能力。这更接近人类检查假设、条件、计算和逻辑的认知过程。

## 2. 方法论

### 核心思想：元推理范式
- LLM扮演“教师”角色，对给定的问题-解答对进行三步评估：
  1. **判断解答整体正确性**（是否所有步骤合理且最终答案正确）。
  2. **定位第一个错误步骤**（若解答有误）。
  3. **分析错误原因并给出修正**。

### 关键技术细节
- **数据来源**：从MMLU（数学、物理、生物、化学、医学）、LogiQA（逻辑）和MHPP（编码）中采样问题，覆盖高中至专业水平。
- **CoT解答生成**：使用GPT-3.5-Turbo、Claude2、Mistral-Medium以温度1采样，鼓励多样化和错误。
- **人类标注流程**：三轮质量控制（两两标注→仲裁→10%抽查），标注内容包括解答正确性、第一个错误步骤编号、错误原因文本、修正步骤。
- **评估指标 MR-Score**：由三个子指标加权组成：
  - **MCC**：解答正确性二分类的Matthews相关系数（归一化至[0,1]）。
  - **ACC_step**：错误解答中正确识别第一个错误步骤的比例。
  - **ACC_reason**：错误解答中同时正确识别第一个错误步骤和错误原因的比例。
  - **权重设定**：通过网格搜索选择最大化模型区分度的权重组合（文中未明确具体值，但强调平衡可解释性与区分度）。

## 3. 实验设计

- **基准数据集**：MR-Ben，包含5,975个问题-解答对，覆盖7个学科（数学、医学、生物、物理、化学、逻辑、编码）。解答正确率约40.3%，平均步骤数9.5。
- **对比方法**：
  - **闭源模型**：Claude3-Haiku, GPT-3.5-Turbo, Doubao-pro-4k, Mistral-Large, Yi-Large, Moonshot-v1-8k, GPT-4o-mini, Zhipu-GLM-4, GPT-4-Turbo, GPT-4o, o1-mini, o1-preview。
  - **开源模型**：小模型（Gemma-2B, Qwen1.5-1.8B, Qwen2-1.5B, Phi3-3.8B）；中等模型（GLM-4-9B, DeepSeek-7B, Deepseek-Coder-33B/7B, LLaMA3-8B, Yi-1.5-9B）；大模型（Qwen1.5-72B, DeepSeek-67B, LLaMA3-70B, DeepSeek-V2-236B, Qwen2-72B）。
- **评估设置**：使用step-wise chain-of-thought提示，分别进行zero-shot和one-shot评估。所有实验使用贪婪解码以保证确定性。

## 4. 资源与算力

- **硬件**：A800 GPU，每台8卡。
- **推理耗时**：对6k基准在70B模型上使用vllm推理约2小时；更小模型更快。
- **未见总训练时长**：论文未披露数据集构建或模型训练的总体算力消耗。

## 5. 实验数量与充分性

- **主要实验**：Table 2报告所有模型在7个学科上的zero-shot和one-shot MR-Score。
- **额外实验**：
  - **Few-shot分析**（Table 9）：测试1-3张学习对性能的影响。
  - **Self-refine实验**（Table 4）：三轮自检与修正。
  - **解答正确性先验实验**（Table 3）：给予解答错误提示后性能变化。
  - **长解答影响分析**（Figure 5）：解答步骤数与MR-Score的相关性。
  - **不同推理范式分析**（Figure 3）：知识、算术、算法、逻辑四类。
  - **错误案例分析**（Appendix F）：对7个学科各2个案例的详细定性分析。
- **充分性评价**：实验覆盖模型规模、来源、学科、提示策略等多个维度，分析全面；对比公平（相同提示格式、相同解码策略），但未提供误差条或统计显著性检验（由于贪婪解码，结果确定）。

## 6. 主要结论与发现

1. **o1-preview表现最优**：在所有学科上显著领先，尤其适合系统2思维（探索、反思、搜索解空间）。
2. **多数模型元推理能力弱**：许多模型能生成正确答案，但难以定位推理过程中的错误并解释原因。
3. **数据合成效果显著**：Phi-3（3.8B）超过许多十倍大小模型，说明高质量合成数据比单纯增大规模更有效。
4. **专门预训练不一定提升算法推理**：Deepseek-Coder未比通用模型在编码推理上表现更好。
5. **Self-refine效果不稳定**：小模型受损，大模型（如GPT-4）边际获益，仅Llama3-70B显著提升（但伴随不一致性增加）。
6. **解答正确性先验对中等模型帮助大，对强模型几乎无益**：说明强模型自身已具备较好的诊断能力。
7. **逻辑推理最难**，而o1-preview和GPT-4在算法推理（编码）上表现突出。

## 7. 优点

1. **过程导向评估**：突破传统结果导向的局限，能揭露隐藏的推理缺陷（如答案正确但过程错误）。
2. **多学科覆盖**：涵盖自然科学、编码、逻辑，共7个领域，难度跨越高中至专业水平。
3. **元推理范式新颖**：要求模型进行“关于推理的推理”，更适合评估系统2思维。
4. **严格质量控制**：三轮人工标注+一致性审查，保证高质量。
5. **指标设计合理**：MR-Score综合分类、定位、解释三方面，且通过权重平衡可解释性与区分度。
6. **详细错误分析**：附录提供大量案例，揭示模型具体失败模式（如“假阳性偏见”）。

## 8. 不足与局限

1. **领域覆盖有限**：局限于自然科学、编码和逻辑，缺乏人文、社会科学等需要整体性或创造性推理的领域。
2. **语言单一**：仅包含英文问题，未涉及多语言推理挑战。
3. **CoT解答来源有限**：仅来自三种LLM（GPT-3.5、Claude2、Mistral-Medium），覆盖的推理模式可能不够全面，人类解答的缺失可能引入偏差。
4. **依赖GPT-4作为评分代理**：虽然与人工有92%一致性，但仍存在模型偏好和自我偏好风险（GPT-4可能更认可自己的推理风格）。
5. **Few-shot效果不稳定**：长样例可能分散注意力，最佳示例数难以确定。
6. **未开源权重和具体权重参数**：MR-Score的权重选择过程虽有网格搜索，但未给出最终权重值，影响可复现性。
7. **实验缺乏统计显著性测试**：由于贪婪解码，结果确定，但若考虑多次运行（不同种子），应报告均值方差。

（完）
