---
title: "ThinkSound: Chain-of-Thought Reasoning in Multimodal LLMs for Audio Generation and Editing"
title_zh: "ThinkSound: 多模态大模型中的思维链推理用于音频生成与编辑"
authors: "Huadai Liu, Kaicheng Luo, Jialei Wang, Wen Wang, Qian Chen, Zhou Zhao, Wei Xue"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=mj8VN4MyrO"
tags: ["query:cot-unfaith"]
score: 7.0
evidence: 在多模态大模型中使用思维链推理进行音频生成
tldr: ThinkSound利用思维链推理实现视频的分步音频生成与编辑。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 480, \"height\": 552}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 400, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 800, \"height\": 800}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-004.webp\", \"caption\": \"\", \"page\": 1, \"index\": 4, \"width\": 800, \"height\": 800}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-005.webp\", \"caption\": \"\", \"page\": 1, \"index\": 5, \"width\": 400, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-006.webp\", \"caption\": \"\", \"page\": 1, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-007.webp\", \"caption\": \"\", \"page\": 1, \"index\": 7, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-008.webp\", \"caption\": \"\", \"page\": 1, \"index\": 8, \"width\": 800, \"height\": 800}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-009.webp\", \"caption\": \"\", \"page\": 1, \"index\": 9, \"width\": 392, \"height\": 392}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-010.webp\", \"caption\": \"\", \"page\": 1, \"index\": 10, \"width\": 400, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-011.webp\", \"caption\": \"\", \"page\": 1, \"index\": 11, \"width\": 400, \"height\": 400}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-012.webp\", \"caption\": \"\", \"page\": 4, \"index\": 12, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-013.webp\", \"caption\": \"\", \"page\": 4, \"index\": 13, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 480, \"height\": 480}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 933, \"height\": 662}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 376, \"height\": 398}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-017.webp\", \"caption\": \"\", \"page\": 4, \"index\": 17, \"width\": 418, \"height\": 404}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-018.webp\", \"caption\": \"\", \"page\": 4, \"index\": 18, \"width\": 772, \"height\": 674}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-019.webp\", \"caption\": \"\", \"page\": 4, \"index\": 19, \"width\": 755, \"height\": 687}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-020.webp\", \"caption\": \"\", \"page\": 9, \"index\": 20, \"width\": 1632, \"height\": 452}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-021.webp\", \"caption\": \"\", \"page\": 9, \"index\": 21, \"width\": 1631, \"height\": 438}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-022.webp\", \"caption\": \"\", \"page\": 9, \"index\": 22, \"width\": 958, \"height\": 472}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-023.webp\", \"caption\": \"\", \"page\": 9, \"index\": 23, \"width\": 953, \"height\": 502}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-024.webp\", \"caption\": \"\", \"page\": 9, \"index\": 24, \"width\": 950, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-025.webp\", \"caption\": \"\", \"page\": 9, \"index\": 25, \"width\": 1601, \"height\": 446}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-026.webp\", \"caption\": \"\", \"page\": 9, \"index\": 26, \"width\": 1628, \"height\": 352}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-027.webp\", \"caption\": \"\", \"page\": 9, \"index\": 27, \"width\": 1633, \"height\": 413}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-028.webp\", \"caption\": \"\", \"page\": 9, \"index\": 28, \"width\": 1469, \"height\": 436}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-029.webp\", \"caption\": \"\", \"page\": 9, \"index\": 29, \"width\": 617, \"height\": 1053}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-030.webp\", \"caption\": \"\", \"page\": 9, \"index\": 30, \"width\": 499, \"height\": 853}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-031.webp\", \"caption\": \"\", \"page\": 9, \"index\": 31, \"width\": 497, \"height\": 855}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-032.webp\", \"caption\": \"\", \"page\": 9, \"index\": 32, \"width\": 501, \"height\": 858}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-033.webp\", \"caption\": \"\", \"page\": 9, \"index\": 33, \"width\": 1632, \"height\": 441}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-mj8vn4myro/fig-034.webp\", \"caption\": \"\", \"page\": 9, \"index\": 34, \"width\": 1630, \"height\": 419}]"
motivation: 端到端视频转音频生成难以捕捉视觉内容的细微差异，需要复杂推理。
method: 提出ThinkSound框架，将过程分解为三个阶段：基础拟音生成、交互式对象中心细化等。
result: 实现了高保真音频生成，能够真实捕捉视觉动态和声学环境。
conclusion: 思维链推理有效提升了多模态音频生成的真实性和可控性。
---

