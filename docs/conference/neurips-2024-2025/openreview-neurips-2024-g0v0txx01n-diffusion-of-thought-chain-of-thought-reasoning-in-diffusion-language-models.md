---
title: "Diffusion of Thought: Chain-of-Thought Reasoning in Diffusion Language Models"
title_zh: 扩散思维：扩散语言模型中的思维链推理
authors: "Jiacheng Ye, Shansan Gong, Liheng Chen, Lin Zheng, Jiahui Gao, Han Shi, Chuan Wu, Xin Jiang, Zhenguo Li, Wei Bi, Lingpeng Kong"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=G0v0TxX01N"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 提出扩散思维，在扩散语言模型中进行推理
tldr: 扩散思维(DoT)将扩散模型与思维链推理结合，实现灵活的计算-推理权衡。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 扩散模型在文本处理中有潜在优势，但缺乏类似CoT的推理能力。
method: 提出Diffusion-of-Thought (DoT)，让推理步骤随时间扩散，而非从左到右生成。
result: 实验证明DoT在多步推理任务上有效。
conclusion: DoT为扩散模型提供了有效的推理机制。
---

## Abstract
Recently, diffusion models have garnered significant interest in the field of text processing due to their many potential advantages compared to conventional autoregressive models.
In this work, we propose Diffusion-of-Thought (DoT),  a novel approach that integrates diffusion models with Chain-of-Thought, a well-established technique for improving the reasoning ability of autoregressive language models. In contrast to autoregressive language models that make decisions in a left-to-right, token-by-token manner, DoT allows reasoning steps to diffuse over time through a diffusion language model and offers greater flexibility in trading-off computation for reasoning performance. Our experimental results demonstrate the effectiveness of DoT in multi-digit multiplication, boolean logic, and grade school math problems. In addition to that, DoT showcases promising self-correction abilities and benefits from existing reasoning-enhancing techniques like self-consistency decoding. Our findings contribute to the understanding and development of reasoning with diffusion language models.

---

## 论文详细总结（自动生成）

# 论文总结：Diffusion of Thought: Chain-of-Thought Reasoning in Diffusion Language Models

## 1. 核心问题与整体含义

- **研究动机**：大型语言模型（LLMs）通过自回归（AR）方式生成中间推理步骤（Chain-of-Thought, CoT）展现了强大的推理能力，但存在误差累积、难以自纠正、效率低下等问题。扩散模型在文本处理中显示出全局规划、自纠正和效率等优势，但其是否也能像AR模型一样通过CoT技术增强复杂推理能力尚不明确。
- **核心问题**：扩散语言模型能否利用CoT式的推理方法获得增强的复杂推理能力？
- **整体含义**：本文首次将扩散模型与CoT推理结合，提出Diffusion-of-Thought（DoT），探索了一种非自回归的推理范式，展示了扩散模型在推理任务中的潜力。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- DoT让推理步骤在扩散时间步内并行“扩散”，而非像AR模型那样从左到右逐token生成。
- 推理路径在隐藏空间中随扩散时间步逐步去噪，最终同时得到所有中间推理步骤和答案。

### 关键技术细节
- **条件生成**：采用DiffuSeq风格的classifier-free guidance，将问题文本（source）固定为条件，只对推理步骤（rationales）部分加噪，确保精确的token级条件控制。
- **单次DoT（Single-Pass）**：所有推理步骤在单个扩散过程中并行生成。
- **多次DoT（Multi-Pass, DoT MP）**：逐思想生成，每步将已生成的思想加入条件，引入因果偏置，增强后续思想的条件信号。
- **训练时采样策略**：
  - **Scheduled Sampling**：在训练时模拟推理阶段的错误暴露（从模型自身预测的`z0`回退采样`zt`），增强模型从错误中自纠正的能力。
  - **Coupled Sampling**（仅用于DoT MP）：训练时不仅对当前思想加噪，也对之前的思想加噪，使模型对前序错误更鲁棒。
- **推理加速**：将连续扩散模型中的ODE Solver（如DPM-Solver）适配为条件形式，将采样步数从4096降至约64步。
- **自一致性（Self-Consistency）**：通过多次采样生成多样推理路径，对最终答案进行多数投票，提升准确性。

### 公式/算法流程（文字说明）
- 训练目标为最小化变分下界（VLB），包括先验损失、扩散损失和取整损失。
- 推理时从`t=T`到`t=0`逐步去噪，每一步用模型预测`z0`并重新采样`zt-1`。

## 3. 实验设计

### 数据集 / 场景
- **简单推理**：4×4和5×5多位数乘法（来自BIG-bench）、布尔逻辑推理（DyVal构造）。
- **复杂推理**：GSM8K（小学算术应用题），使用增强训练数据（Implicit CoT提供）。

