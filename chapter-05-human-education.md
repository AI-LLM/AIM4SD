# 第五章　人的教育

> 第一章 §1.4.2 把 AI 在软件工程中的位置定为"**主体替换**"——它接管的是中初级工程师的执行职能，但能力分布与人类**正交**（§1.4.2.1 锯齿状边界）。第二章把这条锯齿的结果展开成软件质量观的**重新定价**（可读性 / 抽象 / 复用 / 测试 / 文档五条），第一章 §1.3 又给出一份**周边工程没有触及的根性缺陷清单**（P1–P10 的 *残余问题* 一列）。这三件事合起来定义了一个简单的边界条件：**留给人类工程师的不是减法后剩下的活，而是 AI 不能可靠承担、又恰好与产品价值最相关的那一组职责**。本章先把这组职责做出来（§5.1），再做一件**比"把工作项目翻译成课程"更前置的事**——分析这些职责需要什么能力、这些能力从高中毕业生的数理 / 逻辑 / 人文基线出发要补什么、其中哪些**只能**作为亲手做事的副产品长出来（§5.2）；之后选一个公开度最高的本科 CS 课程体系（CMU SCS BS in Computer Science）做对照（§5.3），逐项诊断它在新分工下的具体缺口（§5.4），给出改造既有课程与新增内容的建议（§5.5），最后讨论实施节奏与风险（§5.6）。

---

## 5.1 AI 协作下的人类工程师：新分工

把 §1.4.2.2.1 的 AI 优势轴 (A–E)、§1.4.2.2.2 的 AI 短板轴 (a–d)、第二章五条质量观的迁移，以及 §1.4.3 "建造 + 腐化"双向加速三件事合并，人类工程师在 AI 协作下的剩余职责可以收敛成五个角色。

### 5.1.1 约束设计者 (Constraint Designer)

