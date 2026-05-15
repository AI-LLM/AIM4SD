# AI Methodology for Software Development

Outside of OpenAI and Anthropic, enterprises and the open-source community in 2026 are mostly focused on catching up with, reproducing, and even improving on those companies' results. The fundamental effort of these frontier labs is to manufacture **cheaper knowledge-worker labor that approaches the human-average level of quality**. This book does not take the perspective of these "AI-labor manufacturers." Based on what "AI labor" has already achieved and the trend that it will get even cheaper but is structurally unlikely to exceed the human **average**, this book examines how AI users can leverage "AI labor" to improve existing work, create new value, and address the new problems this new factor introduces — and in particular, in the field of software engineering, answer: after AI takes over part of the development subject, how should software-engineering methodology be rewritten, retained, or accelerated.

---

## Contents

### [Chapter 1. Introduction](chapter-01-introduction-en.md)

<!-- TOC-START: chapter-01-introduction-en.md -->
- [1. Fundamental Issues of LLMs](chapter-01-introduction-en.md#1-fundamental-issues-of-llms)
- [2. The Conceptual Map That Emerged After 2022](chapter-01-introduction-en.md#2-the-conceptual-map-that-emerged-after-2022)
  - [A. Input Layer](chapter-01-introduction-en.md#a-input-layer)
  - [B. Context Layer](chapter-01-introduction-en.md#b-context-layer)
  - [C. Action Layer](chapter-01-introduction-en.md#c-action-layer)
  - [D. Runtime & Infrastructure](chapter-01-introduction-en.md#d-runtime--infrastructure)
  - [E. Inference-Time](chapter-01-introduction-en.md#e-inference-time)
  - [F. Training](chapter-01-introduction-en.md#f-training)
  - [G. Eval & Governance](chapter-01-introduction-en.md#g-eval--governance)
- [3. How Many Fundamental Issues Have These Concepts Solved?](chapter-01-introduction-en.md#3-how-many-fundamental-issues-have-these-concepts-solved)
- [4. The Software-Engineering Context](chapter-01-introduction-en.md#4-the-software-engineering-context)
  - [1.4.1 The LLM "fundamental issues" and software-engineering "old problems" are isomorphic](chapter-01-introduction-en.md#141-the-llm-fundamental-issues-and-software-engineering-old-problems-are-isomorphic)
  - [1.4.2 So what does LLM/Agent actually change in software engineering? — Subject replacement](chapter-01-introduction-en.md#142-so-what-does-llmagent-actually-change-in-software-engineering--subject-replacement)
    - [1.4.2.1 The "jagged frontier" of AI capability](chapter-01-introduction-en.md#1421-the-jagged-frontier-of-ai-capability)
    - [1.4.2.2 In contrast with software engineers](chapter-01-introduction-en.md#1422-in-contrast-with-software-engineers)
  - [1.4.3 The biggest change is the time constant: SDLC acceleration = accelerated construction + accelerated decay](chapter-01-introduction-en.md#143-the-biggest-change-is-the-time-constant-sdlc-acceleration--accelerated-construction--accelerated-decay)
    - [1.4.3.1 How to establish new quality standards and earn user trust](chapter-01-introduction-en.md#1431-how-to-establish-new-quality-standards-and-earn-user-trust)
    - [1.4.3.2 How to maximize the share of "effective time" in an accelerated lifecycle](chapter-01-introduction-en.md#1432-how-to-maximize-the-share-of-effective-time-in-an-accelerated-lifecycle)
    - [1.4.3.3 Lift the constraints of incomplete information and continuous change to create the new value that didn't exist before because of "don't know / can't compute / can't afford"](chapter-01-introduction-en.md#1433-lift-the-constraints-of-incomplete-information-and-continuous-change-to-create-the-new-value-that-didnt-exist-before-because-of-dont-know--cant-compute--cant-afford)
<!-- TOC-END: chapter-01-introduction-en.md -->
- [References](chapter-01-introduction-en.md#references)

### [Chapter 2. New Quality Standards for Software](chapter-02-quality-standards-en.md)

<!-- TOC-START: chapter-02-quality-standards-en.md -->
- [2.1 Several Structural Shocks the Jagged Frontier Delivers to Traditional Software Quality](chapter-02-quality-standards-en.md#21-several-structural-shocks-the-jagged-frontier-delivers-to-traditional-software-quality)
  - [2.1.1 Readability: Readable to *whom*?](chapter-02-quality-standards-en.md#211-readability-readable-to-whom)
    - [Human vs LLM "readability ceiling" — absolute capacity and effective depth](chapter-02-quality-standards-en.md#human-vs-llm-readability-ceiling--absolute-capacity-and-effective-depth)
  - [2.1.2 Abstraction patterns: from "reducing duplication" to "reducing irreversibility"](chapter-02-quality-standards-en.md#212-abstraction-patterns-from-reducing-duplication-to-reducing-irreversibility)
  - [2.1.3 Reuse: from "library" to "capability"](chapter-02-quality-standards-en.md#213-reuse-from-library-to-capability)
  - [2.1.4 Testing and coverage: from "coverage" to "trustworthy signal"](chapter-02-quality-standards-en.md#214-testing-and-coverage-from-coverage-to-trustworthy-signal)
  - [2.1.5 Comments and documentation: from synchronization headache to "Single source of truth"](chapter-02-quality-standards-en.md#215-comments-and-documentation-from-synchronization-headache-to-single-source-of-truth)
<!-- TOC-END: chapter-02-quality-standards-en.md -->
- 2.2 Full-process thinking and generation record
- 2.3 Audit and evaluation
- 2.4 Session + Git approach
- [References](chapter-02-quality-standards-en.md#references)

### Chapter 3. Effective-Time Ratio of the SDLC

- Best practices of Human-In-The-Agentic-Loop //👀 ai4se_white_paper/src/chapter3/03-process-engineering.md
- Continuous refactoring and reversibility //👀 ai4se_white_paper/src/chapter3/04-architecture-and-complexity.md
- Knowledge engineering //👀 ai4se_white_paper/src/chapter3/05-knowledge-engineering.md

### Chapter 4. New Value Creation

- New software and new developers
  - "Disposable / per-user-level" software
  - Elastic software and [Fully Autonomous Systems](https://arxiv.org/abs/2604.09388) //👀 [Heuristic Learning](https://trinkle23897.github.io/learning-beyond-gradients/#zh)
  - The new developer

|   | Category | Degree of artifact reuse (count, users, environment diversity) | Requirements on visual design, testing, business-value realization, etc. |
| --- | --- | --- | --- |
| 1 | Solo developer | Reused by self | <br> |
| 2 | Team / Internal developer | Used by a small group of collaborators | <br> |
| 3 | AI Application developer | AI applications productized | <br> |
| 4 | AI Foundation developer | AI technology productized | <br> |
| 5 | Classic software developer | Traditional software-project delivery or productization | <br> |

- The next generation of "open source" and "inner source" (Super OSS & InnerSource)
- Simulation

### [Chapter 5. Human Education](chapter-05-human-education-en.md)

<!-- TOC-START: chapter-05-human-education-en.md -->
- [5.1 The Human Engineer Under AI Collaboration: A New Division of Labor](chapter-05-human-education-en.md#51-the-human-engineer-under-ai-collaboration-a-new-division-of-labor)
  - [5.1.1 Constraint Designer](chapter-05-human-education-en.md#511-constraint-designer)
  - [5.1.2 Verifier / Eval Designer](chapter-05-human-education-en.md#512-verifier--eval-designer)
  - [5.1.3 Process / Reversibility Designer](chapter-05-human-education-en.md#513-process--reversibility-designer)
  - [5.1.4 Accountable Principal](chapter-05-human-education-en.md#514-accountable-principal)
  - [5.1.5 Taste Steward](chapter-05-human-education-en.md#515-taste-steward)
  - [5.1.6 Capability Matrix](chapter-05-human-education-en.md#516-capability-matrix)
- [5.2 From the High-School-Graduate Baseline to the Job Items of §5.1: Capability-Demand Analysis](chapter-05-human-education-en.md#52-from-the-high-school-graduate-baseline-to-the-job-items-of-51-capability-demand-analysis)
  - [5.2.1 The high-school-graduate baseline](chapter-05-human-education-en.md#521-the-high-school-graduate-baseline)
  - [5.2.2 Capability stacks for the five responsibilities](chapter-05-human-education-en.md#522-capability-stacks-for-the-five-responsibilities)
    - [Constraint Designer (capability stack for §5.1.1)](chapter-05-human-education-en.md#constraint-designer-capability-stack-for-511)
    - [Verifier / Eval Designer (capability stack for §5.1.2)](chapter-05-human-education-en.md#verifier--eval-designer-capability-stack-for-512)
    - [Process / Reversibility Designer (capability stack for §5.1.3)](chapter-05-human-education-en.md#process--reversibility-designer-capability-stack-for-513)
    - [Accountable Principal (capability stack for §5.1.4)](chapter-05-human-education-en.md#accountable-principal-capability-stack-for-514)
    - [Taste Steward (capability stack for §5.1.5)](chapter-05-human-education-en.md#taste-steward-capability-stack-for-515)
  - [5.2.3 Three quadrants: which hand-experience is no longer necessary, which can be replaced by new methods, which is still cognitive scaffolding](chapter-05-human-education-en.md#523-three-quadrants-which-hand-experience-is-no-longer-necessary-which-can-be-replaced-by-new-methods-which-is-still-cognitive-scaffolding)
    - [Quadrant I — AI-peripheral engineering has stably taken over; hand-training is no longer cost-effective in education either](chapter-05-human-education-en.md#quadrant-i--ai-peripheral-engineering-has-stably-taken-over-hand-training-is-no-longer-cost-effective-in-education-either)
    - [Quadrant II — Humans remain ultimately accountable, but **new math / statistics / formal methods** can substitute for line-by-line code reading at scale](chapter-05-human-education-en.md#quadrant-ii--humans-remain-ultimately-accountable-but-new-math--statistics--formal-methods-can-substitute-for-line-by-line-code-reading-at-scale)
    - [Quadrant III — Still generated as a by-product of doing things by hand, with no methodological substitute](chapter-05-human-education-en.md#quadrant-iii--still-generated-as-a-by-product-of-doing-things-by-hand-with-no-methodological-substitute)
    - [Educational synthesis](chapter-05-human-education-en.md#educational-synthesis)
- [5.3 Educational Comparison: CMU SCS BS in Computer Science](chapter-05-human-education-en.md#53-educational-comparison-cmu-scs-bs-in-computer-science)
  - [5.3.1 Why pick CMU](chapter-05-human-education-en.md#531-why-pick-cmu)
  - [5.3.2 Outline of CMU BS in CS curriculum structure (2025–2026 public version)](chapter-05-human-education-en.md#532-outline-of-cmu-bs-in-cs-curriculum-structure-20252026-public-version)
- [5.4 Curriculum Gap Diagnosis](chapter-05-human-education-en.md#54-curriculum-gap-diagnosis)
  - [5.4.1 Gap 1: No instructional vehicle for spec engineering](chapter-05-human-education-en.md#541-gap-1-no-instructional-vehicle-for-spec-engineering)
  - [5.4.2 Gap 2: Insufficient engineering training in evaluation and trustworthy signals](chapter-05-human-education-en.md#542-gap-2-insufficient-engineering-training-in-evaluation-and-trustworthy-signals)
  - [5.4.3 Gap 3: Reversibility and blast radius — no vehicle for abstraction repricing](chapter-05-human-education-en.md#543-gap-3-reversibility-and-blast-radius--no-vehicle-for-abstraction-repricing)
  - [5.4.4 Gap 4: A harness is not a new OS, but it does require a composite engineering discipline cutting across multiple courses](chapter-05-human-education-en.md#544-gap-4-a-harness-is-not-a-new-os-but-it-does-require-a-composite-engineering-discipline-cutting-across-multiple-courses)
  - [5.4.5 Gap 5: Insufficient training intensity for reading-intensive work](chapter-05-human-education-en.md#545-gap-5-insufficient-training-intensity-for-reading-intensive-work)
  - [5.4.6 Gap 6: Training opportunities for taste and aesthetics are compressed](chapter-05-human-education-en.md#546-gap-6-training-opportunities-for-taste-and-aesthetics-are-compressed)
  - [5.4.7 Gap 7: Insufficient hands-on training for trust, accountability, compliance, and safety](chapter-05-human-education-en.md#547-gap-7-insufficient-hands-on-training-for-trust-accountability-compliance-and-safety)
  - [5.4.8 Gap 8: Assessment and academic integrity themselves need to be redesigned](chapter-05-human-education-en.md#548-gap-8-assessment-and-academic-integrity-themselves-need-to-be-redesigned)
- [5.5 Recommendations](chapter-05-human-education-en.md#55-recommendations)
  - [5.5.1 Remodel existing courses](chapter-05-human-education-en.md#551-remodel-existing-courses)
  - [5.5.2 New required and elective courses](chapter-05-human-education-en.md#552-new-required-and-elective-courses)
  - [5.5.3 Teaching method: a three-quadrant assignment system + process audit](chapter-05-human-education-en.md#553-teaching-method-a-three-quadrant-assignment-system--process-audit)
- [5.6 Implementation Risks and Pacing](chapter-05-human-education-en.md#56-implementation-risks-and-pacing)
<!-- TOC-END: chapter-05-human-education-en.md -->
- [References](chapter-05-human-education-en.md#references)
