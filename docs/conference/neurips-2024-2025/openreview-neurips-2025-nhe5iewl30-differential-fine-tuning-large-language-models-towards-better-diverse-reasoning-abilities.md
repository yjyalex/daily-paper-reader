---
title: Differential Fine-Tuning Large Language Models Towards Better Diverse Reasoning Abilities
title_zh: 差分微调大语言模型以提升多样化推理能力
authors: "Xiaosong Yuan, Chen Shen, Shaotian Yan, kaiyuan liu, Xiaofeng Zhang, Sinan Fan, Qingyi Meng, Liang Xie, Wenxiao Wang, Renchu Guan, Ying Wang, Jieping Ye"
date: 2025-05-10
pdf: "https://openreview.net/pdf?id=NhE5IeWl30"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 通过微调提升LLM推理能力
tldr: 差分微调增强了大模型的多项推理能力。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有微调方法在联合训练不同推理任务时产生冲突。
method: 提出差分微调框架来缓解多任务冲突并保留各自益处。
result: 新框架在多个推理任务上超越单任务微调性能。
conclusion: 差分微调有效提升大模型的多任务推理能力。
---

## Abstract
Reasoning abilities of large language models (LLMs) require explicit derivations compared to general question-answering, supervised fine-tuning (SFT) can empower multiple reasoning abilities in LLMs via learning from various datasets. However, neither training the datasets jointly (mix-up) nor continually can maintain the performance of single-dataset SFT, sometimes better while sometimes even worse, illustrating vanilla SFT can not only facilitate reasoning abilities but also introduce conflicts. In this paper, we propose a novel framework to mitigate the conflicts and preserve benefits among different reasoning tasks, and even surpass each task's single dataset SFT performance. We start by exploring the differences between reasoning fine-tuned and base LLMs by analyzing their parameter variations during model inference, and we discover that each reasoning capability has exclusive parameters that benefit itself more evidently than others. In contrast, the overlapped parameters of tasks can bring benefits or conflicts. Inspired by the findings, we propose to update the exclusive and overlapped parameters according to specific reasoning task combinations differentially, thereby avoiding unnecessary conflicts while maintaining benefits. Consistent improvements in mix-up and continual SFT experiments demonstrate that the proposed SFT strategy can achieve better performance on various LLMs (Llama3-8B, Mistral-7B, and Qwen2.5-14B) and diverse reasoning tasks with fewer conflicts, showing the superiority and generality of our analysis findings and the proposed approach.

---

## 论文详细总结（自动生成）

# 论文总结：差分微调大语言模型以提升多样化推理能力

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLM）通过监督微调（SFT）可以获得多种推理能力（如数学、代码、逻辑、常识推理）。然而，当使用多个推理数据集进行联合微调（混合训练）或连续微调（顺序训练）时，模型往往无法保持单数据集微调的性能，甚至出现相互干扰（冲突）或遗忘（灾难性遗忘）现象。部分任务组合能带来正向增益（如数学与代码），而另一些组合则导致性能下降（如逻辑与常识推理）。这表明不同推理能力之间存在复杂的“收益-冲突”关系。
- **整体含义**：本文旨在分析并缓解多任务推理微调中的冲突，同时保留或增强其相互增益。通过深入分析模型参数对推理任务的敏感性，发现每个推理能力对应一组“专属”参数，而重叠的参数可能带来收益或冲突。基于此，提出一种**差分微调（DiFT）**策略，在混合和连续微调场景下，根据任务组合动态地更新或冻结参数，从而在提升目标推理能力的同时减少对无关能力的干扰。

## 2. 方法论：核心思想与关键技术细节

### 核心思想
- 利用一种称为**delta-scale row score**的指标，量化模型每层权重矩阵中每一行（对应输出维度）对特定推理任务的重要性。通过比较微调后模型与基座模型在前向传播中输出激活的差异，定位出对某个推理能力最关键的参数行。
- 对于**混合微调（mix-up SFT）**，只更新所有涉及任务的关键参数行的**并集**中的参数，冻结其他参数，从而学习多任务能力同时避免干扰。
- 对于**连续微调（continual SFT）**，在每轮新任务训练时，只更新当前任务关键参数行中**不属于**之前任一任务关键参数行的**差集**中的参数，以保留历史知识。

