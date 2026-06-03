---
title: "Search and Refine During Think: Facilitating Knowledge Refinement for Improved Retrieval-Augmented Reasoning"
title_zh: 边思考边搜索并精炼：通过知识精炼改进检索增强推理
authors: "Yaorui Shi, Sihang Li, Chang Wu, Zhiyuan Liu, Junfeng Fang, Hengxing Cai, An Zhang, Xiang Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rBlWKIUQey"
tags: ["query:rl-nlplr"]
score: 8.0
evidence: 使用强化学习后训练进行检索增强推理
tldr: AutoRefine应用强化学习在推理中迭代精炼知识，提升准确性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-rblwkiuqey/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 433, \"height\": 415}]"
motivation: 检索增强推理常引入无关噪声，阻碍准确推理。
method: 提出强化学习后训练框架，在思考中插入显式知识精炼步骤。
result: 减少了无关信息干扰，提升了推理准确性。
conclusion: 在推理中动态精炼知识可有效提升检索增强推理。
---

## Abstract
Large language models have demonstrated impressive reasoning capabilities but are inherently limited by their knowledge reservoir.
Retrieval-augmented reasoning mitigates this limitation by allowing LLMs to query external resources, but existing methods often retrieve irrelevant or noisy information, hindering accurate reasoning.
In this paper, we propose **AutoRefine**, a reinforcement learning post-training framework that adopts a new "search-and-refine-during-think" paradigm.
AutoRefine introduces explicit knowledge refinement steps between successive search calls, enabling the model to iteratively filter, distill, and organize evidence before generating an answer.
Furthermore, we incorporate tailored retrieval-specific rewards alongside answer correctness rewards using group relative policy optimization.
Experiments on single-hop and multi-hop QA benchmarks demonstrate that AutoRefine significantly outperforms existing approaches, particularly in complex, multi-hop reasoning scenarios.
Detailed analysis shows that AutoRefine issues frequent, higher-quality searches and synthesizes evidence effectively.

---

## 论文详细总结（自动生成）

以下是对论文《Search and Refine During Think: Facilitating Knowledge Refinement for Improved Retrieval-Augmented Reasoning》的详细中文总结。

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：大语言模型（LLM）的推理能力受限于其内部知识，检索增强推理（Retrieval-Augmented Reasoning）允许模型查询外部资源，但现有方法存在两个关键缺陷：
  - 检索到的文档通常含有大量噪声或弱相关信息，模型缺乏对检索结果进行精炼（refinement）的步骤，容易被无关细节干扰，尤其在多跳推理中容易偏离推理链。
  - 仅依赖最终答案正确性（outcome-based reward）的奖励信号，缺乏对检索过程本身质量（如检索到并提炼了正确答案中的关键信息）的直接监督，导致模型难以学习如何生成更有效的搜索查询和利用文档。

- **含义**：该论文旨在通过引入显式的知识精炼步骤和检索专用奖励，来增强模型在检索辅助推理中的去噪能力和检索质量，从而提升复杂推理任务的准确性和稳健性。

## 2. 方法论

### 2.1 核心思想：Search-and-Refine-During-Think

- 在传统的“思考-搜索-回答”循环中，插入一个显式的 `<refine>...</refine>` 步骤，使模型在每次检索后，对返回的文档进行关键证据的提取、过滤和结构化组织，再基于精炼后的信息制定下一次搜索计划或生成最终答案。
- 推理轨迹由 `<think>`, `<search>`, `<documents>`, `<refine>`, `<answer>` 等结构化动作组成，模型自主决定搜索轮次，动态适应问题复杂度。

### 2.2 关键技术细节

- **奖励设计**（双奖励整合）：
  - **答案奖励** \( R_{\text{Ans}} \)：基于预测答案与真实答案的 F1 分数，范围 [0, 1]。
  - **检索奖励** \( R_{\text{Ret}} \)：检查所有 `<refine>` 块中是否完整覆盖了真实答案的所有成分。若完全覆盖则为 1，否则为 0。
  - **总奖励**：
    - 若 \( R_{\text{Ans}} > 0 \)：总奖励 = \( R_{\text{Ans}} \)
    - 若 \( R_{\text{Ans}} = 0 \) 且 \( R_{\text{Ret}} > 0 \)：总奖励 = 0.1（部分奖励）
    - 否则为 0。
  - 非线性组合方式，优先激励正确答案，同时为无效答案但有效精炼的行为提供微弱正向信号。

- **训练算法**：采用 Group Relative Policy Optimization (GRPO)，从同一 prompt 采样 G 个轨迹，计算组内优势并更新策略，同时通过 KL 散度项约束策略偏离。在损失计算中屏蔽检索文档部分，避免模型对检索内容的过度拟合。

- **流程示例**：系统提示中包含 `<think>`、`<search>`、`<documents>`、`<refine>`、`<answer>` 五个特殊标记，模型通过自回归生成整个推理路径，直到产生 `<answer>` 停止。

## 3. 实验设计

### 3.1 数据集与基准

