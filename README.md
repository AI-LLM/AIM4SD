# AI Methodology for Software Development

除了OpenAI和Anthropic以外的企业和开源社区目前（2026年）更关注如何追赶、复现甚至改进前者的工作成果。而前者的努力本质上是制造**更便宜和更接近人的平均工作质量的知识工作劳动力**。本文不是站在这些“AI劳动力”制造者的角度，而是基于“AI劳动力”已经实现的程度和未来会更快更便宜但无法超过人类**平均**水平这一趋势判断来探讨AI应用者如何利用“AI劳动力”改善现有的工作，创造新的价值，以及解决新因素带来的新问题，特别是在软件工程领域回答：在 AI 介入开发主体之后，软件工程的方法应该如何被改写、保留、或加速。

---

## 目录

### [第一章　绪论](chapter-01-introduction.md)

<!-- TOC-START: chapter-01-introduction.md -->
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
- [四、软件工程语境](chapter-01-introduction.md#四软件工程语境)
  - [1.4.1 LLM 的"基础病"与软件工程的"老难题"是同构的](chapter-01-introduction.md#141-llm-的基础病与软件工程的老难题是同构的)
  - [1.4.2 那 LLM/Agent 在软件工程中真正改变了什么？——主体替换](chapter-01-introduction.md#142-那-llmagent-在软件工程中真正改变了什么主体替换)
    - [1.4.2.1 AI 能力的"锯齿状边界" (Jagged Frontier)](chapter-01-introduction.md#1421-ai-能力的锯齿状边界-jagged-frontier)
    - [1.4.2.2 对比软件工程师](chapter-01-introduction.md#1422-对比软件工程师)
  - [1.4.3 最大的变化是时间常数：SDLC 的加速 = 加速建造 + 加速腐化](chapter-01-introduction.md#143-最大的变化是时间常数sdlc-的加速--加速建造--加速腐化)
    - [1.4.3.1 如何建立新的质量标准、取得用户信任](chapter-01-introduction.md#1431-如何建立新的质量标准取得用户信任)
    - [1.4.3.2 如何在被加速过的生命周期里，让"有效时间"的比例尽可能高](chapter-01-introduction.md#1432-如何在被加速过的生命周期里让有效时间的比例尽可能高)
    - [1.4.3.3 解除不完整信息和持续变化的约束，创造那些原先因为"不知道 / 无法算 / 做不起"而不存在的新价值](chapter-01-introduction.md#1433-解除不完整信息和持续变化的约束创造那些原先因为不知道--无法算--做不起而不存在的新价值)
<!-- TOC-END: chapter-01-introduction.md -->
- [参考文献](chapter-01-introduction.md#参考文献)

### [第二章 软件质量新标准](chapter-02-quality-standards.md)

<!-- TOC-START: chapter-02-quality-standards.md -->
- [2.1 锯齿对传统软件质量观的几条结构性冲击](chapter-02-quality-standards.md#21-锯齿对传统软件质量观的几条结构性冲击)
  - [2.1.1 可读性：面向 *谁* 的可读？](chapter-02-quality-standards.md#211-可读性面向-谁-的可读)
    - [人类 vs LLM 的"可读上限"——绝对量与有效深度](chapter-02-quality-standards.md#人类-vs-llm-的可读上限绝对量与有效深度)
  - [2.1.2 抽象模式：从"减少重复"到"减少不可逆"](chapter-02-quality-standards.md#212-抽象模式从减少重复到减少不可逆)
  - [2.1.3 复用：从"库"到"能力"](chapter-02-quality-standards.md#213-复用从库到能力)
  - [2.1.4 测试与覆盖率：从"覆盖"到"可信信号"](chapter-02-quality-standards.md#214-测试与覆盖率从覆盖到可信信号)
  - [2.1.5 注释与文档：从同步难题到"Single source of truth"](chapter-02-quality-standards.md#215-注释与文档从同步难题到single-source-of-truth)
<!-- TOC-END: chapter-02-quality-standards.md -->
- 2.2 全过程的思考与生成记录
- 2.3 审计与评测
- 2.4 Session + Git 方案
- [参考文献](chapter-02-quality-standards.md#参考文献)

### 第三章 SDLC的有效时间比

- Human In The Agentic-Loop的最佳实践 //👀 ai4se_white_paper/src/chapter3/03-process-engineering.md
- 持续重构与可逆性 (reversibility) //👀 ai4se_white_paper/src/chapter3/04-architecture-and-complexity.md
- 知识工程 //👀 ai4se_white_paper/src/chapter3/05-knowledge-engineering.md

### 第四章 新的价值创造

- 新软件与新开发者
  - "一次性/每用户级”软件
  - 弹性软件与[Fully Autonomous Systems](https://arxiv.org/abs/2604.09388) //👀 [Heuristic Learning](https://trinkle23897.github.io/learning-beyond-gradients/#zh)
  - 新的开发者

|   | 分类 | 产物重复使用的程度（次数、人和环境差异） | 对视觉设计、测试、商业价值实现等方面的要求 |
| --- | --- | --- | --- |
| 1 | Solo developer | 自己重复用 | <br> |
| 2 | Team/Internal developer | 小范围伙伴一起用 | <br> |
| 3 | AI Application developer | AI应用产品化 | <br> |
| 4 | AI Foundation developer | AI技术产品化 | <br> |
| 5 | Classic software developer | 传统软件项目交付或产品化 | <br> |

- 下一代“开源”与“内源”（Super OSS & InnerSource）
- 仿真（Simulation）

### [第五章 人的教育](chapter-05-human-education.md)

<!-- TOC-START: chapter-05-human-education.md -->
- [5.1 AI 协作下的人类工程师：新分工](chapter-05-human-education.md#51-ai-协作下的人类工程师新分工)
  - [5.1.1 约束设计者 (Constraint Designer)](chapter-05-human-education.md#511-约束设计者-constraint-designer)
  - [5.1.2 验证与评测设计者 (Verifier / Eval Designer)](chapter-05-human-education.md#512-验证与评测设计者-verifier--eval-designer)
  - [5.1.3 过程与可逆性设计者 (Process / Reversibility Designer)](chapter-05-human-education.md#513-过程与可逆性设计者-process--reversibility-designer)
  - [5.1.4 信任承担者 (Accountable Principal)](chapter-05-human-education.md#514-信任承担者-accountable-principal)
  - [5.1.5 审美与品味守门人 (Taste Steward)](chapter-05-human-education.md#515-审美与品味守门人-taste-steward)
  - [5.1.6 能力矩阵](chapter-05-human-education.md#516-能力矩阵)
- [5.2 从高中毕业生基线到 §5.1 的工作项目：能力需求分析](chapter-05-human-education.md#52-从高中毕业生基线到-51-的工作项目能力需求分析)
  - [5.2.1 高中毕业生基线](chapter-05-human-education.md#521-高中毕业生基线)
  - [5.2.2 五份职责的能力栈](chapter-05-human-education.md#522-五份职责的能力栈)
    - [约束设计者（§5.1.1 的能力栈）](chapter-05-human-education.md#约束设计者511-的能力栈)
    - [验证与评测设计者（§5.1.2 的能力栈）](chapter-05-human-education.md#验证与评测设计者512-的能力栈)
    - [过程与可逆性设计者（§5.1.3 的能力栈）](chapter-05-human-education.md#过程与可逆性设计者513-的能力栈)
    - [信任承担者（§5.1.4 的能力栈）](chapter-05-human-education.md#信任承担者514-的能力栈)
    - [审美与品味守门人（§5.1.5 的能力栈）](chapter-05-human-education.md#审美与品味守门人515-的能力栈)
  - [5.2.3 三象限：哪些手工经验不再必要、哪些可由新方法替代、哪些仍是认知脚手架](chapter-05-human-education.md#523-三象限哪些手工经验不再必要哪些可由新方法替代哪些仍是认知脚手架)
    - [象限 I — AI 周边工程已稳定接管，手工训练在教育里也不再划算](chapter-05-human-education.md#象限-i--ai-周边工程已稳定接管手工训练在教育里也不再划算)
    - [象限 II — 仍由人最终负责，但**新数学 / 统计 / 形式方法**已可大量替代逐行读代码](chapter-05-human-education.md#象限-ii--仍由人最终负责但新数学--统计--形式方法已可大量替代逐行读代码)
    - [象限 III — 仍以亲手做事的副产品形式产生，没有方法学替代品](chapter-05-human-education.md#象限-iii--仍以亲手做事的副产品形式产生没有方法学替代品)
    - [教育上的合成](chapter-05-human-education.md#教育上的合成)
- [5.3 教育对照：CMU SCS BS in Computer Science](chapter-05-human-education.md#53-教育对照cmu-scs-bs-in-computer-science)
  - [5.3.1 为什么选 CMU](chapter-05-human-education.md#531-为什么选-cmu)
  - [5.3.2 CMU BS in CS 课程结构概要（2025–2026 学年公开版本）](chapter-05-human-education.md#532-cmu-bs-in-cs-课程结构概要20252026-学年公开版本)
- [5.4 课程缺口诊断](chapter-05-human-education.md#54-课程缺口诊断)
  - [5.4.1 缺口一：Spec 工程没有教学载体](chapter-05-human-education.md#541-缺口一spec-工程没有教学载体)
  - [5.4.2 缺口二：评测与可信信号工程化训练不足](chapter-05-human-education.md#542-缺口二评测与可信信号工程化训练不足)
  - [5.4.3 缺口三：可逆性与影响半径——抽象重定价没有载体](chapter-05-human-education.md#543-缺口三可逆性与影响半径抽象重定价没有载体)
  - [5.4.4 缺口四：Harness 不是新 OS，但需要一类横跨多门课的混合工程训练](chapter-05-human-education.md#544-缺口四harness-不是新-os但需要一类横跨多门课的混合工程训练)
  - [5.4.5 缺口五：阅读密集型工作的训练强度不足](chapter-05-human-education.md#545-缺口五阅读密集型工作的训练强度不足)
  - [5.4.6 缺口六：审美与品味的训练机会被压缩](chapter-05-human-education.md#546-缺口六审美与品味的训练机会被压缩)
  - [5.4.7 缺口七：信任、责任、合规与安全的实操不足](chapter-05-human-education.md#547-缺口七信任责任合规与安全的实操不足)
  - [5.4.8 缺口八：考核与学术诚信本身需要重设计](chapter-05-human-education.md#548-缺口八考核与学术诚信本身需要重设计)
- [5.5 改进建议](chapter-05-human-education.md#55-改进建议)
  - [5.5.1 改造既有课程](chapter-05-human-education.md#551-改造既有课程)
  - [5.5.2 新增必修与选修课](chapter-05-human-education.md#552-新增必修与选修课)
  - [5.5.3 教学方法：三象限作业体系 + 过程审计](chapter-05-human-education.md#553-教学方法三象限作业体系--过程审计)
- [5.6 实施风险与节奏](chapter-05-human-education.md#56-实施风险与节奏)
<!-- TOC-END: chapter-05-human-education.md -->
- [参考文献](chapter-05-human-education.md#参考文献)
