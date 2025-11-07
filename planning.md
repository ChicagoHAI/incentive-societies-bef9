# Research Planning Document

## Research Question

**Can formal environment engineering (communication protocols, sanction mechanisms, and audit hooks) render truthful uncertainty reports a subgame-perfect equilibrium in multi-agent LLM systems, thereby enabling reliable meta-knowledge sharing in high-stakes collaborative tasks?**

## Background and Motivation

### Why This Matters

In high-stakes domains (medical diagnosis, financial analysis, autonomous systems), multiple AI agents must collaborate and share their uncertainty honestly. Current multi-agent LLM systems lack formal incentive structures to ensure truthful reporting of confidence/uncertainty. Agents might:
- Overstate confidence to appear more authoritative
- Understate uncertainty to avoid appearing incompetent
- Free-ride on others' information without contributing honestly

### Gap This Fills

Existing research:
- **Mechanism design theory**: Provides tools for incentive-compatible systems
- **LLM calibration**: Focuses on single-agent accuracy
- **Multi-agent LLMs**: Limited work on strategic communication and honesty

**Missing**: Empirical testing of mechanism design principles applied to multi-agent LLM systems where truthful uncertainty reporting is critical for safe coordination.

### Theoretical Foundation

1. **Revelation Principle**: Any desired outcome can be implemented via truthful reporting mechanism
2. **Subgame Perfect Equilibrium**: Incentives must hold at every decision point, not just in aggregate
3. **Environment Engineering**: The interaction structure (environment) shapes emergent behavior
4. **Predictive Safety**: Mandatory constraints prevent unsafe actions (e.g., high-risk decisions without sufficient confidence)

## Hypothesis Decomposition

### Main Hypothesis
Formal environment engineering makes truthful uncertainty reporting subgame-perfect equilibrium.

### Testable Sub-Hypotheses

**H1 (Calibration Improvement)**: Agents under audit-based sanction mechanisms will exhibit better calibration (lower Expected Calibration Error) than baseline cooperative agents.

**H2 (Strategic Honesty)**: The frequency of strategic misreporting (deliberate over/under-confidence) will be lower under mechanisms with ex-post audits.

**H3 (Safety Constraint Effectiveness)**: Mandatory abstention zones will reduce high-confidence errors in low-confidence situations more than peer pressure alone.

**H4 (Cooperation Quality)**: Despite incentive mechanisms, task success rates will remain high or improve due to better信息 quality.

**H5 (Subgame Perfection)**: The advantage of truthful reporting will persist across different decision points and task contexts (robustness test).

## Proposed Methodology

### High-Level Approach

**Comparative Experimental Design**: Test three mechanism variants on cooperative multi-agent tasks requiring uncertainty communication.

1. **Baseline**: Naive cooperative protocol (no incentives beyond task success)
2. **Audit Mechanism**: Ex-post audits with reputation penalties for miscalibration
3. **Safety Constrained**: Mandatory abstention when confidence below threshold

**Justification**: Controlled comparison isolates effect of mechanism design on truthfulness while keeping task, agents, and information constant.

### Experimental Design

#### Task: Collaborative Question Answering

**Setup:**
- Two LLM agents collaborate to answer factual questions
- Each agent independently examines the question and forms initial answer + confidence
- Agents then communicate their answers and confidences
- Final decision made based on communicated information
- Ground truth known (from benchmark datasets)

**Why This Task:**
- Clear ground truth for measuring accuracy
- Natural role for uncertainty communication
- Allows measuring calibration at individual and group level
- Realistic high-stakes analog (collaborative diagnosis, analysis)

#### Agent Architecture

Each agent will:
1. **Perceive**: Receive question and any context
2. **Reason**: Generate answer with chain-of-thought
3. **Self-assess**: Estimate confidence in answer (0-100%)
4. **Communicate**: Report answer + confidence to partner
5. **Decide**: Synthesize information and make final choice

**Models**: Use Claude Sonnet 4.5 or GPT-4.1 (real API calls)

#### Three Mechanisms

**Mechanism 1: Baseline (Cooperative)**
- Protocol: "Work together to answer correctly. Share your answers and confidence honestly."
- Incentive: Task success only
- No penalties for miscalibration

**Mechanism 2: Audit-Based Sanctions**
- Protocol: Same communication, but add audit component
- Audit: After each interaction, check calibration accuracy
- Sanction: Maintain reputation score; penalties for systematic miscalibration
- Reputation affects "trust weight" in future decision aggregation
- **Implementation of Subgame Perfection**: Agents know future interactions depend on current honesty

