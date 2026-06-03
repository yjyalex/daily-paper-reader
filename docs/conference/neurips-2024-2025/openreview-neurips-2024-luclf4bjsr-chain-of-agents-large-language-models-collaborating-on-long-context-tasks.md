---
title: "Chain of Agents: Large Language Models Collaborating on Long-Context Tasks"
title_zh: 智能体链：大型语言模型协作处理长上下文任务
authors: "Yusen Zhang, Ruoxi Sun, Yanfei Chen, Tomas Pfister, Rui Zhang, Sercan O Arik"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=LuCLf4BJsr"
tags: ["query:mas-gen"]
score: 8.0
evidence: 通过自然语言进行多智能体协作处理长上下文任务
tldr: Chain-of-Agents利用多智能体协作进行长上下文聚合与推理。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 470}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 388}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 2893, \"height\": 1478}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 2893, \"height\": 1477}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 2893, \"height\": 1477}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 2567, \"height\": 1451}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 3823, \"height\": 1420}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 2721, \"height\": 1577}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-009.webp\", \"caption\": \"\", \"page\": 22, \"index\": 9, \"width\": 2156, \"height\": 1216}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-luclf4bjsr/fig-010.webp\", \"caption\": \"\", \"page\": 23, \"index\": 10, \"width\": 1569, \"height\": 855}]"
motivation: 现有长上下文处理方法存在信息遗漏或注意力分散问题。
method: 提出Chain-of-Agents框架，通过自然语言实现多智能体信息聚合与上下文推理。
result: 在长上下文任务上有效提升了信息聚合和推理能力。
conclusion: 多智能体协作是处理长上下文任务的有效范式。
---

## Abstract
Addressing the challenge of effectively processing long contexts has become a critical issue for Large Language Models (LLMs). Two common strategies have emerged: 1) reducing the input length, such as retrieving relevant chunks by Retrieval-Augmented Generation (RAG), and 2) expanding the context window limit of LLMs. However, both strategies have drawbacks: input reduction has no guarantee of covering the part with needed information, while window extension struggles with focusing on the pertinent information for solving the task. To mitigate these limitations, we propose Chain-of-Agents (CoA), a novel framework that harnesses multi-agent collaboration through natural language to enable information aggregation and context reasoning across various LLMs over long-context tasks. CoA consists of multiple worker agents who sequentially communicate to handle different segmented portions of the text, followed by a manager agent who synthesizes these contributions into a coherent final output. CoA processes the entire input by interleaving reading and reasoning, and it mitigates long context focus issues by assigning each agent a short context. We perform a comprehensive evaluation of CoA on a wide range of long-context tasks in question answering, summarization, and code completion, demonstrating significant improvements by up to 10% over strong baselines of RAG, Full-Context, and multi-agent LLMs.

---

## 论文详细总结（自动生成）

# 论文总结：Chain of Agents: Large Language Models Collaborating on Long-Context Tasks

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大型语言模型（LLMs）在处理长上下文任务时面临两大挑战：一是输入缩减方法（如RAG）可能遗漏关键信息；二是窗口扩展方法（如长上下文模型）难以聚焦于相关部分，存在“迷失在中间”问题。
- **整体动机**：现有方法在信息覆盖和注意力聚焦上存在权衡，而人类通过有限的工作记忆交错阅读和处理长文本。受此启发，作者提出**Chain-of-Agents (CoA)**，利用多智能体协作（自然语言沟通）实现长上下文信息的聚合与推理，旨在不扩展窗口也不缩减输入的情况下提升性能。

## 2. 论文提出的方法论
- **核心思想**：将长文本分割为多个块（chunks），每个块由一个工作智能体（Worker Agent）处理，工作智能体顺序传递“通信单元（Communication Unit, CU）”，逐步积累和推理信息；最后管理层智能体（Manager Agent）基于最终CU生成答案。
- **关键技术细节**：
  - **Stage 1（工作智能体链式通信）**：每个工作智能体接收当前块、前一个智能体的CU、查询q和任务指令，生成新的CU。CU内容因任务而异（问答中为证据，摘要中为摘要，代码补全中为代码摘要）。
  - **Stage 2（管理智能体集成与生成）**：管理智能体仅接收最后一个工作智能体的CU和查询q，生成最终答案。使用独立的管理智能体解耦分析职责。
  - **算法流程**（伪代码）：首先将输入x分块为c1...cl，初始化CU0为空，然后依次对每个块i调用LLM生成CUi，最后管理智能体使用CU_l和q生成响应。
  - **时间复杂度**：相比于Full-Context的O(n²)编码时间和O(nr)解码时间，CoA编码时间为O(nk)（k是窗口大小，k<<n），解码时间相同。RAG需额外检索开销。
- **无需训练**，任务无关，高度可解释。

