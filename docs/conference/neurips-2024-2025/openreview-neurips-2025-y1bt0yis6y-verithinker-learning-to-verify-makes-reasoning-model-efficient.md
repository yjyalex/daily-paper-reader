---
title: "VeriThinker: Learning to Verify Makes Reasoning Model Efficient"
title_zh: "VeriThinker:学习验证使推理模型高效"
authors: "Zigeng Chen, Xinyin Ma, Gongfan Fang, Ruonan Yu, Xinchao Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Y1bt0YIS6Y"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 通过验证任务提升推理效率
tldr: VeriThinker通过验证训练压缩思考链，减少过度思考。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 2757, \"height\": 687}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 1243, \"height\": 418}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 1058, \"height\": 614}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-004.webp\", \"caption\": \"\", \"page\": 1, \"index\": 4, \"width\": 1058, \"height\": 614}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-005.webp\", \"caption\": \"\", \"page\": 1, \"index\": 5, \"width\": 2757, \"height\": 859}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-006.webp\", \"caption\": \"\", \"page\": 1, \"index\": 6, \"width\": 1243, \"height\": 417}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-007.webp\", \"caption\": \"\", \"page\": 1, \"index\": 7, \"width\": 1058, \"height\": 614}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-008.webp\", \"caption\": \"\", \"page\": 1, \"index\": 8, \"width\": 2757, \"height\": 513}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-009.webp\", \"caption\": \"\", \"page\": 1, \"index\": 9, \"width\": 1243, \"height\": 418}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-010.webp\", \"caption\": \"\", \"page\": 1, \"index\": 10, \"width\": 1058, \"height\": 614}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-011.webp\", \"caption\": \"\", \"page\": 1, \"index\": 11, \"width\": 1680, \"height\": 438}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-012.webp\", \"caption\": \"\", \"page\": 1, \"index\": 12, \"width\": 2348, \"height\": 687}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-013.webp\", \"caption\": \"\", \"page\": 1, \"index\": 13, \"width\": 2238, \"height\": 513}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-014.webp\", \"caption\": \"\", \"page\": 1, \"index\": 14, \"width\": 2238, \"height\": 513}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-015.webp\", \"caption\": \"\", \"page\": 1, \"index\": 15, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-016.webp\", \"caption\": \"\", \"page\": 1, \"index\": 16, \"width\": 1280, \"height\": 1280}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-017.webp\", \"caption\": \"\", \"page\": 1, \"index\": 17, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-018.webp\", \"caption\": \"\", \"page\": 1, \"index\": 18, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-019.webp\", \"caption\": \"\", \"page\": 1, \"index\": 19, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-020.webp\", \"caption\": \"\", \"page\": 1, \"index\": 20, \"width\": 1058, \"height\": 614}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-021.webp\", \"caption\": \"\", \"page\": 5, \"index\": 21, \"width\": 3573, \"height\": 2373}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-022.webp\", \"caption\": \"\", \"page\": 5, \"index\": 22, \"width\": 3574, \"height\": 2372}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-023.webp\", \"caption\": \"\", \"page\": 5, \"index\": 23, \"width\": 3574, \"height\": 2372}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-024.webp\", \"caption\": \"\", \"page\": 9, \"index\": 24, \"width\": 2969, \"height\": 1761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-025.webp\", \"caption\": \"\", \"page\": 9, \"index\": 25, \"width\": 2969, \"height\": 1761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-026.webp\", \"caption\": \"\", \"page\": 9, \"index\": 26, \"width\": 2969, \"height\": 1761}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-027.webp\", \"caption\": \"\", \"page\": 23, \"index\": 27, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-028.webp\", \"caption\": \"\", \"page\": 23, \"index\": 28, \"width\": 1280, \"height\": 1280}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-y1bt0yis6y/fig-029.webp\", \"caption\": \"\", \"page\": 23, \"index\": 29, \"width\": 1280, \"height\": 1280}]"
motivation: 大推理模型思考链过长导致高昂推理成本。
method: 通过辅助验证任务微调模型，使其能判断思考步骤必要性。
result: 有效压缩思考链长度，降低推理成本。
conclusion: 验证任务训练可提升模型推理效率。
---