**Mechanism 3: Safety-Constrained**
- Protocol: Mandatory abstention if confidence < threshold (e.g., <60%)
- If both agents below threshold, defer to "ask human" option
- Safety zones: Very low confidence (<40%) triggers warning, potential task failure if proceeded
- **Implementation of Safety Networks**: Predictive avoidance of high-risk decisions

### Experimental Steps

1. **Setup Environment** (30 min)
   - Install dependencies: OpenAI/Anthropic APIs, numpy, pandas, matplotlib, scipy
   - Set up API clients with retry logic and rate limiting
   - Initialize random seeds (42 for reproducibility)

2. **Create Dataset** (20 min)
   - Sample 150 questions from TruthfulQA or MMLU (diverse difficulty)
   - Split: 30 calibration, 100 test, 20 held-out validation
   - Ensure balanced difficulty distribution

3. **Implement Agent Framework** (60 min)
   - Base Agent class: prompt generation, API calling, confidence extraction
   - Communication protocol: structured message passing
   - Decision aggregation: confidence-weighted voting

4. **Implement Three Mechanisms** (60 min)
   - Baseline: Simple cooperation prompts
   - Audit: Add reputation tracking, calibration measurement, penalty calculation
   - Safety: Add confidence thresholds, abstention logic, safety warnings

5. **Run Experiments** (90 min)
   - Each mechanism: 100 test questions
   - 3 random seeds for robustness
   - Log all: prompts, responses, confidences, decisions, ground truth
   - Cache API responses to avoid redundant calls

6. **Analysis** (45 min)
   - Compute calibration metrics (ECE, MCE, Brier score)
   - Measure task accuracy and success rates
   - Identify strategic misreporting patterns
   - Statistical tests for significance
   - Generate visualizations

7. **Documentation** (30 min)
   - Write REPORT.md with findings
   - Create README.md with setup instructions
   - Document code with clear comments

### Baselines

**Primary Baseline**: Mechanism 1 (Cooperative without explicit incentives)
- Represents current state: agents asked to cooperate honestly

**Secondary Comparisons**:
- Single-agent performance (no collaboration)
- Random confidence assignment (sanity check)
- Perfect calibration (theoretical optimum)

**Justification**: These baselines establish floor (random), current practice (cooperative), and ceiling (perfect).

### Evaluation Metrics

#### Primary Metrics

**1. Expected Calibration Error (ECE)**
- **Measures**: How well confidence matches actual accuracy
- **Computation**: Bin predictions by confidence, measure |confidence - accuracy| per bin, weighted average
- **Why**: Standard metric for calibration, directly tests truthfulness hypothesis
- **Interpretation**: Lower is better; <0.05 is well-calibrated

**2. Task Accuracy**
- **Measures**: % of questions answered correctly
- **Why**: Ensures mechanisms don't harm primary objective
- **Interpretation**: Higher is better; should remain ≥70% for useful system

**3. Strategic Misreporting Rate**
- **Measures**: % of cases where confidence significantly deviates from calibrated level (>15 percentage points)
- **Why**: Detects deliberate gaming of system
- **Interpretation**: Lower indicates more honest reporting

#### Secondary Metrics

**4. Brier Score**
- Proper scoring rule measuring probabilistic accuracy
- Lower is better; incorporates both calibration and resolution

**5. Maximum Calibration Error (MCE)**
- Worst-case calibration in any confidence bin
- Tests tail behavior

**6. Abstention Rate (Safety mechanism)**
- % of questions where agent abstains due to low confidence
- Should correlate with actual difficulty

**7. Agreement Rate**
- % of questions where agents agree on answer
- Tests whether mechanisms affect collaboration

**8. Confidence Distribution**
- Mean, std, range of reported confidences
- Detects over/under-confidence patterns

### Statistical Analysis Plan

**Hypothesis Tests**:
- **H1**: Paired t-test comparing ECE across mechanisms (α=0.05)
- **H2**: Chi-square test for strategic misreporting frequency
- **H3**: Compare error rates in low-confidence regime (t-test)
- **H4**: ANOVA for accuracy across mechanisms
- **H5**: Repeated measures ANOVA across task contexts

**Effect Sizes**:
- Cohen's d for calibration differences
- Odds ratios for categorical outcomes

**Multiple Comparisons**:
- Bonferroni correction when testing multiple metrics

**Confidence Intervals**:
- Bootstrap 95% CIs for all primary metrics

**Sample Size**:
- 100 test questions × 3 seeds = 300 observations per mechanism
- Power analysis: detects effect size d=0.3 with power 0.80

## Expected Outcomes

### Results Supporting Hypothesis

