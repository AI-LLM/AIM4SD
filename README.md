# AI Methodology for Software Development

除了OpenAI和Anthropic以外的企业和开源社区目前（2026年）更关注如何追赶、复现甚至改进前者的工作成果。而前者的努力本质上是制造**更便宜和更接近人的平均工作质量的知识工作劳动力**。本文不是站在这些“AI劳动力”制造者的角度，而是基于“AI劳动力”已经实现的程度和未来会更快更便宜但无法超过人类**平均**水平这一趋势判断来探讨AI应用者如何利用“AI劳动力”改善现有的工作，创造新的价值，以及解决新因素带来的新问题，特别是在软件工程领域回答：在 AI 介入开发主体之后，软件工程的方法应该如何被改写、保留、或加速。

---

## 目录

### [第一章　绪论](chapter-01-introduction.md)

- [一、LLM 的基础性问题](chapter-01-introduction.md#一llm-的基础性问题)
- [二、2022 年以后涌现的概念地图](chapter-01-introduction.md#二2022-年以后涌现的概念地图)
  - [A. 输入侧 (Input Layer)](chapter-01-introduction.md#a-输入侧-input-layer)
  - [B. 上下文侧 (Context Layer)](chapter-01-introduction.md#b-上下文侧-context-layer)
  - [C. 行动侧 (Action Layer)](chapter-01-introduction.md#c-行动侧-action-layer)
  - [D. 运行时与基础设施 (Runtime & Infra)](chapter-01-introduction.md#d-运行时与基础设施-runtime--infra)
  - [E. 推理时增强 (Inference-Time)](chapter-01-introduction.md#e-推理时增强-inference-time)
  - [F. 训练侧 (Training)](chapter-01-introduction.md#f-训练侧-training)
  - [G. 评测与治理 (Eval & Governance)](chapter-01-introduction.md#g-评测与治理-eval--governance)
- [三、这些概念解决了多少基础性问题？](chapter-01-introduction.md#三这些概念解决了多少基础性问题)
- [四、软件工程语境：LLM 没有带来本质变化，只是更快的人手](chapter-01-introduction.md#四软件工程语境llm-没有带来本质变化只是更快的人手)
  - [4.1 LLM 的"基础病"与软件工程的"老病"是同构的](chapter-01-introduction.md#41-llm-的基础病与软件工程的老病是同构的)
  - [4.2 那 LLM/Agent 在软件工程中真正改变了什么？——速度，几乎仅此而已](chapter-01-introduction.md#42-那-llmagent-在软件工程中真正改变了什么速度几乎仅此而已)
  - [4.3 真正的变化是时间常数：SDLC 的加速 = 加速建造 + 加速腐化](chapter-01-introduction.md#43-真正的变化是时间常数sdlc-的加速--加速建造--加速腐化)
    - [4.3.1 如何建立新的质量标准、取得用户信任](chapter-01-introduction.md#431-如何建立新的质量标准取得用户信任)
    - [4.3.2 如何在被加速过的生命周期里，让"有效时间"的比例尽可能高](chapter-01-introduction.md#432-如何在被加速过的生命周期里让有效时间的比例尽可能高)
    - [4.3.3 如何创造那些原先因为"无法算 / 做不起"而不存在的新价值](chapter-01-introduction.md#433-如何创造那些原先因为无法算--做不起而不存在的新价值)
- [参考文献](chapter-01-introduction.md#参考文献)

### [第二章 软件质量新标准](chapter-02-quality-standards.md)

- [2.1 起点：AI 能力的"锯齿状边界" (Jagged Frontier)](chapter-02-quality-standards.md#21-起点ai-能力的锯齿状边界-jagged-frontier)
  - [2.1.1 在哪些维度上 AI 已稳定超过人类平均](chapter-02-quality-standards.md#211-在哪些维度上-ai-已稳定超过人类平均)
  - [2.1.2 在哪些维度上 AI 仍低于人类](chapter-02-quality-standards.md#212-在哪些维度上-ai-仍低于人类)
- [2.2 锯齿对传统软件质量观的几条结构性冲击](chapter-02-quality-standards.md#22-锯齿对传统软件质量观的几条结构性冲击)
  - [2.2.1 可读性：面向 *谁* 的可读？](chapter-02-quality-standards.md#221-可读性面向-谁-的可读)
  - [2.2.2 抽象模式：从"减少重复"到"减少不可逆"](chapter-02-quality-standards.md#222-抽象模式从减少重复到减少不可逆)
  - [2.2.3 复用：从"库"到"能力"](chapter-02-quality-standards.md#223-复用从库到能力)
  - [2.2.4 测试与覆盖率：从"覆盖"到"可信信号"](chapter-02-quality-standards.md#224-测试与覆盖率从覆盖到可信信号)
  - [2.2.5 注释与文档：从同步难题到"Single source of truth"](chapter-02-quality-standards.md#225-注释与文档从同步难题到single-source-of-truth)
- 2.3 全过程的思考与生成记录
- 2.4 审计与评测
- 2.5 Session + Git 方案
- [参考文献](chapter-02-quality-standards.md#参考文献)

### 第三章 SDLC的有效时间比

- Human In The Agentic-Loop的最佳实践 //👀 ai4se_white_paper/src/chapter3/03-process-engineering.md
- 持续重构与可逆性 (reversibility) //👀 ai4se_white_paper/src/chapter3/04-architecture-and-complexity.md
- 知识工程 //👀 ai4se_white_paper/src/chapter3/05-knowledge-engineering.md

### 第四章 新的价值创造

- 新软件与新开发者
  - "一次性/每用户级”软件
  - 弹性软件
  - 新的开发者

|   | 分类 | 产物重复使用的程度（次数、人和环境差异） | 对视觉设计、测试、商业价值实现等方面的要求 |
| --- | --- | --- | --- |
| 1 | Solo developer | 自己重复用 | <br> |
| 2 | Team/Internal developer | 小范围伙伴一起用 | <br> |
| 3 | AI Application developer | AI应用产品化 | <br> |
| 4 | AI Foundation developer | AI技术产品化 | <br> |
| 5 | Classic software developer | 传统软件项目交付或产品化 | <br> |

- 仿真（Simulation）
