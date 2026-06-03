---
title: "SemCoT: Accelerating Chain-of-Thought Reasoning through Semantically-Aligned Implicit Tokens"
title_zh: SemCoT：通过语义对齐的隐式token加速链式推理
authors: "Yinhan He, Wendy Zheng, Yaochen Zhu, Zaiyi Zheng, Lin Su, Sriram Vasudevan, Qi Guo, Liangjie Hong, Jundong Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=1ZuzFUMtx6"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 隐式CoT推理加速与语义对齐
tldr: 提出SemCoT，使用隐式token加速CoT同时保持语义对齐。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: CoT推理冗长，隐式CoT方法存在语义对齐问题。
method: 将推理步骤编码为隐藏嵌入中的隐式token，并强制语义对齐。
result: 在多个推理任务上取得加速且保持准确性。
conclusion: 隐式token能有效加速CoT推理。
---

## Abstract
Chain-of-Thought (CoT) enhances the performance of Large Language Models (LLMs) on reasoning tasks by encouraging step-by-step solutions. However, the verbosity of CoT reasoning hinders its mass deployment in efficiency-critical applications. Recently, implicit CoT approaches have emerged, which encode reasoning steps within LLM's hidden embeddings (termed ``implicit reasoning'') rather than explicit tokens. This approach accelerates CoT reasoning by reducing the reasoning length and bypassing some LLM components. However, existing implicit CoT methods face two significant challenges: (1) they fail to preserve the semantic alignment between the implicit reasoning (when transformed to natural language) and the ground-truth reasoning, resulting in a significant CoT performance degradation, and (2) they focus on reducing the length of the implicit reasoning; however, they neglect the considerable time cost for an LLM to generate one individual implicit reasoning token. To tackle these challenges, we propose a novel semantically-aligned implicit CoT framework termed **SemCoT**. In particular, for the first challenge, we design a contrastively trained sentence transformer that evaluates semantic alignment between implicit and explicit reasoning, which is used to enforce semantic preservation during implicit reasoning optimization. To address the second challenge, we introduce an efficient implicit reasoning generator by finetuning a lightweight language model using knowledge distillation. This generator is guided by our sentence transformer to distill ground-truth reasoning into semantically aligned implicit reasoning, while also optimizing for accuracy. SemCoT is the first approach that enhances CoT efficiency by jointly optimizing token-level generation speed and preserving semantic alignment with ground-truth reasoning. Extensive experiments demonstrate the superior performance of SemCoT compared to state-of-the-art methods in both efficiency and effectiveness. Our code can be found at https://github.com/YinhanHe123/SemCoT/.

---

## 论文详细总结（自动生成）

# 论文总结：SemCoT：通过语义对齐的隐式token加速链式推理

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：Chain-of-Thought (CoT) 通过逐步推理显著提升大语言模型（LLM）在推理任务上的性能，但生成的推理文本冗长，导致推理时间过长，限制了在高效率关键场景中的部署。
- **现有隐式 CoT 方法**：将推理步骤编码为 LLM 的隐藏嵌入（隐式 token），可缩短推理长度并绕过部分组件（如反嵌入层），从而加速推理。但存在两大挑战：
  - **语义对齐不足**：隐式推理与真实推理（自然语言）之间存在格式不匹配，现有方法要么丢弃真实推理，要么仅匹配关键词，导致语义信息丢失，性能下降。
  - **生成效率短板**：尽管缩短了推理长度，但 LLM 生成每个隐式 token 本身耗时巨大（如 DeepSeek-R1 每 token 约 0.1 秒），特别是在大模型上累积显著。
- **本文目标**：在保持推理准确性的前提下，大幅提升 CoT 推理效率，同时确保隐式推理与真实推理在语义上对齐。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：设计一个两阶段框架 SemCoT：
  - **阶段一**：训练一个定制化的句子变换器（Customized Sentence Transformer），用于评估隐式推理与真实推理之间的语义对齐程度。
  - **阶段二**：训练一个轻量级的隐式推理生成器，该生成器利用知识蒸馏从真实推理中学习，并联合优化语义对齐与答案准确性。

- **关键技术细节**：
  - **句子变换器（步骤一）**：提取 LLM 的中间五层作为骨干，加上池化层和线性投影层，将隐式/真实推理映射到统一语义嵌入空间。采用对比学习训练：正例为真实推理与其压缩版本，负例为不同样本对。
  - **轻量级生成器（步骤二）**：使用从原始 LLM 剪枝/蒸馏得到的小模型（如 Sheared-LLaMA-1.3B）作为基础，添加线性投影层将其最后一层隐藏嵌入映射到 LLM 的嵌入空间。生成器以查询拼接 `k` 个 `<CoT>` 特殊 token 作为输入，输出隐式推理 token 序列。
  - **损失函数**：总损失 `L_total = λ * L_sem + (1-λ) * L_pred`：
    - `L_sem`：基于句子变换器的余弦相似度，使生成器的隐式推理嵌入与真实推理嵌入对齐。
    - `L_pred`：标准交叉熵损失，使 LLM 在给定隐式推理后能正确生成答案。
  - **推理阶段**：输入查询后，由轻量级生成器生成隐式 token，再送入 LLM 输出最终答案。