1. **ECE significantly lower** in audit mechanism vs baseline (H1 supported)
2. **Strategic misreporting 50%+ lower** with incentives (H2 supported)
3. **Safety mechanism reduces high-confidence errors** by 30%+ in low-confidence cases (H3 supported)
4. **Accuracy maintained or improved** (within 5%) across mechanisms (H4 supported)
5. **Benefits persist** across difficulty levels and question types (H5 supported)

### Results Refuting Hypothesis

1. **No calibration difference** between mechanisms (H1 refuted)
2. **Incentives increase** strategic behavior (gaming the system) (H2 refuted)
3. **Safety constraints harm** accuracy without improving calibration (H3 refuted)
4. **Task performance degrades** significantly with mechanisms (H4 refuted)
5. **Benefits only in** specific contexts, not general (H5 refuted)

### Alternative Explanations to Consider

- **Model capability ceiling**: LLMs already well-calibrated, no room for improvement
- **Prompt artifacts**: Differences due to prompt wording, not mechanism
- **Task specificity**: Results only hold for factual QA, not other domains
- **Sample size**: Insufficient power to detect real effects
- **API variability**: Results affected by model version changes during experiment

## Timeline and Milestones

**Total Time**: 6 hours (360 minutes)

| Phase | Duration | Tasks |
|-------|----------|-------|
| **Phase 0: Research** | 30 min | ✓ Completed |
| **Phase 1: Planning** | 30 min | ✓ In progress |
| **Phase 2: Setup** | 30 min | Environment, dependencies, data |
| **Phase 3: Implementation** | 120 min | Agent framework, three mechanisms |
| **Phase 4: Experiments** | 90 min | Run all conditions, collect data |
| **Phase 5: Analysis** | 45 min | Statistics, visualizations |
| **Phase 6: Documentation** | 30 min | REPORT.md, README.md |
| **Buffer** | 15 min | Debugging, unexpected issues |

### Critical Path
1. Agent framework must work before mechanism implementation
2. Dataset must be prepared before experiments
3. At least one full mechanism must complete before comparative analysis

## Potential Challenges

### Challenge 1: API Rate Limits
**Risk**: OpenAI/Anthropic may rate-limit during experiments
**Mitigation**:
- Implement exponential backoff and retry logic
- Cache all responses to avoid re-requests
- Use smaller test set (75 questions) if necessary
**Contingency**: Switch to cheaper model (GPT-4o-mini) for development

### Challenge 2: Calibration Detection
**Risk**: LLMs may not output well-formatted confidence scores
**Mitigation**:
- Use structured prompting (JSON output format)
- Parsing fallbacks (regex extraction from text)
- Multiple prompt attempts with validation
**Contingency**: Use log-probability based confidence if self-reported fails

### Challenge 3: Mechanism Complexity
**Risk**: Implementing audit/reputation system may be time-consuming
**Mitigation**:
- Start with simplest version (binary reputation: good/bad)
- Use simple penalty function (linear decay)
- Focus on core concept, not sophisticated implementation
**Contingency**: Simplify to two mechanisms (Baseline + Safety only)

### Challenge 4: Statistical Power
**Risk**: 100 questions may not provide sufficient power
**Mitigation**:
- Use repeated measures (3 seeds) to increase effective n
- Focus on effect sizes, not just p-values
- Qualitative analysis of individual cases
**Contingency**: Run subset of conditions with more questions if time permits

### Challenge 5: Time Constraints
**Risk**: Full implementation may exceed 6-hour window
**Mitigation**:
- Modular implementation (can stop at 2 mechanisms)
- Prioritize core comparison (Baseline vs Audit)
- Analysis can be simplified if needed
**Contingency**: Document partial results, note limitations, suggest follow-ups

## Success Criteria

### Minimum Success (Must Achieve)
1. ✓ At least 2 mechanisms implemented and tested
2. ✓ At least 50 test questions per mechanism
3. ✓ Calibration and accuracy metrics computed
4. ✓ Statistical comparison performed (even if underpowered)
5. ✓ REPORT.md documenting findings and limitations

### Target Success (Goal)
1. ✓ All 3 mechanisms implemented
2. ✓ 100 test questions per mechanism, 3 seeds
3. ✓ All primary and secondary metrics
4. ✓ Statistically significant results with effect sizes
5. ✓ Comprehensive REPORT.md + visualizations

### Stretch Success (If Time Permits)
1. ✓ Ablation studies on mechanism components
2. ✓ Cross-model testing (Claude + GPT)
3. ✓ Multiple task domains
4. ✓ Robustness analysis
5. ✓ Publication-ready write-up

## Methodological Justifications

### Why Real APIs Instead of Simulated Agents?
**Decision**: Use Claude Sonnet 4.5 or GPT-4.1 via API

