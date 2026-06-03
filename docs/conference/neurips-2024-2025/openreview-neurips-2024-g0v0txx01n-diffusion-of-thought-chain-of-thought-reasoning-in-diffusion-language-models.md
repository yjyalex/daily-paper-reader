---
title: "Diffusion of Thought: Chain-of-Thought Reasoning in Diffusion Language Models"
title_zh: 思维扩散：扩散语言模型中的思维链推理
authors: "Jiacheng Ye, Shansan Gong, Liheng Chen, Lin Zheng, Jiahui Gao, Han Shi, Chuan Wu, Xin Jiang, Zhenguo Li, Wei Bi, Lingpeng Kong"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=G0v0TxX01N"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 将思维链与扩散语言模型结合进行推理
tldr: 提出Diffusion-of-Thought，将CoT与扩散模型结合实现灵活推理。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 扩散模型在文本处理中有潜力，但尚未与CoT结合。
method: 在扩散语言模型中加入CoT，允许推理步骤随时间扩散。
result: 在多个任务上展示了有效性和计算-性能权衡。
conclusion: DoT提供了比自回归模型更灵活的推理方式。
---

## Abstract
Recently, diffusion models have garnered significant interest in the field of text processing due to their many potential advantages compared to conventional autoregressive models.
In this work, we propose Diffusion-of-Thought (DoT),  a novel approach that integrates diffusion models with Chain-of-Thought, a well-established technique for improving the reasoning ability of autoregressive language models. In contrast to autoregressive language models that make decisions in a left-to-right, token-by-token manner, DoT allows reasoning steps to diffuse over time through a diffusion language model and offers greater flexibility in trading-off computation for reasoning performance. Our experimental results demonstrate the effectiveness of DoT in multi-digit multiplication, boolean logic, and grade school math problems. In addition to that, DoT showcases promising self-correction abilities and benefits from existing reasoning-enhancing techniques like self-consistency decoding. Our findings contribute to the understanding and development of reasoning with diffusion language models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：自回归语言模型（如GPT）已经展现出强大的推理能力，特别是通过思维链（Chain-of-Thought, CoT）技术，可以生成中间推理步骤来解决复杂问题。然而，自回归模型存在几个固有缺陷：错误在中间步骤中累积难以自我纠正、生成效率低（必须从左到右逐token生成）、缺乏全局规划能力。近年来，扩散模型在图像和文本生成领域取得了显著进展，具有全局规划、自我纠正、并行生成等潜在优势。作者因此提出一个问题：**扩散语言模型能否利用类似CoT的技术来获得更强的复杂推理能力？**
- **核心目标**：探索并提出一种适用于扩散模型的思维链推理方法——Diffusion-of-Thought (DoT)，使得推理步骤可以随扩散时间步并行扩散，从而在推理性能与计算效率之间灵活权衡。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- DoT将推理过程建模为扩散模型的逆向去噪过程：在每个时间步 `t`，模型同时更新所有推理步骤（即“思想”）的潜在表示，而不是像自回归模型那样从左到右逐个生成token。
- 推理路径（由多个中间步骤组成）被视为一个序列，在扩散过程中逐步从噪声中恢复。这种方法允许模型在全局上下文中修正错误，并且可以灵活控制推理步数（即去噪步数）来平衡准确率和计算开销。

### 关键技术细节
1. **条件生成建模**：采用DiffuSeq风格的分类器自由引导（classifier-free guidance），将问题上下文 `s` 与推理步骤 `r1...n` 拼接，在训练和采样时仅对推理部分添加噪声，保持问题部分固定。这避免了梯度引导中无法精确恢复条件token的问题（对于数学推理至关重要）。
2. **单遍 DoT (Single-Pass)**：所有中间推理步骤和最终答案在一次扩散过程中同时并行生成。
3. **多遍 DoT (DoT MP)**：为了引入因果偏置（后续推理步骤依赖前面步骤），按顺序逐条生成推理步骤：先生成 `r1`，然后将其与 `s` 拼接作为条件生成 `r2`，依次类推，最后生成答案。这种方法可以更好地利用先前推理步骤的强条件信号。
4. **训练时采样策略（Scheduled Sampling & Coupled Sampling）**：
   - **Scheduled Sampling**：模拟推理阶段的错误暴露，以概率 `ϵi`（随训练步数从1线性衰减到0.95）使用模型自身预测的$ \hat{z}_0$ 来采样 $z_t$ 替换原本从真实数据直接加噪的 $z_t$，增强模型从错误中恢复的能力。
   - **Coupled Sampling**（用于 DoT MP）：以一定概率不仅对当前推理步骤加噪，也对之前步骤加噪，使模型学习在之前步骤有错误时仍能正确预测后续步骤。
5. **训练目标**：最小化变分下界（VLB），包括先验损失、扩散损失和取整损失（连续模型）或分数熵损失（离散模型）。
6. **推理加速**：采用条件ODE求解器（如DPM-solver++）来加速连续扩散模型的采样，将所需步数从4096降至8~64步而不显著损失性能。
7. **自一致性（Self-Consistency）**：多次采样不同的推理路径，然后通过多数投票选择最一致的答案，进一步提高准确性。

## 3. 实验设计：数据集、基准、对比方法

### 数据集
- **简单任务**：BIG-bench中的四位数×四位数（4×4）和五位数×五位数（5×5）乘法任务；布尔逻辑推理任务（来自PromptBench/DyVal）。
- **复杂任务**：GSM8K（小学应用题数据集），使用增强训练集（augmented）。
- 所有数据集均有1000个测试样本（GSM8K为1319个）。

