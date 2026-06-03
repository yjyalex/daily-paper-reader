---
title: Self-Verifying Reflection Helps Transformers with CoT Reasoning
title_zh: 自我验证反思帮助Transformer进行思维链推理
authors: "Zhongwei Yu, Wannian Xia, Xue Yan, Bo XU, Haifeng Zhang, Yali Du, Jun Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=4MvqmXnCEr"
tags: ["query:cot-unfaith"]
score: 9.0
evidence: 思维链推理中的自我验证反思
tldr: 提出一个最小的框架来研究思维链中的自我验证反思，证明当验证错误有界时性能提升。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有研究表明LLM在思维链中检测错误能力有限，但反思如何提升性能尚不明确。
method: 构建了一个不带自然语言的最小推理框架，支持基本的自我验证反思，并提供了理论保证。
result: 实验证明小型Transformer在满足条件时通过自我验证反思可提升推理准确性。
conclusion: 自我验证反思在误差有界时能保证推理改进。
---

## Abstract
Advanced large language models (LLMs) frequently reflect in reasoning chain-of-thoughts (CoTs), where they self-verify the correctness of current solutions and explore alternatives. However, given recent findings that LLMs detect limited errors in CoTs, how reflection contributes to empirical improvements remains unclear. To analyze this issue, in this paper, we present a minimalistic reasoning framework to support basic self-verifying reflection for small transformers without natural language, which ensures analytic clarity and reduces the cost of comprehensive experiments. Theoretically, we prove that self-verifying reflection guarantees improvements if verification errors are properly bounded. Experimentally, we show that tiny transformers, with only a few million parameters, benefit from self-verification in both training and reflective execution, reaching remarkable LLM-level performance in integer multiplication and Sudoku. Similar to LLM results, we find that reinforcement learning (RL) improves in-distribution performance and incentivizes frequent reflection for tiny transformers, yet RL mainly optimizes shallow statistical patterns without faithfully reducing verification errors. In conclusion, integrating generative transformers with discriminative verification inherently facilitates CoT reasoning, regardless of scaling and natural language.

---

## 论文详细总结（自动生成）

# 论文中文总结：Self-Verifying Reflection Helps Transformers with CoT Reasoning

## 1. 核心问题与整体含义（研究动机和背景）

- **问题背景**：大型语言模型（LLM）在思维链推理中常进行“自我验证反思”（self-verifying reflection），即检查当前步骤的正确性并探索替代方案。然而，最新研究表明LLM在CoT中检测错误的能力相当有限，大量反思并未带来正确解决方案。因此，**反思为何仍能提升推理性能**，以及**强化学习如何激励反思行为**这两个核心问题亟待解释。
- **研究动机**：自然语言的复杂性和LLM的高昂训练成本使理论分析和全面实验难以进行。作者受到“任务特定推理不需要复杂语言”的启发，引入**小型Transformer**（仅百万级参数）来构建一个**无自然语言的极小化推理框架**，以在可控条件下清晰分析自我验证反思的本质作用。
- **整体贡献**：证明当验证误差有界时，自我验证反思能保证推理准确性提升；通过乘法与数独任务实验展示其训练和执行优势；揭示强化学习主要优化浅层统计模式而非真正减少验证错误。

## 2. 方法论：核心思想、关键技术细节

### 2.1 基本推理框架：马尔可夫思维过程（MTP）

- 将CoT推理形式化为一个马尔可夫过程：状态 \(S_t\)，规划策略 \(\pi\) 生成推理步骤 \(R_{t+1}\)，确定性转移函数 \(\mathcal{T}\) 更新状态 \(S_{t+1} = \mathcal{T}(S_t, R_{t+1})\)。
- 例如，整数乘法中状态为表达式 \(x_t \times y_t + z_t\)；数独中状态为9×9棋盘。

### 2.2 自我验证反思框架

- **反思式MTP（RMTP）**：在每一步后，模型同时输出验证标签 \(V_{t+1}\)（“✓”表示正确，“×”表示错误）。若验证为错误，状态保持不变，允许重新采样。转移函数为 \(\tilde{\mathcal{T}}(S_t, (R_{t+1}, V_{t+1}))\)。
- **反思式回溯搜索（RTBS）**：允许在某个状态上最多尝试 \(m\) 次，若全部失败则回溯到父状态，类似深度优先搜索。\(m\) 为搜索宽度。

### 2.3 训练流程

1. **预训练**：以CoT示例为文本语料进行下一词元预测。
2. **非反思式SFT**：通过模仿学习使模型从状态预测步骤。
3. **反思式SFT**：使用专家验证器对采样步骤标注真值验证标签，训练模型同时预测步骤和验证标签，整合规划与验证能力。
4. **强化学习（GRPO）**：基于结果奖励模型（ORM，正确=1/错误=0）进行策略优化，联合优化规划和验证。采样温度设为1.25以促进探索。

### 2.4 理论结果（简化推理原型）

- 设 \(\mu\) 为正向状态上的规划正确率，\(e^-\) 为误拒绝率（拒绝正确步骤），\(e^+\) 为误接受率（接受错误步骤），\(f\) 为负向状态上的拒绝概率。
- **定理1**：当 \(e^- + e^+ \le 1\) 时，RMTP准确率不低于无反思；RTBS在 \(f > \alpha\) 且 \(m > 1/(1-\alpha)\) 时对复杂问题更有效（\(\alpha = \mu e^- + (1-\mu)(1-e^+)\)）。
- **命题1**：RMTP找到正确答案的期望步数为 \(\bar{T} = n / [(1-\mu)e^+ + \mu(1-e^-)]\)，高 \(e^-\) 会大幅增加计算代价。

