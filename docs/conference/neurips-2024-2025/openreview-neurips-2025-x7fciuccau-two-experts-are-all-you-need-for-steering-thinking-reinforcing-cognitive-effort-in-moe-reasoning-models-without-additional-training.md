---
title: "Two Experts Are All You Need for Steering Thinking: Reinforcing Cognitive Effort in MoE Reasoning Models Without Additional Training"
title_zh: 只需两个专家：无需额外训练增强MoE推理模型的认知努力
authors: "Mengru Wang, Xingyu Chen, Yue Wang, Zhiwei He, Jiahao Xu, Tian Liang, Qiuzhi Liu, Yunzhi Yao, Wenxuan Wang, Ruotian Ma, Haitao Mi, Ningyu Zhang, Zhaopeng Tu, Xiaolong Li, Dong Yu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=x7fCiuCCAu"
tags: ["query:cot-unfaith"]
score: 4.0
evidence: 无需训练提升MoE模型的推理深度和效率
tldr: RICE通过nPMI识别认知专家来引导MoE推理模型。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-x7fciuccau/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1002, \"height\": 806}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-x7fciuccau/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 974, \"height\": 888}]"
motivation: 现有推理模型存在过度思考和思考不足等问题。
method: 利用归一化点互信息识别专家，在推理时调整专家激活。
result: 在不需额外训练的情况下改善了推理深度和效率。
conclusion: 推理时专家引导能有效优化MoE模型的推理行为。
---

## Abstract
Mixture-of-Experts (MoE) architectures within Large Reasoning Models (LRMs) have achieved impressive reasoning capabilities by selectively activating experts to facilitate structured cognitive processes. Despite notable advances, existing reasoning models often suffer from cognitive inefficiencies like overthinking and underthinking. To address these limitations, we introduce a novel inference-time steering methodology called Reinforcing Cognitive Experts (RICE), designed to improve reasoning depth and efficiency without additional training or complex heuristics. Leveraging normalized Pointwise Mutual Information (nPMI), we systematically identify specialized experts, termed cognitive experts that orchestrate meta-level reasoning operations characterized by tokens like <think>. Empirical evaluations with leading MoE-based LRMs (DeepSeek-R1 and Qwen3-235B) on rigorous quantitative and scientific reasoning benchmarks (AIME and GPQA Diamond) demonstrate noticeable and consistent improvements in reasoning accuracy, cognitive efficiency, and cross-domain generalization. Crucially, our lightweight approach substantially outperforms prevalent reasoning-steering techniques, such as prompt design and decoding constraints, while preserving the model's general instruction-following skills. These results highlight reinforcing cognitive experts as a promising, practical, and interpretable direction to enhance cognitive efficiency within advanced reasoning models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

大型推理模型（LRM）中广泛采用混合专家（MoE）架构，通过选择性激活专家来实现结构化认知过程，从而获得强大的推理能力。然而，现有推理模型常面临认知效率问题，如**过度思考**（overthinking）和**思考不足**（underthinking），导致推理过程冗长或浅薄。已有方法通过偏好优化、解码惩罚等方式缓解，但通常需要额外训练或复杂规则。受人类大脑功能特化启发，论文提出一个核心问题：**MoE模型中是否存在与推理行为高度相关的“认知专家”，通过操纵这些专家能否在不增加训练成本的前提下提升推理深度和效率？**

## 2. 论文提出的方法论

### 核心思想
利用归一化点互信息（nPMI）量化专家激活与推理标记（如 `<think>`、`</think>`）的共现程度，识别出与元认知推理高度相关的专家，称为**认知专家**（cognitive experts）。在推理时通过放大这些专家的权重（乘以一个调控因子 β）来强化推理过程，无需任何额外训练或监督信号。

### 关键技术细节
- **nPMI 计算公式**：  
  \[
  \text{nPMI}(x, y=E_i) = \frac{\log_2\left(\frac{k_n}{M}\right) + \log_2\left(\frac{T}{K_n}\right)}{\log_2\left(\frac{T}{k_n}\right)}
  \]
  其中 \(x\) 是推理标记（如 `<think>`），\(E_i\) 为专家，\(k_n\) 为专家在推理标记出现时被激活的次数，\(K_n\) 为专家总激活次数，\(M\) 为实例数，\(T\) 为总 token 数。
- **综合评分**：对多个推理标记（`<think>`、`</think>`、`alternatively`）赋予不同系数（如 +1、-1、-1），加权求和得到专家的最终 nPMI 分数。选择分数最高的 l 个专家作为认知专家。
- **推理时增强**：对于激活的认知专家，其权重乘以 β（β≥1）；其他专家保持不变。通过 β 控制增强强度。

