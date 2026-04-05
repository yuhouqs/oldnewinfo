#oldnewinfo

# Discussion Notes: Daniel, Lee, and Naveen (2025) -- "Information Discovery by Analysts"

**Paper:** Daniel, N.D., Lee, S.M., and Naveen, L. (2025). Information Discovery by Analysts. *Journal of Accounting, Auditing & Finance*, 40(4), 1350--1379.

**Date of Discussion:** April 4, 2026

---

## Paper Overview

The paper provides direct evidence that analysts engage in information discovery and that investors value this discovery. The key innovation is manually reading summaries of 3,500+ analyst reports to classify whether each report contains discovery, interpretation, or both -- rather than relying on timing-based classification schemes used in prior literature.

**Key distinctions:**
- **Discovery:** Analyst generates new/private information not readily available to investors
- **Interpretation:** Analyst reacts to and quantifies the value implications of public information

**Discovery sources:**
- **Management sources:** Personal meetings, conference calls, investor/analyst meetings
- **Non-management sources:** Surveys, channel checks, industry contacts

**Sample:** 3,553 reports from 241 analysts covering 176 firms in 1999 (pre-Reg FD) and 2003 (post-Reg FD), sourced from Investext and matched to I/B/E/S.

**Headline finding:** 17.4% of reports contain discovery; investors react more strongly to price target and EPS revisions when accompanied by discovery.

---

## Table 5: Determinants of Discovery (Logistic Regressions)

### Sample Construction

Each column uses a different subsample to create a clean comparison between one specific type of discovery and pure interpretation reports:

- **Column 1 (Personal Meetings):** Keeps reports with discovery from personal meetings + interpretation reports. Drops all other discovery reports. N = 2,563.
- **Column 2 (Management):** Keeps reports with discovery from any management source + interpretation reports. Drops non-management discovery reports. N = 2,824.
- **Column 3 (Non-Management):** Keeps reports with discovery from non-management sources + interpretation reports. Drops management discovery reports. N = 2,553.
- **Column 4 (Discovery overall):** Keeps all reports. No exclusions by discovery type. N = 2,910.

The logic: the baseline (DV = 0) is always pure interpretation, so each column isolates one discovery type against a clean benchmark.

### Most Notable Findings

1. **Reg FD caused substitution, not reduction, in discovery.** Pre-Reg FD coefficient is positive for personal meetings (Column 1) but negative for non-management sources (Column 3). Analysts shifted from management access to independent fieldwork after regulation. The overall discovery coefficient (Column 4) is insignificant because the two effects offset.

2. **All-Star analysts differentiate through non-management discovery.** The All-Star coefficient is insignificant for personal meetings and management but strongly positive for non-management sources. Their edge comes from proprietary fieldwork, not management access.

3. **Optimism buys management access.** Firm-specific optimism is significant only in Column 1 (personal meetings). Management rewards favorable coverage with preferential access, raising questions about the reliability of discovery from personal meetings.

4. **Prestigious brokers have less non-management discovery.** Analysts at large brokerages can free-ride on internal information from colleagues covering related supply-chain firms.

5. **Information asymmetry only drives non-management discovery.** Analysts bridge information gaps by gathering outside information, not by getting more management access.

---

## Table 6: Market Reaction to Discovery

### Main Takeaway

Discovery amplifies market reactions to price target and EPS revisions, but not to recommendation changes.

- **Price Target x Discovery** and **EPS x Discovery** interaction terms are positive and significant across all specifications, including the full model (Column 4).
- **Recommendation x Discovery** interaction is insignificant.
- **Discovery standalone** (b1) is insignificant -- discovery only matters when it accompanies a substantive revision. It serves as a credibility mechanism, not a standalone signal.

### Economic Magnitudes
- One-SD move in price target with discovery: ACAR increases ~23% relative to mean
- One-SD move in EPS with discovery: ACAR increases ~125% relative to mean

### Interpretation
The pattern makes sense: price targets and EPS are quantitative, continuous measures where information precision directly affects credibility. Recommendations are coarse (five-point scale) and bundled with strategic considerations, so discovery adds less incremental credibility.

