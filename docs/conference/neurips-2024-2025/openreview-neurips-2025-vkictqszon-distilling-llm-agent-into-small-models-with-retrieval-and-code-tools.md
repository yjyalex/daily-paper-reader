---
title: Distilling LLM Agent into Small Models with Retrieval and Code Tools
title_zh: 将LLM智能体蒸馏为带有检索和代码工具的小模型
authors: "Minki Kang, Jongwon Jeong, Seanie Lee, Jaewoong Cho, Sung Ju Hwang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=VkicTqszOn"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 将包括思维链在内的推理能力从大模型蒸馏到小模型
tldr: Agent Distillation将推理和工具使用行为从LLM智能体转移到小模型。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 大模型推理能力强但计算昂贵，小模型在罕见知识场景中易产生幻觉。
method: 提出Agent Distillation框架，将推理和工具使用行为从LLM智能体转移到小模型。
result: 小模型在复杂推理任务上性能显著提升，同时保持效率。
conclusion: 蒸馏智能体行为是提升小模型推理能力的有效途径。
---

## Abstract
Large language models (LLMs) excel at complex reasoning tasks but remain computationally expensive, limiting their practical deployment.
To address this, recent works have focused on distilling reasoning capabilities into smaller language models (sLMs) using chain-of-thought (CoT) traces from teacher LLMs.
However, this approach struggles in scenarios requiring rare factual knowledge or precise computation, where sLMs often hallucinate due to limited capability.
In this work, we propose Agent Distillation, a framework for transferring not only reasoning capability but full task-solving behavior from LLM-based agents into sLMs with retrieval and code tools. 
We improve agent distillation along two complementary axes: (1) we introduce a prompting method called first-thought prefix to enhance the quality of teacher-generated trajectories; 
and (2) we propose a self-consistent action generation for improving test-time robustness of small agents.
We evaluate our method on eight reasoning tasks across factual and mathematical domains, covering both in-domain and out-of-domain generalization.
Our results show that sLMs as small as 0.5B, 1.5B, 3B parameters can achieve performance competitive with next-tier larger 1.5B, 3B, 7B models fine-tuned using CoT distillation, demonstrating the potential of agent distillation for building practical, tool-using small agents.

---

## 论文详细总结（自动生成）

# 论文总结：Distilling LLM Agent into Small Models with Retrieval and Code Tools

## 1. 核心问题与整体含义（研究动机和背景）

- **问题背景**：大型语言模型（LLM）在复杂推理任务中表现优异，但推理成本高昂，限制了实际部署。近年来，研究者通过链式思维（CoT）蒸馏将LLM的推理能力迁移到小型语言模型（sLM）中，但sLM在需要罕见事实知识或精确计算时仍容易产生幻觉，因为其内部知识有限。
- **核心挑战**：如何在保留推理能力的同时，让sLM具备根据任务需求主动检索信息、执行代码等外部工具交互的能力，从而弥补记忆和计算的不足。
- **研究意义**：提出一种将LLM智能体的完整任务求解行为（包括推理、动作、观察）蒸馏到sLM的框架，使其能像智能体一样思考并利用工具，而不仅仅模仿静态推理链。

## 2. 论文提出的方法论

### 核心思想
- **Agent Distillation**：将LLM智能体（如ReAct、CodeAct）的交互式轨迹（思考-行动-观察）作为监督信号，训练sLM模仿这种行为，使其学会在需要时使用检索和代码工具解决问题，而非单纯依赖内部知识。
- **与CoT蒸馏的区别**：CoT蒸馏只迁移静态推理链，智能体蒸馏则迁移动态的“推理-行动-观察”循环，学生模型在测试时能自适应地调用工具。

### 关键技术细节
1. **First-thought Prefix (ftp)**：
   - 动机：指令微调后的LLM在智能体提示下可能偏离原有CoT推理模式，导致初始推理方向错误。
   - 方法：先用CoT提示生成第一步推理（`y1`），然后将其作为前缀附加到智能体第一轮思考之前，引导教师智能体产生更高质量的轨迹。公式见式(5)。
   - 仅用于教师轨迹生成，学生推理时无需该前缀。

2. **Self-consistent Action Generation (sag)**：
   - 动机：小模型生成的代码常出现解析错误或执行错误。
   - 方法：在每个动作步骤采样N个思考-动作序列（核采样，温度0.4），过滤掉无效动作（解析或执行错误），然后对成功动作的输出进行多数投票，选择最一致的动作。若所有动作均失败，随机保留一个并利用错误反馈让模型后续修正。
   - 目的：提升学生智能体在测试时的鲁棒性。

### 训练目标（损失函数）
- 学生模型在教师轨迹上的负对数似然最大化，排除环境返回的观测部分：
  ```math
  min_θ -E_{(x,τ)} Σ_t log p_S(r_t, a_t | x, τ_{<t}; θ)
  ```
  其中τ包含多轮思考-动作-观测环。

## 3. 实验设计

### 数据集与场景
- **训练数据**：
  - 事实推理：1,000条HotPotQA（2跳问答）
  - 数学推理：2,000条MATH（大学级数学，分不同难度级别）
- **测试基准**（8个任务，分为域内与域外）：
  - 事实推理（域内）：HotPotQA
  - 事实推理（域外）：Bamboogle（2跳）、MuSiQue（3跳）、2WikiMultiHopQA（2跳）
  - 数学推理（域内）：MATH
  - 数学推理（域外）：GSM-Hard（大数运算）、AIME（奥数）、OlymMATH（奥数）