### 关键技术细节
1. **delta-scale row score 计算**：
   - 对于某一线性层权重矩阵 \( W \in \mathbb{R}^{H \times D} \)，输入激活 \( X \in \mathbb{R}^{L \times D} \)，输出激活 \( Y = XW^T + b \)。
   - 对同一输入，分别用基座模型 \( M_{\text{base}} \) 和微调模型 \( M_{\text{ft}} \) 计算输出，得到第 \( k \) 个输出维度的差异 \( \Delta Y_t^k = Y_{\text{ft}}^k(t) - Y_{\text{base}}^k(t) \)。
   - 对 \( N \) 个 token 求平方差均值：\( s_k = \frac{1}{N} \sum_{t=1}^{N} \| \Delta Y_t^k \|_2^2 \)。
   - 对于每一层，取得分最高的 \( C \) 个行索引作为该任务的关键参数行集合（文中默认 \( C = 100 \)）。

2. **混合微调（Mix-up SFT）**：
   - 计算所有任务关键参数行的并集 \( \text{DSR}_{\text{union}} = \bigcup_{k=0}^{K-1} \text{DSR}_k \)。
   - 冻结基座模型中不在并集内的参数，仅更新并集中的参数，在合并数据集上微调。

3. **连续微调（Continual SFT）**：
   - 对于第 \( k \) 个任务（从 \( k=1 \) 开始），计算差集 \( \text{DSR}_{\text{diff}} = \text{DSR}_k \setminus \bigcup_{j=0}^{k-1} \text{DSR}_j \)。
   - 冻结前一步模型中除该差集外的所有参数，仅更新差集内的参数，在新任务数据集上微调。

- 算法伪代码见附录 B（Algorithm 1）。

## 3. 实验设计

### 数据集
- **训练数据**：每个推理任务采样 20000 条样本。
  - 数学：MathInstruct + 其他数学子集（共 329,254+ 条中采样）。
  - 代码：Code Bagel Hermes 及其他代码数据（共 386,649+ 条中采样）。
  - 逻辑：LogiCoT（20,000 条）。
  - 常识：CommonsenseQA、CoS-e、OpenBookQA、SocialIQA、StrategyQA、WorldTree 的混合（20,000 条）。
- **评估基准**：
  - 数学：GSM8k（0-shot 准确率）。
  - 代码：CodeXGlue（pass rate）。
  - 逻辑：LogiQA2（0-shot 准确率）。
  - 常识：CommonsenseQA（0-shot 准确率）。
  - 通用知识：MMLU（0-shot 准确率，仅作参考）。
- **主要评估指标**：**目标平均精度（ATA）**，即所涉任务的准确率或 pass rate 的平均值（代码 pass rate 乘以 50 以对齐量纲）。

### 对比方法
- **混合微调基线**：Vanilla SFT（混合训练）、DMT（双阶段混合微调）、CoBa（基于收敛平衡的多任务微调）。
- **连续微调基线**：Vanilla SFT（顺序训练）、HFT（半冻结微调）、LoTA（彩票票证自适应微调）。
- 同时与 LoRA（低秩适配）进行了对比（附录 D.3）。
- 另外设置了逆 DiFT 实验（反选关键参数行）作为消融。

### 模型
- 基座模型：Llama3-8B、Mistral-7B、Qwen2.5-14B（均为 base 版本，非 instruct 版本，以避免预训练指令微调引入的混淆）。
- 实验设置：学习率 2e-5，max length 2048，batch size 256，warm-up 0.03，weight decay 0.1，max grad norm 1.0，使用 DeepSpeed ZeRO-2。

## 4. 资源与算力

文中明确提及：
- **分析阶段**：1 张 NVIDIA A100 GPU，每组分析（7B/8B 模型）约消耗 30GB 显存、约 900 秒；14B 模型约 62GB、约 1200 秒。
- **微调阶段**：使用 8 张 A100 的服务器（一台即可完成所有实验）。
- 未报告总训练时长或 GPU 小时数，但强调资源消耗较低，属于可接受范围。

