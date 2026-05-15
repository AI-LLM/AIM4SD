# Chapter 5. Human Education

> Chapter 1 §1.4.2 located AI's position in software engineering as **"subject replacement"** — it takes over the execution functions of junior-to-mid engineers, but its capability distribution is **orthogonal** to the human's (§1.4.2.1, the jagged frontier). Chapter 2 unfolded the consequences of this jaggedness as a **repricing** of software-quality views (readability / abstraction / reuse / tests / docs — five items), and Chapter 1 §3 gave a **list of root defects that peripheral engineering has not touched** (the *residual problems* column for P1–P10). Together these three things define a simple boundary condition: **what is left for human engineers is not "the work remaining after subtraction," but the set of responsibilities that AI cannot reliably bear and that happen to be most directly tied to product value**. This chapter first lays out that set of responsibilities (§5.1), then does a step **more upstream than "translating job items into courses"** — analyzing what capabilities these responsibilities require, what must be added on top of a high-school graduate's math / logic / humanities baseline, and which of those capabilities **can only** grow as a by-product of doing the work by hand (§5.2). It then picks the most openly documented undergraduate CS curriculum (CMU SCS BS in Computer Science) for comparison (§5.3), diagnoses its specific gaps under the new division of labor item by item (§5.4), gives recommendations to remodel existing courses and add new content (§5.5), and finally discusses implementation pacing and risks (§5.6).

---

## 5.1 The Human Engineer Under AI Collaboration: A New Division of Labor

Combining the AI strength axes (A–E) of §1.4.2.2.1, the AI weakness axes (a–d) of §1.4.2.2.2, the migration of the five quality views in Chapter 2, and the "construction + decay" dual acceleration in §1.4.3, the residual responsibilities of the human engineer under AI collaboration can converge into five roles.

### 5.1.1 Constraint Designer

AI can stably approach or even surpass the human average inside narrow domains with verifiable-reward (RLVR) signals (§1.4.2.2.1 D); in **open domains without a verifier**, it does not constitute a reliable subject at all (§1.4.2.2.2 a, b, d). So the human's **first non-transferable responsibility** is: slicing an open problem into AI-attackable, verifiable sub-problems — **writing specs, writing acceptance tests, writing evaluators, writing reward shaping**. This is the direct corollary of François Chollet [[4]](https://x.com/fchollet/status/2024519439140737442)'s claim that "agentic coding is essentially machine learning": here, the human plays the role of *loss-function designer*, not *gradient computer*.

### 5.1.2 Verifier / Eval Designer

Chapter 2 §2.1.4 already flipped "testing" from a labor-quantity bottleneck into a **signal-quality bottleneck** — writing tests is essentially free for AI, but writing tests **with discriminating power** remains scarce. Combined with §3's audit of P6's evaluation crisis (LLM-as-Judge bias, benchmark contamination), the human's second responsibility is **adversarially constructing discriminating sets**: variation testing, property-based testing, production-trace replay, and red-team evaluation targeting "problems that only exist post-training."

### 5.1.3 Process / Reversibility Designer

§1.4.3 described the SDLC as "accelerated construction + accelerated decay." At this magnitude, **any irreversible early decision will be amplified by acceleration into an incident**, so §2.1.2 repriced the goal of abstraction from *DRY* to *locality of change* and *reversibility*. Mapped onto the human side, this is engineering at the **organization / process / version control / blast radius** level, not at the code level. It includes: environment isolation, canary releases, rollback playbooks, ADRs, externalized organizational memory of the `fix_plan.md` kind (§1.4.3.2), and the CI/CD and harness permission model (§3 C14).

### 5.1.4 Accountable Principal

§1.4.2.2.2 c places "trust and accountability" as the one axis that AI **structurally** cannot bear — this is not a capability problem, but a question of **subject capacity** in law / compliance / business commitment. The corresponding human responsibility spans:

- **Externally**: compliance review, commitments to customers / regulators, and being the accepting party for accountability when things go wrong;
- **Internally**: risk grading (which decisions can be left to the agent alone, which must keep a human in the loop), adversarial security review, and architectural-level mitigation of Prompt Injection (§3 P8).

### 5.1.5 Taste Steward

§1.4.2.2.2 d gives "taste and aesthetics" a score near the bottom — AI tends to restate the majority view in the training set; §2.1.1 also formally split "friendly to human decision-makers" and "friendly to agent modifiers" as two kinds of readability. The human's fifth responsibility is **long-term taste judgment**: which APIs are still worth using ten years from now, which abstractions are elegant, which product shapes are decent for humans. This is the most underrated item in education today — because it is **not machine-gradable**, does not enter benchmarks, and does not appear in interview leetcode.

### 5.1.6 Capability Matrix

Lining the five roles up against the arguments of §1.4.2 / §2.1 / §1.4.3 yields a **new-division-of-labor comparison table** — it will serve as the common yardstick for the capability stacks of §5.2 and the curriculum-gap diagnosis of §5.4.

| Human residual responsibility | Corresponding AI weakness / residual problem | Key capabilities | Key deliverables |
|---|---|---|---|
| Constraint Designer | P2 reasoning / a long-horizon planning / b grounding | Formal thinking, problem decomposition, spec writing, designing verifiable goals | PRD / Spec / Acceptance Test / Reward Spec |
| Verifier / Eval Designer | P6 evaluation / d taste / P1 hallucination | Adversarial testing, statistical discrimination, production monitoring, red-teaming | Eval Suite / Property Tests / Trace Replay |
| Process / Reversibility Designer | P5 uncontrollability / P7 token economics / P10 multi-agent coordination | Distributed systems, CI/CD, blast-radius estimation, organizational design | ADR / Harness config / Rollback playbook / Org chart |
| Accountable Principal | c trust and accountability / P8 safety | Compliance, security modeling, social-engineering awareness, acceptance of accountability | Compliance review, risk register, injection drill reports |
| Taste Steward | d taste / long-term readability | Breadth of reading, cross-paradigm comparison, user empathy, long-term aesthetics | Design-review opinions, API aesthetic guidelines, user research |

> Note: this table **does not** list "writing code" as a separate responsibility — because in the new division of labor it has degenerated into a **by-product of executing the five roles above**. But **this assertion only holds in the "industrial practice" context**; in "education practice" finer judgment is needed — certain hand-training is genuinely deletable in the AI era, certain parts can be **replaced** by new mathematical / statistical / formal methods, and certain parts remain incompressible cognitive scaffolding. This **three-quadrant** decomposition is argued separately in §5.2.3, because it will determine the shape of the recommendations in §5.5.

---

## 5.2 From the High-School-Graduate Baseline to the Job Items of §5.1: Capability-Demand Analysis

