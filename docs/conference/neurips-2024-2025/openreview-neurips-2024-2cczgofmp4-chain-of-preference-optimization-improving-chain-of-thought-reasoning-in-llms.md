---
title: "Chain of Preference Optimization: Improving Chain-of-Thought Reasoning in LLMs"
title_zh: 偏好优化链：提升大语言模型思维链推理
authors: "Xuan Zhang, Chao Du, Tianyu Pang, Qian Liu, Wei Gao, Min Lin"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=2cczgOfMP4"
tags: ["query:cot-unfaith"]
score: 8.0
evidence: 使用偏好优化提升思维链推理
tldr: 利用树状思维搜索数据通过偏好优化微调LLM，使标准思维链达到树搜索性能且推理成本低。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 381, \"height\": 941}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 1021, \"height\": 1093}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 328, \"height\": 1093}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 615, \"height\": 942}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 760, \"height\": 941}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 1329, \"height\": 280}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 859, \"height\": 1085}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 1331, \"height\": 280}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 1330, \"height\": 356}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-010.webp\", \"caption\": \"\", \"page\": 4, \"index\": 10, \"width\": 1124, \"height\": 1071}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-011.webp\", \"caption\": \"\", \"page\": 8, \"index\": 11, \"width\": 1177, \"height\": 955}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-012.webp\", \"caption\": \"\", \"page\": 8, \"index\": 12, \"width\": 1358, \"height\": 1117}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-013.webp\", \"caption\": \"\", \"page\": 9, \"index\": 13, \"width\": 1022, \"height\": 955}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-014.webp\", \"caption\": \"\", \"page\": 9, \"index\": 14, \"width\": 1220, \"height\": 617}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-2cczgofmp4/fig-015.webp\", \"caption\": \"\", \"page\": 9, \"index\": 15, \"width\": 1261, \"height\": 559}]"
motivation: 思维链推理路径可能不是最优，而树搜索推理成本高。
method: 使用树状思维搜索构建偏好数据，通过偏好优化微调语言模型。
result: 微调后标准思维链在推理任务上性能接近树搜索，且推理成本大幅降低。
conclusion: 偏好优化微调可以有效提升思维链推理质量并避免推理时的高开销。
---

