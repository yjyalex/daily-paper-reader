---
title: "LogicTree: Improving Complex Reasoning of LLMs via Instantiated Multi-step Synthetic Logical Data"
title_zh: LogicTree：通过实例化的多步合成逻辑数据提升LLM复杂推理
authors: "Zehao Wang, Lin Yang, Jie Wang, Kehan Wang, Hanzhu Chen, Bin Wang, Jianye HAO, Defu Lian, Bin Li, Enhong Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=z4AMrCOetn"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 通过合成数据提升复杂逻辑推理能力
tldr: 提出LogicTree框架合成多步逻辑推理数据以提升LLM推理。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-z4amrcoetn/fig-001.webp\", \"caption\": \"\", \"page\": 26, \"index\": 1, \"width\": 1095, \"height\": 746}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-z4amrcoetn/fig-002.webp\", \"caption\": \"\", \"page\": 29, \"index\": 2, \"width\": 720, \"height\": 405}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-z4amrcoetn/fig-003.webp\", \"caption\": \"\", \"page\": 29, \"index\": 3, \"width\": 720, \"height\": 405}]"
motivation: 现有逻辑推理数据集模板化，不适应真实场景。
method: 通过迭代搜索适用逻辑规则合成多样化的多步推理数据。
result: 合成数据集显著提升LLM在复杂逻辑推理任务上的性能。
conclusion: 多样化的合成逻辑数据是增强LLM推理能力的有效途径。
---

## Abstract
Despite their remarkable performance on various tasks, Large Language Models (LLMs) still struggle with logical reasoning, particularly in complex and multi-step reasoning processes. 
Among various efforts to enhance LLMs' reasoning capabilities, synthesizing large-scale, high-quality logical reasoning datasets has emerged as a promising direction. 
However, existing methods often rely on predefined templates for logical reasoning data generation, limiting their adaptability to real-world scenarios. 
To address the limitation, we propose **LogicTree**, a novel framework for efficiently synthesizing multi-step logical reasoning dataset that excels in both complexity and instantiation.
By iteratively searching for applicable logic rules based on structural pattern matching to perform backward deduction, **LogicTree** constructs multi-step logic trees that capture complex reasoning patterns. 
Furthermore, we employ a two-stage LLM-based approach to instantiate various real-world scenarios for each logic tree, generating consistent real-world reasoning processes that carry contextual significance.   This helps LLMs develop generalizable logical reasoning abilities across diverse scenarios rather than merely memorizing templates.
Experiments on multiple benchmarks demonstrate that our approach achieves an average improvement of 9.4\% in accuracy on complex logical reasoning tasks.

---

## 论文详细总结（自动生成）

# LogicTree：通过实例化的多步合成逻辑数据提升LLM复杂推理——论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大型语言模型（LLM）在复杂、多步逻辑推理任务上表现不佳，现有合成逻辑推理数据集的方法过度依赖预定义模板，缺乏对真实世界场景的适应性和推理复杂度的刻画。
- **研究动机**：高质量的推理数据是提升LLM推理能力的关键，但手动构建成本高、规模小，而现有合成方法生成的推理模式简单、场景脱离现实，导致模型容易记忆模板而非习得通用推理能力。
- **整体贡献**：提出 **LogicTree** 框架，能够高效合成兼具**复杂性**和**实例化**的多步逻辑推理数据集，帮助LLM获得可泛化的逻辑推理能力。

## 2. 论文提出的方法论

### 核心思想
- 基于一阶逻辑规则（FOL）和命题逻辑，通过**结构模式匹配**实现**向后演绎**，递归构建多步逻辑推理树；然后利用LLM的两阶段提示将符号树实例化为具有上下文意义的自然语言推理问题与推理过程。

### 关键技术细节
1. **逻辑推理树生成**（Algorithm 1）：
   - 从随机生成的根公式（结论）出发，迭代执行以下步骤：
     - 随机选择一个叶子节点；
     - 基于该叶子节点的**抽象语法树（AST）** 与规则结论的AST进行**结构模式匹配**（而非完全匹配），筛选出可应用的逻辑规则；
     - 应用规则进行向后演绎，将新推导出的前提作为子节点加入树中；
     - 重复直到达到预设迭代次数（深度2~15），保证树的多样性和复杂度（包含190种逻辑规则，融合命题逻辑和一阶逻辑）。
2. **推理场景实例化**（两阶段LLM方法）：
   - **阶段一（逻辑树实例化）**：将逻辑树的所有叶子节点公式输入LLM，要求为每个原子公式赋予具体、语义一致的真实世界语句（如“政府发起了全国疫苗接种运动”），并确保它们之间的逻辑关系保持成立。
   - **阶段二（推理过程翻译）**：将中间节点按深度降序排列得到符号推理链，引导LLM在该场景下逐步骤翻译为自然语言推理过程，LLM仅需进行符号到文本的映射，避免自主推理引入错误。
3. **后处理与验证**：
   - 要求LLM将实例化后的自然语言语句与对应的逻辑表达式配对输出（如“语句[表达式]”），通过精确字符串比较过滤不一致数据（过滤率8.73%），最终获得约13.8k高质量推理实例。

## 3. 实验设计

### 使用的数据集/场景
- **推理基准（6个）**：
  - LogicBench（单规则评估）
  - LogiQA2.0（人类考试逻辑题）
  - FOLIO（一阶逻辑推理）
  - BBH-Logic（大模型硬基准中三个逻辑子任务）
  - AGIEval (LAST-LR / LAST-AR)（LSAT法律推理）
  - Multi-LogiEval（按推理深度d1~d5分层评估）