## 3. 实验设计
- **数据集与场景**：涵盖9个长上下文数据集，分为三类：
  - **问答**：HotpotQA（多跳）、MuSiQue（多跳、不可回答问题）、NarrativeQA（整本书/电影）、Qasper（NLP论文）、QuALITY（故事/文章选择题）。
  - **摘要**：QMSum（查询式会议摘要）、GovReport（政府报告）、BookSum（书籍级叙事摘要）。
  - **代码补全**：RepoBench-P（GitHub仓库下一行代码）。
- **基准（Baselines）**：
  - **Vanilla**：直接截断输入至窗口大小（Full-Context）。
  - **RAG**：使用SOTA检索器获取最相关块并拼接输入。
  - **多智能体框架**：Merge（各智能体独立回答后多数投票）、Hierarchical（树结构，工作智能体独立生成CU后交给管理智能体）。
- **LLMs**：PaLM 2 (text-bison, text-unicorn)、Gemini 1.0 (gemini-ultra)、Claude 3 (haiku, sonnet, opus)。最大上下文窗口：8k（PaLM2）、32k（Gemini）、200k（Claude 3）。
- **评估指标**：ROUGE几何平均（摘要）、代码相似度（代码）、精确匹配（QuALITY）、F1（其余问答）。

## 4. 资源与算力
- **算力说明**：论文未明确具体GPU数量及训练时长。文中提到使用Vertex model garden API调用LLM，RAG检索模型在A100 GPU上运行。大部分实验为API调用推理，无需大规模训练（CoA为训练免费框架）。时间复杂性分析为理论推导，实际运行时间在HotpotQA上以llama3-8b为例进行了实测，单样本平均推理时间约3.1秒（未使用并行解码），若并行可降至1.32秒。总体算力需求较低。

## 5. 实验数量与充分性
- **实验数量**：非常充分。包括：
  - 9个数据集×6种LLM×多种基线（Vanilla、RAG、CoA）的主要对比。
  - 消融实验：移除管理智能体、改变阅读顺序（右向左、随机）、多路径增强（双向、自一致性、排列集成）。
  - 分析实验：RAG检索失败时表现、输入长度影响、Lost-in-the-Middle现象、信息损失度量、不同窗口大小鲁棒性、NIAH测试等。
- **充分性与公平性**：实验设计较为全面，覆盖多种任务类型和多代LLM。温度设为0以保证确定性，RAG的随机种子固定，确保可复现。对比了RAG和Full-Context两个主流方向，并专门构造了其他多智能体框架（Merge、Hierarchical）进行公平比较。消融实验合理，分析深入。

## 6. 论文的主要结论与发现
- **主要结论**：
  - CoA在所有9个数据集上均显著优于Vanilla和RAG基线，提升幅度高达10%。
  - 即使与具有200k窗口的Claude 3 (Vanilla)相比，CoA（仅用8k窗口）也大幅领先；输入越长，改进越明显。
  - CoA有效缓解了“迷失在中间”问题（lost-in-the-middle）。
  - 多路径增强（如双向、自一致性、排列集成）可进一步提升性能，上限（Oracle）仍有较大提升空间。
  - 信息损失（communication unit比最终输出更好的情况）仅为1-4%，说明链式通信损失可接受。
- **发现**：
  - 工作智能体的顺序协作能够完成多跳推理，而RAG因语义相似度限制难以跨越不相关的第一跳。
  - 管理智能体至关重要，移除后性能显著下降。
  - 左到右阅读顺序优于其他顺序。

## 7. 优点
- **方法论亮点**：
  - **训练免费**：无需对LLM进行额外训练，直接使用预训练模型。
  - **任务与长度无关**：适用于问答、摘要、代码补全等多种任务，且可扩展到任意长输入。
  - **高度可解释**：通信单元（CU）可作为中间推理过程进行查验。
  - **成本有效**：相比Full-Context，时间复杂性从O(n²)降至O(nk)。
- **实验设计亮点**：
  - 大规模多模型、多数据集验证，结论稳健。
  - 针对长上下文LLM（Claude 3 200k）的对比具有参考价值。
  - 消融和分析实验充分，揭示了CoA的工作机制和优势来源。

## 8. 不足与局限
- **实验覆盖**：未在更多开源LLM（如LLaMA）上验证，且部分数据集（如Qasper）性能低于训练过的专用模型。
- **偏差风险**：Prompt设计对结果敏感，不同LLM可能需调整指令；RLHF可能引入“拒绝回答”的偏差，导致推理链崩溃（catastrophic collapse）。
- **应用限制**：
  - 通信单元可能有效信息流失（1-4%），但影响可接受。
  - 未探索更丰富的通信形式（如辩论、讨论），可能存在进一步提升空间。
  - 延迟和成本：虽然理论上可行，但实际运行中CoA调用LLM次数随块数增长，可能比RAG慢（未启用并行约30%更慢）。可进一步优化（如模型路由）。
  - 仅使用相同模型作为所有智能体，未探索异构模型协作。
- **其他**：论文未开源代码和数据（但表示接收后将开源），可复现性需验证。

（完）
