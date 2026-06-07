# 校准与精度笔记（数据可信度自查）

> 配套 `data/research-P.md` 一起读。本文件记录检索口径的校准结果、已知精度问题、各源的坑。
> 证据级别标注：**实测**＝本管线实跑验证；**推断**＝从样本外推。

## 1. OpenAlex 布尔语义（实测，2024 窗口）

`title_and_abstract.search` 的运算符行为已实跑确认：

| 写法 | 含义 | 命中数 | 结论 |
|---|---|---|---|
| `hallucination` | 单词 | 5612 | — |
| `hallucination OR confabulation` | 并集 | 5680 | **OR（大写）生效**（>单词）|
| `hallucination confabulation` | 空格 | 19 | **空格＝AND**（交集）|
| `"large language model"` | 引号短语 | 44069 | **短语精确匹配生效** |
| `(hallucination OR confabulation) "large language model"` | 括号+短语 | 2134 | **括号分组生效** |

→ 故所有 P 的 OpenAlex 查询用 `(同义词 OR …) (scope OR …)` 形式，scope 为 `"large language model" OR LLM OR "foundation model"`。**这是计数主源，精度最高。**

## 2. 分母选择（实测）

LLM 总语料 = `"large language model" OR LLM OR "foundation model"`。2024 命中：
- 仅 `"large language model"` → 44069
- 加 `LLM` → 50114
- 再加 `"foundation model"` → 54595（采用）

逐年（OpenAlex）：2018=405 … 2023=16370 … 2025=116438，**2018→2025 增长约 288×**。这正是"绝对计数无意义、必须看份额"的根据。

## 3. 各 P 检索精度抽查（实测 + 推断）

抽查 2025 年 arXiv 命中前几条标题，人工判断切题度：

- **干净**（specific 词表）：P8 安全（jailbreak / prompt injection）、P10 多智能体（multi-agent）、P7（quantization / sparse inference）、P6（evaluation / knowledge statements）。
- **偏噪**（generic 词渗透）：P1 幻觉、P4 grounding、P5 不可控——这些类的 arXiv `sortBy=submittedDate` 把最新的泛 LLM 论文也捞进来（同一篇 "Human-LLM Agent Collaboration" 同时落入 P1/P2/P4/P5）。
- **特别说明 P5**：`reproducibility / controllability` 对全域 ML 语义渗透强，2018–2021 就有非零基线（OpenAlex 2020 份额 14.8%），份额绝对值偏高，**只可看趋势不可看绝对水平**。

> ⚠ **声明**：正因 arXiv 文本挖掘（体裁/开放措辞/新基准）精度不均，**判定矩阵的分类只以 OpenAlex 份额为主信号**，三个文本信号仅作描述性参考、不作硬判据。

## 4. Semantic Scholar（实测，交叉验证用）

- bulk endpoint 免费但**限流紧**：连续请求触发 429，已实现指数退避（2→4→…→32s）。少数单元格仍失败置 `None`（如 P10/2023）。
- S2 与 OpenAlex **绝对数不同**（覆盖、去重口径不同），**只用于核对形状/量级，不混用绝对数**。

## 5. arXiv（实测）

- **纯 http 端点在本机被拦**；必须 `https://export.arxiv.org` + 自定义 `User-Agent`。
- 调用间 sleep 3s（礼貌规范）。摘要为**抽样**：每 P 每年 ≤150 篇（2020–2026），共 6029 篇。比率信号带抽样噪声。
- 用 `submittedDate:[YYYY01010000 TO YYYY12312359]` 做年份切分。

## 6. 基准源（实测）

- **Epoch AI** `benchmark_data.zip`（~340KB）可直接下；每基准 CSV 含 `Model version / mean_score / Release date`，做 running-max 前沿曲线。
- **Papers with Code 转储不可得**：production-media 主机 TLS 握手失败、HF 镜像 401 gated → 本轮放弃，缺口（P3/P5/P7/P8/P9/P10）显式标注"无基准裁决"。
- 元数据修正：SimpleQA 为静态题库，`contamination_resistant=false`；FrontierMath 在 P6 下标 `reference_only`（只作评测可信度参照，不给 P6 本身分类）。

## 7. 2026 = 年初至今（YTD，实测口径）

实跑核验（分母 LLM 总语料，查询当日 = 2026-06-07）：

| 口径 | 窗口 | 计数 |
|---|---|---|
| 图里的 2026 | 全年 01-01→12-31 | 120,911 |
| 截至当日 | 01-01→06-07 | 120,803（99.9%）|
| 未来刊期 | 06-08→12-31 | 108 |
| 对照：2025 全年 | — | 116,438 |

→ **2026 柱实质就是 YTD 累计，不可与整年直接比。**

两个要点：
1. **年初假峰（每年都有）**：`2026-01-01` 单日堆 14,136 篇（`2025-01-01`=18,757、`2024-01-01`=8,651）——OpenAlex 把"只知年份、缺月份"的论文默认压到 01-01。这是逐年系统性偏差，不是 2026 独有。
2. **真实增长仍陡**：单 `2026-05` 一月即 24,817 篇，约 2025 月均（~9,700）的 2.5×，故才 ~5.4 个月就超 2025 全年。

**所有斜率仅用 2023–2025 窗口**；2026 仅在表/图中展示、不参与判定。

## 8. 复现

**入库的是结论性产物**：`*_counts.csv`、`benchmark_frontier.csv`、`trends_summary.csv`、`research-P.md`、配置 json、脚本。
**不入库的是可再生缓存**（已 gitignore）：`data/raw/`（各 API 原始响应）与 `data/arxiv_abstracts.jsonl`（9MB 摘要语料）。

- 本机（缓存尚在）：`venv/bin/python scripts/analyze_trends.py` 可离线重算。
- 全新克隆（无缓存）：先重新抓取 `fetch_openalex.py` / `fetch_s2.py` / `fetch_arxiv.py` / `fetch_benchmarks.py`（零第三方依赖，纯 stdlib），再跑 analyze。计数微有漂移属正常（论文库持续增补）。
