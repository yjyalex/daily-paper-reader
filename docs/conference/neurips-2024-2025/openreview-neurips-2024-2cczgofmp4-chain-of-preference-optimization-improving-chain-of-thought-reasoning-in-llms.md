---
title: "Chain of Preference Optimization: Improving Chain-of-Thought Reasoning in LLMs"
title_zh: 链式偏好优化：改进大语言模型中的思维链推理
authors: "Xuan Zhang, Chao Du, Tianyu Pang, Qian Liu, Wei Gao, Min Lin"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=2cczgOfMP4"
tags: ["query:rl-nlplr"]
score: 9.0
evidence: 使用链式偏好优化改进思维链推理，与强化学习在逻辑推理中的应用一致
tldr: 通过思维树搜索的偏好优化微调LLM，提升思维链推理性能。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 381, \"height\": 941}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 1021, \"height\": 1093}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 328, \"height\": 1093}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 615, \"height\": 942}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 760, \"height\": 941}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 1329, \"height\": 280}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 859, \"height\": 1085}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 1331, \"height\": 280}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 1330, \"height\": 356}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-010.webp\", \"caption\": \"\", \"page\": 4, \"index\": 10, \"width\": 1124, \"height\": 1071}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-011.webp\", \"caption\": \"\", \"page\": 8, \"index\": 11, \"width\": 1177, \"height\": 955}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-012.webp\", \"caption\": \"\", \"page\": 8, \"index\": 12, \"width\": 1358, \"height\": 1117}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-013.webp\", \"caption\": \"\", \"page\": 9, \"index\": 13, \"width\": 1022, \"height\": 955}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-014.webp\", \"caption\": \"\", \"page\": 9, \"index\": 14, \"width\": 1220, \"height\": 617}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-015.webp\", \"caption\": \"\", \"page\": 9, \"index\": 15, \"width\": 1261, \"height\": 559}]"
motivation: 思维链生成的推理路径并非最优，而思维树搜索开销大。
method: 利用思维树搜索构建偏好数据，通过链式偏好优化微调LLM。
result: 微调后的LLM在推理任务上达到或超过思维树搜索的性能，且无额外推理开销。
conclusion: 偏好优化可以有效将搜索空间的知识蒸馏到思维链中。
---

