# 前沿四主题学术共识研究报告

> 主题：**Scaling Law 边界 / 世界模型 / 因果推理 / 具身学习**
> 窗口：2020–2026（2026 为 YTD，数据截至 `AS_OF = 2026-06-08`）
> 方法：三源论文计量（OpenAlex + Semantic Scholar + arXiv）+ arXiv `cs.*` 摘要文本挖掘
> 全部结论可回溯到本地数据文件；每条结论后附 `↩ 回溯` 指向具体文件与筛选条件。

---

## 一、本报告怎么得出"共识"——方法与判定流程

### 1.1 三个数据源（分两类，不可混比）

| 源 | 模态 | 角色 | 本地原始缓存 |
|---|---|---|---|
| **OpenAlex** | 论文计数（全领域） | 份额主源 | `data/raw/frontier/openalex/<KEY>_counts.json`、`<KEY>_top.json` |
| **Semantic Scholar** | 论文计数（全领域） | 份额交叉验证 | `data/raw/frontier/s2/<KEY>_<year>.json` |
| **arXiv** | 计数 + **全文摘要**（仅 `cs.AI/CL/LG`） | **论点文本挖掘主源** | `data/raw/frontier/arxiv/<KEY>_<year>_0.xml` |

检索词表是单一真相源：`data/frontier_queries.json`。三套 API 查询由 `scripts/_common.py` 各自从同一份 term list 拼装，不在脚本里散落硬编码词。抓取脚本 `scripts/fetch_frontier.py`，派生信号脚本 `scripts/analyze_frontier.py`。

### 1.2 五条方法纪律

1. **计数 ≠ 进度**。全 AI 语料 2020→2025 本身增长约 3.2×（`AI_DENOM` 224045→720665），绝对计数无意义——一律用分母归一化看**份额** `share = 主题计数 ÷ 全 AI 语料计数`。分母用"全 AI 语料"而非"LLM 语料"，因为世界模型 / 具身 / 因果并不全在 LLM 范畴内。
   `↩ 回溯：data/frontier_counts.csv 中 source=openalex,p_class=AI_DENOM`
2. **上升/下降都有歧义** → **三角验证**：份额（OpenAlex）+ 份额复核（S2）+ `cs.*` 摘要里的论点复述率。两个计数源方向一致才认定趋势真实。
3. **分清模态**：两个计数源只比"比值与形状"，不混用绝对数（OpenAlex 与 S2 覆盖/去重不同，绝对数本就有别）；摘要文本挖掘单列。
4. **当前年只有 YTD**：份额（分子分母同为 YTD）可纳入趋势方向；但 2026 斜率仅作"方向确认"，**硬结论只用到 2025 完整年**。
5. **无 ground-truth 时，"共识"降级为"注意力收敛 + 措辞收敛"**——本报告不声称这些方向"已解决"，只声称"学界在如何框定问题上趋于一致"。

### 1.3 一条论点被认定为"共识候选"的判据

同时满足三条，才写进"共识"小节：

- **(a) 复述率**：在 arXiv `cs.*` 摘要语料里被多篇独立论文复述，命中率 `prevalence` 见 `data/frontier_claims.csv`；
- **(b) 综述背书**：至少一篇**综述 / 立场论文**（标题含 survey/review/roadmap/perspective/position）显式陈述该方向，清单见 `data/frontier_surveys.txt`（共 62 篇）；
- **(c) 引用锚点**：存在一篇被引领先的奠基/代表作可指认，见 `data/frontier_top_papers.jsonl`（每主题在题过滤后保留 50 篇，按 `cited_by_count` 降序）。

三者皆有 → 认定"共识候选"，并由人工核读综述原文确认其措辞确为"领域共识"。任一缺失 → 降级为"仍有分歧 / 单点主张"。

### 1.4 本地数据产物清单（结论回溯地图）