## 5. 实验数量与充分性

- **实验组数**：涵盖 3 种模型 × 多种任务组合 × 混合/连续两种场景，主要结果在表 1、表 2 中展示。
  - 混合微调：3 种组合（Math-Code、Code-Logic、Logic-CSQA）在 Llama3-8B 和 Mistral-7B 上，以及 Qwen2.5-14B 上的部分组合。
  - 连续微调：2 种组合（Math-Code、Math-Logic）及不同顺序（Code-Math、Logic-Math）。
  - 此外，还有多任务（3任务、4任务）的实验（图3）、不同 delta-scale 行数的消融（图8）、逆 DiFT 消融（表3）、LoRA 对比（表 D.3）、以及 Qwen2.5-14B 的带置信区间结果（表 C）。
- **充分性评价**：
  - **充分**：覆盖了不同规模模型（7B/8B/14B）、多种推理任务组合、混合与连续两种主流场景，并进行了足够多的消融和基线对比。
  - **客观公平**：所有基线使用相同超参数，实验种子固定（seed=42），评估采用标准开源库 lm-evaluation-harness，结果稳定可复现。作者也报告了 Qwen2.5-14B 的统计误差（±0.2~0.5），表明波动较小。

## 6. 主要结论与发现

1. **不同推理能力之间存在非对称的收益与冲突**：数学与代码可相互增益，而逻辑与常识更容易冲突。
2. **每个推理能力对应一组关键参数行**：通过 delta-scale row 分析，可定位出对某推理任务高度敏感的权重行，且这些行在不同随机种子下保持稳定。
3. **DiFT 策略有效**：
   - 在混合微调中，DiFT 在绝大多数组合下超越了 Vanilla SFT 及 DMT、CoBa 等基线，ATA 提升 1-4%。
   - 在连续微调中，DiFT 能更好保留历史任务能力，同时学习新任务，ATA 提升多达 1-3%，优于 HFT 和 LoTA。
   - 在 14B 模型上同样有效，验证了可扩展性。
4. **逆 DiFT 证实关键参数行的必要性**：若冻结关键行、更新其他参数，则目标能力严重下降，甚至模型崩溃，说明所识别的参数对于推理能力不可或缺。

## 7. 优点

- **方法新颖**：从参数敏感性角度出发，揭示不同推理能力的参数差异，并提出精细化冻结策略，概念清晰，可解释性强。
- **实验设计全面**：不仅涵盖两种常见微调场景，还做了不同顺序、不同数量参数行、逆实验等消融，结果可靠。
- **计算开销低**：分析阶段仅需少量前向传播，资源需求远小于训练成本。
- **通用性强**：在三种不同架构的 LLM 上均取得一致改进，且对 14B 模型有效。
- **对比公平**：所有基线使用相同超参数和评估流程，避免 hidden 优势。

## 8. 不足与局限

- **缺乏理论证明**：delta-scale row 的选取基于启发式经验，没有严格的理论支撑为何“高分行”必然代表任务关键参数。
- **模型规模限制**：仅验证到 14B 参数，未在 30B+ 甚至 70B+ 模型上实验，可扩展性在更大模型上待验证。
- **连续微调中灾难性遗忘未完全解决**：DiFT 缓解了冲突，但对灾难性遗忘的改善有限（例如 Continual-Math-Logic 在 Llama3-8B 上仍然表现很差），作者也承认这一点。
- **仅关注推理任务**：未对通用能力（如对话、指令跟随）进行测试，可能对非推理能力的干扰未知。
- **计算资源描述不够精确**：仅给出了硬件配置，但未报告总 GPU 小时数，难以精确复现训练成本。
- **开放性问题**：delta-scale row 数量 C 的选取（默认 100）通过消融实验确定，但未讨论是否对所有模型和任务最优；且分析仅在 base 模型上进行，instruct 模型是否适用未验证。

（完）
