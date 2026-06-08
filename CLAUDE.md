# CLAUDE.md — AI Methodology for Software Development

> 项目级写作与协作规范。读完这一份就能直接接着写下一节。
> 全局规范（语言、日期格式等）见 `~/.claude/CLAUDE.md`。本文件**附加**，不覆盖。

---

## 项目结构

```
README.md                          # 目录页 + 各章 TOC（自动生成区 + 手写区）
chapter-NN-<slug>.md               # 每章一个文件，两位数章号 + 短 slug
scripts/update_toc.py              # TOC 重新生成脚本（无第三方依赖，Py 3.8+）
scripts/_common.py                 # 学术数据管线共享工具（配置/查询拼装/HTTP/缓存）
scripts/fetch_*.py                 # 四源抓取：openalex / s2 / arxiv / benchmarks
scripts/analyze_trends.py          # P1–P10 派生信号 + 判定矩阵 + 5 图（含 PNG）
scripts/fetch_frontier.py          # 前沿四题（F1–F4）抓取，平行管线、复用 _common
scripts/analyze_frontier.py        # 前沿四题分析 → frontier_*.csv + research-frontier.md
data/queries.json                  # P1–P10 检索词表（单一真相源）
data/queries_frontier.json         # F1–F4 前沿四题检索词表（单一真相源）
data/benchmark_map.json            # P → Epoch 基准映射
data/*.csv, data/research-P.md     # 数据产物 + 图文（入库）
data/frontier_*.csv, research-frontier.md  # 前沿四题数据产物（入库；abstracts.jsonl 同 P，gitignore）
data/figures/*.png                 # PNG 图（入库）
data/raw/**                        # API 原始缓存（gitignore，可重抓）
.claude/commands/update-toc.md     # 斜杠命令：/update-toc
CLAUDE.md                          # 本文件
```

- 章节文件全部在仓库根，不嵌套到 `chapters/` 子目录。
- 写作只编辑 `chapter-NN-*.md` 与 `README.md`；脚本与命令位置稳定不动。

## 章节文件骨架

```
# 第N章　<标题>                  ← H1，章号与标题之间用全角空格
> <章导言（1–2 段卷首语）>

## N.1 起点 / 一、…              ← H2 节；"中文数字"或"N.M"任选其一，同章保持一致
### N.M.K …                       ← H3 小节
#### N.M.K.L …                    ← H4 子目（TOC 默认会包含）

## 参考文献                       ← 每章独立 IEEE 引用，编号从 [1] 开始
```

## 标题与编号

- 一级章名用中文（`第一章`、`第二章`），H1 行内全角空格分隔：`# 第一章　绪论`。
- 节级编号在同一章内**保持单一风格**：要么全部 `## 一、` `## 二、`，要么全部 `## 1.1` `## 1.2`。
- H3 / H4 / H5 全部用 ASCII 数字：`### 1.4.1`、`#### 1.4.3.1`。
- 跨章引用：`§X.Y` 或 `第X章 P9`。
- 第一章建立的两套编号体系，全书复用：
  - **P1–P10**：十类 LLM 基础病
  - **C1–C30**：周边工程化概念

## 中英文混排排版

- 中英 / 中数 之间**保留半角空格**：`AI 在`、`token 数`、`~170k tokens`、`§2.1.1 的 jagged 形态`。
- 句末标点用全角：`。，：；？`。
- `**粗体**` 标论点关键词，`*斜体*` 标英文术语原词。
- 关键结论用 `> blockquote` 单独成段。
- 引用数据要给"研究 + 样本量 + 数字 + 时间"四元组：例 _"Xia et al. TSE 2018，7 项目、79 开发者、~58% 时间花在阅读理解"_。

## 引用规范（IEEE 风格）

- 行内：`[N]` 数字方括号；并列 `[N], [M]`。
- **正文中的 `[N]` 一律渲染为指向出处 URL 的超链接**，格式 `[[N]](URL)`；URL 取自参考文献条目里 `Available: <URL>` 的部分。无 URL 的条目（书籍、纸刊未上网者）正文里保留纯 `[N]`。
- **一个段落多个论点时，每个论点至少配一条引用**，不要堆一行 `[N]` 在末尾。
- 每章引用编号**独立从 `[1]` 开始**，章内严格递增，不重新洗牌。
- 找不到直接证据时：用相邻领域类比并显式声明：

  > ⚠ **声明**：本节判断是从 X 类比推断，仍需面向 Y 做实证评估。

