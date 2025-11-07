# Resources and Literature Review

## Research Gap Assessment

### What Was Provided
- **Hypothesis**: Formal environment engineering with communication protocols, sanction mechanisms, and audit hooks can render truthful uncertainty reports subgame-perfect in multi-agent systems
- **Background References**:
  - Eisenstein et al. (incentive structure yields meta-knowledge)
  - Gardelli et al., 2006 (formal environment engineering)
  - Guo & Bürger, 2019 (Predictive Safety Networks)
  - Plaat et al., 2025 (agentic LLMs survey)
- **No specific datasets or benchmarks specified**
- **No specific baselines specified**
- **No evaluation metrics specified**

### What Was Found Through Research

#### 1. Key Papers and Concepts

**Mechanism Design Foundation:**
- Mechanism design focuses on creating systems where truthful reporting is incentive-compatible
- Revelation principle: For any implementable outcome, there exists an incentive-compatible mechanism
- Subgame perfect equilibrium (SPE) ensures strategies form Nash equilibrium in every subgame

**Multi-Agent Coordination (Gardelli et al. area):**
- Environment as coordination mechanism in MAS
- Coordination artifacts as unifying abstraction
- Environment abstractions provide services for agent goals
- Paper found: "Infrastructures for the environment of multiagent systems" (2006)

**Safety Constraints (Guo & Bürger):**
- Paper: "Predictive Safety Network for Resource-constrained Multi-agent Systems" (CoRL 2020)
- Combines centralized safety policy with decentralized task policy
- Predicts future resource levels to ensure safety
- Applied to warehouse robot coordination

**Agentic LLMs (Plaat et al. 2025):**
- Survey: "Agentic Large Language Models, a survey" (arXiv 2503.23037)
- Defines agentic LLMs as systems that: (1) reason, (2) act, (3) interact
- Applications in high-stakes domains: medical diagnosis, logistics, financial analysis
- Notes risks of LLM assistants taking real-world actions

#### 2. Relevant Benchmarks and Datasets

**LLM Calibration and Uncertainty:**
- **LLM-Uncertainty-Bench** (NeurIPS 2024): Evaluates 9 LLMs across 5 NLP tasks
  - Uses MMLU, CosmosQA, HaluEval datasets
  - 10,000 instances per task
  - Metrics: Accuracy, prediction uncertainty, UAcc (uncertainty-aware accuracy)
  - GitHub: https://github.com/smartyfh/LLM-Uncertainty-Bench

- **LM-Polygraph**: Multilingual claim-level uncertainty quantification
  - Fact-checking in English, Mandarin, Arabic, Russian

**Multi-Agent Systems:**
- Limited existing benchmarks for truthfulness in multi-agent communication
- Most existing work focuses on single-agent calibration
- Gap identified: Need benchmarks for strategic misreporting in multi-agent settings

#### 3. Recent Research on Incentive Mechanisms (2024-2025)

- Information elicitation mechanisms for Bayesian auctions (Springer 2025)
- Federated learning with incentive mechanisms using contract theory
- Mechanism simplicity: Participants must see mechanism is incentive-compatible
- Dynamic reputation systems for truthful participation

### Decisions Made

Given the research findings, I will:

**1. Create Custom Multi-Agent Environment:**
- No existing benchmark directly tests incentive-compatible truthfulness reporting
- Will design a cooperative task requiring uncertainty communication (e.g., collaborative question answering)
- Agents must report confidence/uncertainty to coordinate effectively

**2. Use Real LLM APIs (Critical):**
- Will use Claude Sonnet 4.5 or GPT-4.1/5 via API
- Test REAL agent behavior, not simulations
- Measure actual calibration and truthfulness under different mechanisms

**3. Implement Three Mechanisms:**
- **Baseline (No Incentives)**: Simple cooperative protocol, no penalties
- **Audit-Based Sanctions**: Ex-post audits with reputation penalties for miscalibration
- **Predictive Safety Network inspired**: Mandatory abstention zones for low-confidence cases

**4. Evaluation Metrics:**
- **Truthfulness**: Calibration error (Expected Calibration Error)
- **Cooperation quality**: Task success rate, optimal decision rate
- **Efficiency**: Communication overhead, computation time
- **Strategic behavior**: Frequency of over/under-confidence

**5. Adapt Existing Tools:**
- Use calibration metrics from LLM-Uncertainty-Bench
- Adapt multi-agent frameworks (potential use of simple custom coordination)
- Use conformal prediction techniques for uncertainty sets

### Rationale for Approach

**Why create custom environment:**
- No existing benchmark tests strategic misreporting under different incentive structures
- Need controlled setting to isolate effect of mechanism design
- Can adapt TruthfulQA or MMLU questions as underlying task

**Why use real APIs:**
- Research goal is to test behavior of actual LLMs, not simulated agents
- Recent work shows LLMs behave in complex, emergent ways
- Cost is reasonable (100-500 test cases = $10-50)

**Why these mechanisms:**
- Baseline establishes cooperative behavior without incentives
- Audit mechanism implements subgame-perfect penalties mentioned in hypothesis
- Safety constraints test mandatory abstention zones from Guo & Bürger

### References Collected

1. Plaat, A., et al. (2025). "Agentic Large Language Models, a survey." arXiv:2503.23037
2. Guo, M., & Bürger, M. (2020). "Predictive Safety Network for Resource-constrained Multi-agent Systems." CoRL.
3. Gardelli, L., et al. (2007). Work on multi-agent coordination and environment engineering.
4. Ye, F., et al. (2024). "Benchmarking LLMs via Uncertainty Quantification." NeurIPS 2024.
5. Various 2024-2025 papers on mechanism design for truthful reporting in multi-agent systems.

### Search Summary

**Total search time**: ~15 minutes
**Searches performed**: 7 web searches
**Key findings**:
- Established theoretical foundation (mechanism design, subgame perfection)
- Found relevant recent work (Plaat survey, Guo safety networks, calibration benchmarks)
- Identified gap: No existing benchmark for strategic truthfulness in multi-agent LLM systems
- Decision: Create custom environment, use real APIs, test three mechanism variants

**Next step**: Create detailed experimental plan (planning.md)
