---
title: Corrector Sampling in Language Models
title_zh: 语言模型中的校正采样
authors: "Itai Gat, Neta Shaul, Uriel Singer, Yaron Lipman"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=stpe7UeETz"
tags: ["query:cot-unfaith"]
score: 6.0
evidence: 通过重采样token改善推理性能
tldr: "校正采样(RPT)通过迭代替换之前token改善LLM推理，在推理基准上提升约10%。"
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 自回归语言模型因固定从左到右生成而累积错误，限制推理性能。
method: 提出Resample-Previous-Tokens (RPT)方法，在生成过程中迭代重访并替换之前token。
result: "在8B参数模型上微调100B token后，推理和编码基准相对提升约10%。"
conclusion: RPT通过减少错误累积有效提升推理能力。
---

## Abstract
Autoregressive language models accumulate errors due to their fixed, irrevocable left-to-right token generation. To address this, we propose a new sampling method called Resample-Previous-Tokens (RPT). RPT mitigates error accumulation by iteratively revisiting and potentially replacing tokens in a window of previously generated text. Fine-tuning a pretrained 8B parameter model with RPT for only 100B resulted in ~10% relative improvements on reasoning and coding benchmarks compared to the standard sampling.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

自回归语言模型（AR LMs）在序列生成中采用从左到右的固定生成方式，一旦采样一个token就无法修改，导致误差逐渐累积（error accumulation）。尽管近年来在数据质量、架构改进和强化学习微调等方面取得了进展，但采样过程本身仍相对未被充分探索，大多数模型仍依赖原始的NTP采样。论文旨在解决这一缺陷，提出一种新的采样方法——Resample-Previous-Tokens（RPT），通过在生成过程中迭代地重新访问并可能替换先前生成的token，来减少误差累积，从而提升模型在推理和代码生成等任务上的性能。

## 2. 方法论

### 核心思想
RPT在标准NTP采样的基础上，引入一个长度为w的滑动窗口，在生成过程中对窗口内的token进行迭代重采样，允许模型修正之前可能出错的预测。其基本形式（w=2）为：首先用NTP初始化相邻的两个token (xi, xi+1)，然后交替重采样：
- xi ~ ̂p(xi | x<i, xi+1) （PTP：给定未来token预测前一个token）
- xi+1 ~ ̂p(xi+1 | x<i+1) （标准NTP）

通过多次迭代，RPT的稳态分布误差在理论上可以小于NTP的误差。

### 关键技术细节
- **训练方法**：需要对预训练模型进行微调，使其同时学习NTP和PTP条件概率。具体做法是在训练数据中随机对相邻的w个token进行置换（permute），注入关于输入和目标位置的信息（通过相对位置编码 layer）。训练损失仍然是交叉熵，但目标预测位置会根据置换模式变化。
- **窗口大小 w**：论文主要实验使用w=2或w=3，实际采样中w=2已足够有效。
- **采样实用技巧**：在PTP步骤中使用贪婪解码（arg max），并引入置信度阈值（如0.9），仅接受置信度高的重采样结果。
- **位置编码**：使用相对位置编码（差值 τi - σi）来告知模型token是否被置换。

### 理论分析
论文给出了一个针对两token情况的渐近误差分析，证明在PTP误差（∥ϵi|i+1∥∞）远小于NTP误差（∥ϵi∥1, ∥ϵi+1|i∥∞）的条件下，RPT采样的误差上界比NTP更小，即RPT因子 ρ < 1。实验也验证了PTP的交叉熵损失显著低于NTP。

## 3. 实验设计

### 数据集/场景
- **推理任务**：GSM8K（数学应用题）
- **代码生成任务**：HumanEval+（Python函数补全）、MBPP（代码生成）、MultiPL-E（包含C++, C#, PHP, Bash, Java, TypeScript等非Python语言）
- **误差分析**：使用DCLM、GitHub、wiki、arxiv、se等5个验证集计算总变差距离（TV distance）。

