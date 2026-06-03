---
title: "Search and Refine During Think: Facilitating Knowledge Refinement for Improved Retrieval-Augmented Reasoning"
title_zh: 搜索与精炼：促进知识精炼以改进检索增强推理
authors: "Yaorui Shi, Sihang Li, Chang Wu, Zhiyuan Liu, Junfeng Fang, Hengxing Cai, An Zhang, Xiang Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rBlWKIUQey"
tags: ["query:rl-nlplr"]
score: 8.0
evidence: 强化学习后训练用于推理精炼
tldr: AutoRefine使用强化学习在推理中迭代过滤和提炼证据
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-rblwkiuqey/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 433, \"height\": 415}]"
motivation: 现有检索增强推理常引入噪声信息。
method: 提出AutoRefine，一种搜索-精炼-思考的强化学习后训练框架。
result: 在多个推理任务上显著提升准确率。
conclusion: 检索增强推理可通过显式知识精炼步骤得到改善。
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

## 中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大语言模型（LLM）在推理时依赖内部知识，但知识库有限；检索增强生成（RAG）虽能引入外部知识，但现有方法常检索到噪声或不相关信息，干扰准确推理。特别是在多跳场景中，早期步骤的干扰会逐级放大。
- **整体含义**：提出一种 **“搜索-精炼-思考”**（search-and-refine-during-think）新范式，通过强化学习（RL）后训练，让模型在检索与回答之间显式地过滤、提炼和组织证据，提升检索增强推理的准确性和鲁棒性。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：在现有“搜索-思考”流程中插入显式的**知识精炼步骤**（`<refine>`），并引入**检索特定奖励**（retrieval-specific reward）来指导模型如何从检索文档中提取关键信息。
- **关键技术细节**：
  - **轨迹生成**：模型按照 `<think>` → `<search>` → `<documents>` → `<refine>` → `<answer>` 的顺序循环，直到产生答案。精炼步骤用于从检索文档中提取与答案相关的关键事实。
  - **奖励设计**：
    - 答案奖励 \( R_{\text{Ans}} \)：F1分数，衡量预测答案与真实答案的重合度。
    - 检索奖励 \( R_{\text{Ret}} \)：若精炼步骤（`<refine>`）的内容覆盖真实答案的所有成分，则 \( R_{\text{Ret}} = 1 \)，否则为 0。
    - 整体奖励：
      \[
      R_{\text{Overall}} = 
      \begin{cases}
      R_{\text{Ans}}, & \text{if } R_{\text{Ans}} > 0 \\
      0.1, & \text{if } R_{\text{Ans}} = 0 \text{ and } R_{\text{Ret}} > 0 \\
      0, & \text{else}
      \end{cases}
      \]
  - **训练算法**：采用 **Group Relative Policy Optimization (GRPO)**。对每个问题生成 G=5 条轨迹，计算组内优势，使用裁剪和KL散度约束优化策略。检索文档部分的token不参与损失计算。
- **算法流程**（文字描述）：
  1. 给定问题，由策略模型生成一组轨迹（含思考、搜索、精炼、答案等步骤）。
  2. 对每条轨迹计算答案奖励和检索奖励。
  3. 按整体奖励公式计算每条轨迹的最终奖励。
  4. 在组内标准化奖励，得到每个token的优势。
  5. 用GRPO更新策略模型，同时参考模型提供KL散度约束。

### 3. 实验设计：使用了哪些数据集/场景，benchmark，对比方法
- **数据集**：
  - **单跳QA**：Natural Questions (NQ)、TriviaQA、PopQA。
  - **多跳QA**：HotpotQA、2WikiMultihopQA (2Wiki)、Musique、Bamboogle。
- **训练集**：结合 NQ 和 HotpotQA 的训练集（约16.9万样本）。
- **评估指标**：主要使用 Exact Match (EM)，也报告 F1 和 Cover Exact Match (CEM)。
- **对比方法**：
  - 无检索：直接生成、SFT、R1-like（DeepSeek-R1风格）训练。
  - 单跳检索：Naive RAG（直接用问题检索一次）。
  - 多跳检索：Search-o1、IRCoT、ReSearch、Search-R1（使用不同基座模型）。