## Abstract
The recent development of chain-of-thought (CoT) decoding has enabled large language models (LLMs) to generate explicit logical reasoning paths for complex problem-solving. However, research indicates that these paths are not always deliberate and optimal. The tree-of-thought (ToT) method employs tree-searching to extensively explore the reasoning space and find better reasoning paths that CoT decoding might overlook. This deliberation, however, comes at the cost of significantly increased inference complexity. In this work, we demonstrate that fine-tuning LLMs leveraging the search tree constructed by ToT allows CoT to achieve similar or better performance, thereby avoiding the substantial inference burden. This is achieved through \emph{Chain of Preference Optimization} (CPO), where LLMs are fine-tuned to align each step of the CoT reasoning paths with those of ToT using the inherent preference information in the tree-search process. Extensive experimental results show that CPO significantly improves LLM performance in solving a variety of complex problems, including question answering, fact verification, and arithmetic reasoning, demonstrating its effectiveness. 
Our code is available at [https://github.com/sail-sg/CPO](https://github.com/sail-sg/CPO).

---

## 论文详细总结（自动生成）

# 论文总结：Chain of Preference Optimization (CPO)

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大语言模型（LLM）使用链式思维（CoT）推理时，生成的推理路径往往不是最优的，可能忽略更好的路径。树状思维（ToT）通过树搜索探索多个推理路径，能获得更优的推理质量，但代价是推理延迟显著增加（平均慢50倍以上）。
- **目标**：如何在不增加推理成本的前提下，使标准CoT达到类似ToT的推理性能？
- **整体含义**：本文提出一种微调方法**Chain of Preference Optimization (CPO)**，利用ToT搜索过程中固有的**偏好信息**（每个推理步骤中被选中的“优先”思考与未被选中的“非优先”思考）来训练LLM，使CoT在推理时能像ToT一样选择更好的路径，同时保持CoT的低延迟。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：ToT在推理时生成多个候选思考（thought），并基于自我评估进行剪枝，最终选择一条最优路径。这一过程自然产生了**每步的偏好数据**：被选入最终路径的思考视为“优先”（preferred），其同层其他未被选中的思考视为“非优先”（dispreferred）。CPO利用这些每步偏好数据，通过直接偏好优化（DPO）对LLM进行微调，使其在CoT解码时倾向于生成优先思考。
- **关键技术细节**：
    - **偏好数据构建**：使用ToT（BFS+剪枝）对每个问题生成搜索树，在树中找到最终选择的路径。对于路径中的每个步骤 \(z_i^w\)，以其父状态 \(s_{i-1}^w\) 下的所有非路径子节点作为非优先思考 \(z_i^l\)，形成偏好对 \((z_i^w, z_i^l)\)。
    - **状态评估**：使用LLM自身对每个候选思考进行自我评分（likely=10, impossible=1），无需外部奖励模型或人工标注。
    - **微调目标**：采用DPO损失函数，针对每步偏好对进行优化，损失函数为：
      \[
      \mathcal{L}_i = -\log \sigma\left( \beta \log \frac{\pi_\theta(z_i^w | x, s_{i-1}^w)}{\pi_{\text{ref}}(z_i^w | x, s_{i-1}^w)} - \beta \log \frac{\pi_\theta(z_i^l | x, s_{i-1}^w)}{\pi_{\text{ref}}(z_i^l | x, s_{i-1}^w)} \right)
      \]
      其中 \(\pi_\theta\) 是当前模型，\(\pi_{\text{ref}}\) 是参考模型，\(\beta\) 是正则化系数。
    - **训练后推理**：微调后的LLM使用标准CoT（greedy decoding）进行推理，无需树搜索。
- **与已有方法的区别**：相比TS-SFT（仅用路径做监督微调），CPO利用了每步的偏好对，避免了长公共前缀（LCP）梯度抵消问题（如FPO所示），能更好地优化所有步骤。

## 3. 实验设计
- **数据集与场景**：三类推理任务共7个数据集：
    - **问答（QA）**：Bamboogle、WikiMultiHopQA、HotpotQA。
    - **事实验证（Fact Verification）**：FEVER、FEVEROUS、VitaminC。
    - **算术推理（Arithmetic Reasoning）**：SVAMP。
- **Benchmark与对比方法**：
    - **CoT**：标准链式思维提示（greedy decoding）。
    - **ToT**：树状思维（BFS+剪枝），推理时需要大量生成和评估。
    - **TS-SFT**：使用ToT选择的完整路径进行监督微调（基线）。
    - **CPO**（本文方法）。
- **额外对比**：还对比了Full-path Preference Optimization (FPO，仅在全路径上做DPO)、ReST、self-rewarding等方法（见附录B）。
- **评估指标**：准确率（Accuracy）和每实例平均延迟（Latency）。

## 4. 资源与算力
- **GPU型号**：NVIDIA A100 GPU（显存40GB）。
- **训练细节**：使用LoRA（rank=8, alpha=16）进行高效微调，batch_size（含梯度累积）为32，优化器AdamW，学习率DPO为5e-6、SFT为1e-5，训练4个epoch，早停基于验证集。但论文**未明确说明使用的GPU数量、训练总时长**，仅提到所有实验均在单张A100 40GB上完成延迟测试。偏好数据构建使用了ToT（耗时），但未量化总体生成耗时。

## 5. 实验数量与充分性
- **实验组数**：大量实验，包括：
    - **主实验**：7个数据集 × 3种模型（LLaMA2-7B、LLaMA2-13B、Mistral-7B） × 4种方法（CoT、ToT、TS-SFT、CPO），共 7×3×4=84 个配置，并在表1中报告了平均结果（统计显著性检验，p<0.01）。
    - **消融实验**：4组（图3a-d，图6-8），涉及 dispreferred 选择策略、训练数据量、数据混合、偏好比例、迭代学习等。
    - **对比分析**：FPO、ReST、self-rewarding（附录B表3、表4）。
    - **F1分数**：附加QA任务的F1得分（附录B表5）。
- **充分性与公平性**：
    - **充分**：覆盖多种任务类型、模型规模，消融全面。
    - **公平**：所有数据生成使用同一ToT实现；对比方法设置一致（如TS-SFT使用与CPO相同的路径，但仅用SFT；FPO使用相同路径构造完整路径对）。
    - **局限性**：每个数据集测试样本限制为最多300个（因ToT计算成本高），训练样本也类似，可能影响泛化结论。

## 6. 主要结论与发现
- **CPO显著提升CoT推理性能**：平均准确率提升4.3%（最大9.7%），且推理延迟与原始CoT相当（低至ToT的1/57.5）。
- **CPO性能接近甚至超越ToT**：在多个数据集上，CPO微调后的CoT准确率与ToT相当或更优（表1）。
- **每步偏好数据优于全路径偏好**：FPO因LCP梯度抵消效果差，而CPO的每步优化可避免此问题（图3b）。
- **dispreferred数据有用**：随着利用的dispreferred数据比例增加，性能持续提升（图3d）。
- **少量实例即可有效**：约120个训练实例即可达到较好效果，更多数据后性能收敛（图3c）。
- **混合数据类型训练效果更好**：使用多种任务数据联合训练比单一任务数据提升更大（表2）。
- **迭代学习可进一步改善**：连续多轮CPO训练可带来额外提升（附录B表3）。

## 7. 优点
- **方法创新**：首次将ToT搜索过程中的每步偏好信息用于微调，充分利用了“非最优”思考的监督信号。
- **高效推理**：训练后维持CoT的低延迟，避免了ToT的巨大推理开销（57.5倍加速）。
- **无人工标注**：所有偏好数据由LLM自我评估生成，无需人类反馈或外部奖励模型。
- **避免梯度抵消问题**：每步优化策略有效解决了全路径DPO中长公共前缀梯度抵消的问题（理论分析清晰）。
- **广泛的实验验证**：在7个数据集、3种LLM上进行了充分对比和消融，结果详实。

## 8. 不足与局限
- **数据生成仍耗资源**：构建偏好数据仍需运行ToT，该过程本身计算量较大（论文提到需平衡效率和效果，每个数据集限制在300实例）。
- **任务覆盖有限**：仅涉及问答、事实验证、算术三类文本任务，未测试视觉-语言模型、分类、翻译等其他场景。
- **模型规模限制**：主要使用7B-13B模型，未验证在更大模型（如70B）上的效果和扩展性。
- **潜在偏差风险**：自我评估引入的偏差（LLM自身打分的可靠性）未深入探讨；搜索树构建参数（如k=10, n=5）未进行系统调优。
- **应用限制**：当前方法仅适用于可分解为多步推理的离散任务，对于开放式生成或非链式任务可能不适用。
- **伦理考虑**：论文指出方法可被恶意使用（如生成不当内容），但未提出具体防护措施。

（完）