## Abstract
The recent development of chain-of-thought (CoT) decoding has enabled large language models (LLMs) to generate explicit logical reasoning paths for complex problem-solving. However, research indicates that these paths are not always deliberate and optimal. The tree-of-thought (ToT) method employs tree-searching to extensively explore the reasoning space and find better reasoning paths that CoT decoding might overlook. This deliberation, however, comes at the cost of significantly increased inference complexity. In this work, we demonstrate that fine-tuning LLMs leveraging the search tree constructed by ToT allows CoT to achieve similar or better performance, thereby avoiding the substantial inference burden. This is achieved through \emph{Chain of Preference Optimization} (CPO), where LLMs are fine-tuned to align each step of the CoT reasoning paths with those of ToT using the inherent preference information in the tree-search process. Extensive experimental results show that CPO significantly improves LLM performance in solving a variety of complex problems, including question answering, fact verification, and arithmetic reasoning, demonstrating its effectiveness. 
Our code is available at [https://github.com/sail-sg/CPO](https://github.com/sail-sg/CPO).

---

## 论文详细总结（自动生成）

# 详细中文总结：Chain of Preference Optimization

## 1. 核心问题与整体含义（研究动机和背景）
- **动机**：Chain-of-Thought（CoT）推理虽然能让大语言模型（LLM）生成显式的逻辑推理路径，但研究表明这些路径往往并非最优，存在“单一路径聚焦”导致的非最优问题。Tree-of-Thought（ToT）通过树搜索探索更多推理路径，能够找到更好的解，但推理延迟显著增加（平均比CoT慢50倍以上），限制了实际应用。
- **核心问题**：能否将ToT的战略深度融入CoT，使其在保持高效的同时提升推理质量？
- **整体含义**：本文提出的Chain-of-Preference Optimization（CPO）通过利用ToT搜索树中固有的偏好信息（每步选中的thought vs. 未选中的thought）来微调LLM，使CoT在推理时能达到与ToT相当甚至更优的性能，同时避免高昂的推理开销。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：在ToT的搜索过程中，除了最终选中的推理路径外，还有大量非最优thought被生成和评估，这些thought自然地构成了偏好对（preferred vs. dispreferred）。CPO利用这些偏好信息，在每一步推理上使用Direct Preference Optimization（DPO）进行微调，使模型学会对齐ToT的偏好，从而在CoT解码时生成类似ToT的高质量路径。
- **关键技术细节**：
  - **偏好数据合成**：模仿ToT推理过程，在每个推理步骤生成k个thought，通过LLM自评估（likely=10, impossible=1）打分并剪枝保留top-n。最终选中的推理路径中的每个thought作为preferred（获胜），其兄弟节点（即同一父节点下未被选中的thought）作为dispreferred（失败），构造配对偏好数据。
  - **训练目标**：对于每一步i，给定前序状态sw_{i-1}，DPO损失函数为：
    \[
    L_i(\pi_\theta; \pi_{\text{ref}}) = -\log\sigma\left( \beta \log\frac{\pi_\theta(z^w_i|x, s^w_{i-1})}{\pi_{\text{ref}}(z^w_i|x, s^w_{i-1})} - \beta \log\frac{\pi_\theta(z^l_i|x, s^w_{i-1})}{\pi_{\text{ref}}(z^l_i|x, s^w_{i-1})} \right)
    \]
    总目标为所有步期望之和：\(L_{\text{CPO}} = \mathbb{E}_{(x,z^w_i,z^l_i,s^w_{i-1})\sim\mathcal{D}}[L_i]\)。
- **算法流程**：先通过ToT在训练集上生成包含偏好对的链式数据；然后使用DPO对LLM进行微调，优化每个推理步骤的thought生成概率；推理时采用标准CoT贪婪解码，不增加额外计算。

## 3. 实验设计
- **数据集**：覆盖三类推理任务，共7个数据集：
  - 问答（QA）：Bamboogle、WikiMultiHopQA、HotpotQA
  - 事实验证（Fact Verification）：Fever、Feverous、Vitaminc
  - 算术推理（Arithmetic Reasoning）：SVAMP
- **Benchmark/对比方法**：
  - **CoT**：标准思维链提示+贪婪解码
  - **ToT**：使用树搜索（BFS+剪枝）进行推理
  - **TS-SFT**：用ToT搜索到的完整推理路径进行监督微调（Supervised Fine-Tuning）
  - **CPO**：本文提出的链式偏好优化
- **额外对比**：与ReST、self-rewarding等方法在公平设定下进行了比较；还对比了全路径偏好优化（FPO）。
- **评估指标**：主要报告准确率（Accuracy）和平均每个实例的推理延迟（Latency）。

## 4. 资源与算力
- **GPU型号**：NVIDIA A100 GPU（未明确具体数量，但所有实验均在此类GPU上运行）。
- **训练细节**：
  - 使用LoRA（rank=8, alpha=16）进行高效微调。
  - 学习率：DPO为5e-6，SFT为1e-5；batch size（含梯度累积）为32；优化器AdamW；训练4个epoch，基于验证集早停。
  - 推理延迟：基于单张NVIDIA A100 40GB测量。
- **数据生成**：每个数据集最多使用300个训练实例生成偏好数据（平均约200个实例可产生6531个偏好对），ToT数据生成耗时较大，但仅在训练阶段执行。
- **说明**：论文未明确说明使用的GPU数量，也未给出总训练时长或总计算量估计。

## 5. 实验数量与充分性
- **实验数量**：相当丰富。包括：
  - **主要实验**：在7个数据集上，3种LLM（LLaMA2-7B/13B、Mistral-7B）与3个基线（CoT、ToT、TS-SFT）对比，共7×3×4=84个结果组合（表1）。
  - **消融实验**（图3、图4、图8）：涉及dispreferred thought选择策略、训练数据量、数据类型混合（单任务/统一QA/混合类型）、偏好构建方式（CPO vs. FPO）、迭代学习等。
  - **附加分析**：F1分数、与ReST/self-rewarding的对比、迭代学习效果等。
- **充分性与公平性**：
  - 多次重复（每个实验3次不同随机种子），报告平均结果，并标注统计显著性（p<0.01）。
  - 所有方法的训练数据数量、推理条件保持一致（如CPO与TS-SFT使用相同的ToT生成数据）。
  - 消融实验设计全面，覆盖关键组件，验证了每步偏好、dispreferred信息、数据量等的影响。
  - 不足之处：仅测试文本模型，未在视觉语言模型或更多样化任务上验证。

## 6. 主要结论与发现
- **CPO显著提升推理能力**：在三个LLM和七个数据集上，CPO相比CoT平均提升4.3%，最大提升9.7%；相比TS-SFT平均提升2.7%。
- **CPO推理高效**：推理延迟与CoT相当（平均比ToT快约57.5倍），但性能可与ToT媲美甚至更优。
- **每步偏好优于全路径偏好**：CPO使用链式偏好（per-step）比使用完整路径偏好（FPO）更有效，原因是避免了长公共前缀（LCP）梯度取消问题，FPO相对SFT反而下降。
- **dispreferred thoughts至关重要**：逐步增加dispreferred数据比例，性能持续提升；即使使用所有非选中thought作为dispreferred，不同选择策略（最低分、较低分、全选）差异不大，说明区分关键在于是否在最终选中路径内，而非中间评分。
- **迭代训练可进一步提升**：CPO经过两轮迭代，性能可再提升约4%。
- **数据量效应**：训练实例太少（<80）会导致过拟合，性能反而下降；120个实例后性能稳定提升并超过基线。

## 7. 优点
- **无需人工标注**：完全利用LLM自身生成的偏好信号，属于自监督/自改进方法。
- **训练-推理分离**：将搜索开销转移到训练阶段，推理时保持CoT的高效。
- **细粒度监督**：每步的偏好对比提供了比完整路径更丰富的梯度信号，有效避免梯度消失问题。
- **方法通用性强**：在三个不同架构的LLM（LLaMA2-7B/13B、Mistral-7B）和三类推理任务上均有效。
- **消融实验全面**：验证了多个设计选择（dispreferred选择策略、数据量、混合数据、偏好粒度等）的影响，分析深入。

## 8. 不足与局限
- **数据生成成本高**：仍需要通过ToT生成训练数据，ToT本身推理耗时很长（文中承认是time-consuming process），虽然只发生在训练阶段，但仍需大量计算资源。
- **应用范围有限**：仅测试了文本语言模型上的部分复杂推理任务（QA、事实验证、算术推理），未在视觉语言模型、更多样化任务（如翻译、软件工程）上验证。
- **潜在风险**：该方法可用于生成有害内容，存在滥用可能；虽然不依赖人工标注，但生成的数据可能存在偏见或错误。
- **迭代改进的稳定性**：在迭代学习中，当CoT与ToT性能接近时，继续训练可能导致性能收敛甚至下降，且ToT推理在微调后的模型上有时效果反而变差（推测是由于模型输出多样性降低）。
- **假设限制**：方法依赖于ToT搜索树构建的偏好，若ToT本身质量不高（如评估不准确），可能会影响CPO效果。论文未深入探讨评估噪声的影响。

（完）