AI 在有可验证奖励 (RLVR) 信号的窄域内能稳定逼近甚至超过人类平均（§1.4.2.2.1 D）；它在**没有 verifier 的开放域**几乎不构成可靠主体（§1.4.2.2.2 a, b, d）。所以人类的**第一份不可让渡职责**是：把开放问题切片成 AI 可以攻击的可验证子问题——**写 spec、写 acceptance test、写 evaluator、写 reward shaping**。这是 François Chollet [[4]](https://x.com/fchollet/status/2024519439140737442) 所说"agentic coding 在本质上是机器学习"的直接推论：人类在这里扮演 *loss function 设计者*，不是 *gradient 计算者*。

### 5.1.2 验证与评测设计者 (Verifier / Eval Designer)

第二章 §2.1.4 已经把"测试"从劳动量瓶颈翻转成**信号质量瓶颈**——写测试 AI 几乎免费，写**有判别力**的测试仍是稀缺的。结合 §1.3 对 P6 评测危机的复盘（LLM-as-Judge 偏置、benchmark 污染），人类的第二份职责是**对抗性地构造判别集**：variation testing、property-based、生产 trace 重放、面向"训练后才存在的题"的红队评测。

### 5.1.3 过程与可逆性设计者 (Process / Reversibility Designer)

§1.4.3 把 SDLC 描述为"加速建造 + 加速腐化"。在这个量级上，**任何不可回退的早期决策都会被加速放大成事故**，所以 §2.1.2 把抽象的目标从 *DRY* 重新定价为 *locality of change* 与 *reversibility*。对应到人这一侧，这是一种**组织 / 流程 / 版本控制 / 影响半径**层面的工程，而不是代码层面的。它包含：环境隔离、可灰度发布、回滚预案、ADR、`fix_plan.md` 这类外置组织记忆（§1.4.3.2）、CI/CD 与 harness 的权限模型（§1.3 C14）。

### 5.1.4 信任承担者 (Accountable Principal)

§1.4.2.2.2 c 把"信任与责任"列为 AI 在**结构上**无法承担的一轴——这不是能力问题，而是法律 / 合规 / 商业承诺**主体资格**问题。对应的人的职责覆盖：

- 对**外部**：合规审查、对客户 / 监管的承诺、出事时的问责接受方；
- 对**内部**：风险定级（哪些决策可让 agent 单独完成、哪些必须 human-in-the-loop）、对抗性安全审查、Prompt Injection（§1.3 P8）的架构级缓解。

### 5.1.5 审美与品味守门人 (Taste Steward)

§1.4.2.2.2 d 给"审美与品味"打的分接近底——AI 倾向于复述训练集里的多数派；§2.1.1 又把"对人类决策者友好"和"对 agent 修改者友好"两类可读性正式拆开。人的第五份职责是**长期品味判断**：哪些 API 是十年后还愿意用的、哪些抽象优雅、哪些产品形态对人是体面的。这一项是当前最容易在教育中被低估的——因为它**不可机器评分**、不进 benchmark、也不出现在面试 leetcode 里。

### 5.1.6 能力矩阵

把以上五个角色与 §1.4.2 / §2.1 / §1.4.3 的论据并排，得到一份**新分工对照表**——它将作为 §5.2 能力栈与 §5.4 课程缺口诊断的共同标尺。

| 人类残余职责 | 对应 AI 短板 / 残余问题 | 关键能力 | 关键交付制品 |
|---|---|---|---|
| 约束设计者 | P2 推理 / a 长程规划 / b grounding | 形式化思维、问题分解、规约写作、可验证目标设计 | PRD / Spec / Acceptance Test / Reward Spec |
| 验证与评测设计者 | P6 评测 / d 品味 / P1 幻觉 | 对抗性测试、统计判别、生产监控、red-teaming | Eval Suite / Property Tests / Trace Replay |
| 过程与可逆性设计者 | P5 不可控 / P7 token 经济 / P10 多 agent 协调 | 分布式系统、CI/CD、影响半径估算、组织设计 | ADR / Harness 配置 / 回滚预案 / Org-Chart |
| 信任承担者 | c 信任责任 / P8 安全 | 合规、安全建模、社会工程认知、问责接受 | 合规审查、风险登记册、注入演练报告 |
| 审美与品味守门人 | d 品味 / 长期可读性 | 阅读量、跨范式比较、用户共情、长期审美 | 设计评审意见、API 审美准则、用户研究 |

> 注：这份分工**没有**把"写代码"作为一项独立职责——因为它在新分工里已退化为**实现上述五个角色时的副产品**。但**这一论断只在"工业实践"语境下成立**；在"教育实践"里需要更细的判断——某些手工训练在 AI 时代确实可以裁掉、某些可以被新数学 / 统计 / 形式方法**替代**、某些仍是不可压缩的认知脚手架。这套**三象限**的拆分单独拎到 §5.2.3 论证，因为它会反向决定 §5.5 改进建议的形态。

---

## 5.2 从高中毕业生基线到 §5.1 的工作项目：能力需求分析

§5.1 列出五份残余职责，但**没有回答**一个学生具体要学什么才能做这些事。把工作项目机械翻译为课程——§5.1.1 列了 spec 设计就开一门 spec 课、§5.1.2 列了 eval 设计就开一门 eval 课——会跳过最重要的一个中间步：在**高中毕业生一般具备的数理、逻辑、人文基础**上，要**层叠地**长出哪些能力，§5.1 的五份职责才能被胜任？而且这个问题里还藏着一个结构性悖论——有些能力似乎**只能**通过亲手做被 AI 取代的工作（写代码、写测试、手工调试）才能习得（§5.2.3）。本节先把基线设清楚（§5.2.1），再逐项展开能力栈（§5.2.2），最后单独论证手工经验悖论。

### 5.2.1 高中毕业生基线

以入读 CMU SCS 这一档次的典型新生为参考，可大致预期：

- **数学**：calculus、linear algebra 入门、初等组合与概率；
- **逻辑**：形式逻辑入门（命题、谓词），主要附着在数学证明语境上，不作为独立工艺；
- **统计**：描述性统计 + 假设检验入门（均值方差为主，厚尾几乎不教）；
- **编程**：Python / Java 入门程度，少量 OO、基本递归、调试器与 git 入门；
- **写作**：argumentative essay、文献摘要、source criticism（人文课带出来的）；
- **科学方法**：见过实验报告的模板，但**可证伪原则 (Popper)** 通常未被显式教授为工程纪律；
- **跨学科**：自然科学约一年；法律 / 经济 / 安全 / 组织行为基本未接触。

这个基线**不弱**——足以应付 §5.1 任何一份职责的**底层概念前提**；但与"能胜任"之间的距离大部分**不在概念**，而在**反复磨炼出来的判断力**与**横跨学科的最小整合**。下一节把五份职责拆到这两层。

### 5.2.2 五份职责的能力栈

每份职责拆为三层：**底层认知前提**（基线已具备 / 略补即可）、**中层学科能力**（需要专门课程的独立学科训练）、**上层工艺判断力**（只能通过反复经历长出来的、不可"讲授"的部分）。

#### 约束设计者（§5.1.1 的能力栈）

- **底层**：分辨"意图 vs. 字面"——是法律 / 哲学的语言分析能力。基线的人文写作给了浅层 *audience awareness*；缺的是**对抗性阅读**：读自己写的句子时假设对方有动机曲解。需补一节**法律 / 政策语言**入门或仿写训练。
- **中层**：形式化转写——把"我想要 X"翻译成"对所有输入 i 满足 P(i, output)"。基线的数学证明经验提供了原料，但**没有训练把模糊工程需求"挤"成形式表达**的反复练习。这层是从数学证明到工程规约的**桥课**，独立学科存在。
- **上层**：spec 的**反过拟合**——预见到"spec 写紧了 AI 会找捷径绕过"，对应 ML 里的 reward hacking [[11]](https://arxiv.org/abs/2209.13085) 与 Goodhart's law。基线学生**没有任何接触**这种思维。这一层只能通过反复经历"写 spec → 看 agent 钻空子 → 修 spec"长出来，不能靠讲授。

#### 验证与评测设计者（§5.1.2 的能力栈）

- **底层**：**可证伪原则 (Popper)**——好测试的标志是"它能排除一件事"，不是"它确认一件事"。基线只在哲学课里碰到一次，没有作为工程纪律内化。
- **中层**：实验设计、统计判别（power、effect size、p-hacking 认知）；对抗性思维；以及人类不擅长的**估计稀有事件**（厚尾、Black Swan、Anscombe's quartet 体感）。这层需要一门面向工程的统计判别课。
- **上层（直接对应一个典型疑问）**："不会手写代码，如何判断 AI 生成代码的问题？"——答案不是单一的，它的实质构成分两半：(a) **大部分判断可以由 §5.2.3 象限 II 的统计 / 形式方法学**（多采样一致性、属性测试、变形测试、差分测试、形式验证、经验复杂度回归、counterfactual 扰动、trace 异常聚类等）承担，**与是否会手写代码无关**；(b) 但**从无到有起草**这些方法所需的"什么算好"——spec、rubric、不变式——的判断力仍以亲历为前提，这是 §5.2.3 象限 III 的内容。

#### 过程与可逆性设计者（§5.1.3 的能力栈）

- **底层**：贝叶斯式风险思维——后果不对称下的决策（厚尾、低概率高影响）。基线的概率统计只覆盖前两阶矩，对厚尾几乎不教。
- **中层**：FMEA、fault tree、事故复盘范式（航空、核工业的成熟事故学 [[12]](https://hbr.org/2011/04/how-to-avoid-catastrophe)）；分布式系统的故障模型；组织行为基础（Conway 定律的实践含义）。
- **上层**：把"影响半径"作为设计目标显式优化——必须经历过**自己拥有一个真实运行系统并看着它出故障**才长得出来。校内可靠**长期开源协作 / 校内基础设施运维项目**代偿，但不能完全替代生产经验。

#### 信任承担者（§5.1.4 的能力栈）

- **底层**：理解"主体资格"这个法律 / 社会概念——为什么有些事必须由有责任能力的实体承担。基线人文课通常只擦边讲了"权利"，**没讲"承担后果"的契约结构**。这一项离基线最远，因为它**不是 CS 学科的延伸**而是跨学科最小整合。
- **中层**：威胁建模（STRIDE、attack tree）；社会工程认知（Mitnick 一脉）；合规框架最小集（GDPR、HIPAA、SOC 2 核心要件）；当代 AI 治理热点（EU AI Act、NIST AI RMF）。
- **上层**：在没有先例的 AI 情境下做风险定级——这是**判断而非知识**，需要在课程内由教师引导若干真实案例的讨论才能初步形成。

#### 审美与品味守门人（§5.1.5 的能力栈）

- **底层**：广博阅读——见过足够多好的与坏的，才有比较的基底。基线 humanities 已建立了一些（文学、艺术史的范式），但**没有迁移到工程对象上**。
- **中层**：跨范式比较——能把同一个问题在 Lisp / Haskell / C / Python 里的解法并排评估。这需要**至少深入接触过三种范式**，单一语言出身的学生此层缺口最大。
- **上层**：把审美判断**写成可读的设计评审意见**——而不只是"我觉得这样更好"。这一层既需要写作训练（基线有底），也需要**自己写过让自己用半年后觉得难用的 API**（基线没底）。再次回到 §5.2.3。

### 5.2.3 三象限：哪些手工经验不再必要、哪些可由新方法替代、哪些仍是认知脚手架

一个在 AI 协作语境下反复被提出的典型疑问是：

> "不会手写代码，如何判断 AI 生成代码的问题？"

这个疑问需要被严格拆开来回答，否则容易滑入两种都不对的极端：一端是"AI 既然能做了就让 AI 做"，把全部判断力都委托出去；另一端是"判断力靠手写出来"，于是把高校四年再用一遍 1980s 的训练强度。两端都错。把第一、二章对"AI 改变了什么"的分析作为筛子，传统 CS 教育中的手工训练可以拆到三个象限里。

#### 象限 I — AI 周边工程已稳定接管，手工训练在教育里也不再划算

落在 §1.4.2.2.1 A–E 锯齿优势内的工作，教育上让学生重复练已无边际收益：

- 跨语言机械翻译（C → Python、Java → Kotlin 等）：§1.4.2.2.1 D 已稳定超过人均；
- 单元测试 / docstring 等劳动量型生成：§2.1.4 / §2.1.5 已把它们从"宝贵"重定价为"廉价副产品"；
- 库 API 形状契合、import 顺序、命名一致性：§1.4.2.2.1 B 多采样配置下几乎免费；
- 模板 / 胶水 / 适配器代码：§1.4.2.2.1 C "显式样板比抽象更划算"；
- 标准答案型习题（冒泡排序、字典合并、二分查找等熟题）：RLVR 覆盖窄域 [[49, ch.1]](https://arxiv.org/abs/2407.21787)。

教育上的判断：**这一类手工训练在高强度 CS 项目（CMU / MIT / Stanford 等）里其实早已大幅缩水**——它们不让学生写第十遍冒泡排序，而是直接进入数据结构、系统、理论的核心。但 boot camp、较弱本科、以及"先打 Python 基础再学算法"型的入门程序里仍残留大量 象限 I 训练，这部分应被进一步压缩。

#### 象限 II — 仍由人最终负责，但**新数学 / 统计 / 形式方法**已可大量替代逐行读代码

这是回应用户问题的**核心象限**。判断 AI 生成代码的问题，**不必**通过"读懂每一行"达成；很多场景下读代码反而是最低效的路径。下列方法在 2020s 中后期已工业可用——其中**多数方法本身不新**（1990s 末就有原型），但 AI 把代码产量推到 10× 后，这些方法**首次从可有可无升级为必备工艺**：

| 方法 | 它检测什么 | 数学 / 形式基础 | 替代了哪类"读代码" |
|---|---|---|---|
| **多采样一致性** (multi-sample agreement) | 幻觉、规约歧义、reward hacking | 信息熵 + 多数表决；Brown et al. *Large Language Monkeys* [[49, ch.1]](https://arxiv.org/abs/2407.21787) 给出 RLVR 域内覆盖率随采样数对数线性扩展的硬证据 | 逐行揣测"这段代码会不会胡说" |
| **属性测试** (property-based testing) | 边界条件、不变式破坏 | 随机抽样 + minimal counterexample shrinking；QuickCheck (Claessen & Hughes 2000) [[15]](https://www.cs.tufts.edu/~nr/cs257/archive/john-hughes/quick.pdf) → Hypothesis / Proptest | 手工列举 corner case |
| **变形测试** (metamorphic testing) | 没有"正确答案"可比的开放域输出 | 输入扰动 ↔ 输出关系的代数闭包；Chen et al. 1998 [[16]](https://www.cse.cuhk.edu.hk/~smyiu/publications/HKUST-CS98-01.pdf)；2025 综述见 [[17]](https://dl.acm.org/doi/10.1145/3631971) | 知道"对的输出应长什么样"这一前置需求 |
| **差分测试** (differential testing) | 实现与参考 / 旧版之间的不一致 | 状态空间的 trace 等价性；McKeeman 1998 [[18]](https://www.cs.swarthmore.edu/~bylvisa1/cs91/f15/Papers/Differential-Testing-McKeeman.pdf) | 肉眼比对两份实现 |
| **形式化验证** (Lean / TLA+ / F\*) | 关键路径的功能正确性 | 类型理论 / 模型检查；Lean 4 + Mathlib 在 2024–2025 把工业可用门槛显著降低 [[19]](https://leanprover-community.github.io/)，DeepMind AlphaProof 把 Lean 形式化作为 RL 奖励信号 [[20]](https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/) | 直觉式的"我相信这段代码" |
| **经验复杂度回归** | "O(n log n)" 类性能声明是否名副其实 | 拟合 power-law / poly / exp 到运行时-输入规模曲线 | 手算 O(·) + 凭直觉找反例 |
| **counterfactual 扰动** | reward hacking / spec gaming | Lipschitz 连续性 + 分布漂移检测；与第一章 P2 不忠实 CoT [[25, ch.1]](https://arxiv.org/abs/2503.08679) 检测同源 | 靠经验"嗅"出 agent 钻空子 |
| **trace 重放 + 异常聚类** | 生产环境下的隐性回归 | Z-score / Mahalanobis distance / OOD 检测 | 周期性手工 code review |
| **校准过的 LLM-as-judge** | 大规模主观判断（写作、API 设计、文档质量）| 与人类金标准对齐 + 偏置补偿；§1.3 P6 [[21, ch.1]](https://openreview.net/forum?id=3GTtZFiajM) 提醒 judge 自身的位置 / 冗长 / 立场偏置必须持续校准 | 人类 reviewer 在大尺度生成上不堪重负 |

> 一个关键观察：**这些方法都不要求学生会写出被验证的代码——它们要求学生能"设计验证机制"**。设计验证机制需要的能力是：可证伪原则、形式语义、统计判别、不变式直觉、扰动设计——这些都是**数学 / 统计 / 形式方法学**的能力，与"手写每一行代码"是正交的。这个典型疑问的实质答案因此是：**大部分判断由象限 II 方法学承担，与是否会手写无关**——前提是学生学过这一组方法。

把这层意思反推到课程里：**象限 II 方法应作为一个独立的内容类别加入 CS 本科**。它不属于传统理论（15-251）、不属于传统系统（15-213）、也不属于传统软件工程（17-313），而是横跨它们、把"如何不靠肉眼建立信任"系统化为一门工艺。这是当前 CMU / MIT / Stanford **几乎全空**的一块，也是 §5.5.2-(2) Eval & Trust 必修课的核心内容来源。

#### 象限 III — 仍以亲手做事的副产品形式产生，没有方法学替代品

象限 II 的方法都很强，但**它们都有一个共同前提**：有人定义了"什么算好"。多采样一致性需要一个度量空间；属性测试需要一组不变式；变形测试需要一组等价关系；形式验证需要一份属性陈述。**这一切的起点无法由 象限 II 方法自己生成**——它需要一个有 case base 的人来起草。象限 III 就是这部分。

具体到 CS 教育，象限 III 的内容**窄而硬**：

1. **从无到有起草 spec / rubric / 不变式所需的判断力**。要起草"购物车结算后总金额必须等于商品价格之和"这种看似显然的不变式，前提是知道**它会怎么被破坏**——浮点累加、并发修改、退款边界、税率舍入、优惠券叠加。这份 case base 只能从亲历获得（自己写过、被自己的 bug 咬过、复盘过）。Schank 称之为案例推理 [[14]](https://www.cambridge.org/core/books/dynamic-memory/E0E0FE3D38E5EBABD15F73CB9F36D7A6)，Bjork *desirable difficulties* [[13]](https://bjorklab.psych.ucla.edu/wp-content/uploads/sites/13/2016/04/Bjork_1994.pdf) 提供了"摩擦是后续流利使用工具的认知前提"的实证支撑。
2. **识别全新失败模式**。统计方法只检测**已知模式**；novel 模式只能靠分析师做 analogy reasoning，必须有充分多的相邻领域亲历才能在没有先例时迁移判断。
3. **长期审美决策**。"这份 API 十年后还会被人愿意用吗"——目前没有任何统计代理量；唯一可靠的输入是大量见过、用过、设计过 API 的工程师的体感（§1.4.2.2.2 d / §5.1.5）。
4. **信任与责任的主体资格**。这本就不是能力问题（§5.1.4），不在象限分析范围内，但属于"无方法学替代"。

几类容易被误归入 象限 III、实际上可由 象限 II 部分或大部分替代的能力，应被显式从 象限 III 中剔除：

- "bug 内在模型"中的**已知模式**部分由多采样 + 属性测试 + 差分测试覆盖；只有**novel 模式**部分进入 象限 III（第 2 项）。
- "调试呼吸节奏"在 AI 协作下被**部分替代**：当 trace + property failure + auto-bisect 工具链组装好后，人类 reviewer 看到的是 verifier 给出的 minimal failing case，不是从原 stack trace 起步。剩余部分回到 象限 III 第 1 项——有人能把 verifier 设计好。
- "复杂度肌肉记忆"几乎完全由经验复杂度回归 + property test 替代——除非学生未来要做算法研究本身。

#### 教育上的合成

三个象限直接决定了 §5.5 改进建议的形态：

- **象限 I 的训练量在 CMU 这样的项目里早已被压到接近零**——无需再大幅裁减。但课程**必须明确意识到**：今天的入门作业（实现链表、写小型解释器、扩展 Pebbles 内核）已经不再是 象限 I 的"教语法 / 教 API"，而是 象限 III 的 case base 训练——应该**显式承认这一目的**并据此设计评分，而不是借口"算法基础"或"系统基础"。
- **象限 II 的方法学必须作为新的内容类别加入**——这是当前 CS 本科**最大的内容真空**，也是回应"不会手写代码如何判断 AI 代码"这一典型疑问的实质答案。具体落点：§5.5.2-(2) Eval & Trust 必修课作为主战场；§5.5.1 中 15-251 注入 property-based + metamorphic 模块、21-325 注入 power analysis + 厚尾、17-313 拆出 Spec & Eval 半门。
- **象限 III 的训练强度按"narrow but deep"原则保留**——保留的目的不是产出能干活的初级程序员，而是构建后续与 AI 协作所需的最小 case base。对应作业的考核标尺也随之改变：不评学生独立完成 1000 行项目，而评学生能否说清"如果我让 AI 写这个，它最可能在哪里出错、我用 象限 II 中的哪种方法检验"。

> ⚠ **声明**：三象限的具体划分（特别是哪些传统能力落在 象限 II 可替代区）仍缺直接的对照实验证据。本节判断基于第一、二章已引文献的推论 + 各 象限 II 方法在工业界的成熟度，**不是**对每一项做了独立测量。MIT 2025 一项关于"将编码任务外包给 ChatGPT 的成年人脑部活动减少、记忆力变差"的短期实验 [[23]](https://dl.acm.org/doi/10.1145/3779312) 提供了**与 象限 III 主张方向一致**的初步神经学证据——但其测量的是短期认知负荷而非长期判断力，仍不能等同于"AI 协作下 18 个月后判断力差异"的对照实验。明确标注是设计建议而非实证结论。

---

## 5.3 教育对照：CMU SCS BS in Computer Science

### 5.3.1 为什么选 CMU

挑一个对照对象需要满足三条：(1) 课程结构与每门课的 syllabus **全部公开**；(2) 业内**广泛认可**，毕业生分布广，不会被指为"小众样本"；(3) 课程**显式结构化**（不是只列 60 门选修让学生自选），方便逐项映射缺口。三条同时成立的最佳候选是 **Carnegie Mellon University · School of Computer Science · Bachelor of Science in Computer Science**——其本科核心、限选与通识结构在 SCS 官网逐学期公布，关键课程编号（15-122, 15-150, 15-213, 15-251, 15-451 等）在过去 20 年高度稳定，整套体系常被业界称作"computer science 的样板"[[1]](https://csd.cs.cmu.edu/academics/undergraduate/requirements)。MIT EECS 的 Course 6-3 [[2]](https://catalog.mit.edu/degree-charts/computer-science-engineering-course-6-3/) 与 Stanford CS BS [[3]](https://csmajor.stanford.edu/) 是相近候选，本章在缺口分析处会引作参照。

### 5.3.2 CMU BS in CS 课程结构概要（2025–2026 学年公开版本）

按 SCS 官方 *Undergraduate Catalog* [[1]](https://csd.cs.cmu.edu/academics/undergraduate/requirements)，本科要求大致可拆为 7 块：

| 板块 | 代表课程 | 学分占比（粗略） |
|---|---|---|
| 数学基础 | 21-120/122 微积分、21-241/242 线性代数、15-151/21-127 离散数学、21-325 概率 | ~15% |
| 计算机科学核心 | 15-122 Principles of Imperative Computation、15-150 Principles of Functional Programming、15-210 Parallel & Sequential Data Structures and Algorithms、15-213 Introduction to Computer Systems (CSAPP)、15-251 Great Ideas in Theoretical CS | ~25% |
| 算法 / 理论选修 | 15-451 Algorithm Design and Analysis 或 15-455 Complexity 或 15-453 Formal Languages | ~5% |
| 软件系统选修 | 15-410 OS、15-411 Compilers、15-440 Distributed Systems、15-441 Computer Networks、15-445 Database Systems | ~10% |
| AI / ML 与应用选修 | 10-301/10-601 Introduction to ML、15-381 AI、11-411 NLP、15-462 Graphics 等任选一 | ~5% |
| 软件工程 / 应用 | 17-313 Foundations of Software Engineering（**唯一显式软件工程必选**），或 67-272 Application Design 等 | ~5% |
| 通识 / 写作 | 76-101 Interpretation and Argument、人文 / 社科 / 艺术若干 | ~15% |
| 自由选修 + 第二专业 / minor | —— | 余下 |

观察四点：

1. **理论与系统占绝对主轴**：15-122/150/210/213/251 这五门课构成 SCS 学生的"identity courses"，覆盖命令式、函数式、算法、操作系统视角与可计算理论。这条主轴是 1980s—2010s 的工业现实——执行主体是人——的最优解。
2. **软件工程作为一门 12 学分单课存在**（17-313），不是贯穿四年的工艺训练。
3. **AI / ML 是一个"应用方向选修"**，不是基础课。多数 CS 学生只接触一门 10-301 级别的入门。
4. **写作与论证仅有 76-101 一门必修**，且面向通用写作，不面向技术规约写作。

> 重要前提：CMU 不弱。它在每个传统板块（理论、系统、ML）都站在世界第一梯队。本章下面**所有缺口判断都在"对 AI 协作下的新分工而言"这个特定坐标系里**——不构成对 CMU 整体水平的评价，也不意味着 CMU 比 MIT/Stanford 差；这三家在所讨论的几乎所有缺口上**症状相同**。

---

## 5.4 课程缺口诊断

把 §5.1.6 的五个角色与 §5.2 的能力栈合在一起，逐一对到 §5.3.2 的课程结构上，得到 8 处具体缺口。每一处都给出"现有什么、缺什么、为什么对新分工是问题"的三段式。

### 5.4.1 缺口一：Spec 工程没有教学载体

- **现有什么**：15-150（Hoare logic 的轻量介绍）、15-251（逻辑、形式语言）、15-122（前后置条件的入门 contract programming），以及 17-313 一两周的需求工程内容；
- **缺什么**：把**自然语言需求 → 可执行 spec → 验收测试 → reward signal** 当作主线训练的整门课；以及 §5.2.2 在约束设计者上层提到的 *reward hacking / 反过拟合* 思维——当前形式化方法的训练（如 Coq / TLA+）局限在研究生选修（15-414/15-819），本科生几乎没有写过"喂给 agent 的 spec"，更没经历过被 agent 钻空子的回路。
- **为什么是问题**：§5.1.1 把这一项定为人类的第一份不可让渡职责；§5.2.2 把上层判断力定义为"必须反复经历才长得出来"——但 CMU 没有任何一门课提供这样的反复经历。

### 5.4.2 缺口二：评测与可信信号工程化训练不足

- **现有什么**：15-451 / 15-411 等课布置作业自动评分；10-301 教交叉验证；17-313 一节课讲 unit testing。
- **缺什么**：(a) 把 §1.3 P6 的全部 syndromes（benchmark 污染 [[31, ch.1]]、LLM-as-Judge 偏置 [[21, ch.1]]、生产 trace 重放、对抗集构造、property-based、变异测试）作为**一门工程课**的核心；(b) §5.2.2 中层提到的**实验设计 + 厚尾 / 稀有事件估计**——CMU 的概率课覆盖均值方差，但对 Anscombe's quartet、p-hacking、power analysis 不做工程化训练。当前 CMU 离这条线最近的是 17-355 Program Analysis 与 18-636 Software Engineering for AI——前者偏理论、后者属研究生层。
- **为什么是问题**：§2.1.4 已把"判别力"列为新护城河；§5.1.2 把它列为第二份残余职责；§5.2.3 进一步给出实质构成——**大部分**判别力可由象限 II 方法学（property-based / metamorphic / differential / 多采样一致性 / 形式验证等）承担，**少部分**（起草 rubric 的判断力）属于象限 III，需要亲手吃过弱测试苦头形成 case base。两半合起来要求的复合训练目前一门课都没有。

### 5.4.3 缺口三：可逆性与影响半径——抽象重定价没有载体

- **现有什么**：17-313 的 design pattern 章节、15-214 Principles of Software Construction（讲 OO 抽象、依赖管理）。
- **缺什么**：以 **reversibility / blast radius** 为目标函数的设计课；§5.2.2 中层提到的 FMEA / 事故复盘 / 厚尾决策思维。当前 design pattern 训练仍在教 GoF 的"减少重复"目标；ADR / 风险登记册 / 灰度发布的工程动作没有专门课程载体；学生离"真正运维过一个系统"的距离普遍是零。
- **为什么是问题**：§2.1.2 已经把抽象的目标重定价为 *locality of change* 与 *reversibility*；§1.4.3 把"建造 + 腐化双加速"作为底层约束；§5.2.2 把上层判断力锁定在"有过真实运维经历"——三者合起来要求一种当前 CS 课程不提供的复合训练。

### 5.4.4 缺口四：Harness 不是新 OS，但需要一类横跨多门课的混合工程训练

- **现有什么**：15-410 OS、15-440 Distributed Systems、10-417 Deep Learning Systems、18-330 Computer Security。
- **辨析（先把误判挪开）**：把 harness 比作"新型操作系统"是种过度引申——它与 OS 是**同位异层**关系（harness 跑**在** OS 之上），仅仅"调度对象 / 权限模型 / 可观测性"换了名字并不构成抽象层级的新事物。逐项拆开来看：
  - **调度对象**：OS 调度的是有确定执行语义的进程；harness 调度的是"按 LLM 输出内容决定下一步"的非确定子任务。这意味着 harness 的 scheduling decision **依赖对调度对象输出的语义解释**，这把它从内核层下移到 *shell / workflow engine* 层。Airflow / Temporal / make / shell 早就在做"按上一步输出决定下一步"；harness 的新东西是输出为自然语言、需要 judge / verifier 解释，但**调度学**层面没有翻新。
  - **权限模型**：POSIX DAC + namespaces + cgroups + `capabilities(7)` 在**表达力**上完全可以描述 tool / file / network capability——能不优雅是另一回事，容器生态每天都在做。harness 真正的新约束不在表达力，而在**主体本身可被它处理的数据攻陷**（§1.3 P8 提示注入是架构级的，LLM 在内部不区分代码与数据）。传统 capability 系统的 *confused deputy* 假设是"主体偶尔被骗"；LLM 主体的语境是"处理任何外部内容都等同于把权能委托给该内容"。这把权限设计重心从 *capability / DAC* 谱系推向 **mandatory information flow control / DIFC** 谱系——Bell-LaPadula、SELinux、Asbestos、HiStar 这一脉，对**单一主体内部的数据流**也设标签——而这是研究生层的 OS 安全内容，本科 18-330 不教。
  - **可观测性**：syscall trace 与 prompt-response trace 在工具链上同源（eBPF / journald / OpenTelemetry 早就支持非结构化日志）；真正的差异在 trace 的**地位**——syscall trace 是事后调试品，prompt-response trace 在新分工下是**一等交付物**（§2.1.5），决定了 schema、retention、回放与 diff 的设计要求。这是工程动作层面的差异，不是底层抽象的差异。
  - **非确定性**：进程在给定输入下执行确定；LLM 调用在相同输入下因 batching、温度、并发负载仍会漂移（§1.3 P5 [[29, 30, ch.1]]）。这让 *replay* 与 *deterministic scheduling* 在严格意义上失效——harness 必须像蒙特卡洛仿真平台那样做"随机种子 + 多次复跑 + 统计判别"，这是仿真与实验科学的范畴，与 OS 调度无关。
  - **资源会计**：OS 计 CPU / RAM 配额；harness 还要联合优化 token 成本、verifier 质量分、延迟预算——这是云成本管理 + 质量控制的合成，不是新内核概念。
- **真正的缺口**：上面四条横跨 *workflow / shell 编排语义*（15-440 不覆盖语义层）、*MAC / DIFC 安全*（18-330 不覆盖）、*非确定性下的实验工程*（10-417 不覆盖）、*token × quality × latency 联合资源会计*（无课覆盖）。本科生没有一门课把这四片拼起来；研究生选修也是分散的。§1.3 C14–C18（Harness / Sandbox / Tracing / Cache & Routing）的工程实践目前**完全没有进入本科课程**——但缺的不是"一门新 OS 课"，而是把上述四片黏合的混合工程纪律。
- **为什么是问题**：§5.1.3 把过程与可逆性设计列为第三份残余职责；它的工程载体正是这种横跨多门课边界的纪律——而正因为它横跨边界，没有任何一门现有必修自然地承担它。

### 5.4.5 缺口五：阅读密集型工作的训练强度不足

- **现有什么**：15-213 让学生读 CSAPP 教材并实现 shell / proxy / malloc；15-410 让学生扩展一个内核。
- **缺什么**：**长期、规模化的代码库导航 / 维护 / 重构课**。当前作业的"读"是为了"写自己的实现"，而不是为了"在一个百万行规模的活体代码库里做一次审慎修改"。§2.1.1 引用 Xia et al. (TSE 2018) [[13, ch.2]] 给出"58% 时间花在阅读理解"的硬数字；§5.2.3 又把"调试呼吸节奏"划为**部分可由 trace + property failure + auto-bisect 工具链替代**，但 verifier 的**设计**一环仍是 象限 III——综合下来当前作业既不训练大代码库的耐心阅读、也不教 trace 工具链的工程使用、也不积累足够小时数的 verifier 设计经验，三层都薄。
- **为什么是问题**：reviewer 是 AI 协作下产出量的真实瓶颈——AI 写代码的速度远超审阅速度（§1.4.2.2.1 A），人类如果不被训练成更快的读者，整个回路就崩在这一环。

### 5.4.6 缺口六：审美与品味的训练机会被压缩

- **现有什么**：15-150 的 elegance 训练、15-410 的代码品味要求（"good kernel hackers"）、15-462 Graphics 对视觉审美的潜移默化。
- **缺什么**：以"长期审美 / 跨范式比较 / 用户共情"为显式目标的课。CS 主轴课程默认审美训练是**副产品**；§5.2.3 把"长期审美决策"明确列为象限 III 的核心项之一——它没有有效的统计代理量，仍依赖工程师亲历积累的体感（抽象的代价感、接口设计的体感）。当 AI 把"产生看似合理的代码"变成几乎免费时，副产品式的审美训练会让学生**失去强化机会**，因为他们不再从零写一个 parser，也就不再在跌跌撞撞中体会"这个 AST 为什么应该长这样"。
- **为什么是问题**：§5.1.5 把审美列为第五份残余职责。AI 的取舍倾向于训练集多数派（§1.4.2.2.2 d），人类如果失去独立审美能力，最后会被自己的工具拉回平均水平。

### 5.4.7 缺口七：信任、责任、合规与安全的实操不足

- **现有什么**：18-330 Computer Security、15-330 Introduction to Computer Security（每隔几年开一次）、17-200 Ethics and Policy Issues in Computing。
- **缺什么**：把 **Prompt Injection（§1.3 P8）、AI 出错时的问责链、合规审查动作、红队演练**作为一组**实操**而非论辩的课程；以及 §5.2.2 中层列的跨学科最小整合（法律契约结构、当代 AI 治理框架）。17-200 偏伦理思辨，18-330 偏密码与系统漏洞——两者都没有覆盖 AI 时代真正的安全 / 信任工艺，跨学科一环更是空白。
- **为什么是问题**：§5.1.4 把信任承担列为不可让渡的第四份职责；如果毕业生不会做 prompt-injection 红队、不会写风险登记册、不知道什么时候必须 human-in-the-loop，他们就无法**作为 principal** 接管 AI 系统。

### 5.4.8 缺口八：考核与学术诚信本身需要重设计

- **现有什么**：15-122/150/213 等课的作业禁止使用 AI 协助，或要求"声明使用范围"；课程评分依赖 final exam 与编程作业的"独立完成"假设。
- **缺什么**：把"使用 AI 是默认状态"作为前提的考核机制——评的是**过程 trace、决策记录、verifier 设计、对 agent 输出的批判性接受**；但与此同时，§5.2.3 的外部约束要求**手工训练强度不被削弱**——所以新的考核机制必须**同时**做到两件看似矛盾的事：一是让学生在大量作业里直接面对 AI、二是在某些被显式标记为"赤手作业"的环节强制脱离 AI 完成。
- **为什么是问题**：这一项把前面 7 个缺口的修复**机制性地堵死**——只要考核仍按单一的"独立 + 笔试"模式组织，老师就既不能要求学生在某些作业里设计 spec / eval / harness（因为这些动作本身需要 AI 参与才有意义），又不能确保学生在另一些作业里完成必要的手工训练。

---

## 5.5 改进建议

按"先改造既有课程、再新增、最后重设计教学方法"的顺序给出。**所有建议都受 §5.2.3 的两条外部约束**：(i) identity courses 的手工训练强度只能加重不能减弱；(ii) 评价标尺从"独立 ship"转向"能否说清 AI 在哪里会错"。

### 5.5.1 改造既有课程

每行第三列标注**主要象限定位**（§5.2.3）——这一栏决定了该课程的考核标尺。

| 课程 | 现在的形态 | 建议改造 | 象限定位 |
|---|---|---|---|
| 15-122 Imperative Computation | 教 contract programming 入门 | 把 contract 提升到 spec 工程入门——为每个作业先写**机器可检的规约**（前后置、不变式、property test），并提交"如果让 agent 实现这个 spec 会有哪些歧义"的反思 | **III 为主**（建 spec / case base 的 deliberate practice）；实现部分由学生独立完成 |
| 15-150 Functional Programming | 教 SML 与等式推理 | 引入 *spec → 多个候选实现 → 自动 verifier 择优* 的练习，对应 §1.4.3.3 第 2 点；显式加入 **QuickCheck / Hypothesis 风格的 property-based test** 教学（应对 §5.2.3 象限 II） | **III + II**：等式推理是 III，property test 是 II |
| 15-213 CSAPP | 让学生独立实现 shell / proxy / malloc | 保留全部赤手实验**不打折**——它们是 §5.2.3 象限 III 第 1 项 "写 spec / 不变式所需 case base" 的最强训练场；额外加"在 1k+ 文件真实开源项目里完成受约束 bugfix"作业训练 §5.4.5 阅读密集 | **III 主**；开源作业部分进入 II（trace 工具链） |
| 15-251 Great Ideas | 教逻辑、可计算、复杂度 | 加入**评测理论模块**：判别力、对抗集、benchmark 污染、judge 偏置（§1.3 P6）；引入 reward hacking / Goodhart 的形式化讨论；新增 **metamorphic relation 与 differential testing 的理论基础** | **II 加重**：把 象限 II 方法的数学基础落在这门课 |
| 15-410 OS | 让学生扩展 Pebbles 内核 | 保留内核作业不打折（§5.2.3 象限 III 的另一最强训练场）；加一周 *harness 不是 OS* 辨析单元（§5.4.4），引入 namespaces / cgroups / `capabilities(7)` 工业用法 | **III 主**；harness 单元属 II |
| 18-330 Computer Security | 偏密码与系统漏洞 | 引入 *mandatory information flow control / DIFC* 模块（Bell-LaPadula、SELinux、Asbestos、HiStar），并把 LLM 主体的提示注入作为其核心驱动场景；加入 **taint analysis / 符号执行** 实操 | **II**（信息流方法学）+ 象限 III 中安全的 case base |
| 17-313 Foundations of SE | 一学期讲完需求 / 设计 / 测试 / 维护 | 拆成两门：**17-313A Spec & Eval Engineering**、**17-313B Process, Reversibility & Harness**（见 §5.5.2）| 主要 **II**（方法学）+ 三象限分类的元认知 |
| 76-101 Interpretation and Argument | 通用写作 | 增加 *technical specification writing*、*change proposal / ADR writing*、一周**法律 / 政策语言入门**仿写（应对 §5.2.2 约束设计者底层） | **III**：spec 写作的判断力 |
| 21-325 概率 | 描述性 + 假设检验入门 | 加 power analysis、effect size、厚尾分布、p-hacking 案例、贝叶斯模型证据；这些是几乎所有 象限 II 方法的数学基础 | **II 的底盘** |

### 5.5.2 新增必修与选修课

建议在 SCS 增设以下 4 门：

1. **15-3xx Specification and Verification Engineering**（必修，原 17-313 的一半 + 新内容）。
   主题：自然语言需求 → 形式 / 半形式 spec → property-based test → 验证矩阵；reward shaping；spec 反过拟合 (Goodhart / reward hacking)。**关键设计**：每个作业要求学生先写 spec，再让 agent 试图实现，**故意奖励"agent 钻空子"**——学生在被钻空子后重写 spec 直到 agent 无空可钻。这就是 §5.2.2 上层"反复经历"的工业化形式。先修 15-150、15-251。
2. **15-3xx Eval & Trust Engineering for AI-augmented Systems**（必修）。
   **这门课是 §5.2.3 象限 II 方法学的主战场**——内容以系统化教授九种 象限 II 方法为骨架：
   - 多采样一致性（Brown et al. *Large Language Monkeys* [[49, ch.1]](https://arxiv.org/abs/2407.21787) 的覆盖率扩展曲线作为底层数学）；
   - 属性测试（QuickCheck / Hypothesis 实操）[[15]](https://www.cs.tufts.edu/~nr/cs257/archive/john-hughes/quick.pdf)；
   - 变形测试（写 metamorphic relation）[[16]](https://www.cse.cuhk.edu.hk/~smyiu/publications/HKUST-CS98-01.pdf)；
   - 差分测试（multi-implementation + 旧版基线）[[18]](https://www.cs.swarthmore.edu/~bylvisa1/cs91/f15/Papers/Differential-Testing-McKeeman.pdf)；
   - 形式化验证入门（Lean / TLA+ 实操）[[19]](https://leanprover-community.github.io/)；
   - 经验复杂度回归 + counterfactual 扰动 + trace 异常聚类；
   - 校准过的 LLM-as-judge（含偏置补偿，§1.3 P6 [[21, ch.1]](https://openreview.net/forum?id=3GTtZFiajM)）。
   配套实验：给一个开源 AI 系统设计并交付一份**可被第三方复核的 eval suite**，要求至少使用上述四种方法的组合。**关键设计**：考核要求学生**先赤手写一个有意义的 bug**（覆盖 §5.2.3 象限 III 第 1 项），再设计能逮住它的 eval——这把"写弱测试的痛"显式纳入训练；它直接回应"不会手写代码如何判断 AI 代码"这一典型疑问——这门课的答案是**学这九种方法，并用它们的组合代替逐行肉读**。
3. **15-4xx Agent Harness Engineering**（选修）。
   定位**不是** "15-410 在新主体上的复制"——§5.4.4 已论证 harness 在抽象层级上不与 OS 同层。这门课的作用是把 15-440（编排语义）、18-330（DIFC / 信息流安全）、10-417（非确定性实验工程）、加上无课覆盖的 token×quality×latency 联合资源会计**黏合**起来。学期项目：从零搭一个**最小可用 harness** 并用它跑通一个真实 bugfix 流，交付 *spec + eval + trace + 成本 / 质量曲线*。
4. **17-2xx Reading Engineering**（必修，1 学期 6–9 学分的实践课）。
   主题：在多个百万行级开源项目里做受约束的导航 / 重构 / 改 bug；显式训练 chunking、依赖追踪、批判性阅读 AI 生成代码。**关键设计**：要求 deliverable 是**可被项目维护者接受的 PR + 阅读笔记**——和写代码的作业是镜像的；学生必须能解释"为什么这段 AI 建议的修改是错的"（§5.2.3-(1)/(5) 的直接训练目标）。

另建议把以下两门提到**强烈推荐**层（不强制但默认选）：

- **17-3xx AI Safety, Security & Accountability**：覆盖 §5.4.7 与 §5.2.2 信任承担者的跨学科最小整合（法律契约结构、AI 治理框架、红队实操）。
- **15-3xx Design Taste Workshop**：以 case study + 设计评审 + 跨范式作品比较为主，没有 leetcode-style 评分；学生需提交"自己半年前 / 一年前作业的回顾"用于审美对照（§5.2.3-(2)/(3) 的直接训练形式）；覆盖 §5.4.6。

### 5.5.3 教学方法：三象限作业体系 + 过程审计

§5.4.8 已经指出新分工要求考核机制同时做到几件看似矛盾的事。下面的五条机制按 §5.2.3 三象限组织：

1. **作业按象限三分**：
   - **象限 I 作业**（"AI 全权"）：劳动量型任务（boilerplate 生成、跨语言翻译、文档书写），由 AI 完成，学生只评"AI 是否正确"且评得快；这类作业占比应**主动压缩**，因为重复练它已无边际收益。
   - **象限 II 作业**（"AI 默认型 + 方法学验证"）：学生设计并应用一组 象限 II 方法（属性测试 / 变形测试 / 差分测试 / 多采样一致性 / 形式验证 / 经验复杂度回归 / counterfactual 扰动等）来**验证 AI 的输出**，主交付物是**验证报告 + session trace**，不是代码本身。这是新增的主流作业形态。
   - **象限 III 作业**（"赤手"）：学生独立完成，**禁止 AI 协助**，目的是构建 象限 III 的 case base（起草 spec 的判断力、识别 novel 失败模式、长期审美）。这类作业**份额随年级下降但不归零**——大一约占 50%，大四仍保留约 15%。用**封闭网络的考试机房 / 远程监考**确保完成纪律。
2. **象限 II 作业评的是方法学的设计与执行**。分数由四件事决定：(a) 选了哪几种 象限 II 方法、为什么；(b) 这些方法的不变式 / metamorphic relation / property 写得是否有判别力；(c) 决策点是否记录了取舍（包括"为什么没用形式验证而用属性测试"这种取舍）；(d) 对 AI 输出的拒绝点是否说得清"用方法 X 在这里抓到了问题 Y"。代码本身是否 "work" 由 verifier 自动给出，不进入主观评分。
3. **象限 III 作业评的是 case base 的构建质量**——不是产出量。包括：能否在没有 AI 提示下定位非典型 bug 并复盘出可迁移的教训、能否手写一份**有判别力**的 spec（被故意找空子的 AI 实现钻不破）、能否做出**自己半年后还愿意维护**的设计。"独立 ship 1000 行项目"**不是**评分维度。
4. **期末"对照考"作为锁扣**。期末考核分两段：闭卷部分检查 象限 III 基础（手算复杂度、构造反例、读未见过的代码）；开卷带 AI 部分给一个未见过的工程问题，评学生**怎么用 象限 II 方法**给出验证报告。两段都通过才算合格。这把 §5.2.3 三象限的训练目标"绑"到同一个评分锚点上。
5. **审美由 case-based 评审承担**。设立"设计评审日"——学生提交一段 API、一份 spec、一段重构提案，并附**自己半年前 / 一年前同类作品的对比**，由教师与高年级同学组成评审组**口头答辩**。这是 §5.4.6 与 §5.2.3 象限 III 第 3 项的合并训练。

> ⚠ **声明**：以上五条机制目前在主流 CS 本科教育中尚无大样本对照研究。本节判断是从软件工程方法论的近邻领域（agentic coding 实践、CS 写作型课程的 portfolio 评分经验、传统赤手考试机制）类比推断，仍需面向真实课堂做实证评估。

### 5.5.4 学校教育的范围边界：为什么不能依赖产业接续

Russinovich & Hanselman 在论文里提出一条诱人的退路：让本科教育止步在概念基础，把判断力训练交给产业内的 mentorship / residency 制度——借鉴医学教育，结构化轮岗、senior + junior pair 一年以上、导师工作作为组织 KPI、终结认证；Hanselman 的护士—工程师类比："正如护士需证明临床操作能力，工程师亦应" [[21]](https://dl.acm.org/doi/10.1145/3779312)。

这个方案在**制度形式上**是吸引人的——它把"毕业到能独立工作的两年"显式纳入训练管道，与第五章 §5.1–§5.5 设计的本科四年形成连续。但**它依赖的产业激励基础是结构性不稳定的**，本节论证为什么不能把它当作可依赖的下游保障。

**第一层不稳定性：资深工程师的 disincentive**。在 AI 已能替代中初级执行职能的语境下，资深工程师投入大量时间指导可能取代自己的学员，是与个人职业利益直接对立的。论文承认这点，并寄希望于"组织 KPI"将其纠正——把 mentorship 计入 senior 的考核与报酬。但 KPI 设计本身在多数公司也由资深工程师群体主导，结构上存在 *谁去推动这条 KPI* 的循环依赖问题。

**第二层不稳定性来自 Charity Majors 的从业观察**："过去几年里，在我见过的每一家开始招聘初级工程师的公司中，这项举措都是由资深工程师主导并推动的" [[22]](https://leaddev.com/career-development/a-model-for-growing-the-next-generation-of-developers)。这反过来说明——**目前仍在维护初级管道的，恰好是那批在前-AI 时代完成了完整训练、保有"初级是未来资深"认知框架的资深工程师**。他们退休或更换雇主后，新一代资深（在 AI 协作下成长起来、本身未经历完整管道）是否会保有同样的认知动机，没有结构性保障——这是一个**以一代为周期**的衰减风险。

合起来：产业 residency 是值得追求的**理想形式**，但**不能作为本科课程设计可依赖的下游保障**。这把一个本来可以被推到毕业后的训练目标——novel 失败模式识别（§5.2.3 象限 III 第 2 项）、长期审美决策（§5.1.5 / 象限 III 第 3 项）、生产可观测性 + 事件响应、组织内部信任与责任承担（§5.1.4）——**全部压回到本科四年之内**。

对前面几节的具体影响：

- §5.5.1 改造表中"案例 case base 训练"列出的赤手实验强度**不能减弱**——产业不一定继续提供训练机会；
- §5.5.2-(4) Reading Engineering 必修课的"在百万行级开源项目里完成受约束 bugfix"作业**必须保留为本科必修**而不能改为企业实习项目；
- §5.6 第 5 条提到的"教育改革须与产业 mentorship 同步"必须降级表述为：**理想情况下同步**；**现实情况下教育不能依赖产业接续，必须独立承担到位**。

> ⚠ **声明**：本节关于"senior disincentive 在多大程度上决定产业管道维护的可持续性"的论证，主要依赖于 Charity Majors 的从业观察 [[22]](https://leaddev.com/career-development/a-model-for-growing-the-next-generation-of-developers) 与 Reddit / The Register 论坛讨论中反映的从业者观察，**尚缺定量的行业研究**。但这是 *设计取向* 而非定量结论——即便 senior disincentive 只有 30% 概率成为主导，把判断力训练完全押在产业接续上就承担了不可接受的失败风险，教育独立承担到位是 risk-dominant 的选择。

---

## 5.6 实施风险与节奏

不是所有缺口都同等紧迫，也不是所有改造同等廉价。下表给出**紧迫度 × 实施成本**的二维排序，并标出建议落地节奏。

| 改造 / 新增项 | 紧迫度 | 实施成本 | 建议节奏 |
|---|---|---|---|
| 5.5.3 教学方法转型（三象限作业体系 + 过程审计）| **极高** | **低**（主要是规则与公约调整） | **第 1 学期**就启动 |
| 5.5.1 既有课程局部改造（15-122 / 76-101 / 17-313 / 21-325 内容微调） | 高 | 低—中 | 第 1–2 学期内完成 |
| 5.5.2-(1) Spec & Verification 必修课 | 高 | 中（需要新教材、配套 verifier 实验环境） | 第 2–4 学期 |
| 5.5.2-(2) Eval & Trust 必修课 | 高 | 中 | 第 2–4 学期 |
| 5.5.2-(4) Reading Engineering 必修课 | 高 | 中—高（需要长期开源协作关系） | 第 3–6 学期 |
| 5.5.2-(3) Harness Engineering 选修课 | 中 | 中（依赖外部工业实践沉淀） | 第 4–8 学期，可作为系列研讨课先开 |
| 17-2xx Safety/Accountability 强推 | 中 | 低（多为案例 + 红队工作坊） | 第 2–4 学期 |
| Design Taste Workshop | 中 | 低 | 第 2–4 学期 |

四个主要落地风险：

1. **教师侧的工具熟悉度不均**。"过程审计"型评分对教师本身的 AI 工程熟练度有要求；建议先在助教培训中引入 §1.4.2 / §2.1 / §5.2 的概念框架，让助教先成为"懂得评 spec 与 eval"的 reviewer。
2. **学术诚信框架的滞后**。多数高校的诚信条款仍按"独立完成"组织——这与 §5.5.3-(1) 的双轨制直接冲突。建议先以课程层面 opt-in 的形式试点，再推动制度层面的修订。
3. **新增必修课的学分挤占**。CMU SCS 本科总学分已接近上限，新增三门必修必然要替换或合并旧课。建议把 15-251 / 17-313 / 15-122 / 21-325 四门作为"内容更新"的主入口（注入 spec / eval / 阅读理论 / 厚尾统计模块），减少独立新课的数目。
4. **象限 III 作业的偷工陷阱**。三象限作业里只有 象限 III 一类禁止 AI，份额又最小；学生很容易在这里偷用 AI——尤其是当他们看到"反正最后期末考会带 AI"时。这会绕开 §5.2.3 象限 III 的整个 case base 构建目的。**期末闭卷段必须是真正可以挂科的硬阀门**，否则三象限会塌成"全部象限 II / I"——所有判断力的认知前提都丢失。
5. **产业管道反馈环**。第五章假定毕业生进入一个仍会招他们的产业；这条假设被实证反驳——哈佛 GPT-4 后跟踪研究显示 22–25 岁 AI 相关岗位就业率下降约 13% [[21]](https://dl.acm.org/doi/10.1145/3779312)；行业 2022 年以来初级开发者招聘量降 67% [[21]](https://dl.acm.org/doi/10.1145/3779312)。Russinovich & Hanselman [[21]](https://dl.acm.org/doi/10.1145/3779312) 把此现象称为"金字塔窄化假说"：AI 消除了初级开发者赖以学习的入门级工作后，培养下一代资深工程师的人才梯队结构性崩塌，构成自我强化的反馈环（无初级 → 长期无资深 → 训练数据质量下降 → 模型与人都退化）。这条上游约束**不能由教育侧独立解决**——但反过来也意味着教育侧**不能依赖产业侧的接续训练**，详见 §5.5.4 的论证。

---

最后值得点明的一件事：本章给出的建议**没有任何一条**要求学生少学传统 CS 内容。Brooks 的 *essence vs accident*（§1.4.1）在 AI 时代依然成立——**本质复杂性不可消除**。算法、操作系统、可计算理论、离散数学这些课程训练的正是**应对本质复杂性**的能力，它们在新分工里不是被削弱而是被**更稀缺地需要**——因为 AI 已经把 *accidental complexity* 的处理成本压到了接近 0，留给人类的全部都是 essential 的那一半。

但 §5.2.3 把这件事进一步**拆细**：accidental complexity 的手工训练**应该被有选择地**重新分配——AI 已稳定接管的部分（象限 I）应让位、象限 II 方法学可以验证的部分应交给方法学（这是教育上的**新增**内容）、**仅有作为后续判断力 case base 的窄部分**（象限 III）才作为手工训练保留下来。教育的任务于是变成三件事：(1) 把传统理论 / 系统主轴的本质复杂性训练继续讲；(2) **新增** 象限 II 方法学作为独立内容；(3) **保留但收窄** 象限 III 的手工训练。这与两种直觉都不同——既不是"既然 AI 能做就让 AI 做"，也不是"以前怎么写现在还怎么写"。

---

## 参考文献

[1] Carnegie Mellon University, School of Computer Science, "Undergraduate Program Requirements — Bachelor of Science in Computer Science," *CMU CSD Official Catalog*. [Online]. Available: <https://csd.cs.cmu.edu/academics/undergraduate/requirements>

[2] Massachusetts Institute of Technology, "Computer Science and Engineering (Course 6-3) — Degree Chart," *MIT Course Catalog (Bulletin)*. [Online]. Available: <https://catalog.mit.edu/degree-charts/computer-science-engineering-course-6-3/>

[3] Stanford University Computer Science Department, "Undergraduate Major in Computer Science — Program Requirements," *Stanford CS Major Handbook*. [Online]. Available: <https://csmajor.stanford.edu/>

[4] F. Chollet, "Sufficiently advanced agentic coding is, in essence, machine learning," *X (formerly Twitter)*, Feb. 2026. [Online]. Available: <https://x.com/fchollet/status/2024519439140737442>

[5] E. Mollick, "Co-Intelligence and the Classroom: How to Teach with AI in the Room," *One Useful Thing*, 2025. [Online]. Available: <https://www.oneusefulthing.org/p/co-intelligence-and-the-classroom>

[6] ACM / IEEE-CS / AAAI Joint Task Force, "Computer Science Curricula 2023 (CS2023): Curriculum Guidelines for Undergraduate Degree Programs in Computer Science," *ACM*, 2023. [Online]. Available: <https://csed.acm.org/>

[7] Stack Overflow, "2025 Developer Survey: AI Tooling, Adoption and Sentiment," *Stack Overflow*, 2025. [Online]. Available: <https://survey.stackoverflow.co/2025>

[8] D. L. Parnas, "Software Engineering: Multi-Person Development of Multi-Version Programs," in *Lecture Notes in Computer Science*, vol. 6707, Springer, 2011. (Argues that the gap between "programming" and "software engineering" is the gap CS curricula chronically under-teach.)

[9] J. Prather *et al.*, "The Robots are Here: Navigating the Generative AI Revolution in Computing Education," in *Proc. ITiCSE-WGR 2023*, ACM, 2023. (Working group report on how CS programs are adapting — or failing to adapt — to LLM-assisted programming.) [Online]. Available: <https://dl.acm.org/doi/10.1145/3623762.3633499>

[10] B. A. Becker *et al.*, "Programming Is Hard — Or at Least It Used to Be: Educational Opportunities and Challenges of AI Code Generation," in *Proc. SIGCSE 2023*, ACM, 2023. [Online]. Available: <https://dl.acm.org/doi/10.1145/3545945.3569759>

[11] J. Skalse, N. H. R. Howe, D. Krasheninnikov, and D. Krueger, "Defining and Characterizing Reward Hacking," in *Advances in Neural Information Processing Systems (NeurIPS)*, 2022; *arXiv preprint*, arXiv:2209.13085. [Online]. Available: <https://arxiv.org/abs/2209.13085>

[12] C. H. Tinsley, R. L. Dillon, and P. M. Madsen, "How to Avoid Catastrophe," *Harvard Business Review*, April 2011. (Drawing on the safety records of high-reliability industries: aviation, nuclear, healthcare.) [Online]. Available: <https://hbr.org/2011/04/how-to-avoid-catastrophe>

[13] R. A. Bjork, "Memory and Metamemory Considerations in the Training of Human Beings," in *Metacognition: Knowing about Knowing*, J. Metcalfe and A. Shimamura, Eds., MIT Press, 1994, pp. 185–205. (Origin of the "desirable difficulties" framework: introducing friction into practice yields better long-term retention and transfer.) [Online]. Available: <https://bjorklab.psych.ucla.edu/wp-content/uploads/sites/13/2016/04/Bjork_1994.pdf>

[14] R. C. Schank, *Dynamic Memory: A Theory of Reminding and Learning in Computers and People*. Cambridge University Press, 1982. (Origin of case-based reasoning: experts develop and index libraries of past failure/success cases — a *case base* — and reason by analogy from them.) [Online]. Available: <https://www.cambridge.org/core/books/dynamic-memory/E0E0FE3D38E5EBABD15F73CB9F36D7A6>

[15] K. Claessen and J. Hughes, "QuickCheck: A Lightweight Tool for Random Testing of Haskell Programs," in *Proc. ACM SIGPLAN Int. Conf. Functional Programming (ICFP)*, 2000, pp. 268–279. (Foundational property-based testing paper; spawned Hypothesis (Python), Proptest (Rust), Hedgehog, and the entire PBT ecosystem.) [Online]. Available: <https://www.cs.tufts.edu/~nr/cs257/archive/john-hughes/quick.pdf>

[16] T. Y. Chen, S. C. Cheung, and S. M. Yiu, "Metamorphic Testing: A New Approach for Generating Next Test Cases," HKUST Tech. Report HKUST-CS98-01, 1998. (Foundational paper on metamorphic testing: instead of requiring an oracle that says "what is the right output", define *metamorphic relations* between inputs and outputs — e.g., `sort(reverse(L)) == reverse(sort(L))` — and test those.) [Online]. Available: <https://www.cse.cuhk.edu.hk/~smyiu/publications/HKUST-CS98-01.pdf>

[17] S. Segura, G. Fraser, A. B. Sánchez, and A. Ruiz-Cortés, "A Survey on Metamorphic Testing: Recent Advances and Open Challenges," *ACM Computing Surveys*, 2025. (Comprehensive recent survey on metamorphic testing, including its application to ML/LLM systems.) [Online]. Available: <https://dl.acm.org/doi/10.1145/3631971>

[18] W. M. McKeeman, "Differential Testing for Software," *Digital Technical Journal*, vol. 10, no. 1, pp. 100–107, 1998. (Foundational paper introducing differential testing as a black-box technique to detect divergence between independent implementations of the same specification — exactly the technique now used to validate AI-generated code against a reference implementation.) [Online]. Available: <https://www.cs.swarthmore.edu/~bylvisa1/cs91/f15/Papers/Differential-Testing-McKeeman.pdf>

[19] Lean Prover Community, "Lean 4 and Mathlib: Industrial-Strength Formal Verification," 2024–2025. (Project page documenting the rapid maturation of Lean 4 + Mathlib in 2024–2025: industrial users (AWS, Microsoft) reporting ROI-positive use of Lean for verifying critical infrastructure; tactic frameworks (Aesop) dramatically lowered the proof-engineering cost.) [Online]. Available: <https://leanprover-community.github.io/>

[20] Google DeepMind, "AI Achieves Silver-Medal Standard Solving International Mathematical Olympiad Problems," *DeepMind Blog*, July 2024. (AlphaProof: an RL system that uses Lean formalization as a verifiable reward signal — a working example of formal verification serving as the reward function for AI training, directly tied to Quadrant II reward-hacking detection.) [Online]. Available: <https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/>

[21] M. Russinovich and S. Hanselman, "Programming Is Not Software Engineering: The Crisis in Junior Developer Mentorship," *Communications of the ACM*, vol. 69, no. 4, April 2026. (Introduces the "pyramid narrowing hypothesis"; cites Harvard 22–25 employment data showing ~13% decline post-GPT-4 in AI-adjacent roles, industry-wide 67% drop in junior developer hiring since 2022, and the MIT 2025 cognitive debt study summarized in [23].) [Online]. Available: <https://dl.acm.org/doi/10.1145/3779312>

[22] S. Hanselman, "A Model for Growing the Next Generation of Developers" (interview, with extended discussion of mentor incentives and Charity Majors' observation that "every company I've seen restart junior hiring did so on the senior engineers' initiative"), *LeadDev*, 2026. [Online]. Available: <https://leaddev.com/career-development/a-model-for-growing-the-next-generation-of-developers>

[23] MIT cognitive debt study referenced in Russinovich & Hanselman [21] (2025): adults outsourcing coding tasks to ChatGPT exhibit reduced neural activity and degraded memory retention compared to controls. (Primary citation pending; data summarized via [21].) [Online]. Available: <https://dl.acm.org/doi/10.1145/3779312>