| 文件 | 内容 | 行数/规模 |
|---|---|---|
| `data/frontier_counts.csv` | 三源逐年计数 `p_class,name,source,year,count` | 98 行 |
| `data/frontier_share.csv` | 份额 `p_class,source,year,topic,denom,share`（OpenAlex+S2） | 56 行 |
| `data/frontier_claims.csv` | 论点复述率 `p_class,claim_id,n_hits,n_total,prevalence` | 20 行 |
| `data/frontier_genre.csv` | 体裁 `p_class,year,n_sampled,n_survey,n_open` | 28 行 |
| `data/frontier_surveys.txt` | 抽样到的 62 篇综述/立场论文（题目+年） | 62 行 |
| `data/frontier_arxiv_abstracts.jsonl` | **3135** 条 `cs.*` 摘要（论点挖掘原料） | 3135 行 |
| `data/frontier_top_papers.jsonl` | 4 主题各 50 篇在题顶引论文（含摘要、DOI） | 200 行 |
| `data/raw/frontier/**` | 三源 API 原始响应（gitignore，可重抓） | — |

---

## 二、注意力趋势总览（四主题份额）

份额 `share × 10⁴`（即每万篇 AI 论文中的占比），两计数源并列以做三角验证。
`↩ 回溯：data/frontier_share.csv（source 列区分 openalex/s2）`

| 主题 | 源 | 2020 | 2021 | 2022 | 2023 | 2024 | 2025 | 2026(YTD) | 形状 |
|---|---|---|---|---|---|---|---|---|---|
| **SCALE** | OpenAlex | 88.1 | 77.1 | 71.9 | 59.9 | 54.8 | 72.4 | 109.1 | **U 形**：先降后升 |
| | S2 | 87.8 | 71.1 | 56.4 | 48.8 | 46.7 | 50.1 | 72.5 | U 形（一致） |
| **WORLD** | OpenAlex | 3.0 | 3.3 | 3.3 | 5.8 | 7.5 | 12.1 | 21.5 | **单调陡升** ~7× |
| | S2 | 2.9 | 3.4 | 4.2 | 6.0 | 8.3 | 12.5 | 25.4 | 单调陡升（一致） |
| **CAUSAL** | OpenAlex | 13.9 | 14.1 | 15.9 | 17.1 | 20.4 | 34.9 | 32.7 | **稳升 + 2025 跳变** |
| | S2 | 12.7 | 13.0 | 15.1 | 16.1 | 18.5 | 30.3 | 37.0 | 稳升（一致） |
| **EMBODIED** | OpenAlex | 22.1 | 20.5 | 20.2 | 21.1 | 27.3 | 45.9 | 72.7 | **先平后陡升** |
| | S2 | 22.4 | 20.1 | 21.4 | 21.8 | 28.6 | 49.2 | 100.5 | 先平后陡升（一致） |

**两源方向完全一致**，趋势判定成立。`cs.*` 干净计数（仅预印本）形状同向，可佐证：
`↩ 回溯：data/frontier_counts.csv，source=arxiv`

- SCALE arXiv：33→46→69→163→368→512→374(YTD)
- WORLD arXiv：16→42→55→115→194→349→273(YTD)
- CAUSAL arXiv：61→78→107→174→244→385→202(YTD)
- EMBODIED arXiv：63→82→118→202→356→700→578(YTD)

> **总览结论**：四主题里，**世界模型**与**具身学习**是注意力增速最快的两条线（份额 7× / 3×+），**因果推理**稳升并在 2025 出现跳变，**Scaling Law** 走出独特的 U 形——这条 U 形本身就是下一节的核心论点。

---

## 三、Scaling Law 边界

### 3.1 趋势读解：U 形不是噪声，是范式转折

SCALE 份额 2020→2024 持续**下降**（88→55，OpenAlex），2025–2026 强烈**反弹**（72→109）。两源同向。下降段对应"既然 scaling 有效，按 Kaplan/Chinchilla 定律照着放大即可"的工程共识期——讨论"定律本身"的论文占比反而缩小；反弹段对应预训练边际收益见顶、讨论"边界在哪 / 怎么绕过"重新成为显学。
`↩ 回溯：data/frontier_share.csv，p_class=SCALE`

引用锚点（奠基三件套，均在本地顶引表）：
`↩ 回溯：data/frontier_top_papers.jsonl，p_class=SCALE`
- *Scaling Laws for Neural Language Models*（2020，被引 1503）——幂律奠基。
- *Emergent Abilities of Large Language Models*（2022，被引 1029）——涌现能力命题。
- *Training Compute-Optimal Large Language Models*（Chinchilla，2022，被引 662）——计算最优配比。