- 条目格式：
  ```
  [N] Authors, "Title," *Venue*, vol., no., pp., Month Year. [Online]. Available: <URL>
  ```
- arXiv：
  ```
  [N] X. Y et al., "Title," *arXiv preprint*, arXiv:NNNN.NNNNN, MMM YEAR. [Online]. Available: <https://arxiv.org/abs/NNNN.NNNNN>
  ```
- 当引用支撑了**具体数字**，把数字写进条目末尾的括号注释（方便读者抽查）：
  ```
  [18] Xia et al., "...", IEEE TSE 2018. (7 projects, 79 devs, 3244 hours; ~58% time on comprehension.) [Online]. Available: <...>
  ```
- **经典文献引用原文**（Brooks, Conway, Lehman, Parnas, Gray, Mitnick, Miller, Cowan, Hofstadter…），不要引二手综述。
- **当代论点配近 1–2 年 arXiv / 顶会 / 顶级博客**（Karpathy, Mollick, Chollet, Anthropic / OpenAI 官方等）。

## 可视化（mermaid）

- 复杂关系优先 mermaid：`sankey-beta` / `radar-beta` / `xychart-beta` / `flowchart` / 表格。
- 图前一段"读图说明"，图后一段"由此推导"。
- **节点名含 `/`、`-`、空格、中文标点时用双引号包裹**：`"C2 CoT_ToT"`、`"P1 Hallucination"`。
- mermaid 节点 **ID 必须是 ASCII**；显示标签可中文。ID 用中文常导致解析失败。
- Sankey 隐藏数字：`config: sankey: showValues: false`。
- xychart 跨多个数量级：默认线性 + 文字解释；线性差距大到一根尺子量不出时，改 log10 并在文字里注明。
- 雷达图：人类基准恒取 5，对比对象在 0–10 间相对浮动；标注分数是主观估计、仅用于呈现形状。

## 学术研究数据：收集与分析方法

> 当某个论点需要"论文量随时间 / 能力随时间"的**量化背书**时（典型：判断 P1–P10 是否趋向解决），用本管线，别手凑数字。管线在 `scripts/`，产物在 `data/`，零第三方依赖（部分图的 PNG 渲染需可选 `matplotlib`，缺失则自动回退 mermaid/表格）。

### 四个数据源（分两类）

**三个"论文计数"源（可直接比）+ 一个"基准分数"源（不可与计数比）**：

| 源 | 模态 | 覆盖 | 用途 | 端点 / 坑（已实测） |
|---|---|---|---|---|
| **OpenAlex** | 论文计数 | 全领域（刊+会+预印） | **计数主源** | `api.openalex.org/works?filter=title_and_abstract.search:<q>&group_by=publication_year`；免 key，**加 `mailto=` 进礼貌池**；布尔：`OR`(大写)/空格=AND/`"短语"`/`()` 均生效；只知年份的论文压到 `01-01` 假峰 |
| **Semantic Scholar** | 论文计数 | 全领域，略窄 | 交叉验证 | `/graph/v1/paper/search/bulk?query=<q>&year=`；bulk 限流，**429 须指数退避** |
| **arXiv** | 计数 + **全文摘要** | **仅 cs.\* 预印本** | **摘要文本挖掘**（综述/开放措辞/新基准） | **必须 https + 自定义 User-Agent**（纯 http 被拦）；调用间 `sleep 3s`；`submittedDate:[YYYYMMDD0000 TO …]` 切年 |
| **Epoch AI** | **基准分数 over time** | 前沿模型×基准 | 能力前沿"硬"裁决 | `epoch.ai/data/benchmark_data.zip`；每基准 CSV 有 `Release date`+`mean_score`，做 **running-max** 前沿曲线 |

> Papers with Code 转储已不可得（主机 TLS 失败 / HF 镜像 401）；缺口基准用各官方榜手工补，并**显式标注"无基准裁决"**。