## Abstract
Large Reasoning Models (LRMs) have garnered considerable attention for their ability to tackle complex tasks through the Chain-of-Thought (CoT) approach. However, their tendency toward overthinking results in unnecessarily lengthy reasoning chains, dramatically increasing the inference costs. To mitigate this issue, we introduce VeriThinker, a novel approach for CoT compression. Unlike conventional methods that fine-tune LRMs directly on the original reasoning task using synthetic concise CoT data, we innovatively fine-tune the model solely through an auxiliary verification task. By training LRMs to accurately verify the correctness of CoT solutions, the LRMs inherently become more discerning about the necessity of subsequent self-reflection steps, thereby effectively suppressing overthinking. Extensive experiments validate that VeriThinker substantially reduces reasoning chain lengths while maintaining or even slightly improving accuracy. When applied to DeepSeek-R1-Distill-Qwen-7B, our approach reduces reasoning tokens on MATH500 from 3790 to 2125 while improving accuracy by 0.8% (94.0% to 94.8%), and on AIME25, tokens decrease from 14321 to 10287 with a 2.1% accuracy gain (38.7% to 40.8%). Additionally, our experiments demonstrate that VeriThinker can also be zero-shot generalized to speculative reasoning.

---

## 论文详细总结（自动生成）

# VeriThinker: 学习验证使推理模型高效 — 论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大型推理模型（LRM）如DeepSeek-R1、GPT-o1通过思维链（CoT）解决复杂任务，但存在严重的“过度思考”现象——频繁进行自我验证，导致推理链过长、推理成本大幅增加。
- **现有方法局限**：传统压缩方法（SFT、RL）依赖生成高质量的合成简短CoT数据作为目标，但该数据生成本身计算昂贵、耗时，且难以同时兼顾简洁性与关键自省步骤的保留，压缩后精度往往下降。
- **本文贡献**：提出**VeriThinker**，通过**监督验证微调（SVFT）**，在不依赖任何合成目标链的情况下，直接训练LRM对CoT解决方案的正确性进行二分类验证，从而天然地抑制过度思考，实现高效CoT压缩。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：模型在推理中决定是否自我反思本质上是一个二分类问题（判断先前步骤是否正确）。若能提升模型对步骤正确性的判别能力，则能减少不必要的反思。VeriThinker通过训练模型进行**CoT解决方案的正确性验证**来达到这一目标。
- **关键技术细节**：
    1. **构建验证数据集**：收集问题（来自PRM12K、GSM8K、LIMO、Numina-Math），使用小型非推理模型（Qwen-2.5系列、Qwen-Math系列）生成不含自省步骤的短CoT解决方案，并基于最终答案标注正确/错误标签。最终约340k实例（160k正确+190k错误）。
    2. **监督验证微调（SVFT）**：对于每个数据实例，输入格式为“问题+解决方案+验证指令”，输出为固定模板：正确时“Yes, I’m sure that every step is absolutely correct.”，错误时“No, I think there might be some mistakes in the proposed solution.”。训练时**仅对验证响应部分计算交叉熵损失**：
        \[
        \mathcal{L}_{\text{SVFT}} = -\sum_{t \in y} \log p_\theta(y_t \mid x, y_{<t})
        \]
    3. **机制分析**：
        - SVFT本质类似**对比学习**，教会模型区分正确与错误方案，而非记忆具体语义（实验显示将响应替换为“North”“South”仍有效）。
        - 通过分析“Wait”触发词的概率，发现SVFT模型在正确步骤后反思概率显著降低，错误步骤后反思概率略有增加，而SFT模型则全面降低反思概率，导致精度损失。
- **扩展：解决方案级推测推理（SSR）**：利用SVFT获得的验证能力，让短CoT模型快速生成草案，SVFT模型验证其正确性；若正确则直接输出，否则触发长CoT推理，从而大幅提升吞吐量。

## 3. 实验设计
- **数据集**：
    - 训练集：自构建验证数据集（约340k问题-方案对）。
    - 评估基准：**MATH500**、**AIME2024**、**AIME2025**、**GSM8K**、**GPQA-Diamond**（跨域泛化）。
- **评估指标**：推理精度（Accuracy）、平均推理token数（Tokens）、吞吐量（Throughput）、长CoT激活率（AcR）。
- **对比方法**：
    - 原始模型（Original）
    - 直接截断（Truncation）
    - 快速提示压缩（Fast-Prompting）
    - CoT-Valve（可控长度）
    - 基于合成目标链的SFT