### 3.2 共识论点

**C-SCALE-1：预训练单纯放大的边际收益在递减，"数据墙"是真约束。**
复述率 `S-pretrain-plateau = 53/748 = 7.1%`。代表作如 *Perplexity-Aware Data Scaling Law*（2025）明确"simply increasing data for CPT 的边际增益 diminish rapidly"。综述层面由 *A Comprehensive Survey of Small Language Models*（2024）等背书：大模型受参数/算力/隐私所限，需转向更高效路径。
`↩ 回溯：data/frontier_claims.csv（SCALE,S-pretrain-plateau）；data/frontier_arxiv_abstracts.jsonl 搜 "diminish rapidly"；data/frontier_surveys.txt SCALE 2024`

**C-SCALE-2：算力的去向正从"预训练"转向"推理/测试时计算"。**
复述率 `S-testtime-shift = 20/748 = 2.7%`（绝对率不高，但 2025 后集中出现，且与 Large Reasoning Model 浪潮吻合）。代表作 *Energy-Aware Routing to Large Reasoning Models*、*Parallel Scaling Law*（2025）把"RL 后训练 + 长链推理"作为新的扩展轴。
`↩ 回溯：data/frontier_claims.csv（SCALE,S-testtime-shift）；data/frontier_arxiv_abstracts.jsonl 搜 "Large Reasoning Models"`

**C-SCALE-3：小模型/蒸馏是与"更大"并列的合法前沿，而非退而求其次。**
复述率 `S-distill-small = 55/748 = 7.4%`。*A Comprehensive Survey of Small Language Models in the Era of LLMs*（2024）以整篇综述确立 SLM 作为独立研究方向。
`↩ 回溯：data/frontier_claims.csv（SCALE,S-distill-small）；data/frontier_surveys.txt SCALE 2024`

**C-SCALE-4：计算最优配比（compute-optimal）是讨论扩展时的默认坐标系。**
复述率最高 `S-compute-optimal = 96/748 = 12.8%`，Chinchilla 框架已成为几乎所有 scaling 讨论的公共语言。
`↩ 回溯：data/frontier_claims.csv（SCALE,S-compute-optimal）`

### 3.3 仍有分歧

**涌现能力是真相还是度量假象**——这是 SCALE 内**最高热度的未决争论**：复述率 `S-emergent-debate = 93/748 = 12.4%`，仅次于 compute-optimal。本地语料同时含命题方 *Emergent Abilities of LLMs*（2022）与反方 ***Are Emergent Abilities of Large Language Models a Mirage?***（2023，指出"sharpness/unpredictability 可能是非线性度量造成的假象"）。两方共存即说明：**"涌现是否真实"尚无共识，只有共识性的争论框架**。
`↩ 回溯：data/frontier_arxiv_abstracts.jsonl，p_class=SCALE 搜 "Mirage" 与 "Emergent Abilities"；data/frontier_claims.csv（SCALE,S-emergent-debate）`

---

## 四、世界模型（World Models）

### 4.1 趋势读解：从边缘概念到 AGI 中心范式

WORLD 份额从 2020 的 3.0 升到 2026 的 21.5（OpenAlex，~7×），两源同向单调上升，是四主题里增速最快的。`cs.*` 计数 16→349（2025），增速同样最猛。多篇 2024–2026 综述把世界模型直接定位为"通往 AGI 的中心范式"。
`↩ 回溯：data/frontier_share.csv，p_class=WORLD`

### 4.2 共识论点

**C-WORLD-1（最强共识）：世界模型 = 智能体内部用于"预测/规划/想象"的环境模拟器。**
复述率 `W-modelbased-plan = 285/678 = 42.0%`（全报告所有论点中最高）——近半数摘要把世界模型与 model-based RL / planning / imagination / rollout 绑定。综述 *World Models: A Comprehensive Survey*（2026）开宗明义："internal simulators that learn the structure and dynamics of an environment, enabling agents to predict, plan, and reason within learned representations"。
`↩ 回溯：data/frontier_claims.csv（WORLD,W-modelbased-plan）；data/frontier_surveys.txt WORLD 2026`

