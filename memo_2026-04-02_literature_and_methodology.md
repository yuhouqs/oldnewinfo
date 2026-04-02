#oldnewinfo

# Research Memo — April 2, 2026

## Topic: Literature on "New Information" in Analyst Reports & Methodology Discussion

---

## 1. Literature Review: What Constitutes "New Information" in Analyst Reports?

### The Consensus

The academic literature does NOT define "new information" narrowly as only information that has never been publicly released. Instead, it recognizes a **spectrum**:

1. **Narrowest (pure discovery):** Genuinely private, previously unknown facts — channel checks, proprietary data, private management meetings. Daniel et al. (2025) find this in 17% of reports; Huang et al. (2018) call these "exclusive topics" (31% of content).
2. **Intermediate (mosaic/synthesis):** Novel conclusions derived from combining individually non-material public and private fragments — the mosaic theory.
3. **Broadest (interpretation/processing):** Skilled reinterpretation, clarification, and quantification of publicly available information. Womack (1996) shows this drives most of the price impact of analyst recommendations.
4. **Operationally (price impact):** In the Grossman-Stiglitz (1980) and market microstructure traditions, anything that generates significant abnormal returns is de facto "informative."

### Two Recognized Analyst Roles

- **Information Discovery:** Analysts produce genuinely new information not previously available to investors (Huang et al. 2018: 31% of content; Daniel et al. 2025: 17% of reports).
- **Information Interpretation:** Analysts add value by processing, clarifying, and reinterpreting publicly available information (Livnat & Zhang 2012; Womack 1996).

### Key References

| Paper | Journal | Key Contribution |
|---|---|---|
| Asquith, Mikhail & Au (2005) | JFE | First detailed taxonomy of report components |
| Womack (1996) | JF | Price impact even without new facts |
| Huang, Lehavy, Zang & Zheng (2018) | Management Science | Discovery vs. interpretation via LDA topic modeling (159K reports) |
| Livnat & Zhang (2012) | RAS | Investors value interpretation role |
| Huang, Zang & Zheng (2014) | The Accounting Review | Textual content informativeness |
| Brown, Call, Clement & Sharp (2015) | JAR | Survey of 365 analysts on information sources |
| Daniel, Lee & Naveen (2025) | JAAF | Hand-coded discovery prevalence (3,553 reports) |
| Frankel, Kothari & Weber (2006) | JAE | Determinants of informativeness |
| Bradley, Gokkaya & Liu (2017) | JF | Industry experience and information quality |
| Tetlock (2011) | RFS | Stale vs. new information and price reversal |
| Fedyk & Hodson (2023) | JFE | Recombined old information fools markets |
| Grossman & Stiglitz (1980) | AER | Impossibility of fully efficient markets |

---

## 2. Deep Dive: Daniel, Lee & Naveen (2025, JAAF) — "Information Discovery by Analysts"

### Summary

Hand-read summaries of 3,553 analyst reports to classify as discovery or interpretation. Found 17.4% contain discovery — 79.4% from management sources (personal meetings, conference calls, analyst meetings), 20.6% from non-management sources (channel checks, surveys, industry contacts). Reports with EPS or price target changes generate stronger market reactions when accompanied by discovery.

### Why It Didn't Make a Top Journal

**Biggest concern: Discovery has no significant direct effect on market reactions.** The coefficient on Discovery alone (beta_1) is insignificant in every specification (Table 6: ranges from -0.151 to 0.180). The unconditional ACARs are identical for discovery and interpretation reports (both 4.24%). Results only emerge through interaction terms (Discovery x Change in Price Target, Discovery x Change in EPS).

Additional concerns:
- **Small sample:** 3,553 reports from 176 firms — tiny fraction of the analyst report universe
- **Questionable classification choices:** Conference calls classified as "discovery" in baseline, but these are public events
- **No clean identification strategy:** No quasi-experiment or instrumental variable approach
- **Incremental to Huang et al. (2018):** That paper already addressed discovery vs. interpretation with a 45x larger sample using topic modeling

### Takeaway for Our Project

The fundamental tradeoff this paper illustrates — accuracy of classification (hand-reading) vs. scale (automated methods) — is precisely what our LLM approach could resolve.

---

## 3. Deep Dive: Fedyk & Hodson (2023, JFE) — "When Can the Market Identify Old News?"

### Summary

Two-part design: (1) experiment on 155 finance professionals showing they can't distinguish recombined old information from new information; (2) empirical analysis of 17 million Bloomberg articles showing recombination news generates larger price moves that subsequently reverse.

### How They Differentiate New vs. Old Information

