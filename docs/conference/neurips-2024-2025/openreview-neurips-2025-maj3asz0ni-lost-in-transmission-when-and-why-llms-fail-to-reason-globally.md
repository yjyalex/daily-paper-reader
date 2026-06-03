---
title: "Lost in Transmission: When and Why LLMs Fail to Reason Globally"
title_zh: 传输丢失：大语言模型何时及为何全局推理失败
authors: "Tobias Schnabel, Kiran Tomlinson, Adith Swaminathan, Jennifer Neville"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=MaJ3ASZ0NI"
tags: ["query:cot-unfaith"]
score: 5.0
evidence: 分析大语言模型何时及为何全局推理失败
tldr: 提出理论模型解释大语言模型因注意力带宽限制导致全局推理失败
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 大语言模型在需要复杂推理的任务上仍然表现不佳。
method: 提出有界注意力前缀预言机模型，形式化注意力头的带宽限制。
result: 实验验证了多种大模型在BAPO困难问题上的失败。
conclusion: 注意力带宽限制是大语言模型全局推理失败的重要原因。
---

## Abstract
Despite their many successes, transformer-based large language models (LLMs) continue to struggle with tasks that require complex reasoning over large parts of their input. We argue that these failures arise due to capacity limits on the accurate flow of information within LLMs. To formalize this issue, we introduce the bounded attention prefix oracle (BAPO) model, a new computational framework that models bandwidth constraints on attention heads, the mechanism for internal communication in LLMs. We show that several important reasoning problems like graph reachability require high communication bandwidth for BAPOs to solve; we call these problems BAPO-hard. Our experiments corroborate our theoretical predictions: GPT-4o, Claude, and Gemini succeed on BAPO-easy tasks and fail even on relatively small BAPO-hard tasks. BAPOs also reveal another benefit of chain of thought (CoT): we prove that breaking down a task using CoT can turn any BAPO-hard problem into a BAPO-easy one. Our results offer principled explanations for key LLM failures and suggest directions for architectures and inference methods that mitigate bandwidth limits.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：基于 Transformer 的大语言模型（LLM）在需要整合整个输入信息进行复杂推理的任务中（如链式三段论、函数组合、形式语言识别）仍表现不佳。作者假设这些失败源于 LLM 内部不同残差流之间信息传输的容量限制，即有效带宽有限。
- **整体含义**：通过形式化“通信带宽约束”这一概念，解释为何 LLM 在面对全局推理问题时系统性失败，并揭示思维链（Chain-of-Thought, CoT）能降低带宽需求的内在机制，为改进架构和推理方法提供理论指导。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**有界注意力前缀预言机（Bounded Attention Prefix Oracle, BAPO）模型**，抽象 Transformer 中因果注意力导致的信息流约束。BAPO 将问题分解为前缀和后缀，前缀预言机 `f` 输出有限比特（前缀带宽 `a`），注意力函数 `g` 最多选择 `b` 个前缀 token（注意力带宽 `b`），后缀预言机 `h` 结合这些信息求解。
- **关键技术细节**：
  - **BAPO 定义**（Definition 1）：`(a,b)-BAPO` 由 `f: Σ* → {0,1}^a`、`g: Σ*×N×Σ×N → {0,1}`、`h: {0,1}^a × ∪_{i=0}^b (Σ×N)^i × Σ* × N → Σ` 组成，要求对所有可能的前缀‑后缀划分及任意被选中的 `b` 个 token 都能正确求解。
  - **BAPO-easy vs BAPO-hard**：如果问题能被常数带宽（相对于输入长度 n）的 BAPO 解决，则称 BAPO-easy，否则为 BAPO-hard。需额外注意，若带宽需随词汇表大小 |Σ| 增长，则称为 BAPO-Σ-hard。
  - **理论证明**：
    - **BAPO-easy 问题**：INDEX（(0,1)-BAPO）、EQUALITY 和 DISJOINTNESS（(1,1)-BAPO）、正则语言（(|Q|,0)-BAPO，|Q| 为状态数）。
    - **BAPO-hard 问题**：REACHABILITY（图可达性）、MAJORITY（多数判定）、MATCH 3n（三数之和）、UNIQUE、SET DIFF。证明采用构造冲突族使注意力饱和、前缀预言机碰撞的方法。
    - **CoT 的作用**：Theorem 8 证明通过 CoT，任何可判定问题都能被常数带宽 (2,3)-BAPO-CoT 解决，因为模拟图灵机单步是低带宽问题。