**Justification**:
- Research hypothesis is about REAL LLM behavior under incentives
- Simulated agents cannot capture emergent strategic behavior
- LLMs have complex, learned representations of honesty, cooperation, reputation
- Cost is manageable (300-500 API calls ≈ $10-30)
- Results generalizable to actual deployed systems

**Alternatives Rejected**:
- Simulated agents with hand-coded behavior: Not realistic, circular reasoning
- Single-agent experiments: Missing multi-agent strategic dynamics
- Human subjects: Too expensive/time-consuming for mechanism testing

### Why Factual QA Task?
**Decision**: Use questions from TruthfulQA/MMLU

**Justification**:
- Clear ground truth for measuring accuracy
- Well-established benchmarks with quality control
- Natural uncertainty gradient (easy to hard questions)
- High-stakes analog (medical/financial decisions often fact-based)
- Allows clean calibration measurement

**Alternatives Rejected**:
- Open-ended generation: No clear ground truth for calibration
- Game-theoretic tasks: Too abstract, hard to measure truthfulness
- Robotics coordination: Requires simulation environment, different focus

### Why Three Specific Mechanisms?
**Decision**: Baseline, Audit-Based, Safety-Constrained

**Justification**:
- **Baseline**: Necessary control, represents current practice
- **Audit**: Directly implements subgame-perfect sanctions from hypothesis
- **Safety**: Tests mandatory abstention (safety networks concept)
- Together test two distinct approaches from theoretical literature
- Feasible to implement in time constraints

**Alternatives Rejected**:
- Market-based mechanisms: Too complex to implement in time frame
- Explicit payments/tokens: LLMs don't respond to monetary incentives directly
- Pure reputation without audits: Harder to enforce credibly

## Dependencies and Assumptions

### Technical Dependencies
- OpenAI API or Anthropic API access (API keys available in environment)
- Python 3.10+
- Core libraries: numpy, pandas, matplotlib, scipy, openai/anthropic clients
- Stable internet connection for API calls

### Assumptions
1. **LLMs can report confidence**: Models will provide numerical confidence estimates when prompted
2. **Ground truth available**: Selected questions have clear, verifiable answers
3. **Strategic capability**: LLMs sophisticated enough to engage in strategic behavior
4. **Prompt stability**: Mechanism differences not confounded by prompt variations
5. **API stability**: Model versions remain constant during experiment

### Validation Steps
- Test confidence extraction on sample questions
- Verify ground truth accuracy for selected dataset
- Pilot test one mechanism end-to-end before full run
- Check prompt equivalence across mechanisms (word count, structure)
- Log all API parameters (model, temperature, seed) for reproducibility

## Risk Assessment

### High Risks
1. **API failures**: Rate limits, downtime, version changes
2. **Time overrun**: Implementation complexity exceeds estimates

### Medium Risks
1. **Null results**: No significant differences between mechanisms
2. **Confounds**: Prompt differences affect results more than mechanisms
3. **Power issues**: Insufficient sample size for statistical significance

### Low Risks
1. **Code bugs**: Fixable with debugging time
2. **Data quality**: Benchmark datasets are well-validated
3. **Analysis errors**: Standard metrics with established implementations

## Ethical Considerations

- **No human subjects**: Using only API calls to commercial LLMs
- **No personally identifiable information**: Public benchmark datasets only
- **Transparency**: All prompts, code, and results will be documented
- **Dual use**: Research could inform both beneficial (safety) and harmful (manipulation) applications
- **Cost responsibility**: API costs within reasonable research budget ($30-50)

## Next Steps After This Research

### Immediate Follow-Ups (If Results Positive)
1. Test on additional task domains (summarization, diagnosis, planning)
2. Cross-model validation (Claude, GPT, Gemini, open-source models)
3. Longer interaction sequences (repeated games, reputation building)
4. Human-AI hybrid teams (mixed human and agent populations)
5. Deploy in real collaborative tool (e.g., research assistant, medical consultation)

### Broader Extensions
1. Formal game-theoretic analysis of implemented mechanisms
2. Learned incentive structures (meta-learning optimal mechanisms)
3. Integration with uncertainty quantification methods (conformal prediction)
4. Multi-stakeholder scenarios (conflicting incentives across agents)
5. Robustness to adversarial agents

### Open Questions
1. Do results generalize to non-factual tasks (creative, subjective domains)?
2. How do mechanisms scale to >2 agents?
3. Can agents learn to game sophisticated mechanisms?
4. What is the minimal mechanism complexity for effectiveness?
5. How do human users perceive and trust agents under different mechanisms?

---

**Prepared by**: Claude Research Agent
**Date**: 2025-11-07
**Status**: Ready to proceed to Phase 2 (Implementation)
**Estimated completion**: 5.5 hours from now