## 3. 实验设计
- **数据集**：五个代表性推理数据集：
  - **数学推理**：GSM8K、SVAMP、MultiArith
  - **常识推理**：CommonsenseQA
  - **符号推理**：CoinFlip
- **评估基准 (Benchmark)**：两个开源 LLM：**Llama-2-7b-chat-hf** 和 **Mistral-7B-Instruct-v0.2**。
- **对比方法**：
  - **Pause**：用统一隐式 token 替换真实推理。
  - **ICoT-SI** 和 **COCONUT**：渐进编码方法，逐步将显式推理转为隐式。
  - **CODI**：自蒸馏策略，教师学显式推理，学生学隐式生成。
  - **SoftCoT**：用小型 LM 生成 LLM 的隐式推理 token（不压缩长度）。
- **评估指标**：
  - **有效性**：回答准确率（Accuracy）。
  - **效率**：平均壁钟时间（Wall-clock time）。
- **核心设置**：
  - 训练时使用 5 个隐式 token，推理时仅用 1 个 token（公平比较）。
  - 句子变换器输出维度 768，优化器 AdamW。

## 4. 资源与算力
- **硬件**：多台配备 NVIDIA H100 80GB GPU 的机器，CUDA 12.4。
- **算力细节**：论文未明确说明具体 GPU 数量、训练时长或能耗。附录提供了超参数配置和代码地址（GitHub），但未给出完整训练日志。因此算力消耗的精确数据缺失，但可推断使用了较昂贵的计算资源（H100）。

## 5. 实验数量与充分性
- **实验数量**：
  - **主实验**：在 5 个数据集 × 2 个 LLM × 6 个方法（含 SemCoT）共 60 组对比，每组重复 3 次取平均和标准差。
  - **消融实验**：三种变体（去掉语义对齐损失、替换余弦相似度、使用全模型生成隐式 token）在全部数据集和 LLM 上进行。
  - **参数敏感性**：针对 λ（语义损失权重）和隐式 token 数量进行调参分析。
  - **案例研究**：使用 PCA 可视化展示语义对齐效果。
- **充分性与客观性**：
  - 覆盖了数学、常识、符号三类领域，涵盖主流推理任务。
  - 对比了 5 种代表性基线方法，含近期工作（SoftCoT, COCONUT 等）。
  - 消融实验验证各组件贡献，参数分析探索最优设置。
  - 案例研究通过语义变体查询验证隐式推理的一致性。
  - 但未测试超长链推理（如多步复杂推理）或专业化领域（如医学、法律），且仅在两个 7B 规模模型上验证，未在更大模型（如 70B）上实验，泛化性有待商榷。

## 6. 主要结论与发现
- SemCoT 在所有数据集和模型上几乎均实现了最高准确率，同时推理时间接近最快（略慢于 SoftCoT 但显著优于其他方法）。
- 消融实验表明：
  - 语义对齐损失（`L_sem`）对性能贡献最大，缺失后准确率显著下降。
  - 使用轻量级 LM 生成隐式 token 比直接微调原 LLM 更有效（避免灾难性遗忘）。
- 隐式 token 数量少（1个）时性能最优，说明压缩表达能力足够。
- 案例研究显示 SemCoT 的隐式推理在语义变体查询下具有更好的聚类一致性，证明其成功捕捉了核心语义。

## 7. 优点
- **创新性**：首次联合优化**语义对齐**和**生成速度**，系统性地解决隐式 CoT 的两大短板。
- **方法设计**：定制句子变换器巧妙地将隐式嵌入与自然语言嵌入在统一空间对比，避免了直接翻译的难题。
- **效率提升**：采用剪枝/蒸馏的小模型生成隐式 token，大幅降低每 token 生成时间（理论加速可达数倍）。
- **实验严谨**：多数据集、多基线、多消融、参数分析与可视化，结论支撑充分。
- **可复现性**：代码开源，超参公开。

## 8. 不足与局限
- **额外训练开销**：句子变换器的训练需要额外数据和计算资源，对资源受限环境不友好。
- **领域局限**：仅在标准推理基准上评估，未涉及专业领域（如医学、法律）或极长链推理场景。
- **模型规模**：仅在 7B 模型上验证，对更大规模模型（如 70B）的性能和效率提升未知。
- **解释性降低**：隐式推理为连续嵌入，无法直接解读推理过程，降低了人类可理解性。
- **语义对齐假设**：依赖于句子变换器度量质量，若变换器本身有偏差可能影响对齐效果。
- **时间测量**：仅测试了推理时间，未对比训练时间；且对轻量级生成器本身的推理时间未做详细分析（可能存在瓶颈）。

（完）