### 五条方法纪律（核心，违反则结论无效）

1. **计数 ≠ 进度**。整领域 2018→2025 增长约 288×，绝对计数毫无意义——**必须用分母归一化**（分母 = LLM 总语料 `"large language model" OR LLM OR "foundation model"`），看**份额** share = P 计数 ÷ 分母。
2. **上升、下降都有歧义**（上升＝热门未解 vs 盘子变大；下降＝已解决 vs 被放弃）→ **三角验证**：份额（OpenAlex）+ 体裁/开放措辞/新基准（arXiv 摘要）+ 基准饱和（Epoch）。三者合看才能区分"趋向解决 / 热门未解 / 被放弃"。
3. **分清模态**：计数源之间可比（报**比值与形状**，不混用绝对数——OpenAlex×S2 实测 r≈1.00、S2≈OpenAlex 的 69%）；基准源与计数**正交**，单列。
4. **当前年只有 YTD，分清"计数 vs 比值"**：①**绝对计数**（如总量图）当年不全，不可与整年比——要么只用到上一完整年，要么**等比折算年化并标注"假设全年均速、仅供量级"**；②**比值/份额**（分子分母同为 YTD，部分年在比值里抵消）**当前年 YTD 可直接代表全年纳入斜率**。两者别混用同一条规则。
5. **威胁有效性必随图列出**：关键词 precision/recall、概念漂移、`01-01` 假峰、arXiv 偏 CS、探照灯效应、基准三陷阱（饱和退役=幸存者偏差 / 污染抬高 / Goodhart）。**无 ground-truth 时，"解决"一律降级为"注意力收敛"。**

### 复现与产物约定

- **词表是单一真相源**：改检索口径只动 `data/queries.json` / `data/benchmark_map.json`，三套 API 查询由 `_common.py` 各自拼装，不在脚本里散落硬编码词。
- **缓存可再生、产物入库**：原始响应缓存到 `data/raw/`（**gitignore**）；入库只留结论性产物（`*_counts.csv`、`benchmark_frontier.csv`、`trends_summary.csv`、`research-P.md`、`research-frontier.md`、`figures/*.png`、`calibration_notes.md`）。
- **⚠ 原始数据必须落盘 `data/raw/`（硬性规定，不限于四源 API 管线）**：任何取数活动——四源脚本、`deep-research` / WebSearch / WebFetch 工作流、临时 `curl`、手工抓的榜单——产生的**原始响应一律完整保存到 `data/raw/<子目录>/`**，**绝不只把结论留在会话里就丢掉原始数据**。具体要求：
  - 每个数据批次建一个自述子目录 `data/raw/<slug>/`，内附 `README.md` 写清**来源、口径、Run/Task ID、文件清单、复现命令**（范例见 `data/raw/research-frontier/`）。
  - deep-research / 工作流类：完整保存**结构化输出 JSON + 工作流脚本 + 全部子 agent 转录（`*.jsonl`，原始 WebSearch/WebFetch 响应即在其中）**。
  - 正文/笔记里出现的**每个数字与引用都必须能在 `data/raw/` 里回溯到原始出处**；找不回原始数据的结论不得入库。
  - `data/raw/` 虽 gitignore，但**本机必须留存**；会话结束前确认已落盘。
- **跑法（P1–P10）**：`fetch_openalex.py → fetch_s2.py → fetch_arxiv.py → fetch_benchmarks.py → analyze_trends.py`；缓存在则 `analyze_trends.py` 可离线重算。
- **跑法（前沿四题 F1–F4）**：`fetch_frontier.py [openalex|s2|arxiv|all] → analyze_frontier.py`；平行管线、复用 `_common`、不污染 P 管线。与 P 的差别：这四题是部分独立于 LLM 的成熟大领域，故 OpenAlex 同时抓 **unscoped（领域全量）+ scoped（LLM 语料内交集）**，share=scoped÷LLM 分母（与 P 同尺度）；S2/arXiv 取 unscoped。**禁止用 WebSearch/deep-research 凑这类"逐年论文量"分析——只在 OpenAlex group_by 拿不到时才退而求其次。**
- **口径写进 `data/calibration_notes.md`**：API 布尔语义校准、精度抽查、各源偏置、YTD 折算基准日（`AS_OF`，固定值保证可复现）。
- **结论进正文时**：引用按上文 IEEE 规范，数字标依据级别（多为"实测"），并把口径/局限放脚注或指向校准笔记——**不把"X 倍""r=0.99"这类数字裸放正文而不可回溯**。