### 基准方法
- **Answer-only**：直接输出答案（无中间步骤）。
- **Chain-of-Thought (CoT)**：自回归模型生成中间推理步骤后给出答案。
- **Implicit CoT**：将推理步骤压缩到Transformer层内部加速推理。
- **ChatGPT (gpt-3.5-turbo-1106)**：使用5-shot CoT提示。

### 对比模型
- 自回归基线：GPT-2 small/medium/large。
- 扩散基线：Plaid (1.3B)、SEDD-small (170M)、SEDD-medium (424M) 作为预训练扩散语言模型进行微调；以及从零训练的12层Transformer做DiffuSeq结构。

## 4. 资源与算力

- **GPU配置**：所有实验在8块NVIDIA V100-32G GPU上进行。
- **训练时间**：连续扩散模型（Plaid）上的DoT单遍训练约29小时，多遍约10小时。离散模型（SEDD）训练步数200k（未明确给出时间，但类似规模）。
- **精度**：使用fp16，未使用bf16（因为V100不支持）。
- **注释**：论文中明确给出了硬件和训练时长，算力信息完整。

## 5. 实验数量与充分性

- **实验数量丰富**：覆盖了3个简单任务（4×4乘法、5×5乘法、布尔逻辑）和1个复杂任务（GSM8K）。在GSM8K上还进行了消融实验（表2）和超参数分析（图3、图4）。
- **消融实验**：
  - 对比了DoT单遍和多遍变体。
  - 对比了不同采样策略（scheduled sampling、coupled sampling）的效果。
  - 分析了推理步数（timesteps）对性能和速度的影响（图4a）。
  - 分析了自一致性采样数量对准确率的影响（图4b）。
  - 比较了ODE求解器加速的效果（图3）。
- **公平性**：基线结果取自原论文并验证了可复现性，同时报告了吞吐量（throughput）来公平比较效率。但部分基线（如ChatGPT）的吞吐量仅为响应速度，不能完全代表实际生成速度，作者对此做了说明。
- **总体评价**：实验设计系统、覆盖全面，消融充分，统计显著性有验证（多轮运行）。但GSM8K上只有1个复杂任务，泛化性验证略显不足。

## 6. 论文的主要结论与发现

1. **DoT在简单推理任务上优于或持平自回归CoT**：在4×4和5×5乘法及布尔逻辑上，DoT达到100%准确率，同时速度比GPT-2 CoT快最多27倍（由于只需1~2步扩散即可收敛）。
2. **复杂任务上小模型超越大模型**：在GSM8K上，SEDD-medium（424M）的DoT准确率53.5%，超过参数4.6倍大的GPT-2 medium（355M）的CoT（43.9%），甚至接近7B的Llama CoT（59.0%）。Plaid（1.3B）的DoT准确率32.6%（单遍）和37.7%（多遍），优于GPT-2 small（124M）的39.0%（两者架构相似）。
3. **灵活性**：DoT可以通过增加去噪步数来提升性能（如SEDD-medium在64步时超过GPT-2 medium），体现了计算-推理性能的灵活权衡。
4. **自纠正能力**：实验展示了DoT在推理过程中能够修正中间错误（表3），例如在前几步生成错误中间结果后后续步骤自行纠正，这是自回归模型难以做到的。
5. **自一致性提升明显**：采用自一致性（m=20）后，DoT和DoT MP的准确率均显著提高（如SEDD-small从45.3%升至51.8%），得益于扩散模型自然产生的多样性。

## 7. 优点

- **方法创新**：首次将CoT思想系统融入扩散语言模型，提出了单遍和多遍两种变体，并设计了针对扩散模型的训练时采样策略来增强自我修正能力。
- **效率优势明显**：在简单任务上速度提升巨大（27倍），在复杂任务上也能以较少步数达到不错性能。
- **灵活性**：可以通过调整生成步数（timesteps）来控制推理成本，适应不同难度需求。
- **广泛适用性**：方法兼容连续（Plaid）和离散（SEDD）扩散模型，且不限于特定任务（可推广到其他序列生成任务）。
- **实验充分**：多数据集、多模型、多消融，结果有说服力。

## 8. 不足与局限

- **需要额外训练**：DoT需要对扩散模型进行微调或从零训练才能实现推理，无法像ChatGPT那样通过提示直接使用（few-shot场景效果差，仅28.1%准确率）。
- **预训练扩散模型规模有限**：目前最大规模的预训练扩散模型（Plaid 1.3B）仍远小于主流LLM（如GPT-3、Llama），导致在复杂任务上绝对准确率偏低（GSM8K上最高53.5%），与7B+模型差距明显。
- **泛化能力不足**：仅在小学应用题这一种复杂数据类型上测试，没有验证在更多样化的推理任务（如符号推理、常识推理）上的表现。
- **多遍变体的效率瓶颈**：DoT MP虽然引入了因果偏置，但推理时需多次调用模型，吞吐量显著低于单遍版本（只有0.1~0.2 it/s），限制了实际应用。
- **扩散模型的固有局限**：生成质量仍受限于预训练质量（如Plaid对条件token的精确恢复能力不足导致表2中0.5%的准确率），且推理速度仍慢于同等参数的自回归模型（未启用优化时）。
- **潜在偏见与社会影响**：作者在附录中承认生成模型可能被滥用制造虚假信息，且训练数据中的偏见可能被放大，但未提出具体缓解措施。

（完）
