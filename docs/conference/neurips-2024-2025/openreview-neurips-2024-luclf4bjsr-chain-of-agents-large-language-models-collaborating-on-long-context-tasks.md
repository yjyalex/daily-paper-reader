---
title: "Chain of Agents: Large Language Models Collaborating on Long-Context Tasks"
title_zh: 智能体链：大语言模型协作处理长上下文任务
authors: "Yusen Zhang, Ruoxi Sun, Yanfei Chen, Tomas Pfister, Rui Zhang, Sercan O Arik"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=LuCLf4BJsr"
tags: ["query:mas-gen"]
score: 8.0
evidence: 多智能体协作处理长上下文任务
tldr: 提出智能体链框架实现多智能体协作处理长上下文任务。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 470}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 388}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 2893, \"height\": 1478}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 2893, \"height\": 1477}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 2893, \"height\": 1477}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 2567, \"height\": 1451}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 3823, \"height\": 1420}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 2721, \"height\": 1577}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-009.webp\", \"caption\": \"\", \"page\": 22, \"index\": 9, \"width\": 2156, \"height\": 1216}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-010.webp\", \"caption\": \"\", \"page\": 23, \"index\": 10, \"width\": 1569, \"height\": 855}]"
motivation: 现有方法无法在长上下文中平衡信息覆盖与聚焦。
method: 通过自然语言实现多智能体信息聚合与上下文推理。
result: CoA在长上下文任务上表现优异。
conclusion: 多智能体协作有效解决了长上下文处理难题。
---

## Abstract
Addressing the challenge of effectively processing long contexts has become a critical issue for Large Language Models (LLMs). Two common strategies have emerged: 1) reducing the input length, such as retrieving relevant chunks by Retrieval-Augmented Generation (RAG), and 2) expanding the context window limit of LLMs. However, both strategies have drawbacks: input reduction has no guarantee of covering the part with needed information, while window extension struggles with focusing on the pertinent information for solving the task. To mitigate these limitations, we propose Chain-of-Agents (CoA), a novel framework that harnesses multi-agent collaboration through natural language to enable information aggregation and context reasoning across various LLMs over long-context tasks. CoA consists of multiple worker agents who sequentially communicate to handle different segmented portions of the text, followed by a manager agent who synthesizes these contributions into a coherent final output. CoA processes the entire input by interleaving reading and reasoning, and it mitigates long context focus issues by assigning each agent a short context. We perform a comprehensive evaluation of CoA on a wide range of long-context tasks in question answering, summarization, and code completion, demonstrating significant improvements by up to 10% over strong baselines of RAG, Full-Context, and multi-agent LLMs.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大语言模型（LLMs）在处理长上下文任务时面临两大挑战：一是通过输入缩减（如RAG）可能丢失关键信息，导致覆盖不全；二是通过扩展上下文窗口（如Claude 3的200k token）会导致模型难以聚焦于任务所需信息，出现“lost-in-the-middle”等问题。
- **研究动机**：受人类在有限工作记忆下交错阅读与处理长文本的启发，提出一种通过多智能体协作来聚合信息和推理的框架，旨在无需训练、任务无关的前提下提升长上下文任务性能。
- **整体含义**：提出Chain-of-Agents（CoA）框架，利用多个工作智能体（worker agents）顺序处理文本不同分段，并通过自然语言通信传递信息，最后由管理智能体（manager agent）综合生成最终输出。该方法同时解决了输入缩减和窗口扩展的局限性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将长文分成多个chunk，每个chunk分配给一个工作智能体。工作智能体按顺序读取各自chunk并接收前一智能体的通信单元（Communication Unit, CU），输出更新后的CU。最后一轮CU传递给管理智能体，由其生成最终答案。
- **关键技术细节**：
  - **Stage 1: Worker Agent - Segment Comprehension and Chain-Communication**：每个工作智能体W_i输入当前chunk c_i、查询q、任务指令I_W以及前一智能体的CU_{i-1}，输出CU_i。通信是单向的，从W_i传递到W_{i+1}。
  - **Stage 2: Manager Agent - Information Integration and Response Generation**：管理智能体M接收最后一个工作智能体的CU_l和查询q，输出最终响应Response。
  - 公式化表示：
    - CU_i = LLM_Wi( I_W, CU_{i-1}, c_i, q )
    - Response = LLM_M( I_M, CU_l, q )
  - **算法流程**（Algorithm 1）：
    1. 将源文本x按窗口大小k分割成l个chunk。
    2. 初始化CU_0为空字符串。
    3. 对i=1到l，计算CU_i。
    4. 返回管理智能体基于CU_l和q生成的答案。
- **时间复杂度**：CoA编码复杂度为O(nk)，解码复杂度为O(nr)，相比Full-Context的O(n²)编码效率更高，与RAG相近。

## 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法