## 3. 实验设计

### 3.1 数据集与场景

- **任务1：整数乘法（Mult）**：计算两个非负整数的乘积。难度由较大乘数的位数 \(d\) 定义：ID Easy（1-5位）、ID Hard（6-8位）、OOD Hard（9-10位）。
- **任务2：数独（Sudoku）**：填充9×9矩阵的空格。难度由空格数 \(b\) 定义：ID Easy（9-35空格）、ID Hard（36-53空格）、OOD Hard（54-62空格）。
- 训练集包含ID Easy和ID Hard样本，测试集额外包含OOD Hard。

### 3.2 基准方法

- **无反思基线**：直接MTP（无验证）。
- **RMTP**：反思式MTP（即时拒绝错误步骤）。
- **RTBS**：反射式回溯搜索（宽度 \(m=4\)）。
- 对比验证类型：**二元验证**（单个标签） vs **详细验证**（每个元件检查）。
- 对比模型大小：1M、4M、16M参数。
- 额外对比LLM：GPT-4o、DeepSeek-R1、OpenAI o3-mini。

### 3.3 消融与扩展

- 状态表示：精简状态（仅保留必要信息） vs 完整历史状态 vs 直接输出。
- 验证类型：无验证、二元验证、详细验证、可选详细验证（RL时分析反思频率）。
- 训练方法：非反思SFT、反思SFT、GRPO、PPO（附录）。

## 4. 资源与算力

- **确切说明**：附录D.5给出了详细资源描述。
- **GPU**：5块GPU（1块NVIDIA RTX-3090 + 4块NVIDIA A10）。
- **训练时长**：完整流程（预训练+SFT+RL）约需2天/单GPU。
- **最大显存**：1M模型约4GB，4M模型约12GB，16M模型约16GB。
- **总模型数量**：共训练78个模型（含不同规模、任务、验证类型、RL算法）。

## 5. 实验数量与充分性

- **实验组数**：SFT部分训练30个模型并执行54次测试；RL部分报告了GRPO和PPO结果，超过30组，覆盖不同模型大小、验证类型、难度级别。
- **充分性评估**：
  - **覆盖范围**：跨越2个完全不同类型的推理任务、3个难度级别（含OOD泛化）、3种模型规模、2种验证粒度、2种搜索策略，实验设计较为全面。
  - **公平性**：所有模型在同一流程下训练（相同数据、超参数搜索策略），对比LLM时提示模板保持一致。
  - **局限性**：未报告误差棒（error bar），因训练成本过高；每个设置只运行一次，随机种子影响未量化。

## 6. 主要结论与发现

1. **学习自我验证显著促进前向推理**：即使测试时不启用反思，反思式SFT也能提升非反思执行准确率（例如1M乘法模型提升超过70%）。
2. **反思提升准确率需误拒绝率足够低**：高 \(e^-\) 会严重损害RMTP性能；理论上要求 \(e^- + e^+ \le 1\)。
3. **RL能激励反思频率**：当规划策略能以高温度有效探索时，GRPO使反思频率大幅上升（从<10%升至>80%），与DeepSeek-R1现象一致。
4. **RL提升主要靠统计优化而非真实推理能力**：RL降低 \(e^-\) 但升高 \(e^+\)，形成乐观偏差；OOD泛化几乎无提升，说明未学到可迁移技能。
5. **回溯搜索（RTBS）在错误状态可鉴别时有用**：数独中RTBS优于RMTP，而乘法中不明显，验证了理论条件。

## 7. 优点

- **理论严谨**：给出了反思有效性的充分必要条件，并分析了期望步数等实际代价。
- **框架极简**：去除了自然语言的干扰，使实验可解释性极高，且计算成本极低（百万参数规模）。
- **与LLM研究高度对齐**：训练流程模仿LLM（预训练→SFT→RL），结果（如反思频率增长）直接印证大型模型现象。
- **实验设计系统**：覆盖多种难度、验证粒度、搜索策略、模型大小，并包含OOD测试以评估泛化。
- **开放代码**：提供了完整可复现代码。

## 8. 不足与局限

- **泛化能力有限**：即使RL后，OOD准确率依然很低（乘法OOD Hard < 10%），表明方法未解决根本泛化问题。
- **任务局限**：仅测试乘法（符号推理）和数独（约束满足），未扩展到更丰富的自然语言推理任务。
- **未考虑连续修正**：反思仅限于重新采样，未使用自然语言中的“修正性反思”（如将错误步骤改为正确），导致协同优化空间不足。
- **评估偏差风险**：
  - 无误差棒报告，无法评估结果波动性。
  - 每个超参设置仅运行一次，随机种子影响未分析。
- **应用限制**：框架依赖状态化简（将历史信息压缩为精简状态），对需要完整历史的任务不适用。
- **缺乏与真实LLM的桥接**：如何从小模型结论推广到自然语言模型尚未验证，且作者承认框架未利用LLM的语言涌现能力。

（完）