They use a **bag-of-words approach** — purely textual/mechanical:
1. Extract unique words, remove stop words, stem using Porter (1980)
2. Look back at articles about the same firm in preceding 3 days
3. Compute textual overlap with 5 most similar preceding articles
4. Classify: **Novel** (>=40% words not seen before), **Reprint** (<40% novel AND >=80% spanned by single prior article), **Recombination** (<40% novel but NOT spanned by any single article)

**Critical limitation:** This measures whether *words* have appeared before, not whether the underlying *facts* were previously known.

### Key Findings

- Old news: 11.5 bps smaller absolute abnormal returns per 10% increase
- Recombinations: 17.6 bps LARGER returns than reprints (markets fooled)
- Overreaction to recombinations fully reverses within 5-10 trading days
- Effect strengthened over time: +5 bps in 2001 to +24 bps in 2014
- Investors got better at screening reprints but worse at screening recombinations

---

## 4. LLM vs. Bag-of-Words: Advantages of Our Approach

| Dimension | Bag of Words (Fedyk & Hodson) | LLM (Our Approach) |
|---|---|---|
| **Semantic understanding** | None — treats synonyms as different info | Understands paraphrase and meaning |
| **Contextual judgment** | Only asks "did these words appear recently?" | Reasons about whether facts were publicly known |
| **Information structure** | All words treated equally | Distinguishes facts vs. opinions vs. boilerplate vs. forecasts |
| **Granularity** | Article-level classification | Sentence-level classification |
| **Threshold dependence** | Arbitrary cutoffs (40%, 80%, 3-day window) | Holistic judgment without hard cutoffs |
| **Cross-document reasoning** | Only compares to same-firm articles | Can draw on broad world knowledge |
| **Chinese language** | Especially problematic (same concept, different characters) | Handles synonyms and reformulations naturally |

**One honest caveat:** Bag-of-words is fully transparent and replicable. LLM is a black box — requires validation protocol (5-10% manual sample + bias analysis).

---

## 5. Alternative Method: Comparing Firm Financial Reports to Analyst Reports

### Concept

Analyst Report Content - Firm Disclosure = {Other Public Info} + {Private/New Info}

Information present in analyst reports but absent from the firm's own financial disclosures is potentially new to investors.

### Justifications

1. **Semi-strong EMH baseline:** Firm filings are the most incorporated public information; content beyond them has highest probability of being incrementally informative
2. **Analyst as information intermediary:** Directly operationalizes the theory that analysts add value by going beyond firm disclosures
3. **Regulatory disclosure framework:** Firms must disclose all material info through official channels; analyst additions represent value-added

### Pros

- Clear, observable, replicable benchmark (concrete document comparison)
- Strong academic precedent (Huang et al. 2018; Chen, Cheng & Lo 2010)
- Works naturally with NLP/LLM tools (both documents are text-based)
- Captures full analyst value-added without needing to categorize the source
- Well-suited to Chinese context (standardized 年报, 季报, 业绩快报, 业绩预告)

### Cons

- **Over-counts "new" information (biggest problem):** Much info is public but not firm-sourced (government statistics, industry data, competitor filings, news, prior analyst reports). These would be falsely flagged as "new"
- **Under-counts interpretive additions:** Analyst spin on firm-reported facts (e.g., "revenue of 5.2B missed our estimate by 8%") would be matched as "old" despite containing new analysis
- **Timing and document matching complexity:** Need to construct cumulative disclosure set up to analyst report date
- **Format mismatch:** Financial reports are numerical/tabular; analyst reports are narrative
- **Binary classification only:** Loses the four-category granularity (OLD/REINTERPRETED/NEW/UNCERTAIN)

### Recommendation: Use as Complementary Method

Combine with LLM classification for triangulation:
1. LLM classifies each sentence as OLD/REINTERPRETED/NEW/UNCERTAIN
2. Separately flag whether each sentence's content appears in firm's financial report
3. Validate: LLM-classified NEW sentences should be disproportionately absent from firm filings
4. Identify edge cases: LLM-classified OLD sentences absent from firm filings = "other public information" that pure comparison would misclassify

This cross-validation addresses weaknesses of both methods and would be compelling to reviewers.

---

## Summary of Methodological Options

| Method | Strength | Weakness | Best Use |
|---|---|---|---|
| Bag of words / textual overlap | Transparent, scalable | No semantic understanding | Baseline/benchmark |
| Hand-reading reports | Accurate classification | Doesn't scale | Gold standard validation |
| LLM classification | Semantic + scalable | Black box, needs validation | Primary classification |
| Firm report comparison | Observable benchmark, replicable | Over-counts, binary only | Complementary validation |
| **Combined LLM + firm comparison** | **Cross-validates, addresses both weaknesses** | **More complex pipeline** | **Recommended approach** |