- **数据集**：涵盖三类任务共9个数据集。
  - **问答（QA）**：HotpotQA、MuSiQue、NarrativeQA、Qasper、QuALITY（来自LongBench和SCROLLS）。
  - **摘要**：QMSum（查询式摘要）、GovReport（通用摘要）、BookSum（长篇叙事摘要）。
  - **代码补全**：RepoBench-P（仓库级代码补全）。
- **Benchmark**：标准评价指标——QA用F1 score（QuALITY用Exact Match），摘要用ROUGE几何平均，代码补全用代码相似度。
- **对比方法**：
  - **Vanilla**：直接截断或使用完整上下文（根据模型窗口限制）。
  - **RAG**：用SOTA检索器重排序后取top-n chunks输入LLM。
  - **其他多智能体框架**：Merge（多数投票）、Hierarchical（树形结构，工人间不通信）。
  - 使用6种LLM作为骨干：PaLM 2 (text-bison, text-unicorn)、Gemini 1.0 (gemini-ultra)、Claude 3 (haiku, sonnet, opus)。

## 4. 资源与算力

- **文中未明确说明具体GPU型号、数量及训练时长**。论文强调了CoA是training-free框架，无需额外训练。实验通过API调用预训练模型完成。时间分析部分仅给出理论复杂度，并在附录A中证明效率。未提及实际硬件配置。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验（Table 4 & Table 5）：在9个数据集上，对3种模型（PaLM 2、Gemini、Claude 3）分别对比Vanilla和RAG，总计数十组结果。
  - 消融实验（Table 7）：
    - 移除Manager（w/o Manager）对比。
    - 不同阅读顺序（Left-to-Right, Right-to-Left, Permutation）。
  - 多路径增强（Table 8）：双向、自一致性、排列路径，用投票/LLM法官/最优选择。
  - 信息损失分析（Table 10）。
  - 实际时间分析（Table 9）。
  - 鲁棒性实验（Fig 6）：不同窗口大小对性能影响。
  - NIAH测试（Fig 7）。
- **充分性与公平性**：实验覆盖多个任务类型和模型，基线与CoA使用相同骨干模型，控制变量。RAG使用了SOTA检索器。所有基线均为强基线（Vanilla使用完整上下文或长窗口模型）。实验设计客观，结果具有统计意义。但未提供误差棒等统计显著性指标。

## 6. 论文的主要结论与发现

- CoA在所有9个数据集上均显著优于Vanilla和RAG，平均提升可达10%以上。
- 在长上下文LLM（Claude 3 200k）上，CoA（仅8k窗口）仍能大幅超越Vanilla（200k），且随着输入长度增加，优势更明显。
- CoA能有效缓解“lost-in-the-middle”问题，性能波动更小。
- 多智能体协作（链式通信）优于并行或树形结构，因为链式通信允许信息跨chunk流动，支持多跳推理。
- 移除Manager或改变阅读顺序会降低性能，说明自然阅读顺序和综合管理的重要性。
- 多路径（如自一致性）能进一步提升性能，但仍有较大提升空间（oracle远高于当前集成方法）。
- 信息损失仅为1%-4%，表明链式通信有效保留了关键信息。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 无需训练，即插即用，任务无关。
  - 高度可解释：CU可追溯，工人智能体输出可读。
  - 成本效率：编码复杂度低于Full-Context，且可通过并行解码进一步降低延迟。
  - 自然模拟人类认知过程，交错阅读与推理。
- **实验亮点**：
  - 覆盖三种不同类型的长上下文任务（QA、摘要、代码），泛化性强。
  - 对比了多种基线（包括长窗口模型、RAG、多种多智能体设计），全面公平。
  - 消融实验和鲁棒性分析深入，验证了各组件必要性。
  - 使用多种LLM（PaLM 2, Gemini, Claude 3）确保结果不依赖于单一模型。
  - 专门分析了“lost-in-the-middle”现象和NIAH测试，证明CoA的实际收益。

## 8. 不足与局限

- **实验覆盖**：未涵盖所有长上下文任务（如多模态长文本、长视频理解等），仅限纯文本。
- **偏差风险**：所有实验使用英文数据集，未测试其他语言；模型均为闭源商业模型，可能引入未知偏见。
- **应用限制**：
  - 需要多次API调用，导致延迟和成本高于简单截断模型，尽管理论效率高。
  - 通信单元可能引入信息损失（虽有控制但无法完全消除）。
  - 对工人智能体的prompt设计敏感，不恰当prompt可能导致推理链崩溃（catastrophic collapse，如模型重复“不知道”）。
  - 当前未探索更复杂的通信形式（如辩论、讨论），可能错过进一步提升机会。
- **可重复性**：代码和数据未公开（论文承诺接受后开放），但提供了详细算法和prompt模板，理论上可复现。
- **统计显著性**：未提供误差棒或置信区间，可能因模型输出确定（temperature=0），但不同随机种子结果可能波动。

（完）