## Abstract
While end-to-end video-to-audio generation has greatly improved, producing high-fidelity audio that authentically captures the nuances of visual content remains challenging. Like professionals in the creative industries, this generation requires sophisticated reasoning about items such as visual dynamics, acoustic environments, and temporal relationships. We present **ThinkSound**, a novel framework that leverages Chain-of-Thought (CoT) reasoning to enable stepwise, interactive audio generation and editing for videos. Our approach decomposes the process into three complementary stages: foundational foley generation that creates semantically coherent soundscapes, interactive object-centric refinement through precise user interactions, and targeted editing guided by natural language instructions. At each stage, a multimodal large language model generates contextually aligned CoT reasoning that guides a unified audio foundation model. Furthermore, we introduce **AudioCoT**, a comprehensive dataset with structured reasoning annotations that establishes connections between visual content, textual descriptions, and sound synthesis.  Experiments demonstrate that ThinkSound achieves state-of-the-art performance in video-to-audio generation across both audio metrics and CoT metrics, and excels in the out-of-distribution Movie Gen Audio benchmark. The project page is available at https://ThinkSound-Project.github.io.

---

## 论文详细总结（自动生成）

# 论文总结：ThinkSound – 基于思维链推理的多模态大模型音频生成与编辑

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：端到端视频转音频（V2A）生成虽取得进步，但难以输出高保真、精细匹配视觉内容的音频。原因在于生成过程需要理解复杂的视觉动态、声学环境与时序关系（如“猫头鹰何时鸣叫、何时振翅”），而现有模型仅能识别物体，缺乏分层推理能力。
- **动机**：受专业声音设计师多阶段工作流程启发——先分析视觉内容，推理声学属性，再合成并精调音频——本文提出将**思维链（Chain-of-Thought, CoT）推理**引入多模态大模型，实现分步、交互式的音频生成与编辑。
- **整体含义**：该框架首次将复杂V2A任务显式分解为可解释的推理步骤，并通过用户交互实现精细控制，填补了自动化生成与创意意图之间的鸿沟。

## 2. 方法论：核心思想、关键技术细节与算法流程
### 核心思想
- **三步交互框架**：将音频生成分解为三个互补阶段，每个阶段由微调后的多模态大模型生成CoT推理，指导统一的音频基础模型。
  1. **阶段一：基础拟音生成** – 分析整个视频，生成语义和时序匹配的声音景观。
  2. **阶段二：交互式对象中心细化** – 用户通过点击选择特定物体，模型生成该物体的CoT推理，并将新声音与已有音频融合。
  3. **阶段三：基于指令的音频编辑** – 用户用自然语言发出添加、移除、修补、扩展等指令，MLLM将其转化为精确操作。

### 关键技术细节
- **多模态大模型（MLLM）**：基于VideoLLaMA2进行微调，在**AudioCoT**数据集上训练，使其具备音频中心理解、结构化CoT分解和多模态指令跟随能力。训练目标为标准交叉熵（下一个词预测）。
- **统一音频基础模型**：
  - 使用预训练VAE将音频编码为潜在表示，采用**条件流匹配**训练速度场预测。
  - 支持任意模态组合输入（视频、文本、音频），通过**分类器无关指导丢弃**实现。
  - 文本编码采用双路径：MetaCLIP编码视觉描述（场景级），T5-v1-xl编码结构化CoT（细节推理）。
  - 模型架构为增强的**MM-DiT（多模态扩散Transformer）**，包含多流Transformer块（各模态独立参数，共享注意力）和单流Transformer块。通过**门控融合**将视频特征上采样后与音频潜在特征融合，利用**AdaLN**注入全局条件（CLIP视频特征+同步特征）。

### 算法流程（文字描述）
1. **训练阶段**：先训练VAE（24 A800, 500k步），再冻结编码器训练解码器（500k步）。然后训练基础流匹配模型（8 A100, 100k步），最后任务微调（8 A100, 50k步）。
2. **推理阶段**：用户输入视频（或视频+ROI），MLLM生成对应阶段的CoT文本；该CoT经T5编码，与视觉特征、音频上下文等一起输入基础模型，经流匹配采样输出音频。

## 3. 实验设计
### 数据集
- **训练**：VGGSound（音频-视频）、AudioSet子集（非语音）、AudioSet-SL、Freesound、AudioCaps、BBC Sound Effects。构造了**AudioCoT**数据集，包含结构化CoT注释。
- **评测**：
  - **视频→音频**：VGGSound测试集（域内）和MovieGen Audio Bench（域外OOD）。
  - **对象聚焦生成**：从AudioCoT中选取含清晰声学物体的样本（约2000条）。
  - **音频编辑**：从AudioCoT中选取适合四种操作（添加、移除、修补、扩展）的样本（约2000条）。