### Benchmark与对比方法
- **基模型**：
  - 自回归基线：GPT-2（small/medium/large）、ChatGPT（few-shot）。
  - 扩散基线：Plaid（1.3B）、SEDD（small 170M / medium 424M），以及从零训练的DoT（12层Transformer）。
- **对比方法**：
  - Answer-only、CoT（自回归）、Implicit CoT（知识蒸馏方法）。
- **评估指标**：准确率（Acc）、吞吐量（it/sec，batch size=1）。

## 4. 资源与算力

- **硬件**：8块NVIDIA V100-32G GPU。
- **训练时长**：
  - DoT从零训练：约60k步，batch size=128。
  - Plaid DoT微调：120k步（单次）或30k步（多次）。
  - SEDD DoT微调：200k步。
  - 具体GPU小时数未明确给出，但文中提到训练DoT和DoT MP分别需要29h和10h（指具体实验？）。
- **精度**：半精度fp16（因V100不支持bf16）。

## 5. 实验数量与充分性

- **实验组数**：
  - 三个简单推理任务（乘法×2 + 布尔逻辑）各在所有方法上测试。
  - GSM8K任务：所有方法对比，含消融实验（Table 2）、自一致性实验（Figure 4b）、推理步数扫描（Figure 4a, 4b）。
  - 消融实验：包含Plaid DoT的几种变体（继续预训练、单次/多次、采样策略等）。
  - 自我纠正案例分析（Table 3）展示扩散过程的逐步改进。
- **充分性评估**：
  - 实验覆盖了简单和复杂任务，对比了多种基线（AR小模型、大模型ChatGPT、Implicit CoT）。
  - 消融实验验证了关键组件（scheduled sampling、coupled sampling、ODE solver、self-consistency）的有效性。
  - 公平性：基线和自研方法的训练/评估条件尽量对齐（使用相同数据集、训练步数等）。但未报告多次重复实验的误差棒，统计显著性仅提及“已通过多次运行验证”。
  - 局限性：当前扩散预训练模型规模远小于现代LLMs（如GPT-4、Llama），因此与这些模型没有直接可比性。

## 6. 主要结论与发现

1. **简单推理任务**：DoT在4×4、5×5乘法和布尔逻辑上达到100%准确率，同时实现最大27×的推理加速（相比GPT-2 CoT）。
2. **复杂推理任务**：在GSM8K上，基于SEDD-medium的DoT MP以424M参数超越355M的GPT-2 CoT约10%准确率。小扩散模型可超越4.6倍大的AR模型。
3. **灵活的效率-准确率权衡**：DoT可以通过调整扩散步数（推理时间）平滑控制推理性能，在简单任务上仅需极少步数，在复杂任务上可通过增加步数持续提升准确率。
4. **自纠正能力**：DoT扩散过程天然支持逐步修正错误，案例表明模型能在后续步骤中修正前序错误，且思维可以不按从左到右顺序出现。
5. **自一致性提升**：自一致性解码对DoT有显著收益（得益于扩散模型的多样生成能力）。
6. **ODE求解器**：将连续扩散的推理步数从4096降至8可保持较好性能，大幅提升吞吐量。

## 7. 优点

- **方法创新**：首次将CoT推理引入扩散模型，提出并行生成推理路径的范式，打破了自回归的线性约束。
- **灵活高效**：在简单任务上极快推理，在复杂任务上可通过增加步数提升性能，提供连续的计算-性能权衡。
- **自纠正机制**：扩散模型固有的多步去噪过程加上训练时的模拟错误暴露，使模型具备比AR模型更强的自我纠错能力。
- **实验设计全面**：对比多种自回归和扩散基线，包含消融、自一致性、推理步数扫描、案例可视化等分析。
- **开源代码**：已公开代码（https://github.com/HKUNLP/diffusion-of-thoughts）。

## 8. 不足与局限

- **模型规模受限**：当前扩散预训练模型远小于主流的自回归LLMs（如GPT-4、Llama），因此DoT无法直接与这些大模型竞争，实验结果仅在小规模对比中有效。
- **需要额外训练**：DoT方法依赖任务特定的微调或从零训练，缺乏零样本或少样本推理能力，而自回归大模型可以通过提示词直接进行CoT推理。
- **GSM8K上性能有限**：即使最佳设置（SEDD-medium + 多步 + 自一致性），准确率仅约60%，与ChatGPT的61.5%接近，但远低于更先进的大模型（如GPT-4）。
- **实验覆盖不足**：
  - 仅评估了数学和逻辑类推理，未涉及更通用的推理任务（如常识推理、问答）。
  - 未在更大规模扩散语言模型（如10B+）上验证。
  - 未报告多次运行的标准差，统计显著性验证不充分。
- **实现复杂性**：训练时需精心设计采样策略（scheduled/coupled sampling），超参数敏感（如`ϵmin`、γ），且需要平衡连续与离散扩散的优化。

（完）