- **泛化实验（6个域）**：ProofWriter（逻辑）、MathQA（数学）、GPQA（科学）、HumanEval（代码）、CommonsenseQA（常识）、MNLI（自然语言推理）

### 对比方法
- Vanilla（未训练）
- PARARULE（基于模板的演绎数据合成）
- LogicAsker（原子逻辑规则评估与微调）
- FLD×2（多步规则随机组合合成）

### 骨干模型
- **小规模**：Llama-3.1-8B, Qwen2.5-7B/1.5B/3B, Mistral-7B-v0.3, DeepSeek-R1-Distill系列（7B/8B）
- **大规模**：Llama-3.1-70B（使用LoRA微调）

## 4. 资源与算力
- 论文在附录B.2中给出了训练超参数（学习率、批次大小、epoch数、精度），并提到使用DeepSpeed梯度检查点和BF16精度优化显存。
- **未明确说明**使用的GPU型号、数量以及具体训练时长。仅可推算：最大模型70B使用LoRA，小模型全微调，整体算力需求中等，但缺乏详细披露。

## 5. 实验数量与充分性

### 实验数量
- **主实验**（Table 1）：4个骨干模型 × 6个基准，每个结果报告均值与标准差。
- **多步推理实验**（Figure 2, Table 7）：在Multi-LogiEval上按d1~d5分层比较，覆盖8B/70B模型。
- **泛化实验**（Table 2）：Llama-3.1-8B在6个领域任务上测试。
- **消融实验**（Table 3）：独立分析实例化场景、推理过程、多样性三个组件的贡献，并考察不同实例化数量（1~5）的影响。
- **额外模型实验**（Table 5, 6）：包括DeepSeek-R1-Distill系列及Qwen2.5小模型（1.5B, 3B）。
- **数据质量评估**（Table 8）：由GPT-4o和DeepSeek-R1评估合成数据的逻辑一致性（>96%）。
- **多样性统计**（Figure 4）：覆盖50+主题的场景分布。

### 充分性与公平性
- 实验覆盖了从1.5B到70B的多种模型家族（Llama, Qwen, Mistral, DeepSeek），且后两个为最新推理蒸馏模型。
- 对比方法均为近年有代表性的合成逻辑数据工作，并在相同训练设置下比较。
- 所有结果报告标准差，并在消融中严格分离变量。
- **总体充分、客观、公平**。

## 6. 论文的主要结论与发现
1. **LogicTree显著提升LLM的复杂逻辑推理能力**：在6个基准上平均提升9.4%（Llama-3.1-8B），且在BBH-Logic等困难任务上提升高达13.9%。
2. **在多步推理上优势尤为突出**：在Multi-LogiEval的d3~d5深度任务上，LogicTree准确率远高于所有基线，且随深度增加性能下降幅度最小。
3. **实例化场景和多样性是关键**：消融实验表明，移除真实世界实例化或降低场景多样性会导致性能大幅下降（5~8个百分点）；当只使用一个场景时，模型甚至出现过拟合。
4. **泛化能力增强**：除了逻辑任务，在数学（MathQA↑5.2%）、代码（HumanEval↑6.2%）等领域也观察到提升，表明模型习得的是通用推理技能而非表面模式。

## 7. 优点
（1）**方法论创新**：
- 首次将结构模式匹配的向后演绎用于生成多步逻辑树，支持一阶逻辑与命题逻辑的混合使用，突破了模板化规则拼接的限制。
- 两阶段LLM实例化策略（先赋实体再翻译）有效避免了LLM自主推理的错误，同时保证了上下文语义连贯性。

（2）**实验设计严谨**：
- 覆盖多种模型规模和家族，包含当前最强推理模型（DeepSeek-R1-Distill）。
- 多步推理深度分析（d1~d5）直接验证了方法对复杂推理的核心价值。
- 消融实验系统定量分析了三个关键组件。

（3）**数据质量保障**：
- 自动过滤机制（8.73%无效数据）结合LLM验证，使合成数据的逻辑一致性达96%以上。
- 场景多样性控制（50+主题）有效防止模型记忆特定事实。

## 8. 不足与局限

### 实验覆盖方面
- **未与其他推理范式结合**：论文指出当前方法仅关注形式逻辑，尚未纳入常识推理、因果推理等，限制了综合推理能力的提升（论文“Limitations”部分明确提及）。
- **知识密集型任务提升较小**：在CommonsenseQA和GPQA（科学）上增益有限，因为LogicTree主要强化推理泛化而非知识记忆。

### 偏差风险
- 实例化过程依赖LLM（GPT-4），可能引入语言模型自身的社会文化偏见或事实错误，尽管逻辑一致性验证可部分缓解，但无法完全消除。
- 合成数据主题由人工设定的50+主题驱动，可能仍存在主题覆盖不均衡问题。

### 应用限制
- 训练参数（如树深度、规则数量）预设为固定范围（2~15），在不同应用场景下可能需要重新调整。
- 生成成本：虽然论文声称比搜索式方法更低（附录C.1表9），但每次实例化仍需多次调用LLM，对于大规模数据生成仍有计算开销。
- 未在商业闭源模型（如GPT-4）上进行微调验证，因此结论主要适用于开源模型。

（完）