## 3. 实验设计

### 数据集与基准
- **数学推理**：AIME2024、AIME2025（各30题）
- **科学推理**：GPQA Diamond（198题，涵盖物理、化学、生物）
- **通用指令跟随**：ArenaHard（500题，随机抽取50题评估）
- **附加验证**：GSM8K、MATH-500、HLE（各采样部分数据）

### 对比方法
- 基线：原版 DeepSeek-R1 / Qwen3-235B 模型
- 提示工程：在 `<think>` 前/后插入结构化提示
- 解码约束：类似TIP的方法，惩罚 `</think>` 标记
- 消融实验：随机选择专家、不同专家数量（1~5）、不同 β 值

## 4. 资源与算力

论文明确提及：DeepSeek-R1 实验使用 **16 块 H20 GPU**，采用 vllm==0.7.0；Qwen3-235B 实验使用 vllm==0.8.5（兼容性要求）。未说明具体训练时长，但因方法仅需单次前向传播识别专家，推理时仅修改权重，算力需求极低。

## 5. 实验数量与充分性

- 共涉及 **两个模型**（DeepSeek-R1、Qwen3-235B）
- **主要基准**：AIME24/AIME25、GPQA Diamond四个子领域
- **消融实验**：
  - 专家数量（1~5）与 β 值（2~512）的调优
  - 跨领域专家迁移（数学→物理、化学、生物等）
  - 随机专家对比
  - 带/不带归一化的效果对比
  - Pass@k 采样实验（16个样本，温度0.6，top-p 0.95）
  - 通用能力评估（ArenaHard）、附加数据集（GSM8K、MATH-500、HLE）
- **充分性判断**：实验设计较为全面，覆盖推理精度、效率、跨领域泛化、通用能力影响等多个维度，且与多种基线方法公平比较。但缺乏统计显著性检验，部分结果（如AIME25的Pass@1）存在波动，但整体趋势一致。

## 6. 论文的主要结论与发现

1. **存在高度特化的认知专家**：通过nPMI可有效识别，且跨领域存在共享专家（如数学与物理学领域专家高度重叠），表明通用推理机制。
2. **仅增强前2-5个认知专家即可显著提升推理精度**：在AIME24上 DeepSeek-R1 从73.3%提升至83.3%，AIME25 从63.3%提升至73.3%；Qwen3-235B 在AIME25 从66.7%提升至73.3%。
3. **效率提升**：增强后平均 token 使用量下降（例如 DeepSeek-R1 在AIME24从9,219降至8,317），思考步骤（Thought）数也减少，说明推理更集中高效。
4. **跨领域泛化良好**：数学认知专家可用于物理、化学领域，但生物领域需专用专家；通用领域专家可带来平均提升。
5. **不损害通用指令跟随能力**：ArenaHard 得分稳定或略有上升（91%→92%~94%）。
6. **优于提示工程和解码约束**：平均准确率提升幅度最大（从68.3%到78.7%）。

## 7. 优点

- **无需训练**：仅需单次前向传播识别专家，推理时只需权重调整，算力消耗极低。
- **轻量高效**：仅修改两个专家权重即可取得显著提升，且不增加推理延迟。
- **可解释性**：nPMI 提供了与推理行为相关的可解释专家定位。
- **跨域迁移性**：专家选择在不同推理任务间可复用，展现了通用认知功能。
- **安全性**：未发现对通用能力的负面影响，甚至略有提升。

## 8. 不足与局限

- **专家识别依赖推理标记**：仅通过 `<think>` 等标记可能无法完全捕捉复杂认知过程（如回溯、验证），可能遗漏其他关键专家。
- **架构限制**：仅在 DeepSeek-R1 和 Qwen3-235B 上验证，其他 MoE 模型（如 Mixtral、Llama-4）未测试，泛化性未知。
- **β 调优敏感**：过大的 β 会导致模型崩溃（输出无意义重复），需谨慎选择；不同领域可能需要不同 β。
- **统计显著性缺失**：未报告误差棒或置信区间，多次运行的结果波动未量化。
- **AIME25 Pass@1 下降**：DeepSeek-R1 上 Pass@1 从68.5%降至67.7%，说明增强并非在所有情况下都能提升单次最优。
- **部分领域效果有限**：生物领域使用通用专家反而下降，说明领域特化与通用之间的权衡。

（完）