- 每个测试集限制为500条（AIME为90条），以减少评估成本。

### 对比方法
- **基线**：
  - CoT Prompting（直接提示小模型）
  - CoT Distillation（蒸馏静态CoT轨迹）
  - CoT Distillation + RAG（在蒸馏和推理时引入检索增强）
  - Agent Prompting（直接提示小模型按智能体方式回答）
- **本文方法**：
  - Agent Distillation（基础版）
  - + ftp（使用第一思考前缀）
  - + sag（使用自洽动作生成）
  - + ftp + sag（两者结合）

### 模型
- **教师模型**：Qwen2.5-32B-Instruct
- **学生模型**：Qwen2.5-Instruct系列（0.5B, 1.5B, 3B, 7B）；另用Llama-3.2-1B-Instruct、Phi-4-mini-instruct（3.8B）验证泛化性。
- 额外测试：使用代码专用模型Qwen2.5-Coder系列作为教师或学生。

## 4. 资源与算力

- **硬件**：4块 NVIDIA A100 80GB GPU
- **训练细节**：
  - 参数高效微调：LoRA（rank=64），应用于所有线性层
  - 训练轮次：2 epoch
  - 批次大小：8
  - 学习率：2×10⁻⁴
  - 轨迹采样：每个问题从教师获得1条轨迹，过滤错误后约2,000条用于蒸馏
- **未明确说明**：具体训练时长（小时数）未提及。

## 5. 实验数量与充分性

- **主要实验组数**：
  - 4种学生尺寸（0.5B,1.5B,3B,7B）× 8个任务 × 多种方法（基线+本文变体）→ 表2呈现约96个性能数据点。
  - 跨模型系列验证（Llama-3.2-1B, Phi-4-mini）→ 表4
  - 代码专用模型对比（表3）
  - 消融分析：ftp效果（图4、图8）、sag效果（图7）、自洽性对比（图5）
  - 标记计数分析（图6）
  - 检索调用次数分析（图8）
  - 温度消融（附录表6）
  - 全微调 vs LoRA（附录表8）
  - 方差分析（附录表7，AIME上5次运行）
- **充分性与公平性**：
  - 覆盖域内和域外任务，评估泛化能力。
  - 对比了多种基线（含RAG增强版本），公平性较好。
  - 但仅报告单次运行结果（除AIME外），缺乏统计误差条，可能削弱结论稳健性。
  - 超参数（温度、N=8）经过一定调优，但未系统搜索。

## 6. 主要结论与发现

1. **Agent蒸馏显著优于CoT蒸馏**：在所有学生尺寸上，Agent蒸馏在平均性能（8个任务）上超越CoT蒸馏，尤其在域外任务中优势更大。
2. **小模型可匹敌更大的CoT模型**：0.5B Agent ≈ 1.5B CoT；1.5B Agent ≈ 3B CoT；3B Agent > 7B CoT；7B Agent > 32B CoT（教师本身）。
3. **ftp提升复杂推理**：对MATH高难度级别（4-5级）和AIME有显著帮助，但有时导致检索次数减少，增加幻觉风险。
4. **sag有效减少代码错误**：对0.5B模型效果最明显，在AIME上提升显著。
5. **标记开销分析**：事实推理中Agent生成更多token（多次检索），数学推理中Agent生成更少token（用循环代码代替重复计算）。
6. **跨模型系列有效**：Llama-3.2-1B和Phi-4-mini同样受益。
7. **代码专用模型影响有限**：使用Qwen2.5-Coder作为教师或学生仅带来边际提升。

## 7. 优点

- **方法创新**：将智能体行为（交互式工具使用）引入小模型蒸馏，超越了静态CoT蒸馏的范式。
- **实用技巧**：提出的ftp和sag简单有效，无需额外训练，仅改变采样策略或前缀，即能提升蒸馏质量和测试鲁棒性。
- **实验全面**：覆盖8个任务、4种尺寸、多种模型家族，并进行大量消融分析，验证了方法的通用性。
- **性能惊艳**：展示了极小型模型（0.5B）通过工具使用即可达到远大于其尺寸的模型水平，具有实际部署价值。
- **开放代码**：提供GitHub仓库，促进可复现性。

## 8. 不足与局限

- **模型泛化性验证不足**：仅使用Qwen2.5系列作为主力，额外验证仅限于两种模型，未在更大范围（如Gemma、DeepSeek）上测试。
- **教师单一性**：仅使用32B教师，未探索更强教师（如GPT-4）或不同规模教师的影响。
- **轨迹数量未研究**：每个问题仅采样一条教师轨迹（过滤后），未探讨多轨迹对蒸馏的影响（与CoT蒸馏文献对比欠缺）。
- **评估缺乏统计稳健性**：除附录AIME外，未报告多次运行的方差或置信区间，单次结果可能存在偶然性。
- **任务覆盖局限**：仅限事实和数学推理，未涉及具身智能体、网页代理、多模态等更广泛的智能体应用。
- **潜在安全问题**：小模型执行代码时可能生成不安全操作，文中仅提及需用沙箱环境，未做深入评估或缓解。
- **ftp的副作用**：在部分任务中，ftp导致模型依赖内部知识而非检索，增加幻觉风险（见图8分析）。
- **数学性能权衡**：在MATH500上，Agent蒸馏（尤其3B、7B）略逊于CoT蒸馏，说明某些数学问题更适合纯推理而非工具调用（如解析型问题）。
- **资源消耗未量化**：训练时间、推理延迟、内存占用等实际部署关键指标未报告。

（完）