---

## Table 8: Personal Meetings vs. Other Discovery Sources

### What the Table Tests

Splits Discovery into Discovery(Personal Meetings) and Discovery(Others), each interacted with three revision types. Tests whether personal meetings are more valuable as H2 predicts.

### Results Pattern

| Interaction | Personal Meetings | Other Sources |
|---|---|---|
| Recommendation x Discovery | Positive, significant | Insignificant |
| Price Target x Discovery | **Negative, significant** | Positive, significant |
| EPS x Discovery | Positive, significant | Mixed (depends on conference call classification) |

### Key Critique: Results Resist a Coherent Narrative

This is a significant weakness of the paper. No single story rationalizes all six coefficients simultaneously:

- If investors discount personal meetings due to management capture/spin, why do they positively value EPS revisions from the same meetings?
- If independent sources are more credible, why don't they amplify recommendation changes?
- The mixed EPS results for other sources (sensitive to conference call classification) further complicate interpretation.

The authors are honest about the mixed findings but fail to provide a plausible explanation, which weakens this section of the paper.

### Likely Root Cause: Small Samples and Compositional Differences

- Personal meetings = only 152 reports out of 3,553 (4.3%). After excluding reports near earnings announcements, effective counts are even smaller.
- Interaction effects are estimated off very thin subsamples where outliers can drive results.
- **More fundamentally:** reports with personal meetings and reports with other discovery sources likely differ systematically on omitted dimensions (firm size, industry, analyst characteristics, report detail, timing), and these differences may drive the inconsistent pattern rather than the source distinction itself.

---

## Identified Weaknesses and Potential Extensions

### Weakness 1: No Analysis of Discovery Content

The paper treats all discovery as homogeneous -- a personal meeting confirming Q1 trends are on track gets the same coding as one revealing a major unannounced product line. This likely introduces substantial noise and limits what can be inferred about why discovery is valued.

**Potential approaches to address this:**

1. **Revision direction relative to consensus (simplest).** Compare analyst's revision direction against recent consensus drift (prior 30-60 days). Same-direction = confirmatory, opposite-direction = contradictory. Uses standard I/B/E/S data.

2. **Forecast boldness.** Following Clement and Tse (2005), measure whether post-discovery forecasts deviate further from (bold) or closer to (herding) consensus. Interact Discovery x Bold to test whether bold discovery-backed revisions get stronger reactions.

3. **LLM-based content classification.** Use LLMs to classify report excerpts on dimensions like confirmatory vs. contradictory, quantitative specificity, topic (revenue/costs/strategy), and tone. Ground classification by providing consensus EPS/recommendation as context. Validate against hand-coded subsample.