### Benchmark
- 主要指标：Pass@1（最佳温度在{0.0, 0.05, 0.1}中通过oracle选择）
- 对比基线：完全训练的8B AR模型（AR-F，1T tokens）、中间检查点（AR-C，900B tokens），以及RPT微调后的模型采用不同迭代次数k（k=0为NTP，k>0为RPT）。

### 对比方法
- 标准NTP采样（即k=0）
- 不同迭代次数k=0.5,1,1.5,2等
- 消融实验：是否使用贪婪解码、是否使用置信度阈值、不同窗口大小（w=2 vs w=3）、不同温度。

## 4. 资源与算力

论文明确说明：
- 预训练模型：8B参数，在1T token数据上训练240K迭代。
- 微调阶段：使用256张H100 GPU，batch size 4M tokens，额外训练100B token（约16K迭代）。
- 优化器：AdamW，学习率峰值1e-3，预热2000步，余弦调度。
- 训练数据：同一语料库，数据顺序保持相同。

## 5. 实验数量与充分性

论文进行了较为充分的实验：
- **主实验**：在9个基准上报告结果（HumanEval+, MBPP, GSM8K, 以及6种MultiPL-E语言），对比AR-F、AR-C和RPT不同k值。
- **误差分析**：在5个验证集上计算TV距离，展示RPT迭代k=0~3的误差下降趋势。
- **概率增益分析**：展示RPT对验证token概率的改善（64.5%案例中提高）。
- **消融实验**：
  - 采样策略：有无贪婪解码、有无置信度阈值。
  - 窗口大小：w=2 vs w=3。
  - 温度分析：不同温度下Pass@1变化。
- **理论验证**：合成实验（词汇表V=20），在不同噪声水平下对比NTP和RPT的总变差距离和最大误差。

实验设计基本客观公平：使用相同的温度选择策略（oracle over best temperature），报告相对变化。但未提供多次运行的标准差或统计显著性检验，仅报告单次结果。

## 6. 主要结论与发现

- RPT微调后模型（k=0，即NTP采样）在多数基准上已优于或持平完全训练的AR-F，表明微调本身可能有益。
- RPT采样（k≥1）在所有9个基准上均一致优于NTP采样（k=0），相对提升约5%~10%（具体数值见表1）。
- PTP损失远低于NTP损失，证明利用未来token可以更准确预测当前token。
- 在实际采样中，仅需1~2次RPT迭代即可获得大部分收益，更多迭代提升有限。
- RPT在低温度下收益更显著，但在高温度下也有稳定改善。

## 7. 优点

- **方法简单有效**：仅需在预训练模型上微调少量token（10%），即可获得显著性能提升，且不破坏原NTP能力。
- **理论支撑**：提供了渐近误差分析的数学框架和合成实验验证，增强了方法可信度。
- **兼容性强**：可无缝集成到现有AR模型和代码中，保留KV缓存等加速机制。
- **采样灵活**：支持贪婪解码、置信度阈值等实用技巧，易于工程落地。
- **多语言验证**：在非Python语言（C++, Java等）上也观察到一致提升，表明泛化性良好。

## 8. 不足与局限

- **实验覆盖有限**：
  - 仅使用8B规模模型，未在更大模型（如70B）上验证。
  - 仅在100B token上微调，未探索更长微调或从头训练的效果。
  - 未在通用语言理解基准（如MMLU、HellaSwag等）上评估，仅聚焦推理和代码任务。
- **未提供统计不确定性**：所有结果均为单次运行，缺少误差条或显著性测试，难以判断改进是否稳定。
- **窗口大小探索有限**：仅测试w=2和w=3，理论可支持更大窗口但未深入。
- **采样计算开销**：RPT需要多次迭代（即使次数少），相比标准NTP会增加推理延迟，论文未量化实际延迟影响。
- **偏差风险**：仅在特定数据集和训练设置下验证，未讨论模型可能对某些类型错误更加敏感的风险。
- **缺乏开放代码/数据**：论文声明未公开代码或数据，仅称实现简单，但不利于复现。

（完）