- **模型**：
    - 三个长CoT模型：DeepSeek-R1-Distill-Qwen-7B/14B、DeepSeek-R1-Distill-Llama-8B
    - 三个短CoT模型：Qwen-2.5-Math-7B/1.5B-Instruct、Qwen-2.5-7B-Instruct

## 4. 资源与算力
- **训练**：使用**LoRA**高效微调（QKVO模块），配置因模型而异（rank 128/256，alpha 128/512）。**4块RTX 6000 Ada GPU**，DeepSpeed ZeRO-2优化，每模型训练2 epoch。
- **数据生成**：使用4块NVIDIA A6000 GPU，约3-4小时生成300k问题的短CoT方案。
- **推理**：vLLM框架，同样使用RTX 6000 Ada GPU。
- **未明确说明总训练时长**，但给出了具体硬件配置。

## 5. 实验数量与充分性
- **主要压缩实验**（表1）：3个模型 × 3个基准 × 6种方法（含原始、4个基线、VeriThinker），共约54组结果。
- **验证能力实验**（表2）：3个VeriThinker模型在2个基准上评估验证精度（准确率、精确率、召回率、F1）。
- **推测推理实验**（表3 & 图3）：3个LRM × 2个草案模型 × 2个基准，报告吞吐量、AcR等。
- **短CoT LLM实验**（表4）：3个短CoT模型在2个基准上。
- **消融与机制分析**（图2、图4）：不同响应格式的影响、反思概率变化、错误步数的token减少对比。
- **跨域泛化**（附录C）：在GPQA-Diamond、仅用PRM12K+GSM8K训练后在AIME测试等。
- **充分性评价**：实验覆盖多模型、多难度、多基准，对比多种主流方法，验证了机制假设，还包含跨域测试，设计较为充分、客观。SFT与VeriThinker使用相同LoRA配置、相同数据基础，保证了公平性。

## 6. 主要结论与发现
- VeriThinker在**显著减少推理token**（平均约30%）的同时，**保持或略微提升推理精度**（最多+3.1%），尤其在极难数据集（AIME）上表现突出。
- 核心原因：SVFT使模型更精准地判断步骤正确性——正确步骤后反思概率大幅下降，错误步骤后反思概率略增，从而避免过度思考但保留必要纠错。
- VeriThinker可零样本推广到**推测推理**，大幅提升吞吐量（最高7.1×），激活率低（5%-25%）。
- 验证能力本身达到了较高精度（>88%），且比大型闭源模型（GPT-4o、GPT-4.1）更优（90.4% vs 86.4%）。
- 对短CoT LLM同样有正向效果（准确率提升）。

## 7. 优点
- **不依赖合成目标链**：避免了昂贵的短链数据生成过程，仅需简单标注最终答案是否正确。
- **方法简洁高效**：仅使用LoRA进行二分类微调，计算成本低；机制清晰（对比学习本质）。
- **压缩与精度兼顾**：在多个模型和基准上一致性优于传统SFT和RL方法，尤其在高难度任务上。
- **可拓展性强**：自然支持推测推理，验证能力可直接用于协同推理管线。
- **跨域泛化能力**：即使在非数学领域（GPQA）也有效，验证了方法的鲁棒性。

## 8. 不足与局限
- **小模型效果有限**：在1.5B参数级别微调后容易遗忘原任务，无法有效压缩（论文第5节专门指出）。
- **架构敏感性**：在LLaMA-8B模型上压缩比率较小（约24%），相比Qwen-7B（约30%）偏低，可能对特定架构依赖。
- **验证数据集构造偏差**：依赖多个小短CoT模型生成方案，这些模型的错误模式可能影响训练；仅由最终答案判断正确性，未考虑推理过程部分正确的情况。
- **任务覆盖局限**：主要集中于数学推理，虽在GPQA上测试，但多领域（如科学、常识）验证尚不充分。
- **推测推理场景限制**：问题难度越大，长CoT激活率越高，吞吐量提升可能减弱。
- **未提供详细训练时间、超参数搜索细节**，部分消融仅在7B模型上进行，泛化性待进一步验证。

（完）