**C-WORLD-2：视频生成模型正被重新诠释为"隐式世界模型"。**
复述率 `W-video-as-world = 75/678 = 11.1%`。立场综述 *Simulating the Visual World with AI: A Roadmap*（2025）明确"video foundation models function as implicit world models, simulating physical dynamics, agent-environment interactions and task planning"。Sora 类模型是这条共识的催化剂。
`↩ 回溯：data/frontier_claims.csv（WORLD,W-video-as-world）；data/frontier_surveys.txt WORLD 2025`

**C-WORLD-3：世界模型有"理解现状"与"预测未来"双功能划分。**
综述 *Understanding World or Predicting Future? A Comprehensive Survey of World Models*（2024）把领域系统地二分为"构建内部表征以理解世界机制"与"预测未来动态"两类——这一分类法被后续综述沿用，构成领域公共词汇。
`↩ 回溯：data/frontier_surveys.txt WORLD 2024；data/frontier_arxiv_abstracts.jsonl 搜 "Understanding the present state"`

### 4.3 仍有分歧

**生成内容的物理一致性/长程一致性是公认软肋，但无公认解法。**
复述率 `W-consistency-fail = 39/678 = 5.8%`，*World Models: The Safety Perspective*（2024）专门审视其可靠性。**"隐式 vs 显式""像素空间 vs 隐空间（JEPA 路线）"路线之争未定**：JEPA/隐空间预测复述率仅 `W-jepa-latent = 16/678 = 2.4%`，"通用世界模型"仅 `3.4%`——说明架构尚未收敛。
`↩ 回溯：data/frontier_claims.csv（WORLD,W-consistency-fail / W-jepa-latent / W-general-world）；data/frontier_surveys.txt WORLD 2024 "Safety Perspective"`

---

## 五、因果推理（Causal Reasoning）

### 5.1 趋势读解：稳升 + 2025 跳变

CAUSAL 份额 13.9（2020）→ 34.9（2025，OpenAlex），两源同向，2025 出现明显跳变——与"用 LLM 做因果"成为热点同步。引用锚点 *Toward Causal Representation Learning*（Schölkopf 等，2021，被引 **1007**，本地顶引表第一）是该方向的理论纲领。
`↩ 回溯：data/frontier_share.csv，p_class=CAUSAL；data/frontier_top_papers.jsonl，p_class=CAUSAL 首条`

> ⚠ **口径声明**：OpenAlex/S2 的 CAUSAL 计数含医学/流行病学的"causal inference"（即便加了 AI scope，仍有提及 ML 的医学论文混入）。因此 CAUSAL 的**绝对份额偏高**。但 arXiv `cs.*` 干净计数（61→385）方向一致，趋势结论不依赖被污染的绝对值。`↩ 回溯：见 §七 威胁有效性`

### 5.2 共识论点

**C-CAUSAL-1：反事实/干预是因果推理的操作化核心。**
复述率最高 `C-counterfactual = 281/846 = 33.2%`——三分之一摘要触及 counterfactual / intervention / do-operator，Pearl 的干预语义已是领域公共语言。
`↩ 回溯：data/frontier_claims.csv（CAUSAL,C-counterfactual）`

**C-CAUSAL-2：因果发现（causal discovery）已从传统约束法转向深度学习驱动。**
复述率 `C-discovery = 252/846 = 29.8%`。综述 *A Review and Roadmap of Deep Learning Causal Discovery*（2022）明确"causal discovery 已从传统方法迁移到深度学习的模式识别范畴"。
`↩ 回溯：data/frontier_claims.csv（CAUSAL,C-discovery）；data/frontier_surveys.txt CAUSAL 2022`

**C-CAUSAL-3：因果表征学习（CRL）以可识别性（identifiability）为中心难题。**
复述率 `C-crl-identify = 119/846 = 14.1%`。代表作如 *Unsupervised Causal Representation Learning via Latent Additive Noise Model*（2025）直指"从观测数据解耦因果变量的可识别性"为核心挑战，呼应 Schölkopf 纲领。
`↩ 回溯：data/frontier_claims.csv（CAUSAL,C-crl-identify）；data/frontier_arxiv_abstracts.jsonl 搜 "identifiability"`