- **检索设置**：使用2018年12月Wikipedia快照，检索器为E5-base-v2，默认返回 top-3 文档。

### 4. 资源与算力
- **GPU**：8块 NVIDIA A100-80GB GPU。
- **训练方式**：全参数微调，使用 Fully Sharded Data Parallelism (FSDP)，BF16精度。
- **训练步数**：200步。
- **框架**：VeRL 用于RL训练，vLLM 用于高效生成 rollout。
- **未明确说明**：未给出总训练时长或单个实验的耗时，但结合批量大小和步数可估算。

### 5. 实验数量与充分性
- **实验数量**：非常充分。
  - 整体性能对比（表1）：覆盖7个数据集、多种基线，使用不同模型变体（Base/Instruct）和规模（3B/7B）。
  - 搜索行为分析（图4）：搜索频率、搜索成功率随训练步数变化。
  - 知识精炼有效性（图5）：精炼步骤的召回率和token长度对比。
  - 检索深度影响（图6）：不同k值（1~7）的鲁棒性测试。
  - 关键组件消融（表2、图7）：移除检索奖励、移除精炼步骤的影响。
  - 模型规模与指标消融（表3）：3B vs 7B，EM/F1/CEM。
  - 外部精炼器对比（表4）：使用BART或Qwen模型代替精炼步骤。
  - 奖励设计对比（表8）：线性/非线性组合，奖励作用对象（文档 vs 精炼）。
  - 统计显著性（表7）：多轮实验T检验。
  - 复杂答案分析（表9）：长答案子集上不同检索奖励设计。
  - 案例研究（表10）：具体示例展示。
- **充分性评价**：实验设计全面，公平性较好（与基线使用相同数据集和检索环境），消融深入，统计显著。但缺乏在更大模型（如7B以上）或更多样化检索器上的验证。

### 6. 论文的主要结论与发现
- AutoRefine 显著超越现有方法：在7个基准上平均准确率提升6.9%（3B-Base）和6.0%（3B-Instruct），尤其在多跳任务上提升更明显（如2Wiki提升8.3%，Musique提升4.5%）。
- 模型学会动态调整搜索频率：对多跳问题使用更多搜索（~2.5次），对单跳问题使用更少（~1.2次）。
- 搜索质量高：检索成功率达到50%以上（多跳）和70%（单跳），高于对比方法。
- 精炼步骤有效：仅用100~200 token即可保留答案关键信息，而原始文档需600+ token。
- 对不同检索深度（k=1~7）鲁棒，性能稳定提升。
- 检索特定奖励对精炼质量和搜索行为有显著促进作用。

### 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次在RL训练中引入显式的知识精炼步骤和检索激励，将检索质量与最终答案关联。
- **奖励设计巧妙**：非线性的连续奖励（正确答案全分，仅检索正确给0.1，否则零分）平衡了探索与利用。
- **实验分析深入**：从搜索频率、搜索质量、精炼召回等多角度剖析模型行为，可视化展示训练动态。
- **公平比较**：与多个SOTA方法在相同设置下对比，包括复现ReSearch，使用相同检索语料和评估指标。
- **代码开源**：提供GitHub仓库便于复现。

### 8. 不足与局限
- **评估指标单一**：仅使用EM/F1，可能忽略语义等价的正确回答，不适合开放生成型任务。
- **检索环境静态**：使用固定Wikipedia快照，缺乏实时性，与实际web搜索场景有差距。
- **奖励设计依赖答案覆盖**：对长答案或复杂答案，CEM奖励可能过严；文中尝试了更细粒度的召回奖励并显示改进，但未作为默认。
- **仅测试3B和7B模型**：未见在更大模型（如13B、70B）上的验证，泛化能力待检验。
- **算力描述不够详细**：未提供训练耗时、内存需求等具体数值，可复现性细节稍显不足。
- **对比方法未覆盖最新**：如R1-Searcher、Zerosearch等同期工作未被纳入比较。
- **可能存在的偏差**：训练集仅包含NQ和HotpotQA，可能偏向于这两种数据分布；多跳数据集中部分答案较短，简化了精炼目标。

（完）
