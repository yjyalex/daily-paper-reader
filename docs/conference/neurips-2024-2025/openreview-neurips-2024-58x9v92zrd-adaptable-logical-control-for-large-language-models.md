---
title: Adaptable Logical Control for Large Language Models
title_zh: 面向大语言模型的自适应逻辑控制
authors: "Honghua Zhang, Po-Nien Kung, Masahiro Yoshida, Guy Van den Broeck, Nanyun Peng"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=58X9v92zRd"
tags: ["query:llm-fo"]
score: 7.0
evidence: 用于遵循逻辑约束的神经符号化约束解码框架
tldr: Ctrl-G将大语言模型与隐马尔可夫模型结合，强制执行逻辑约束，超越GPT-4。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-58x9v92zrd/fig-001.webp\", \"caption\": \"\", \"page\": 17, \"index\": 1, \"width\": 823, \"height\": 772}]"
motivation: 大语言模型在推理时难以严格遵循逻辑约束进行生成。
method: 将大语言模型与隐马尔可夫模型结合，通过确定性有限自动机引导生成遵守逻辑约束。
result: "在文本编辑任务上，Ctrl-G比GPT-4在遵循约束上高出30%以上。"
conclusion: 神经符号化方法能有效提升大语言模型在结构化输出方面的可控性。
---

## Abstract
Despite the success of Large Language Models (LLMs) on various tasks following human instructions, controlling model generation to follow strict constraints at inference time poses a persistent challenge. In this paper, we introduce Ctrl-G, a neuro-symbolic framework that enables tractable and adaptable control of LLM generation to follow logical constraints reliably. Ctrl-G combines any production-ready LLM with a Hidden Markov Model (HMM), guiding LLM outputs to adhere to logical constraints represented as deterministic finite automata. We show that Ctrl-G, when a TULU2-7B model is coupled with a 2B-parameter HMM, outperforms GPT4 in text editing: on the task of generating text insertions/continuations following logical constraints, our approach achieves over 30% higher satisfaction rate in human evaluation. When applied to medium-size language models (e.g., GPT2-large), Ctrl-G also beats its counterparts on standard benchmarks by large margins. Additionally, as a proof-of-concept study, we use Ctrl-G to assist LLM reasoning on the GSM benchmark, foreshadowing the application of Ctrl-G, as well as other constrained generation approaches, beyond traditional language generation tasks.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：大语言模型（LLM）虽在众多任务上表现出色，但在推理时难以可靠地遵循严格的逻辑约束（如必须包含特定关键词、长度限制等）。对LLM进行条件化以服从约束在计算上通常是难解的（intractable），现有方法（如搜索式解码、近似分类器、采样方法）要么伸缩性差，要么无法保证约束满足，要么需要针对不同约束重新训练。
- **研究动机**：提出一种既能保证约束满足、又能灵活适应不同逻辑约束、且无需针对每个约束重新训练的推理时控制框架。
- **整体含义**：Ctrl-G是神经符号（neuro-symbolic）框架，将任意生产级LLM与经蒸馏的隐马尔可夫模型（HMM）结合，利用确定性有限自动机（DFA）紧凑表示逻辑约束，在推理时通过HMM高效计算条件概率来引导LLM生成，从而可靠地满足约束。

#### 2. 论文提出的方法论
- **核心思想**：利用可高效进行概率推理的HMM作为LLM的白盒近似，在每一步生成时，用HMM计算给定当前已生成前缀条件下未来能满足约束的概率 \(\text{phmm}(\alpha \mid x_t, x_{<t})\)，将其作为因子乘到LLM的下一token分布上，从而得到引导后的分布。
- **关键技术细节**（三步流程）：
  1. **蒸馏（Distillation）**：从LLM采样大量序列，训练一个HMM以最小化其与LLM分布的KL散度。
  2. **约束规范（Constraint Specification）**：将用户指定的逻辑约束（如关键词包含、长度控制）构造为DFA。利用KMP算法、交集、并集、补集等操作可组合复杂约束。
  3. **推理（Inference）**：在每一步自回归生成时，计算 \(\text{phmm}(\alpha \mid x_t, x_{<t})\) 作为近似。其核心是利用HMM与DFA的马尔可夫性质，通过递归关系（公式(4)）高效计算条件概率 \(p(S_n \in F \mid z_t, s_t)\)，其中 \(z_t\) 是HMM隐状态，\(s_t\) 是DFA状态。然后采样下一token为：
     \[
     p_{\text{ctrl-g}}(x_t \mid x_{<t}, \alpha) \propto p_{\text{llm}}(x_t \mid x_{<t}) \cdot p_{\text{hmm}}(\alpha \mid x_t, x_{<t})
     \]
- **算法流程**：参见论文Algorithm 1：先反向计算所有时刻所有隐状态/DFA状态下的条件概率（预处理），然后前向生成时每一步根据公式(3)计算当前约束概率，结合LLM概率采样token，并更新DFA状态。
- **时间复杂度**：Theorem 3.2：采样长度为 \(n\) 的序列，DFA有 \(m\) 条边，HMM有 \(h\) 个隐状态，总时间为 \(O(n m h^2)\)。