§5.1 lists five residual responsibilities, but **does not answer** the question of what a student must specifically learn to do these jobs. Mechanically translating job items into courses — §5.1.1 mentions spec design, so open a spec course; §5.1.2 mentions eval design, so open an eval course — skips the most important intermediate step: on top of the **math, logic, and humanities foundation a high-school graduate typically has**, what capabilities must **layer up** so that the five responsibilities of §5.1 can be carried out competently? And there is a structural paradox lurking inside the question — some capabilities seem to **only** be acquired by doing, by hand, the very work that AI is replacing (writing code, writing tests, hand-debugging) (§5.2.3). This section first sets the baseline (§5.2.1), then unfolds the capability stacks item by item (§5.2.2), and finally argues the hand-experience paradox separately.

### 5.2.1 The high-school-graduate baseline

Using a typical freshman entering a CMU-SCS-tier program as a reference, we can roughly expect:

- **Math**: calculus, intro linear algebra, elementary combinatorics and probability;
- **Logic**: intro formal logic (propositional, predicate), mostly attached to math proofs, not as an independent craft;
- **Statistics**: descriptive statistics + intro hypothesis testing (mostly mean/variance; heavy tails are barely taught);
- **Programming**: introductory level Python / Java, some OO, basic recursion, intro use of a debugger and git;
- **Writing**: argumentative essay, literature summary, source criticism (carried over from humanities classes);
- **Scientific method**: has seen the template of an experimental report, but the **falsifiability principle (Popper)** is usually not explicitly taught as an engineering discipline;
- **Cross-disciplinary**: about a year of natural science; law / economics / security / organizational behavior largely untouched.

This baseline is **not weak** — sufficient to underpin the **conceptual prerequisites** of any of the five responsibilities in §5.1; but most of the distance between it and "being able to do it competently" lies **not in concepts**, but in **judgment hammered out by repetition** and **minimum integration across disciplines**. The next subsection splits each of the five responsibilities into these two layers.

### 5.2.2 Capability stacks for the five responsibilities

Each responsibility is split into three layers: **lower-layer cognitive prerequisites** (baseline already has it / small top-up), **mid-layer disciplinary capabilities** (require dedicated courses with independent disciplinary training), and **upper-layer craft judgment** (the part that grows only through repeated experience, not teachable).

#### Constraint Designer (capability stack for §5.1.1)