**C-CAUSAL-4：因果是修复 RL/LLM"靠相关性决策"短板的公认药方。**
综述 *Unifying Causal Reinforcement Learning*（2025）把"传统 RL 依赖 correlation-driven decision-making，在分布漂移/混杂下失效"作为引入因果的动机；这是"因果 × 下游任务"的共识叙事。
`↩ 回溯：data/frontier_surveys.txt CAUSAL 2025 "Unifying Causal Reinforcement Learning"`

### 5.3 仍有分歧

**LLM 究竟"会不会"因果推理，是公认的开放问题。**
复述率 `C-llm-not-causal = 80/846 = 9.5%`：大量论文指出 LLM "依赖 spurious correlations / shortcut 而非真正因果"（如 *CIP*（2025）把幻觉归因于"rely on spurious correlations rather than genuine causal relationships"）。同时 `C-llm-aid-causal = 5.4%` 的论文又主张 LLM 能为因果发现提供先验。综述 *Causal MAS*（2025）直言 LLM 因果能力"remains an area of active development"。**"LLM 是因果鹦鹉"与"LLM 是因果助手"两种立场并存，未收敛。**
`↩ 回溯：data/frontier_claims.csv（CAUSAL,C-llm-not-causal / C-llm-aid-causal）；data/frontier_surveys.txt CAUSAL 2024/2025`

---

## 六、具身学习（Embodied Learning）

### 6.1 趋势读解：先平台期，后 2024–2026 陡升

EMBODIED 份额 2020–2023 基本持平（~20–22），2024 起陡升至 2026 的 72.7（OpenAlex）/100.5（S2）。`cs.*` 计数 2024→2025 几乎翻倍（356→700）。这条线对应 VLA（Vision-Language-Action）模型范式确立后的爆发。早期范式由 *A Survey of Embodied AI: From Simulators to Research Tasks*（2021）定调："从 internet AI 到 embodied AI 的范式转移——通过与环境交互、以自我中心感知学习。"
`↩ 回溯：data/frontier_share.csv，p_class=EMBODIED；data/frontier_surveys.txt EMBODIED 2021`

### 6.2 共识论点

**C-EMB-1：Vision-Language-Action（VLA）模型是当前通用具身控制的主流范式。**
复述率 `E-vla = 199/863 = 23.1%`。综述 *From Human Videos to Robot Manipulation*（2026）："generalizable embodied control has been driven by large-scale pretraining of Vision-Language-Action models." VLA 是 2024 后爆发的直接载体。
`↩ 回溯：data/frontier_claims.csv（EMBODIED,E-vla）；data/frontier_surveys.txt EMBODIED 2026`

**C-EMB-2：数据稀缺（机器人演示昂贵）是公认的头号瓶颈。**
复述率 `E-data-bottleneck = 188/863 = 21.8%`。共识叙事："robot demonstrations 昂贵且与具体本体强耦合"，故转向人类视频、遥操作、internet-scale 数据。
`↩ 回溯：data/frontier_claims.csv（EMBODIED,E-data-bottleneck）；data/frontier_arxiv_abstracts.jsonl 搜 "costly to obtain"`

**C-EMB-3：基础模型/大规模预训练范式已迁移进机器人学。**
复述率 `E-foundation = 171/863 = 19.8%`。*Large Language Models for Robotics: A Survey*（2023）、*ChatGPT for Robotics*（2024，被引 429，本地顶引表）确立"用基础模型驱动机器人"为主线。
`↩ 回溯：data/frontier_claims.csv（EMBODIED,E-foundation）；data/frontier_top_papers.jsonl，p_class=EMBODIED 搜 "ChatGPT for Robotics"`

### 6.3 仍有分歧

**对未见场景的泛化/鲁棒性是公认最大短板，且无定论。**
复述率最高的恰是问题面 `E-eval-gap = 278/863 = 32.2%`：大量摘要围绕 generalization-to-unseen / brittle / out-of-distribution / long-horizon。值得注意的是 **sim-to-real 复述率仅 `E-sim2real = 27/863 = 3.1%`**——相对 2021 前的具身研究，注意力已从"跨越仿真-现实鸿沟"显著转移到"用大规模真实/人类数据直接学"。这是一个**范式重心的迁移信号**，而非该问题已解决。
`↩ 回溯：data/frontier_claims.csv（EMBODIED,E-eval-gap / E-sim2real）`

