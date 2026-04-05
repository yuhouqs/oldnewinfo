#oldnewinfo

# Fedyk & Hodson (2023, JFE) - Discussion Notes

## Paper: "When Can the Market Identify Old News?"

**Authors:** Anastassia Fedyk (UC Berkeley Haas), James Hodson (Jozef Stefan Institute / AI for Good Foundation)
**Journal:** Journal of Financial Economics, Vol. 149, pp. 92-113
**Core question:** Why do markets react to old news, and what type of old news is hardest to identify?

---

## 1. Paper Summary

The paper proposes **correlation neglect** as the mechanism behind market overreactions to old news. The key distinction is between:

- **Reprints**: old news drawn from a single prior source
- **Recombinations**: old news that combines content from multiple prior sources

### Main findings:
- Even sophisticated finance professionals rate recombinations as significantly more novel than reprints (3.03 vs. 2.61 on a 7-point scale), despite both being equally "old"
- Recombination news triggers larger absolute abnormal returns and trading volumes than reprints
- These excess reactions fully reverse within a week, consistent with overreaction
- The recombination effect persists across news sentiment, ambiguity, and investor attention levels
- The effect has *increased* over time (from ~5 bps in 2001 to ~24 bps in 2014), even as reactions to simple reprints have declined

---

## 2. Experiment Design

- **Participants:** 155 active finance professionals recruited through HBS alumni network (Goldman Sachs, State Street, Citadel, Point72, etc.)
- **Task:** Rate novelty of 40 sequential news headlines (20 experimental about a fictitious firm + 20 real Reuters fillers)
- **Experimental headlines:** 10 novel, 5 reprints, 5 recombinations
- **Key control:** Reprints and recombinations designed to be identical on length, ordering, and total % of old content (both exactly 81.1% spanned). Only difference is information structure (one source vs. multiple sources)
- **Time pressure:** 10 seconds per headline to simulate real-world cognitive load
- **Incentives:** Top 5 performers received $50 bonuses
- **Results:** 68% of participants perceived recombinations as more novel than reprints; only 19% showed the reverse
- **Retail replication:** 776 retail investors showed even greater susceptibility to all old news, though the recombination-vs-reprint gap remained significant

---

## 3. Empirical Design - Key Methodological Points

### Firm-day level analysis (not article-level)
The authors do not try to attribute returns to individual articles. Instead, they aggregate all articles about firm *i* on date *t* into proportion-based measures:
- **PrcOld**: share of the day's articles classified as old news
- **PrcRecombination**: share classified as recombinations

This sidesteps the problem that many articles are released on the same day, making it impossible to attribute market reactions to any single article.

### Residualization of news measures
The raw PrcOld and PrcRecombination are mechanically related to news flow characteristics (article count, article length). To isolate genuine informational staleness from mechanical variation, the authors regress PrcOld and PrcRecombination on log article count, log average article length, and its square in daily cross-sections, and keep only the residuals (AbnPrcOld and AbnPrcRecombination).

**Note:** By the Frisch-Waugh-Lovell theorem, this is econometrically equivalent to including those controls directly in the final Fama-MacBeth regression. The two-step approach is chosen for interpretability and presentation economy, not for econometric necessity.

### Old news classification
- Extract unique stemmed words (unigrams) from each article
- Compare against the 5 most similar articles about the same firm in the preceding 3 days
- **Old news:** >= 60% of content spanned by preceding articles
- **Reprint:** among old news, >= 80% of old content from a single closest neighbor
- **Recombination:** among old news, < 80% of old content from the single closest neighbor
- Robust to alternative thresholds, look-back windows, number of comparison articles, and continuous measures

### Main regressions
Fama-MacBeth cross-sectional regressions of:
1. Same-day |AbnRet| and AbnVol on AbnPrcOld and AbnPrcRecombination (+ controls)
2. Subsequent [t+2, t+5] returns on day-t returns interacted with AbnPrcOld and AbnPrcRecombination (reversal test)

---

## 4. Investor Attention Measure

