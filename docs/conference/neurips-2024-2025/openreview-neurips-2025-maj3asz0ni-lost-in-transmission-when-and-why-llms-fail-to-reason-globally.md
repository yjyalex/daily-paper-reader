---
title: "Lost in Transmission: When and Why LLMs Fail to Reason Globally"
title_zh: 传输丢失：大型语言模型何时以及为何无法进行全局推理
authors: "Tobias Schnabel, Kiran Tomlinson, Adith Swaminathan, Jennifer Neville"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=MaJ3ASZ0NI"
tags: ["query:cot-unfaith"]
score: 5.0
evidence: 通过带宽约束分析LLM全局推理失败原因
tldr: 提出BAPO模型解释因信息流带宽限制导致的LLM推理失败。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM在需要全局推理的任务上表现不佳，原因不明。
method: 提出有界注意力前缀预言机(BAPO)模型，模拟注意力头带宽限制。
result: 证明图可达性等推理问题对LLM是BAPO困难的，实验验证了理论。
conclusion: LLM推理失败源于内部通信带宽限制。
---

## Abstract
Despite their many successes, transformer-based large language models (LLMs) continue to struggle with tasks that require complex reasoning over large parts of their input. We argue that these failures arise due to capacity limits on the accurate flow of information within LLMs. To formalize this issue, we introduce the bounded attention prefix oracle (BAPO) model, a new computational framework that models bandwidth constraints on attention heads, the mechanism for internal communication in LLMs. We show that several important reasoning problems like graph reachability require high communication bandwidth for BAPOs to solve; we call these problems BAPO-hard. Our experiments corroborate our theoretical predictions: GPT-4o, Claude, and Gemini succeed on BAPO-easy tasks and fail even on relatively small BAPO-hard tasks. BAPOs also reveal another benefit of chain of thought (CoT): we prove that breaking down a task using CoT can turn any BAPO-hard problem into a BAPO-easy one. Our results offer principled explanations for key LLM failures and suggest directions for architectures and inference methods that mitigate bandwidth limits.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：基于Transformer的大型语言模型（LLM）在需要跨输入全局推理的任务（如链式三段论、函数组合、形式语言识别）上持续表现不佳。作者假设这些失败源于LLM内部信息流准确传递的容量限制，即“有效带宽”的限制。
- **核心问题**：为何LLM难以解决全局推理问题？如何形式化并预测这种失败？
- **整体含义**：提出有界注意力前缀预言机（BAPO）模型，从信息论角度解释LLM推理失败的根本原因，并指出链式思维（CoT）可降低带宽需求，为设计更优架构和推理方法提供理论基础。

## 2. 论文提出的方法论

### 核心思想
- **BAPO模型**：一种计算框架，模拟Transformer中因果注意力导致的跨残差流信息传输带宽约束。模型将输入划分为前缀和后缀，信息通过前缀预言机（输出最多a比特）和注意力函数（可选择最多b个前缀令牌）传递至后缀预言机，最终输出答案。
- **BAPO-easy vs BAPO-hard**：若问题可由常数带宽的BAPO求解，则为BAPO-easy；否则为BAPO-hard。BAPO-hard意味着问题对LLM是困难的。

### 关键技术细节
- 定义（a,b）-BAPO：a为前缀带宽（比特），b为注意力带宽（令牌数）。
- 理论结果：
  - **BAPO-easy问题**：INDEX、EQUALITY、DISJOINTNESS、MATCH 2n、正则语言（状态复杂度决定带宽）。
  - **BAPO-hard问题**：REACHABILITY（图可达性）、MAJORITY（多数判定）、MATCH 3n、UNIQUE、SET DIFF。证明采用构造性矛盾法：通过饱和注意力（使注意力集中于共享前缀令牌）和鸽巢原理导致前缀预言机冲突，构造两个实例使BAPO无法区分。
  - **链式思维（CoT）**：证明任何可判定问题均可由常数带宽的BAPO-CoT求解（模拟图灵机一步为低带宽问题），因此CoT可将BAPO-hard问题转化为BAPO-easy步骤序列。
- 模型扩展：score-BAPO、multi-layer BAPO、full-attention BAPO。这些变体不改变REACHABILITY、MAJORITY、MATCH 3n的BAPO-hard性，但可解决induction heads任务。

## 3. 实验设计

### 使用的数据集/场景
- **合成任务**：INDEX、EQUALITY、DISJOINTNESS、MATCH 2、REACHABILITY、MAJORITY、MATCH 3、UNIQUE、SET DIFF。每个任务生成100个独立同分布实例，输入长度n取6、50、100、200等。
- **真实世界任务**：
  - 酒店评论聚合（SPACE数据集）：分为“查找负面评论”（类INDEX）和“判断多数评论是否正面”（类MAJORITY）。
  - 代码变量追踪（扩展自RULER基准）：判断最终变量值，属于REACHABILITY特例。