- **训练集**：Natural Questions (NQ) 与 HotpotQA 的训练集合并（共约 16.9 万样本）。
- **评估**：7 个开放域问答基准，分为两类：
  - **单跳 QA**：NQ、TriviaQA、PopQA
  - **多跳 QA**：HotpotQA、2WikiMultihopQA、Musique、Bamboogle
- 评价指标：Exact Match (EM) 为主，辅以 F1 和 Cover Exact Match (CEM)。

### 3.2 对比方法

- **无检索**：Direct Generation、SFT、R1-like 训练（R1-Instruct/Base）
- **单跳检索**：Naive RAG（直接用原始问题检索一次）
- **多跳检索**：Search-o1、IRCoT、ReSearch（Instruct/Base）、Search-R1（Instruct/Base）

### 3.3 实现细节

- 知识源：2018 年 12 月的 Wikipedia dump，使用 E5-base-v2 作为检索编码器，默认返回 top-3 文档。
- 骨干模型：Qwen2.5-3B（Base 和 Instruct 变体），另对 7B 模型进行扩展实验。

## 4. 资源与算力

- **训练硬件**：8 块 NVIDIA A100-80GB GPU
- **框架**：VeRL，采用全参数微调（FSDP），BF16 混合精度
- **训练步数**：200 步，batch size 256，micro batch size 64
- **细节**：使用 vLLM 进行采样加速（GPU 内存利用率 0.6），每个问题采样 5 条轨迹，最大搜索轮次 5。
- **未明确说明**：单次训练耗时时长未在文中给出。

## 5. 实验数量与充分性

- **累计实验组数**：约 20 组以上（含主表、多维度分析、消融、模型尺寸、检索深度、统计显著性等）
- **主要实验**：
  - 主性能对比（Table 1）：7 数据集 × 10+ 方法
  - 搜索行为分析（图 4）：搜索频率、搜索质量随时间变化
  - 精炼效果分析（图 5）：精炼步骤的召回率与长度压缩
  - 检索深度鲁棒性（图 6）：k = 1~7
  - 消融研究（Table 2, 图 7）：关键组件（精炼步骤、检索奖励）
  - 模型缩放（Table 3）：3B vs 7B
  - 检索奖励设计变体（Table 8）：线性组合、作用于文档 vs 精炼
  - 复杂答案子集分析（Table 9）：长答案样本上的奖励设计比较
  - 外部精炼器对比（Table 4）：BART、Qwen 摘要 vs RL 驱动的精炼
  - 统计显著性检验（Table 7）：T-test，p < 0.01
- **充分性判断**：实验覆盖面广，从性能、行为、设计空间、泛化性、统计显著性等多个角度验证方案，对比方法均为近期代表性工作，设置公平（统一检索语料、相同训练数据集）。消融实验扎实，结论可信。

## 6. 主要结论与发现

- AutoRefine 在 7 个数据集上的平均 EM 准确率相比最强基线（Search-R1）提升 6.9%（Base 变体 0.405 vs 0.312），在多跳任务上提升尤为突出（2Wiki 提升 21%，Musique 提升 26.7%）。
- 模型学会自适应调整搜索次数：对多跳问题搜索更多（平均 2+ 次），对单跳问题搜索较少（约 1.2 次）。
- 搜索质量显著优于基线方法：在多跳任务上检索包含答案的成功率超过 50%，比基线高 10%–15%。
- 精炼步骤能在保留关键证据的同时，将文档长度压缩至原始 1/4 左右。
- 在不同检索深度（k=1~7）下均保持稳定增益，且当检索深度增大（噪声增加）时增益更明显。
- 检索奖励对提升精炼质量至关重要，移除后精炼成功率下降约 20%。

## 7. 优点

1. **方法简洁有效**：仅通过引入一个显式 `<refine>` 步骤和检索奖励，就显著提升了检索增强推理性能，无需复杂的外部模块。
2. **奖励设计精巧**：非线性组合方式平衡了最终答案正确性与中间过程质量，避免过度强调中间行为。
3. **实验全面严谨**：涵盖多种数据集、模型规模、消融、统计检验；分析了搜索行为、精炼效果、鲁棒性等多个维度，结论可靠。
4. **代码开源**：便于复现和后续研究。

## 8. 不足与局限

1. **评估指标单一**：仅使用精确匹配（EM/F1），可能忽略语义等价但表述不同的正确答案，不适合长文本或开放型答案评估。
2. **静态检索语料**：使用 2018 年的 Wikipedia 快照，无法反映实时信息的检索需求，实际应用中动态搜索场景受限。
3. **计算资源需求较大**：基于 GRPO 的 RL 训练需要多 GPU 和高数据量，资源门槛较高。
4. **未探索跨领域泛化**：仅在 QA 任务上验证，未考察摘要、对话、信息抽取等其他检索增强场景。
5. **小型模型实验为主**：虽然在 7B 模型上验证，但主要实验基于 3B 参数，更大规模模型（如 70B）上的表现未知。

（完）