## 3. 实验设计：数据集/场景、基准、对比方法

- **合成任务**：INDEX、EQUALITY、DISJOINTNESS、REACHABILITY、MAJORITY、MATCH 2、MATCH 3、UNIQUE、SET DIFF。输入长度 n 取 {6, 50, 100, 200} 等，每个条件生成 100 个 i.i.d. 实例，保证正负例均衡。
- **真实世界任务**：
  - 酒店评论情感聚合（来自 SPACE 数据集）对应 MAJORITY（多数为正？）和 INDEX（找负面评论）。
  - 变量追踪代码任务（类似 RULER benchmark）对应 REACHABILITY。
- **基准**：随机猜测（50% 准确率）作为下限。
- **对比方法**：三个模型家族：GPT（GPT-4o, GPT-4o mini）、Claude（Sonnet, Haiku）、Gemini（1.5 Pro, Flash）。同时测试 CoT 变体（250 词软限制）以及推理模型 o3 和 Gemini 2.5 Flash。
- **平台**：使用商业 API，强制 JSON 输出格式。

## 4. 资源与算力

- 论文仅在附录 D.1 提及实验花费约 **$400 API credits**（其中 o3 占 $93），初步实验额外约 $150。**未明确说明 GPU 型号、数量或训练时长**，因为实验均通过 API 调用闭源模型完成。

## 5. 实验数量与充分性

- **实验数量**：对每个合成任务每个长度（4–5 个）及每个模型，各运行 100 次，总共约 **数千个独立实验**；此外还进行了 CoT 变体实验和真实世界任务实验（n ≤ 100）。
- **充分性评价**：
  - 覆盖了 BAPO-easy 和 BAPO-hard 多种问题，验证了理论预测的准确性。
  - 对比了不同规模、不同家族的模型，观察尺度效应。
  - 进行了 CoT 消融，验证 CoT 对 BAPO-hard 问题的提升。
  - **局限性**：仅依赖商业 API，无法控制模型内部细节；未测试开源模型；真实世界任务样本量偏少。

## 6. 论文的主要结论与发现

- **结论 1**：BAPO 复杂度与 LLM 实际表现高度一致。BAPO-easy 问题上所有模型均保持高准确率，BAPO-hard 问题上准确率迅速下降至接近随机，即使模型规模增大也无法避免。
- **结论 2**：CoT 能缓解 BAPO-hard 问题，但非推理模型在有限 CoT 预算下提升有限；而 o3 和 Gemini 2.5 Flash 通过大量内部推理 token（数千至上万）几乎完美解决 BAPO-hard 任务。
- **结论 3**：现实任务（评论聚合、变量追踪）中 BAPO-hard 成分同样是 LLM 的瓶颈。
- **结论 4**：理论证明了 CoT 可将任意 BAPO-hard 问题降为 BAPO-easy，解释了 CoT 成功的另一机制。

## 7. 优点

- **理论模型简洁有力**：BAPO 抽象出通信带宽这一关键因素，不依赖 Transformer 底层细节，广泛适用且直观。
- **严格理论证明**：对多个经典问题给出了清晰的下界和上界，包括注意力与前缀带宽的权衡。
- **实验验证充分**：覆盖多个模型家族、多种任务，并延伸至真实场景，支撑理论预测。
- **揭示 CoT 新优势**：证明了 CoT 降低带宽需求，为理解推理链效果提供新视角。

## 8. 不足与局限

- **模型不完整**：BAPO 未完全模拟真实 Transformer（如多层注意力、残差连接、数值精度等），部分细节（如 induction heads）需扩展才能捕捉。
- **不涵盖所有失败模式**：BAPO-easy 不保证 LLM 成功，其他因素（分词、位置编码、训练偏差）仍会导致失败。
- **带宽受限的根本原因未知**：论文未解释为何 LLM 有效带宽很小，仅推测可能与泛化能力有关。
- **理论下界不紧**：部分问题的 BAPO 下界与平凡上界仍有差距，需进一步研究。
- **实验依赖闭源 API**：无法复现内部状态，且仅测试了英文任务，可能限制泛化性。

（完）