## 目录（README）维护

README 的每个 chapter TOC 段位于一对 HTML 标记之间：

```markdown
<!-- TOC-START: chapter-NN-<slug>.md -->
- [自动生成的标题列表](chapter-NN-<slug>.md#anchor)
<!-- TOC-END: chapter-NN-<slug>.md -->
- 未写小节占位（保留在标记外）
- [参考文献](chapter-NN-<slug>.md#参考文献)
```

- **绝不手动改 `<!-- TOC-START/END -->` 之间的内容**——下次脚本运行会覆盖。
- 改完章节标题，立即跑：
  - CLI：`python3 scripts/update_toc.py`
  - 斜杠命令：`/update-toc`
- 脚本规则（保持与之一致）：
  - 包含 H2–H4；跳过 H1（章名）和命名为 `参考文献` / `References` 的 H2。
  - Anchor slug 算法：小写 → 删除 `[字母数字 / -_ / CJK]` 之外的字符 → 空白 `→ -`。
- 新增章节文件时：在 README 对应章节标题下手动插入一对空标记，再跑脚本填充。

## 论证纪律

1. **实证 > 经验法则 > 直觉**。每个论断明示依据级别（"实测"、"经验法则"、"推断"、"主观估计"）。
2. 改任何标题的**任何字符**（含标点）都会改 anchor — 改完一定跑 `/update-toc` 同步 README。
3. 不写"业界普遍认为"、"据说"、"一般认为"这类无出处断言。
4. 每个具体数字 / 比例 / "X 倍"必须可回溯到引用条目。
5. 跨章引用同套编号体系（P*、C*、§X.Y），不为同一概念发明新代号。

## 反模式（不要做）

- 在 `<!-- TOC-START/END -->` 之间手写内容（会被脚本覆盖）。
- 章号或编号"跳号"（例如从 2.1.2 直接到 2.1.5）。
- mermaid 节点 ID 用中文（会解析失败）。
- 正文里堆 emoji（除非用户明确要求）。
- 一段长达 3+ 个引用 `[N]` 都堆在段末，没有逐句对应——读者无法定位。
- 临时性的"待办笔记"写进正文（请放到单独的 TODO 文件或 issue 里）。
- 经典论点引二手综述而不引原文。
- **正文出现"元修改过程"语言**：诸如"第一版 / 上一稿 / 此前列的 / 刚才说的 / 有意 X 而不是 Y / 这里改了 / 把 Z 重写为 W"等指向**写作 / 修改流程本身**的 meta-prose。最终成稿只保留**结论**，不保留**走到结论的路径**。元过程的讨论只出现在 Claude Code 终端 / 会话输出里，不进入交付章节。如果需要表达"从无到有的起草"这一**语义**（而非元过程），用"起草 / 从无到有起草 / 从零起 / 起点 / 草创"等表述，不要用"第一版"。
- **正文出现"作者—读者对话"语言**：诸如"用户问 / 用户的诘问 / 用户那条诘问 / 你刚才提到的 / 读者的问题 / 我在前一节回答了"等指向**当前对话与具体提问者**的称谓——这与上一条同源，都是把会话上下文渗入交付文档。正文里若要引出一个具体疑问，用"一个典型疑问 / 实践中常被问到 / 一个常见反问"等通用引子，不要指向某个具体提问者。

## 工作流惯例

- 写完一节，立即跑 `/update-toc`。
- **不主动 git commit**；用户明确说"提交"再操作。
- 一次会话改完所有相关章节再统一刷新 TOC，避免无意义中间状态。
- 当章节文件 / 标题被 linter 或用户手动改动时，先 `grep -nE '^#{1,5} '` 看一下当前结构再决定下一步。