- **Lower layer**: distinguishing "intent vs. literal" — a language-analysis capability from law / philosophy. Baseline humanities writing gives a shallow *audience awareness*; what is missing is **adversarial reading**: reading your own sentences while assuming the other party has motive to misinterpret. A unit of intro **legal / policy language** or imitative writing is needed.
- **Mid layer**: formal transcription — translating "I want X" into "for all inputs i, P(i, output) holds." Baseline math-proof experience supplies raw material, but **there is no repeated practice of "squeezing" a vague engineering need into formal expression**. This layer is a **bridge course** from math proof to engineering specification, an independent discipline.
- **Upper layer**: **anti-overfitting of specs** — anticipating that "a too-tight spec will let the AI find shortcuts around it," corresponding to reward hacking in ML [[11]](https://arxiv.org/abs/2209.13085) and Goodhart's law. The baseline student has **no exposure** to this thinking. This layer can only grow through repeatedly going through "write spec → watch agent exploit a loophole → fix spec"; it cannot be taught by lecture.

#### Verifier / Eval Designer (capability stack for §5.1.2)

- **Lower layer**: the **falsifiability principle (Popper)** — the mark of a good test is that "it can rule something out," not that "it confirms something." The baseline encounters this once in a philosophy class but has not internalized it as an engineering discipline.
- **Mid layer**: experimental design, statistical discrimination (power, effect size, awareness of p-hacking); adversarial thinking; and the area humans are bad at — **estimating rare events** (heavy tails, Black Swan, the felt sense of Anscombe's quartet). This layer needs an engineering-oriented statistical-discrimination course.
- **Upper layer (a direct answer to one typical question)**: "Without being able to hand-write code, how can one judge problems in AI-generated code?" — the answer is not single; its substance breaks into two halves: (a) **most judgments can be carried by the statistical / formal methods of §5.2.3 Quadrant II** (multi-sample agreement, property testing, metamorphic testing, differential testing, formal verification, empirical-complexity regression, counterfactual perturbation, trace-anomaly clustering, etc.), **regardless of whether one can hand-write code**; (b) but the judgment of "what counts as good" needed to draft these methods from scratch — specs, rubrics, invariants — still depends on first-hand experience, which is the content of §5.2.3 Quadrant III.

#### Process / Reversibility Designer (capability stack for §5.1.3)

- **Lower layer**: Bayesian-style risk thinking — decision under asymmetric consequences (heavy tail, low probability + high impact). Baseline probability / statistics covers only the first two moments and barely teaches heavy tails.
- **Mid layer**: FMEA, fault tree, the post-mortem paradigm from mature accident-science fields (aviation, nuclear [[12]](https://hbr.org/2011/04/how-to-avoid-catastrophe)); fault models of distributed systems; basics of organizational behavior (the practical implications of Conway's law).
- **Upper layer**: explicitly optimizing for "blast radius" as a design goal — requires having gone through **owning a real running system and watching it fail**. Reliable proxies on campus include **long-term open-source collaboration / on-campus infrastructure operations projects**, but they cannot fully substitute for production experience.

#### Accountable Principal (capability stack for §5.1.4)

- **Lower layer**: understanding the legal / social concept of "subject capacity" — why some things must be borne by an entity capable of accountability. Baseline humanities typically only grazes "rights," and **does not teach the contract structure of "bearing consequences."** This item is furthest from the baseline, because it is **not an extension of the CS discipline** but a minimum integration across disciplines.
- **Mid layer**: threat modeling (STRIDE, attack tree); social-engineering awareness (the Mitnick lineage); minimum set of compliance frameworks (GDPR, HIPAA, SOC 2 essentials); current AI-governance hotspots (EU AI Act, NIST AI RMF).
- **Upper layer**: risk grading under unprecedented AI contexts — this is **judgment, not knowledge**, and requires guided discussion of real cases inside the course to begin to form.

#### Taste Steward (capability stack for §5.1.5)

- **Lower layer**: broad reading — having seen enough good and bad to have a basis for comparison. Baseline humanities builds some (paradigms of literature and art history), but **has not transferred this to engineering objects**.
- **Mid layer**: cross-paradigm comparison — being able to place the solutions of the same problem in Lisp / Haskell / C / Python side by side and evaluate them. This requires having **dug into at least three paradigms**; students from a single-language background have the biggest gap here.
- **Upper layer**: writing aesthetic judgment **as a readable design-review opinion** — not merely "I feel this is better." This layer requires both writing training (baseline has the foundation) and **having written an API that you yourself find painful to use half a year later** (baseline has nothing). Again, this returns to §5.2.3.

### 5.2.3 Three quadrants: which hand-experience is no longer necessary, which can be replaced by new methods, which is still cognitive scaffolding

A typical question repeatedly raised in the AI-collaboration context is:

> "Without being able to hand-write code, how can one judge problems in AI-generated code?"

This question must be carefully unpacked, otherwise it slips into either of two wrong extremes: one is "if AI can do it, let AI do it," outsourcing all judgment; the other is "judgment is built by hand-writing," recreating 1980s training intensity for another four undergraduate years. Both are wrong. Using the Chapter 1 and Chapter 2 analysis of "what AI changed" as a sieve, the hand-training in traditional CS education splits into three quadrants.

#### Quadrant I — AI-peripheral engineering has stably taken over; hand-training is no longer cost-effective in education either

Work that falls inside the AI-strength jagged region (§1.4.2.2.1 A–E) yields zero marginal educational return from making students repeat it:

- Mechanical cross-language translation (C → Python, Java → Kotlin, etc.): §1.4.2.2.1 D, stably above human average;
- Labor-quantity generation like unit tests / docstrings: §2.1.4 / §2.1.5 repriced these from "precious" to "cheap by-products";
- Library-API conformance, import ordering, naming consistency: §1.4.2.2.1 B, near-free under multi-sample configuration;
- Template / glue / adapter code: §1.4.2.2.1 C, "explicit boilerplate beats abstraction in cost";
- Textbook-answer exercises (bubble sort, dictionary merging, binary search, and similar drills): RLVR-covered narrow domain [[49 ch.1]](https://arxiv.org/abs/2407.21787).

The educational call: **in high-intensity CS programs (CMU / MIT / Stanford) this kind of hand-training has long been compressed**. They no longer have students write bubble sort for the tenth time but jump straight to the core of data structures, systems, and theory. However, boot camps, weaker undergraduate programs, and "first build a Python foundation, then learn algorithms" intro programs still retain large amounts of Quadrant I training, and this portion should be further compressed.

#### Quadrant II — Humans remain ultimately accountable, but **new math / statistics / formal methods** can substitute for line-by-line code reading at scale

This is the **core quadrant** of the answer. Judging problems in AI-generated code **does not require** "understanding every line"; in many cases reading the code is the least efficient path. The methods below have been industrially usable since the late 2020s — and **most of the methods are not new** (prototypes existed in the late 1990s), but once AI pushed code volume up 10×, these methods **were upgraded from "optional" to "essential craft" for the first time**:

| Method | What it detects | Mathematical / formal basis | What kind of "code reading" it replaces |
|---|---|---|---|
| **Multi-sample agreement** | Hallucination, spec ambiguity, reward hacking | Information entropy + majority vote; Brown et al. *Large Language Monkeys* [[49 ch.1]](https://arxiv.org/abs/2407.21787) provides the hard evidence that coverage in RLVR domains scales log-linearly with sample count | Line-by-line speculation as to "is this segment hallucinating?" |
| **Property-based testing** | Boundary conditions, invariant violations | Random sampling + minimal counterexample shrinking; QuickCheck (Claessen & Hughes 2000) [[15]](https://www.cs.tufts.edu/~nr/cs257/archive/john-hughes/quick.pdf) → Hypothesis / Proptest | Manually enumerating corner cases |
| **Metamorphic testing** | Open-domain outputs without a "correct answer" oracle | Algebraic closure of input-perturbation ↔ output relations; Chen et al. 1998 [[16]](https://www.cse.cuhk.edu.hk/~smyiu/publications/HKUST-CS98-01.pdf); 2025 survey [[17]](https://dl.acm.org/doi/10.1145/3631971) | The prerequisite of knowing "what the correct output should look like" |
| **Differential testing** | Inconsistency between implementations / between current and prior versions | Trace equivalence over state spaces; McKeeman 1998 [[18]](https://www.cs.swarthmore.edu/~bylvisa1/cs91/f15/Papers/Differential-Testing-McKeeman.pdf) | Eyeballing two implementations side by side |
| **Formal verification (Lean / TLA+ / F\*)** | Functional correctness of critical paths | Type theory / model checking; Lean 4 + Mathlib significantly lowered the industrial-usability bar in 2024–2025 [[19]](https://leanprover-community.github.io/); DeepMind AlphaProof uses Lean formalization as an RL reward signal [[20]](https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/) | The intuitive "I trust this code" |
| **Empirical-complexity regression** | Whether "O(n log n)"-style performance claims live up to their name | Fitting power-law / poly / exp to a runtime-vs-input-size curve | Hand-computing O(·) + finding counterexamples by intuition |
| **Counterfactual perturbation** | Reward hacking / spec gaming | Lipschitz continuity + distribution-drift detection; same family as the unfaithful-CoT detection in Chapter 1 P2 [[25 ch.1]](https://arxiv.org/abs/2503.08679) | "Sniffing" by experience that the agent is gaming the spec |
| **Trace replay + anomaly clustering** | Latent regressions under production | Z-score / Mahalanobis distance / OOD detection | Periodic manual code review |
| **Calibrated LLM-as-judge** | Large-scale subjective judgment (writing, API design, doc quality) | Alignment with human gold standard + bias compensation; §3 P6 [[21 ch.1]](https://openreview.net/forum?id=3GTtZFiajM) reminds that the judge's own position / verbosity / stance biases must be continuously calibrated | Human reviewers are overwhelmed at the scale of generated output |

> A key observation: **none of these methods requires the student to be able to write the code under verification — they require the student to be able to "design the verification mechanism"**. Designing the verification mechanism requires: the falsifiability principle, formal semantics, statistical discrimination, intuition for invariants, perturbation design — all of these are capabilities of **math / statistics / formal methods**, orthogonal to "hand-writing every line of code." The substantive answer to the typical question is therefore: **most judgment is borne by the Quadrant II methodology, independent of whether the student can hand-write code** — provided the student has learned this set of methods.

Carrying this back into curricula: **Quadrant II methods should be added to the CS undergraduate program as a standalone content category**. It belongs neither to traditional theory (15-251), nor to traditional systems (15-213), nor to traditional software engineering (17-313); it cuts across them and systematizes "how to build trust without eyeballing" as its own craft. This is currently an **almost-empty** slot at CMU / MIT / Stanford, and it is the core content source for the §5.5.2-(2) Eval & Trust required course.

#### Quadrant III — Still generated as a by-product of doing things by hand, with no methodological substitute

The methods in Quadrant II are all powerful, but **they share one prerequisite**: someone has defined "what counts as good." Multi-sample agreement requires a metric space; property testing requires a set of invariants; metamorphic testing requires a set of equivalence relations; formal verification requires a property statement. **None of these starting points can be generated by Quadrant II methods themselves** — they require a person with a case base to draft them. Quadrant III is that part.

Concretely, the content of Quadrant III in CS education is **narrow and hard**:

1. **The judgment needed to draft specs / rubrics / invariants from scratch**. To draft a seemingly obvious invariant like "after checkout, the cart total must equal the sum of product prices," one must know **the ways it can be broken** — floating-point accumulation, concurrent modification, refund boundaries, tax rounding, coupon stacking. This case base can only be acquired by first-hand experience (having written code, been bitten by your own bugs, post-mortemed them). Schank calls this case-based reasoning [[14]](https://www.cambridge.org/core/books/dynamic-memory/E0E0FE3D38E5EBABD15F73CB9F36D7A6); Bjork's *desirable difficulties* [[13]](https://bjorklab.psych.ucla.edu/wp-content/uploads/sites/13/2016/04/Bjork_1994.pdf) provides empirical support for the claim that "friction is a cognitive prerequisite for later fluent use of tools."
2. **Identifying entirely new failure modes**. Statistical methods only detect **known patterns**; novel patterns can only be caught by an analyst doing analogy reasoning, which requires sufficiently broad first-hand experience in adjacent domains to transfer judgment when there is no precedent.
3. **Long-term aesthetic decisions**. "Will this API still be willingly used by people ten years from now" — there is currently no statistical proxy; the only reliable input is the felt sense of engineers who have seen, used, and designed many APIs (§1.4.2.2.2 d / §5.1.5).
4. **Subject capacity for trust and accountability**. This is not a capability question to begin with (§5.1.4) and is outside the scope of quadrant analysis, but belongs to "no methodological substitute."

Several capabilities easily misclassified into Quadrant III, but actually partially or largely replaceable by Quadrant II, should be explicitly removed from Quadrant III:

- The **known-pattern** portion of "internal bug-mental-model" is covered by multi-sample + property testing + differential testing; only the **novel-pattern** portion enters Quadrant III (item 2).
- "Breathing rhythm of debugging" is **partially replaced** under AI collaboration: once the trace + property-failure + auto-bisect toolchain is assembled, the human reviewer sees a verifier-produced minimal failing case, not a raw stack trace from scratch. The remainder returns to Quadrant III item 1 — someone has to design the verifier well.
- "Muscle memory of complexity" is almost entirely replaceable by empirical-complexity regression + property tests — unless the student plans to do algorithm research itself.

#### Educational synthesis

The three quadrants directly determine the shape of the recommendations in §5.5:

- **The amount of Quadrant I training in programs like CMU is already compressed near zero** — no further drastic trimming is needed. But the curriculum **must explicitly recognize** that today's intro exercises (implementing linked lists, writing a small interpreter, extending the Pebbles kernel) are no longer Quadrant I "teach syntax / teach API" but Quadrant III case-base training — they should **explicitly acknowledge this purpose** and grade accordingly, instead of using "algorithm fundamentals" or "systems fundamentals" as the pretext.
- **The Quadrant II methodology must be added as a new content category** — this is currently the **largest content vacuum** in CS undergraduate programs, and the substantive answer to the typical question "without being able to hand-write code, how to judge AI code." Concrete landing points: §5.5.2-(2) Eval & Trust required course as the main battleground; in §5.5.1, inject property-based + metamorphic modules into 15-251, power analysis + heavy tails into 21-325, and split out a half-course of Spec & Eval from 17-313.
- **Quadrant III training intensity is preserved on a "narrow but deep" principle** — the purpose of keeping it is not to produce a junior programmer who can ship, but to construct the minimum case base needed for later AI collaboration. The grading yardstick for these assignments changes accordingly: it is not whether the student can independently complete a 1000-line project, but whether the student can articulate "if I asked AI to write this, where is it most likely to err, and which Quadrant II method would I use to verify."

> ⚠ **Disclaimer**: the specific carving of the three quadrants (especially which traditional capabilities fall into the Quadrant II replaceable region) still lacks direct controlled-experiment evidence. The judgments here are inferred from the references already cited in Chapters 1–2 plus the industrial maturity of the various Quadrant II methods, **not** from independent measurement on each item. It is explicitly marked as design proposal rather than empirical conclusion.

---

## 5.3 Educational Comparison: CMU SCS BS in Computer Science

### 5.3.1 Why pick CMU

Picking a comparison target requires three conditions: (1) the curriculum structure and the syllabus of every course are **fully public**; (2) it is **widely recognized** in the industry, with a broad graduate distribution so it cannot be dismissed as a "niche sample"; (3) the curriculum is **explicitly structured** (rather than just listing 60 electives for students to self-pick), so gaps can be mapped item by item. The best candidate satisfying all three is **Carnegie Mellon University · School of Computer Science · Bachelor of Science in Computer Science** — its undergraduate core, restricted electives, and general-education structure are published per-semester on the SCS website, the key course numbers (15-122, 15-150, 15-213, 15-251, 15-451, etc.) have been highly stable over the past 20 years, and the whole system is often called a "template of computer science" in industry [[1]](https://csd.cs.cmu.edu/academics/undergraduate/requirements). MIT EECS Course 6-3 [[2]](https://catalog.mit.edu/degree-charts/computer-science-engineering-course-6-3/) and Stanford CS BS [[3]](https://csmajor.stanford.edu/) are close candidates, and this chapter cites them as parallels in the gap analysis.

### 5.3.2 Outline of CMU BS in CS curriculum structure (2025–2026 public version)

Per the SCS official *Undergraduate Catalog* [[1]](https://csd.cs.cmu.edu/academics/undergraduate/requirements), undergraduate requirements break roughly into 7 blocks:

| Block | Representative courses | Approx. credit share |
|---|---|---|
| Math foundation | 21-120/122 Calculus, 21-241/242 Linear Algebra, 15-151/21-127 Discrete Math, 21-325 Probability | ~15% |
| CS core | 15-122 Principles of Imperative Computation, 15-150 Principles of Functional Programming, 15-210 Parallel & Sequential Data Structures and Algorithms, 15-213 Introduction to Computer Systems (CSAPP), 15-251 Great Ideas in Theoretical CS | ~25% |
| Algorithms / theory electives | 15-451 Algorithm Design and Analysis, or 15-455 Complexity, or 15-453 Formal Languages | ~5% |
| Software-systems electives | 15-410 OS, 15-411 Compilers, 15-440 Distributed Systems, 15-441 Computer Networks, 15-445 Database Systems | ~10% |
| AI / ML and application electives | 10-301/10-601 Introduction to ML, 15-381 AI, 11-411 NLP, 15-462 Graphics, etc. (pick one) | ~5% |
| Software engineering / application | 17-313 Foundations of Software Engineering (the **only explicit SE required**), or 67-272 Application Design, etc. | ~5% |
| General education / writing | 76-101 Interpretation and Argument; humanities / social science / arts (several) | ~15% |
| Free electives + double major / minor | — | the remainder |

Four observations:

1. **Theory and systems dominate the spine**: the five courses 15-122/150/210/213/251 form the SCS student's "identity courses," covering imperative, functional, algorithms, OS perspective, and theory of computation. This spine is the optimal solution for the industrial reality of the 1980s–2010s — where the execution subject was a human.
2. **Software engineering exists as a single 12-unit course** (17-313), not as a craft training threading through all four years.
3. **AI / ML is an "application-track elective"**, not a foundation course. Most CS students touch only one 10-301-level intro.
4. **Writing and argumentation has only one required course (76-101)**, and it is aimed at general writing, not technical-spec writing.

> Important premise: CMU is not weak. It sits in the world's first tier in every traditional block (theory, systems, ML). **All gap judgments below are inside the specific coordinate system of "with respect to the new division of labor under AI collaboration"** — they do not constitute an evaluation of CMU's overall level, nor do they imply CMU is worse than MIT/Stanford; the three institutions exhibit **the same symptoms** on almost all gaps discussed.

---

## 5.4 Curriculum Gap Diagnosis

Combining the five roles of §5.1.6 with the capability stack of §5.2 and mapping them onto the curriculum structure of §5.3.2 produces 8 specific gaps. For each one, a three-part diagnosis is given: "what currently exists, what is missing, why it is a problem for the new division of labor."

### 5.4.1 Gap 1: No instructional vehicle for spec engineering

- **What exists**: 15-150 (light intro to Hoare logic), 15-251 (logic, formal languages), 15-122 (intro contract programming with pre/post conditions), and one or two weeks of requirements engineering in 17-313;
- **What is missing**: an entire course making **natural-language requirement → executable spec → acceptance test → reward signal** the through-line; plus the *reward hacking / anti-overfitting* thinking listed at the upper layer of the Constraint Designer in §5.2.2 — the current formal-methods training (Coq / TLA+) is confined to graduate electives (15-414 / 15-819), and undergraduates have essentially never written "a spec fed to an agent," let alone gone through the loop of being exploited by an agent.
- **Why it is a problem**: §5.1.1 sets this as the human's first non-transferable responsibility; §5.2.2 defines its upper-layer judgment as "only grown through repeated experience" — but CMU offers no course providing such repeated experience.

### 5.4.2 Gap 2: Insufficient engineering training in evaluation and trustworthy signals

- **What exists**: 15-451 / 15-411 etc. issue auto-graded assignments; 10-301 teaches cross-validation; 17-313 has a single lecture on unit testing.
- **What is missing**: (a) a **full engineering course** taking all syndromes of §3 P6 (benchmark contamination [[31 ch.1]], LLM-as-Judge bias [[21 ch.1]], production trace replay, adversarial-set construction, property-based, mutation testing) as its core; (b) the **experimental design + heavy-tail / rare-event estimation** at the mid layer of §5.2.2 — CMU's probability course covers means and variances but does not engineering-train Anscombe's quartet, p-hacking, or power analysis. CMU's closest current courses to this line are 17-355 Program Analysis and 18-636 Software Engineering for AI — the former is theoretical, the latter at the graduate level.
- **Why it is a problem**: §2.1.4 already lists "discriminating power" as the new moat; §5.1.2 lists it as the second residual responsibility; §5.2.3 further gives the substantive composition — **most** of the discriminating power can be borne by the Quadrant II methodology (property-based / metamorphic / differential / multi-sample agreement / formal verification, etc.); **a smaller part** (the judgment to draft rubrics) belongs to Quadrant III, requiring first-hand experience with the pain of weak tests to form a case base. The combined training that both halves demand is offered by no course today.

### 5.4.3 Gap 3: Reversibility and blast radius — no vehicle for abstraction repricing

- **What exists**: the design-pattern chapter of 17-313, 15-214 Principles of Software Construction (OO abstraction, dependency management).
- **What is missing**: a design course whose objective function is **reversibility / blast radius**; the FMEA / post-mortem / heavy-tail decision thinking listed at the mid layer of §5.2.2. The current design-pattern training still teaches the GoF "reduce duplication" goal; the engineering actions of ADR / risk register / canary releases have no dedicated course vehicle; students are essentially at zero distance from "actually operating a system in production."
- **Why it is a problem**: §2.1.2 already repriced the goal of abstraction to *locality of change* and *reversibility*; §1.4.3 sets "dual acceleration of construction + decay" as the underlying constraint; §5.2.2 anchors the upper-layer judgment to "having had real operational experience" — together they demand a composite training that current CS curricula do not provide.

### 5.4.4 Gap 4: A harness is not a new OS, but it does require a composite engineering discipline cutting across multiple courses

- **What exists**: 15-410 OS, 15-440 Distributed Systems, 10-417 Deep Learning Systems, 18-330 Computer Security.
- **Clarification first (clearing a misjudgment out of the way)**: framing the harness as a "new-class operating system" is an over-extrapolation — it stands in a **same-position-different-layer** relationship with the OS (the harness runs **on top of** the OS), and merely renaming "scheduling object / permission model / observability" does not constitute a new abstraction level. Item by item:
  - **Scheduling object**: the OS schedules processes with deterministic execution semantics; the harness schedules non-deterministic sub-tasks whose "next step is decided by LLM output." This means a harness scheduling decision **depends on semantic interpretation of the scheduled object's output**, which pushes it down from the kernel layer to the *shell / workflow engine* layer. Airflow / Temporal / make / shell have long been "deciding the next step from the previous step's output"; the new thing in the harness is that the output is natural language and needs a judge / verifier to interpret, but at the level of **scheduling theory** there is nothing new.
  - **Permission model**: POSIX DAC + namespaces + cgroups + `capabilities(7)` are in **expressive power** fully able to describe tool / file / network capability — whether elegantly is another matter, and the container ecosystem does this daily. The real new constraint of harnesses is not in expressive power but in **the subject itself being compromisable by the data it handles** (§3 P8, prompt injection is architectural, the LLM does not internally distinguish code from data). The traditional capability-system *confused deputy* assumption is "the subject is occasionally fooled"; the LLM-subject context is "handling any external content is equivalent to delegating capability to that content." This pushes the center of permission design from the *capability / DAC* lineage toward the **mandatory information flow control / DIFC** lineage — Bell-LaPadula, SELinux, Asbestos, HiStar — which label data flows **even inside a single subject**; this is graduate-level OS security content, and undergraduate 18-330 does not teach it.
  - **Observability**: syscall trace and prompt-response trace share toolchain ancestry (eBPF / journald / OpenTelemetry have long supported unstructured logs); the real difference is the **status** of the trace — syscall traces are post-hoc debug artifacts; under the new division of labor, prompt-response traces are **first-class deliverables** (§2.1.5), which determines the design requirements of schema, retention, replay, and diff. This is a difference at the engineering-action layer, not at the underlying-abstraction layer.
  - **Non-determinism**: a process given the same input executes deterministically; an LLM call under the same input still drifts due to batching, temperature, and concurrent load (§3 P5 [[29, 30 ch.1]]). This makes *replay* and *deterministic scheduling* strictly invalid — the harness must, like a Monte Carlo simulation platform, do "random seed + multiple re-runs + statistical discrimination," which is the domain of simulation and experimental science, unrelated to OS scheduling.
  - **Resource accounting**: the OS accounts CPU / RAM quotas; the harness additionally jointly optimizes token cost, verifier quality score, and latency budget — a composition of cloud-cost management and quality control, not a new kernel concept.
- **The real gap**: the four items above cut across *workflow / shell orchestration semantics* (15-440 does not cover the semantic layer), *MAC / DIFC security* (18-330 does not cover), *experiment engineering under non-determinism* (10-417 does not cover), and *joint resource accounting of token × quality × latency* (no course covers). No course glues these four pieces together for undergraduates; graduate electives are also scattered. The engineering practice of Chapter 1 §3 C14–C18 (Harness / Sandbox / Tracing / Cache & Routing) is currently **completely absent from undergraduate curricula** — but what is missing is not "a new OS course," but the composite engineering discipline that binds the four pieces above.
- **Why it is a problem**: §5.1.3 lists process and reversibility design as the third residual responsibility; its engineering vehicle is precisely this kind of discipline cutting across multiple course boundaries — and precisely because it cuts across boundaries, no existing required course naturally carries it.

### 5.4.5 Gap 5: Insufficient training intensity for reading-intensive work

- **What exists**: 15-213 has students read the CSAPP textbook and implement shell / proxy / malloc; 15-410 has students extend a kernel.
- **What is missing**: **long-term, large-scale codebase navigation / maintenance / refactoring courses**. Current assignments' "reading" is for "writing your own implementation," not for "making a careful modification inside a million-line, live codebase." §2.1.1 cites Xia et al. (TSE 2018) [[13 ch.2]] for the hard number "58% of time spent on reading and understanding"; §5.2.3 classifies "breathing rhythm of debugging" as **partially replaceable by the trace + property-failure + auto-bisect toolchain**, but the **design** of the verifier remains Quadrant III — taken together, current assignments train neither the patient reading of a large codebase, nor the engineering use of the trace toolchain, nor accumulate enough hours of verifier-design experience; all three layers are thin.
- **Why it is a problem**: the reviewer is the real throughput bottleneck under AI collaboration — AI writes code far faster than humans review it (§1.4.2.2.1 A); if humans are not trained into faster readers, the entire loop collapses at this link.

### 5.4.6 Gap 6: Training opportunities for taste and aesthetics are compressed

- **What exists**: 15-150's elegance training, 15-410's code-taste demands ("good kernel hackers"), and 15-462 Graphics' implicit visual-aesthetic influence.
- **What is missing**: courses with explicit goals of "long-term aesthetics / cross-paradigm comparison / user empathy." CS spine courses default to aesthetic training as a **by-product**; §5.2.3 explicitly lists "long-term aesthetic decisions" as one of the core items of Quadrant III — it has no effective statistical proxy and still relies on the felt sense engineers accumulate first-hand (the felt cost of abstraction, the felt sense of interface design). Once AI makes "producing seemingly reasonable code" nearly free, by-product-style aesthetic training will rob students of their **reinforcement opportunities**, because they will no longer write a parser from scratch and so no longer feel, through stumbles, "why this AST should look this way."
- **Why it is a problem**: §5.1.5 lists aesthetics as the fifth residual responsibility. AI's trade-offs gravitate to the training-set majority view (§1.4.2.2.2 d); if humans lose independent aesthetic capability, they will end up dragged back to average by their own tools.

### 5.4.7 Gap 7: Insufficient hands-on training for trust, accountability, compliance, and safety

- **What exists**: 18-330 Computer Security, 15-330 Introduction to Computer Security (offered every few years), 17-200 Ethics and Policy Issues in Computing.
- **What is missing**: courses that treat **Prompt Injection (§3 P8), the accountability chain when AI errs, compliance-review actions, and red-team drills** as a set of **hands-on exercises** rather than debate; plus the minimum cross-disciplinary integration listed at the mid layer of §5.2.2 (legal contract structures, modern AI-governance frameworks). 17-200 leans toward ethical debate, 18-330 toward cryptography and system vulnerabilities — neither covers the real AI-era security / trust craft, and the cross-disciplinary piece is entirely vacant.
- **Why it is a problem**: §5.1.4 lists bearing accountability as the non-transferable fourth responsibility; if graduates cannot run a prompt-injection red team, cannot write a risk register, and do not know when a human-in-the-loop is mandatory, they cannot take over an AI system **as the principal**.

### 5.4.8 Gap 8: Assessment and academic integrity themselves need to be redesigned

- **What exists**: courses like 15-122 / 150 / 213 prohibit AI assistance on assignments, or require "declare use scope"; course grading depends on the "independently completed" assumption for the final exam and programming assignments.
- **What is missing**: an assessment mechanism that takes "using AI is the default state" as its premise — grading is over **process traces, decision records, verifier design, and critical acceptance of agent output**; but at the same time, the external constraint of §5.2.3 requires that **hand-training intensity not be weakened** — so the new assessment mechanism must **simultaneously** do two seemingly contradictory things: (i) make students face AI directly in many assignments, and (ii) force AI-free completion in some assignments explicitly marked "bare-hands."
- **Why it is a problem**: this gap **mechanically jams** the fix for all 7 prior gaps — as long as assessment is still organized as a single "independent + written-exam" mode, the instructor can neither demand that students design spec / eval / harness in some assignments (those actions are meaningless without AI participation), nor ensure students complete the necessary hand-training in others.

---

## 5.5 Recommendations

Given in the order "first remodel existing courses, then add new ones, finally redesign teaching methods." **All recommendations are bound by the two external constraints of §5.2.3**: (i) the hand-training intensity of identity courses may only be increased, not weakened; (ii) the grading yardstick shifts from "ship independently" to "can the student articulate where AI will err."

### 5.5.1 Remodel existing courses

The third column tags the **primary quadrant location** (§5.2.3) — that column determines the grading yardstick for the course.

| Course | Current form | Suggested modification | Quadrant location |
|---|---|---|---|
| 15-122 Imperative Computation | Teaches intro contract programming | Promote "contract" to "intro spec engineering" — for each assignment, first write a **machine-checkable spec** (pre/post, invariants, property tests), and submit a reflection on "what ambiguities would an agent encounter if asked to implement this spec" | **III main** (deliberate practice of building specs / case base); implementation is done independently |
| 15-150 Functional Programming | Teaches SML and equational reasoning | Introduce *spec → multiple candidate implementations → auto-verifier select-best* exercises (corresponds to §1.4.3.3 point 2); explicitly add **QuickCheck / Hypothesis-style property-based testing** instruction (addresses §5.2.3 Quadrant II) | **III + II**: equational reasoning is III, property tests are II |
| 15-213 CSAPP | Has students independently implement shell / proxy / malloc | Preserve all bare-hands labs **with no cuts** — they are the strongest training ground for §5.2.3 Quadrant III item 1 "case base for writing spec / invariant"; add an "implement a constrained bugfix inside a real 1k+-file open-source project" assignment, training §5.4.5 reading-intensive work | **III main**; the open-source assignment enters II (trace toolchain) |
| 15-251 Great Ideas | Teaches logic, computability, complexity | Add an **evaluation-theory module**: discriminating power, adversarial sets, benchmark contamination, judge bias (§3 P6); introduce formal discussion of reward hacking / Goodhart; add **the theoretical basis of metamorphic relations and differential testing** | **II strengthened**: the mathematical basis of Quadrant II methods is anchored in this course |
| 15-410 OS | Has students extend the Pebbles kernel | Preserve the kernel assignment with no cuts (another strongest training ground for §5.2.3 Quadrant III); add a one-week *harness-is-not-OS* clarification unit (§5.4.4), introducing industrial use of namespaces / cgroups / `capabilities(7)` | **III main**; the harness unit is II |
| 18-330 Computer Security | Leans toward cryptography and system vulnerabilities | Introduce a *mandatory information flow control / DIFC* module (Bell-LaPadula, SELinux, Asbestos, HiStar), with LLM-subject prompt injection as its core driving scenario; add hands-on **taint analysis / symbolic execution** | **II** (information-flow methodology) + the security case base of Quadrant III |
| 17-313 Foundations of SE | One semester covering requirements / design / testing / maintenance | Split into two courses: **17-313A Spec & Eval Engineering**, **17-313B Process, Reversibility & Harness** (see §5.5.2) | Primarily **II** (methodology) + meta-cognition of the three-quadrant taxonomy |
| 76-101 Interpretation and Argument | General writing | Add *technical specification writing*, *change proposal / ADR writing*, and a one-week **intro legal / policy language** imitation unit (addresses the Constraint Designer lower layer in §5.2.2) | **III**: spec-writing judgment |
| 21-325 Probability | Descriptive + intro hypothesis testing | Add power analysis, effect size, heavy-tailed distributions, p-hacking case studies, Bayesian model evidence; these are the mathematical bedrock of almost all Quadrant II methods | **II foundation** |

### 5.5.2 New required and elective courses

The following 4 courses are recommended to be added in SCS:

1. **15-3xx Specification and Verification Engineering** (required, half of the old 17-313 + new content).
   Topics: natural-language requirement → formal / semi-formal spec → property-based test → verification matrix; reward shaping; anti-overfitting of specs (Goodhart / reward hacking). **Key design**: each assignment requires the student to first write a spec, then have the agent attempt to implement it, **deliberately rewarding "agent exploits a loophole"** — after being exploited, the student rewrites the spec until the agent has no hole to exploit. This is the industrialized form of the §5.2.2 upper-layer "repeated experience." Prerequisites: 15-150, 15-251.
2. **15-3xx Eval & Trust Engineering for AI-augmented Systems** (required).
   **This course is the main battleground of the §5.2.3 Quadrant II methodology** — its content is built around systematically teaching the nine Quadrant II methods:
   - Multi-sample agreement (Brown et al. *Large Language Monkeys* [[49 ch.1]](https://arxiv.org/abs/2407.21787)'s coverage scaling curve as the underlying mathematics);
   - Property testing (hands-on QuickCheck / Hypothesis) [[15]](https://www.cs.tufts.edu/~nr/cs257/archive/john-hughes/quick.pdf);
   - Metamorphic testing (writing metamorphic relations) [[16]](https://www.cse.cuhk.edu.hk/~smyiu/publications/HKUST-CS98-01.pdf);
   - Differential testing (multi-implementation + prior-version baseline) [[18]](https://www.cs.swarthmore.edu/~bylvisa1/cs91/f15/Papers/Differential-Testing-McKeeman.pdf);
   - Intro formal verification (hands-on Lean / TLA+) [[19]](https://leanprover-community.github.io/);
   - Empirical-complexity regression + counterfactual perturbation + trace-anomaly clustering;
   - Calibrated LLM-as-judge (with bias compensation, §3 P6 [[21 ch.1]](https://openreview.net/forum?id=3GTtZFiajM)).
   Accompanying lab: design and deliver a **third-party-reproducible eval suite** for an open-source AI system, using at least four of the methods above in combination. **Key design**: the assessment requires the student to **first write a meaningful bug by hand** (covering §5.2.3 Quadrant III item 1), then design an eval that catches it — this explicitly incorporates "the pain of weak tests" into training. It directly answers the typical question "without being able to hand-write code, how to judge AI code" — the course's answer is **learn these nine methods and use their combinations to replace line-by-line eyeball reading**.
3. **15-4xx Agent Harness Engineering** (elective).
   This course is **not** "15-410 replicated on a new subject" — §5.4.4 has argued that the harness is not at the same abstraction level as the OS. The role of this course is to **glue together** 15-440 (orchestration semantics), 18-330 (DIFC / information-flow security), 10-417 (experiment engineering under non-determinism), plus the no-course-covered joint resource accounting of token × quality × latency. Semester project: build a **minimum viable harness** from scratch and use it to run a real bugfix workflow end-to-end, delivering *spec + eval + trace + cost / quality curve*.
4. **17-2xx Reading Engineering** (required, a one-semester 6–9-unit practicum).
   Topics: doing constrained navigation / refactoring / bugfixing inside multiple million-line open-source projects; explicitly training chunking, dependency tracking, and critical reading of AI-generated code. **Key design**: the deliverable is required to be **a PR acceptable by the project's maintainers + reading notes** — the mirror image of code-writing assignments; the student must be able to articulate "why this AI-suggested modification is wrong" (a direct training target for §5.2.3-(1)/(5)).

Two additional courses are recommended to be promoted to **strongly encouraged** (not required, but default-selected):

- **17-3xx AI Safety, Security & Accountability**: covers §5.4.7 and the Accountable Principal cross-disciplinary minimum integration of §5.2.2 (legal contract structures, AI-governance frameworks, hands-on red-teaming).
- **15-3xx Design Taste Workshop**: built around case studies + design reviews + cross-paradigm work comparison, with no leetcode-style grading; students must submit "a retrospective of their own assignments from six months / one year ago" for aesthetic comparison (a direct training form for §5.2.3-(2)/(3)); covers §5.4.6.

### 5.5.3 Teaching method: a three-quadrant assignment system + process audit

§5.4.8 already pointed out that the new division of labor demands that the assessment mechanism simultaneously do several seemingly contradictory things. The five mechanisms below are organized along the three quadrants of §5.2.3:

1. **Assignments are split by quadrant**:
   - **Quadrant I assignments** ("AI takes the wheel"): labor-quantity tasks (boilerplate generation, cross-language translation, doc writing), done by the AI; the student only evaluates "is the AI correct" and does so quickly; the share of such assignments should be **actively compressed**, because repeated practice has no marginal return.
   - **Quadrant II assignments** ("AI-default + methodology verification"): the student designs and applies a set of Quadrant II methods (property testing / metamorphic / differential / multi-sample agreement / formal verification / empirical-complexity regression / counterfactual perturbation, etc.) to **verify the AI's output**; the main deliverable is the **verification report + session trace**, not the code itself. This is the new mainstream form of assignment.
   - **Quadrant III assignments** ("bare hands"): the student works alone, **AI assistance prohibited**, in order to build the Quadrant III case base (the judgment to draft specs, identification of novel failure modes, long-term aesthetics). The share of these assignments **declines with year but does not go to zero** — about 50% in freshman year, and roughly 15% retained in senior year. Use **closed-network exam halls / remote proctoring** to enforce the discipline.
2. **Quadrant II assignments are graded on the design and execution of the methodology**. The grade is determined by four things: (a) which Quadrant II methods were chosen and why; (b) whether the invariants / metamorphic relations / properties were written with discriminating power; (c) whether the decision points record trade-offs (including trade-offs like "why property testing was used instead of formal verification"); (d) whether the points of rejecting AI output can articulate "method X caught issue Y here." Whether the code itself "works" is given automatically by the verifier and does not enter subjective grading.
3. **Quadrant III assignments are graded on the quality of case-base construction** — not on quantity of output. Including: can the student locate an atypical bug without AI hints and post-mortem out a transferable lesson; can the student hand-write a spec **with discriminating power** (one that an AI implementation deliberately seeking loopholes cannot break); can the student produce a design **they would still be willing to maintain six months later**. "Ship a 1000-line project independently" is **not** a grading dimension.
4. **An end-of-term "contrastive exam" as the latch**. The final exam splits into two parts: a closed-book part inspecting Quadrant III fundamentals (computing complexity by hand, constructing counterexamples, reading unseen code); an open-book AI-allowed part presenting an unseen engineering problem, where the student is graded on **how to use Quadrant II methods** to deliver a verification report. Passing requires passing both parts. This "binds" the three quadrants of §5.2.3 to a single grading anchor.
5. **Aesthetics is carried by case-based reviews**. Establish a "design-review day" — the student submits an API, a spec, a refactoring proposal, accompanied by **a comparison with their own similar work from six months / one year ago**, and orally defends it before a panel of instructors and senior students. This is the joint training for §5.4.6 and §5.2.3 Quadrant III item 3.

> ⚠ **Disclaimer**: there is no large-sample controlled study of the five mechanisms above in mainstream CS undergraduate education. The judgments here are inferred by analogy from adjacent domains in software-engineering methodology (agentic coding practice, portfolio grading experience in CS writing-style courses, traditional bare-hand exam mechanisms), and still require empirical evaluation in real classrooms.

---

## 5.6 Implementation Risks and Pacing

Not all gaps are equally urgent, and not all changes are equally cheap. The table below gives a two-dimensional ranking of **urgency × implementation cost**, with the suggested pacing.

| Modification / new item | Urgency | Implementation cost | Suggested pacing |
|---|---|---|---|
| 5.5.3 teaching-method transition (three-quadrant assignment system + process audit) | **Very high** | **Low** (mainly rule and convention adjustment) | Launch in **semester 1** |
| 5.5.1 local modifications to existing courses (15-122 / 76-101 / 17-313 / 21-325 content tweaks) | High | Low–medium | Complete in semesters 1–2 |
| 5.5.2-(1) Spec & Verification required course | High | Medium (new textbook needed, plus a supporting verifier lab environment) | Semesters 2–4 |
| 5.5.2-(2) Eval & Trust required course | High | Medium | Semesters 2–4 |
| 5.5.2-(4) Reading Engineering required course | High | Medium–high (requires long-term open-source collaboration relationships) | Semesters 3–6 |
| 5.5.2-(3) Harness Engineering elective | Medium | Medium (depends on outside industrial-practice maturation) | Semesters 4–8, can start as a series of seminars |
| 17-2xx Safety / Accountability strongly recommended | Medium | Low (mostly case studies + red-team workshops) | Semesters 2–4 |
| Design Taste Workshop | Medium | Low | Semesters 2–4 |

Four main implementation risks:

1. **Uneven instructor familiarity with the tools**. "Process-audit"-style grading has demands on the instructor's own AI-engineering fluency; recommendation: introduce the conceptual framework of §1.4.2 / §2.1 / §5.2 into TA training first, so TAs become reviewers "who know how to grade spec and eval."
2. **Lag in the academic-integrity framework**. Most universities' integrity clauses are still organized around "completed independently" — which directly conflicts with §5.5.3-(1)'s dual-track scheme. Recommendation: pilot the change opt-in at the course level first, then push for revision at the institutional level.
3. **Credit crowding from new required courses**. The CMU SCS undergraduate total credits are already near the cap; adding three required courses inevitably means replacing or merging existing ones. Recommendation: use 15-251 / 17-313 / 15-122 / 21-325 as the four main entry points for "content updates" (injecting spec / eval / reading theory / heavy-tail statistics modules), reducing the number of standalone new courses.
4. **The cheating trap of Quadrant III assignments**. Among the three quadrants, only Quadrant III prohibits AI, and its share is the smallest; students will easily sneak in AI here — especially when they see that "the final exam allows AI anyway." This bypasses the entire purpose of building the Quadrant III case base in §5.2.3. **The closed-book section of the final exam must be a real, fail-able hard gate**, otherwise the three quadrants collapse into "all-II / I" — and all cognitive prerequisites of judgment are lost.

---

One final point worth making explicit: **not a single recommendation** in this chapter calls for students to study less traditional CS content. Brooks's *essence vs accident* (§1.4.1) still holds in the AI era — **essential complexity cannot be eliminated**. Algorithms, operating systems, theory of computation, discrete mathematics — these courses train exactly the capability for **handling essential complexity**, which under the new division of labor is not weakened but **needed more scarcely** — because AI has already pushed the cost of handling *accidental complexity* near zero, leaving humans only the essential half.

But §5.2.3 splits this further: the hand-training for accidental complexity **should be selectively reallocated** — the parts AI has stably taken over (Quadrant I) should yield; the parts verifiable by Quadrant II methodology should be handed over to the methodology (this is the **new** educational content); **only the narrow part needed as a case base for downstream judgment** (Quadrant III) is retained as hand-training. The task of education thus becomes three things: (1) continue teaching the essential-complexity training of the traditional theory / systems spine; (2) **add** the Quadrant II methodology as a standalone content category; (3) **retain but narrow** the Quadrant III hand-training. This differs from both intuitions — neither "let AI do it since AI can" nor "keep writing as we used to."

---

## References

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