Uses Bloomberg's "News Heat - Daily Max Readership" from Ben-Rephael, Da, and Israelsen (2017):
- Security-day level measure based on the daily maximum of 8-hour rolling windows of news reads/searches
- Benchmarked against preceding 30 days for the same security
- Integer values 0-4 (0 = no spike above 80th percentile; 4 = above 96th percentile)
- Split: 0 (low attention, 61% of sample) vs. 1+ (high attention, 39%)
- Finding: high attention helps screen out reprints, but the recombination effect persists even under high attention

---

## 5. Ambiguity Measure

Follows Fedyk (2021), using supervised machine learning:
- **Training data:** 10,000 articles manually labeled by human experts for sentiment and hard/soft information
- **Features:** story length, topics, unigram/bigram/trigram indicators, syntactic complexity, syntactic/semantic patterns
- **Classifier:** Support Vector Machine (SVM)
- **Ambiguity score:** continuous 0-1, averaging (i) confidence of sentiment classification (distance from SVM hyperplane) and (ii) hard/soft information classification confidence
- Low ambiguity example: earnings report with specific numbers
- High ambiguity example: article about employee satisfaction

### Potential LLM upgrade
LLMs could significantly improve on this approach:
- No need for hand-labeled training data or engineered features
- Can assess ambiguity directly at the semantic level
- Can capture richer dimensions: hedging language, factual precision, strategic vagueness, source reliability
- The prompt itself serves as a transparent, reproducible codebook
- **Noted as a potential new project direction** (see Section 7 below)

---

## 6. Connection to My AnalystEdge Project

### Direct parallels
- Both projects ask how much of a financial text is genuinely new vs. recycled, and whether the market distinguishes correctly
- Analyst reports are inherently in the business of recombination (synthesizing earnings calls, filings, industry data, channel checks)
- If the Fedyk & Hodson cognitive limitation extends to analyst reports, analysts who merely repackage public information from multiple sources could still move prices

### Methodological contrast
- Fedyk & Hodson use word overlap (surface-level textual similarity)
- My HAID-inspired LLM approach operates at the semantic level, correctly identifying old information even when expressed in original language
- This contrast is useful framing for why an LLM-based approach is needed

### Applicable test
- The **return reversal test** translates directly: if analyst reports with high "old" content (per LLM classification) generate initial price reactions that subsequently reverse, that provides market-based evidence that investors are being fooled by repackaged information

### Ambiguity as moderating variable
- Analyst reports that are more ambiguous may amplify the old-vs-new confusion
- Can serve as a control or interaction variable in my design

---

## 7. New Project Idea: LLM-Based Ambiguity Measurement

**Inspiration:** Fedyk & Hodson's SVM-based ambiguity measure has clear limitations (small training set, lossy feature engineering). LLMs can directly assess ambiguity without hand-crafted features, potentially capturing richer dimensions.

**Possible dimensions to measure:**
- Hedging language
- Factual precision
- Strategic vagueness
- Source reliability
- Forward-looking uncertainty

**Key implementation notes:**
- Validate against human-labeled subsample
- Document model and prompt for reproducibility
- Test stability across prompt variants
- Could apply to both news articles and analyst reports

Try to work on it soon.

---

## 8. Key References from This Paper

- Tetlock (2011) - "All the news that's fit to reprint" - baseline old news study
- Ben-Rephael, Da & Israelsen (2017) - Bloomberg investor attention measure
- Fedyk (2021) - Ambiguity/sentiment classification methodology
- DeMarzo, Vayanos & Zwiebel (2003) - Persuasion bias / correlation neglect theory
- Enke & Zimmermann (2019) - Correlation neglect in belief formation
- Gilbert, Kogan, Lochstoer & Ozyildirim (2012) - Overreaction to summary statistics (LEI)
- Hirshleifer, Hsu & Li (2018) - Positive role of recombination in patent innovation (contrasting finding)
- Huberman & Regev (2001) - Classic NYT old news anecdote (330% price increase)

---

*Notes compiled from discussion on April 5, 2026*