#### 3. 实验设计
- **数据集与场景**：
  - **CommonGen**（常识生成）：输入3~5个概念，生成包含所有概念的句子。测试标准BLEU-4、ROUGE-L、CIDEr、SPICE，以及约束满足率。
  - **文本填充（Text Infilling）**：来自ROC Stories，将故事中部分片段掩码，模型需填充。不同掩码比例（13%、21%、32%、40%），指标为BLEU-4和ROUGE-L。
  - **交互式文本编辑**：基于CoAuthor数据集，构建800条测试用例，分为**续写**和**插入**两种场景，可同时施加**关键词包含**和**词数控制**约束。共8种设置（2种场景 × 2种约束存在与否的4种组合）。
- **基准方法**：
  - CommonGen：FUDGE、NADO、NeuroLogic A*esque、GeLaTo（均为GPT2-large为基础）。
  - 文本填充：ILM（GPT2-small有监督训练）。
  - 交互式文本编辑：TULU2-7B（纯提示）、GPT3.5、GPT4、以及指令微调版TULU2-7B。
- **评估方式**：自动评估 + 人工评估（Amazon MTurk，三个标注者从流畅度、一致性、总体质量评分，并计算约束满足率与综合满意率）。

#### 4. 资源与算力
- 论文明确提到：运行时分析在**NVIDIA A100 GPU（80GB内存）**上完成。蒸馏HMM时，从TULU2-7B模型采样500万条训练数据，HMM隐状态数32768（约20亿参数），训练40个EM步。对于GPT2-large，采样400万条，隐状态数32768（通用）或4096（部分实验）。
- 未详细说明训练总耗时、GPU数量、训练天数等。仅给出了每token生成时间的实验数据（见Fig.5）。

#### 5. 实验数量与充分性
- **实验组数**：三个主要场景共涉及多种设定：
  - CommonGen：3、4、5概念（有监督/无监督）及6~9概念的扩展（CommonGen+），共约12种组合。
  - 文本填充：4种掩码比例。
  - 文本编辑：8种设置（续写/插入 × 无约束/关键词/词数/两者均有），每种100条测试，共800条。
- **充分性与公平性**：
  - 自动评估指标全面（BLEU-4、ROUGE-L、CIDEr、SPICE）。
  - 人工评估采用三标注者，计算了一致性（Kendall系数0.449，中等一致）。
  - 对比基线均为该领域近期代表性方法，且在同一基座模型上比较（如GeLaTo、FUDGE均使用相同GPT2-large）。GPT系列使用相同提示。
  - 约束满足率均报告，Ctrl-G在约束满足上达100%。
- **消融实验**：未专门进行HMM规模或蒸馏步骤的消融，但通过对比不同隐状态数（4096 vs 32768）的运行时体现出了扩展性。局限：缺少对蒸馏误差影响的消融。

#### 6. 论文的主要结论与发现
- **性能领先**：在文本编辑任务（续写+逻辑约束）中，Ctrl-G（TULU2-7B + 2B HMM）在综合满意率上比GPT-4高出30%以上；在插入任务中，生成质量与GPT-4相当或更好。
- **100%约束满足**：在所有测试场景中，Ctrl-G生成的输出均严格满足指定的逻辑约束（关键词包含、长度控制），而GPT-3.5/4的满足率最高仅59%。
- **通用性强**：Even无约束时，Ctrl-G生成质量与GPT-4匹配；随着约束复杂度增加，GPT-4质量下降，而Ctrl-G保持稳定。
- **多基准碾压**：在CommonGen和文本填充基准上，Ctrl-G不仅100%满足约束，而且自动评估指标显著优于所有对比方法（包括有监督训练模型）。
- **推理辅助潜力**：在GSM数学推理上，通过强制生成问题中所有数字，使模型准确率提升3.4%，展示了Ctrl-G在更广任务中的应用可能。

#### 7. 优点
- **神经符号结合**：将LLM的概率分布与HMM的可控推理结合，既利用了LLM的生成能力，又保证了约束的可满足性。
- **无需重新训练**：HMM蒸馏一次后，可适用于任意可表示为DFA的约束，无需针对每个约束重新训练分类器或调整模型。
- **可扩展性**：支持复杂逻辑组合（交集、并集、补集、连接），且算法时间复杂度与DFA大小线性相关。
- **高效实现**：通过张量化在GPU上并行计算，显著快于GeLaTo（表2）和搜索方法（A*esque）。
- **可用性高**：提供简洁API（示例Fig.2仅需5行代码），代码开源。

#### 8. 不足与局限
- **约束表示能力受限**：仅适用于可表示为DFA的约束，对于语义约束（如“情感积极”）、数值推理等难以直接编码。
- **蒸馏误差**：HMM只是LLM的近似，蒸馏偏差可能影响引导的准确性；当LLM分布与HMM差异大时，约束概率估计可能不准确（论文未系统分析此误差）。
- **计算开销**：虽然比搜索方法快，但每步仍需O(mh²)的计算，当h很大（如32768）时，开销仍然可观（Fig.5显示每token约数毫秒，但比base LLM增加了几倍）。
- **GSM实验有限**：仅基于模型未使用所有数字的293个例子做概念验证，提升幅度小（3.4%），且未与其他约束解码方法对比。
- **社会影响与偏见**：未讨论生成内容可能包含的偏见、毒性等风险，也未提供缓解措施。
- **实验覆盖**：未在更大型LLM（如Llama2-70B、GPT-4本身）上测试Ctrl-G的适用性；HMM参数规模需随LLM增大而增大，可能带来可扩展性挑战。

（完）