4. **Market-based inference (indirect).** Interact Discovery with surprise measures (deviation of analyst's new forecast from consensus). Positive interaction suggests discovery is more valued when it produces contrarian information. Weakness: circularity.

**Recommended approach:** Combine approaches 1 and 2 as primary tests (I/B/E/S data only), use approach 3 as corroborating evidence. Strongest paper uses quantitative primary test with LLM classification as supplementary.

### Weakness 2: Omitted Variable Problem in Table 8

Reports with personal meetings vs. other discovery sources are not randomly assigned. They are produced by different analysts, covering different firms, under different circumstances. Systematic differences on omitted dimensions could explain the inconsistent interaction patterns.

**Potential approaches to address this:**

1. **Matching.** Propensity score or coarsened exact matching pairing personal meeting reports with other discovery reports on firm characteristics (size, information asymmetry, following), analyst characteristics (experience, All-Star, optimism), and report characteristics (revision magnitude, timing).

2. **Richer fixed effects.** Apply analyst-firm fixed effects to Table 8 specification. Asks: for the same analyst covering the same firm, do personal meeting vs. other discovery reports generate different market reactions? Requires sufficient within-pair variation.

3. **Additional controls.** Add report-level variables likely correlated with discovery source:
   - Report length/detail
   - Whether report contains both discovery and interpretation
   - Number of discovery sources cited
   - Industry fixed effects
   - Time gap since analyst's last report on the firm

4. **Selection model.** First-stage model predicting discovery source, second-stage adjustment via inverse Mills ratio. Possible exclusion restriction: geographic proximity between analyst and firm headquarters (predicts personal meeting probability but should not directly affect market reaction).

5. **Subsample of overlapping cases.** Restrict to analyst-firm pairs where both personal meeting and other discovery reports are observed. Conditions on analyst having capability/willingness to use both source types.

**Most practical steps:** Add industry fixed effects and report-level controls first, then run matching analysis, and present descriptive statistics comparing the two groups to make the omitted variable concern transparent.

---

## Implications for My Project: Distinguishing New Information from Old

My project focuses on how to tell new information from old information. The Daniel et al. paper is directly relevant because it tackles a version of this problem in the analyst setting, and both its strengths and its weaknesses offer lessons for my research design.

### Lesson 1: The Fundamental Measurement Challenge

Daniel et al. show that even a seemingly simple classification -- does this report contain new information? -- is far harder than it appears. Their main critique of the prior literature is that timing-based proxies (reports issued before vs. after earnings) are noisy classifiers. Their Table 4 demonstrates this: discovery reports appear both before and after earnings announcements (42% pre, 54% post), so timing alone cannot separate new from old information.

**Implication for my project:** Whatever approach I use to classify information as new vs. old, I need to be explicit about what "new" means and validate that my classification captures it. Possible definitions include:
- New to the market (not previously reflected in prices)
- New to the analyst (not previously in their information set)
- New relative to consensus (deviates from prevailing expectations)
- New in substance (covers topics not previously discussed)

Each definition implies a different measurement strategy, and conflating them will create noise.

### Lesson 2: Source vs. Content -- and Why Content Matters More

The paper's biggest limitation is that it classifies discovery by source (personal meeting, channel check, etc.) rather than by the content of what was discovered. This means a meeting that confirms existing expectations gets the same coding as one that reveals a material surprise. The mixed results in Table 8 likely stem partly from this limitation.

**Implication for my project:** I should prioritize content-based classification over source-based classification. Specifically:
- Measure whether the information confirms or contradicts what the market/consensus already believes
- Measure the quantitative specificity of the information (precise numbers vs. vague directional statements)
- Measure the topical novelty (does the report raise issues not previously discussed by other analysts or in prior filings?)

Content-based measures are harder to construct but more directly capture what makes information "new."

### Lesson 3: Operationalizing "Newness" -- Practical Approaches

From our discussion, several concrete strategies emerged for measuring information newness:

**Quantitative approaches (using structured data):**
- **Revision direction vs. consensus drift:** Compare the analyst's revision direction against the trend in consensus over the prior 30-60 days. Opposite-direction revisions are more likely to reflect genuinely new information.
- **Forecast boldness:** Following Clement and Tse (2005), bold forecasts (moving away from consensus) are more likely to reflect proprietary information, while herding forecasts (moving toward consensus) likely reflect interpretation of common information.
- **Deviation from consensus level:** The absolute distance between the analyst's new forecast and the current consensus. Larger deviations imply the analyst has information the market doesn't.

**Text-based approaches (using report/disclosure text):**
- **LLM classification:** Feed the report text along with the prevailing consensus to an LLM and ask it to classify whether the information is confirmatory, contradictory, or orthogonal (raises new dimensions). Ground the classification in observable benchmarks (consensus EPS, recent price action) rather than leaving it to pure LLM judgment.
- **Textual similarity:** Measure the semantic distance between the analyst report and recent public disclosures (earnings call transcripts, press releases, 8-K filings). High similarity suggests interpretation of existing information; low similarity suggests new information. Embedding-based similarity measures would be natural here.
- **Topic novelty:** Use topic modeling or keyword extraction to identify whether the report discusses topics not present in recent public disclosures about the firm. New topics are more likely to reflect discovery.

**Market-based approaches (using price/volume data):**
- **Price reaction asymmetry:** If information is truly new, the market reaction should be larger than for reports that repackage known information. Interact the newness measure with revision magnitude and test for differential market response.
- **Post-revision drift:** Genuinely new information should generate more follow-on activity (other analysts revising, continued price drift) as the market digests the new signal. This can serve as validation for the newness classification.

### Lesson 4: The Endogeneity of Information Production

Daniel et al. show that discovery is not random -- it is predicted by analyst characteristics (optimism, All-Star status, busyness), firm characteristics (information asymmetry), and regulatory regime (pre/post Reg FD). This means any study of new vs. old information must grapple with selection: the types of firms and analysts that produce new information differ systematically from those that do not.

**Implication for my project:**
- I need to think carefully about what drives the production or revelation of new information in my setting. If the decision to produce/reveal new information is endogenous, then comparing market reactions to new vs. old information will conflate the information effect with selection effects.
- Matching approaches, fixed effects (analyst-firm, firm-quarter), or instrumental variable strategies may be needed to isolate the effect of information newness from the characteristics of who produces it and when.
- The Reg FD natural experiment in Daniel et al. is a good model for how regulation can serve as a shock to information production patterns. If my setting involves a regulatory change or an exogenous shock that shifts the mix of new vs. old information, that would strengthen identification.

### Lesson 5: Validation is Critical

The paper's manual coding approach is its greatest strength and its greatest limitation. It's precise for the reports they read, but it's subjective and not scalable. The authors cannot demonstrate inter-coder reliability or apply it to larger samples.

**Implication for my project:**
- If I use LLM-based or NLP-based classification of information newness, I should validate against a hand-coded subsample. This gives me the best of both worlds: scalability from automation and credibility from human validation.
- I should report inter-rater agreement (between the LLM and human coders, or between multiple human coders) to establish that "newness" is being measured reliably.
- I should also show that my newness measure correlates with expected outcomes (e.g., stronger market reactions, more follow-on analyst activity) as a form of construct validation.

### Lesson 6: Decomposing the Market Reaction

Table 6 shows that discovery amplifies price target and EPS revisions but not recommendation changes. This suggests that the market's response to new information depends on how it is packaged and communicated.

**Implication for my project:**
- I should not assume that new information has a uniform effect on market outcomes. The channel through which new information reaches the market (forecast revision, qualitative discussion, recommendation change) likely matters.
- Testing for heterogeneity in the market reaction by information type, delivery channel, and content characteristics could be a meaningful contribution.
- The finding that discovery alone (without a revision) does not move markets is important: new information may only be valuable when it translates into a concrete, verifiable output that investors can act on.

### Research Design Sketch for My Project

Based on the lessons above, a potential research design for my project could follow this structure:

1. **Define "new information" precisely** -- choose a definition grounded in theory (e.g., information not previously reflected in consensus forecasts or public disclosures).

2. **Construct a newness measure** -- combine a quantitative measure (forecast boldness or deviation from consensus) with a text-based measure (semantic distance from recent public disclosures) for robustness.

3. **Validate the measure** -- show that the newness measure predicts stronger market reactions, more follow-on analyst activity, and larger post-revision drift. Hand-code a subsample to confirm LLM/NLP classification accuracy.

4. **Test main hypotheses** -- examine how market participants respond differentially to new vs. old information, and what moderates this differential response.

5. **Address endogeneity** -- use fixed effects to absorb analyst/firm heterogeneity, and if possible, identify an exogenous shock that shifts the production of new information.

6. **Explore content heterogeneity** -- decompose new information by whether it confirms vs. contradicts consensus, its topical focus, and its quantitative specificity. This directly addresses the content gap in Daniel et al.

---

## References Mentioned in Discussion

- Clement, M.B. and Tse, S.Y. (2005). Financial analyst characteristics and herding behavior in forecasting. *Journal of Finance*, 60, 307--341.
- Daniel, N.D., Lee, S.M., and Naveen, L. (2025). Information Discovery by Analysts. *Journal of Accounting, Auditing & Finance*, 40(4), 1350--1379.
