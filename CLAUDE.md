# CLAUDE.md — AI Methodology for Software Development

> 项目级写作与协作规范。读完这一份就能直接接着写下一节。
> 全局规范（语言、日期格式等）见 `~/.claude/CLAUDE.md`。本文件**附加**，不覆盖。

---

## 项目结构

```
README.md                          # 目录页 + 各章 TOC（自动生成区 + 手写区）
chapter-NN-<slug>.md               # 每章一个文件，两位数章号 + 短 slug
scripts/update_toc.py              # TOC 重新生成脚本（无第三方依赖，Py 3.8+）
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
- **正文中的 `[N]` 一律渲染为指向出处 URL 的超链接**，格式 `[[N]](URL)`；URL 取自参考文献条目里 `Available: <URL>` 的部分。条目本身（`## 参考文献` 段内）保持纯 `[N] ...` 形态，不要再链接。无 URL 的条目（书籍、纸刊未上网者）正文里保留纯 `[N]`。
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

## 工作流惯例

- 写完一节，立即跑 `/update-toc`。
- **不主动 git commit**；用户明确说"提交"再操作。
- 一次会话改完所有相关章节再统一刷新 TOC，避免无意义中间状态。
- 当章节文件 / 标题被 linter 或用户手动改动时，先 `grep -nE '^#{1,5} '` 看一下当前结构再决定下一步。