---

## 七、威胁有效性（必读）

1. **关键词 precision/recall**：主题用短语+布尔检索，CAUSAL/WORLD 加了 AI scope 仍非完美。**CAUSAL 的 OpenAlex/S2 绝对份额被医学因果推理抬高**（顶引表里 Mendelian randomization 等流行病学论文混入即为证）——故 CAUSAL 趋势以 arXiv `cs.*` 干净计数为准，绝对份额仅供形状参考。
   `↩ 回溯：data/raw/frontier/openalex/CAUSAL_top.json`
2. **OpenAlex 顶引排序被邻接高被引领域污染**：`*_top.json` 经在题子串过滤后仍有个别离题命中（如 WORLD 混入 PyTorch、SCALE 混入纳米颗粒论文）。本报告**只指认明确在题的奠基作**（Kaplan/Wei/Chinchilla/Schölkopf 等），未用被污染条目下结论。
   `↩ 回溯：data/frontier_top_papers.jsonl`
3. **arXiv 偏 CS + 抽样上限**：摘要文本挖掘仅覆盖 `cs.AI/CL/LG`，且每 (主题,年) 最多抽 150 篇近期论文（高产年是"最近 150 篇"切片）。复述率 `prevalence` 是**相对信号**，不是总体真值；跨主题比较复述率需谨慎（分母不同）。
   `↩ 回溯：data/frontier_genre.csv 的 n_sampled 列；data/raw/frontier_run.log 的 sampled 数`
4. **探照灯效应**：复述率高只说明"被讨论得多"，不等于"被解决"。本报告据此把所有论点分为"共识候选（如何框定问题趋同）"与"仍有分歧（答案未定）"，**不声称任何主题已解决**。
5. **2026 为 YTD**：份额纳入趋势方向，但硬斜率只用到 2025；2026 数字标注 YTD。
   `↩ 回溯：data/frontier_queries.json 的 as_of 字段`
6. **复述率正则的召回不完备**：claim 正则是保守匹配，会漏掉换词表达，故复述率是**下界**。原始判定可在 `analyze_frontier.py` 的 `CLAIMS` 字典核对、并对 `frontier_arxiv_abstracts.jsonl` 重跑复算。

---

## 八、一页结论

| 主题 | 注意力形状（两源一致） | 最强共识（复述率） | 最大未决争论（复述率） |
|---|---|---|---|
| **Scaling Law 边界** | U 形：先降后升 | compute-optimal 是公共坐标系（12.8%） | 涌现能力是真相还是度量假象（12.4%） |
| **世界模型** | 单调陡升 ~7× | 世界模型=可预测/规划的内部模拟器（42.0%） | 物理/长程一致性 + 隐空间vs像素路线之争（5.8%/2.4%） |
| **因果推理** | 稳升 + 2025 跳变 | 反事实/干预为操作核心（33.2%） | LLM 是"因果鹦鹉"还是"因果助手"（9.5%/5.4%） |
| **具身学习** | 先平后陡升 | VLA 为主流范式（23.1%） | 对未见场景的泛化/鲁棒（32.2%） |

> **跨主题的元共识**：四个方向都在把"单纯把模型做大"替换为**结构化的、可预测/可规划/可干预的世界表征**——世界模型给"可预测/规划"，因果推理给"可干预",具身学习给"与环境交互的接地"，而 Scaling Law 的 U 形反弹正是"放大本身不够"这一认识的计量投影。这一元共识由四主题各自的复述率与综述叙事共同支撑，可回溯到上述各 `↩` 标注的本地数据。

---

*复现：`python3 scripts/fetch_frontier.py`（重抓三源，缓存入 `data/raw/frontier/`）→ `python3 scripts/analyze_frontier.py`（离线重算派生信号）。词表改 `data/frontier_queries.json`，论点正则改 `scripts/analyze_frontier.py` 的 `CLAIMS`。*