### 对比方法
- **视频→音频**：See&Hear, V-AURA, FoleyCrafter, Frieren, V2A-Mapper, MMAudio（部分使用作者提供的生成结果，部分用官方模型复现）。
- **音频编辑**：AudioLDM-2, Edit Friendly DDPM（适配本文设置）。

### 评估指标
- **客观指标**：FD（OpenL3特征）、KL divergence（PaSST和PaNNs）、DeSync（Synchformer）、CLAP score（caption和CoT版本）。
- **主观指标**：MOS-Q（音频质量）、MOS-A（与视频和CoT的对齐）。15名评估员在受控环境下评分。

## 4. 资源与算力
| 训练阶段            | GPU类型及数量 | 步数         | 批大小 |
|---------------------|---------------|--------------|--------|
| VAE训练             | 24×A800       | 500,000      | 144    |
| VAE解码器微调       | 24×A800       | 500,000      | -      |
| 基础流匹配模型训练  | 8×A100        | 100,000      | 256    |
| 任务微调            | 8×A100        | 50,000       | 256    |

优化器均为AdamW，学习率分别为3e-5/6e-5（VAE）和1e-4（流匹配）。论文明确提供了以上细节。

## 5. 实验数量与充分性
- **实验组数**：主实验（表1）、OOD实验（表2）、对象聚焦（表3）、音频编辑（表4）共4项任务对比；消融实验包括：
  - 文本编码策略（CLIP vs T5 vs 两者，表5）
  - 多模态集成机制（仅音频、线性视频、门控视频，表6）
  - 模型大小（Small/Medium/Large，表9）
  - 难度级别（Easy/Medium/Hard，表10）
  - CoT粒度（粗 vs 细，表11）
  - CoT结构（随机/仅标签 vs 有序，表12）
  - CoT冗长度（表13）
  - MLLM推理质量人工+LLM评估（表14）
- **充分性评价**：实验设计系统，覆盖了主要任务、多种消融、不同难度和规模，且包含主观评估。对比方法选择主流最新模型，指标全面。论文还公开了数据集和代码计划，可复现性较好。总体充分且客观。

## 6. 主要结论与发现
1. **ThinkSound在V2A上达到SOTA**：在VGGSound上，FD、KL、CLAP_CoT等指标均优于所有基线，主观MOS也最高。
2. **CoT推理关键**：移除CoT（w/o CoT）后，各指标显著下降（如CLAP_CoT从0.46降至0.41），证明CoT提供了必要的语义和时序信息。
3. **对象聚焦与音频编辑同样领先**：在对象聚焦（表3）和音频编辑（表4）任务中，ThinkSound均优于对比方法。
4. **模型规模正相关**：大模型（1.3B）全面优于中型和微型模型（表9）。
5. **细粒度CoT优于粗粒度**：精细的CoT描述带来更好的对齐和质量（表11）。
6. **结构化CoT逻辑重要性**：随机打乱CoT句子或仅用标签会导致性能大幅下降（表12），证明T5编码器有效利用了推理结构。
7. **MLLM推理能力**：微调后的VideoLLaMA2在CoT生成质量上优于Qwen2.5-VL和Qwen2-Audio（表14）。

## 7. 优点（方法/实验亮点）
- **创新三步推理框架**：将复杂任务显式分解为可解释、可交互的步骤，用户可在各阶段进行点击或语言控制，直观且实用。
- **统一音频基础模型**：支持视频、文本、音频任意组合输入，通过CFG dropout和门控融合实现灵活性与高性能。
- **构造了AudioCoT数据集**：自动化流水线生成结构化CoT注释，并经过严格质量控制（CLAP过滤、人工校验），为有监督训练和未来研究提供基础。
- **全面的评估体系**：既包含标准客观指标（FD、KL、DeSync、CLAP），也包含主观MOS，且针对CoT特性引入了CLAP_CoT指标。
- **丰富的消融实验**：验证了CoT结构、粒度、模型规模、模态融合方式等多种设计选择的有效性，结论可靠。

## 8. 不足与局限
- **MLLM的时空理解局限**：当前MLLM在精确的事件时间戳定位和空间关系推断上仍不可靠，可能导致CoT中语义正确但时序不准。
- **数据集多样性与覆盖不足**：开源视频-音频数据集中于缺乏罕见或文化特定声音事件，模型可能在这些场景下表现不佳。
- **社会负面影响**：可能被用于生成虚假音频/深度伪造，加剧虚假信息传播；训练数据不平衡可能导致声音与特定群体的刻板关联。
- **评估数据量有限**：虽然设计了难度分级，但每个级别约2000样本，规模仍相对较小，OOD测试仅使用MovieGen Audio Bench一个基准。
- **计算资源需求高**：1.3B参数量的大模型训练需要多卡（24×A800 / 8×A100），部署成本较高。

（完）