### Benchmark
- 无特定外部benchmark，作者自行构建任务。
- 对比了三种主流LLM家族：GPT（GPT-4o、GPT-4o mini）、Claude（Claude 3.5 Sonnet、Claude 3.5 Haiku）、Gemini（Gemini 1.5 Pro、Gemini 1.5 Flash），以及推理模型o3和Gemini 2.5 Flash。

### 对比方法
- 直接推理 vs 链式思维（CoT，软限制250词） vs 无限制内部推理（o3、Gemini 2.5 Flash）。

## 4. 资源与算力

- **文中明确说明**：全部实验耗时≤1天，花费约$400 API credits（其中o3占$93），初步实验额外花费约$150。未提及GPU型号、数量或训练时长（因为全部为API调用，不涉及本地训练或部署）。
- 注意：未提供本地算力细节，因为实验仅涉及推理调用。

## 5. 实验数量与充分性

- **实验组数**：
  - 基础实验：6个BAPO-easy/hard问题（每个问题在多个n值下各100个实例），涉及3个模型家族共6个模型。
  - CoT实验：同问题同模型，加入CoT提示。
  - 真实世界任务：两个场景（酒店评论、变量追踪），多个n值。
  - 扩展实验：UNIQUE和SET DIFF在更大n（200-1000）下的测试；对DISJOINTNESS整数变体（INT DISJOINTNESS）进行额外实验。
  - 推理模型实验：o3和Gemini 2.5 Flash在BAPO-hard任务上的表现。
- **充分性与公平性**：
  - 覆盖了多种模型规模和家族，排除了单一家族偏差。
  - 对每个条件生成100次独立重复，报告95% t检验置信区间，统计严谨。
  - 正负实例均衡（除INDEX、UNIQUE、SET DIFF外均为yes/no二元输出，猜中率50%）。
  - 设计实例时避免明显捷径或启发式，迫使模型全面考虑输入。
  - 不足之处：未进行超参数搜索（温度固定为0）；未在更大规模模型（如GPT-4超大版本）上测试；真实世界任务仅两个场景，泛化性待验证。

## 6. 论文的主要结论与发现

1. **BAPO复杂度预测LLM失败**：在BAPO-hard任务（REACHABILITY、MAJORITY、MATCH 3）上，所有非推理模型在n=200时准确率接近随机猜测；BAPO-easy任务（INDEX、EQUALITY、MATCH 2）表现稳定较好。即使增大模型规模也无法避免BAPO-hard任务的退化。
2. **链式思维（CoT）有所帮助但有限**：带有限CoT（250词）的非推理模型在BAPO-hard任务上仅带来适度提升，尤其在小n下。推理模型o3和Gemini 2.5 Flash（使用数千甚至上万推理token）几乎完美解决BAPO-hard任务，验证了CoT降低带宽需求的理论。
3. **真实世界任务验证**：包含MAJORITY（聚合评论）或REACHABILITY（变量追踪）成分的真实任务同样表现出BAPO-hard特征，而包含INDEX成分的任务则容易解决。
4. **理论贡献**：BAPO模型可解释LLM全局推理失败的根本原因，并指明CoT降低带宽需求的机制。常数带宽的BAPO-CoT是图灵完备的。

## 7. 优点

- **理论创新**：提出BAPO这一简洁的计算模型，将LLM推理失败归因于信息带宽约束，而非传统的表达能力限制，为理解学习与泛化之间的权衡提供新视角。
- **理论与实验紧密结合**：理论预测与LLM实际行为高度一致，验证了假说。
- **实验设计严谨**：多次重复、置信区间、正负平衡、无提示作弊。
- **跨模型家族覆盖**：包括闭源最先进模型，结论具有普遍性。
- **实用性**：为从业者提供识别BAPO-hard/hard任务并采取CoT、工具调用等缓解策略的指南。
- **可扩展性**：模型变体分析（score-BAPO、多层等）显示核心BAPO-hard问题具有鲁棒性。

## 8. 不足与局限

- **模型抽象与真实Transformer仍有差距**：BAPO假设完美位置编码、二进制注意力等，而真实模型使用软注意力、分数值、层间交互等，可能遗漏某些能力或失败模式。
- **有效带宽根因未探明**：论文指出LLM有效带宽很小，但未分析导致这一现象的原因（如训练目标、架构设计、优化景观等）。
- **降阶边界不紧**：部分下界（如REACHABILITY）与平凡上界之间差距较大，开放问题。
- **实验覆盖有限**：
  - 仅测试了三个模型家族，未包含开源模型（如Llama、Mistral）。
  - 真实世界任务仅两个场景，可能不足以代表所有全局推理应用。
  - 未测试更长输入（>200）或更大词汇量（UNIQUE/SET DIFF随词汇增长）。
- **CoT实验结果解释存在模糊性**：非推理模型CoT表现提升有限，可能是因为提示词限制（250词）或模型未训练有效利用CoT；而推理模型使用大量token，是否可归因于带宽降低之外的机制（如记忆、搜索）未排除。
- **潜在风险**：模型可能因BAPO-easy/硬分类而误导实践者，但作者已说明BAPO-easy不保证模型能解决（如MATCH 2仍出现下降）。

（完）
