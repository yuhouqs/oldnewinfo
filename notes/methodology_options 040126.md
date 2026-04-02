#oldnewinfo

# Methodology Options: Identifying Old vs. New Information in Chinese Analyst Reports

Based on pilot experiments by co-author (Claude and GPT-5-3 tests on industry deep-dive and earnings review reports) and Bai et al. (2025) "Executives vs. Chatbots: Unmasking Insights through Human-AI Differences in Earnings Conference Q&A."

---

## Option 1: Benchmark Report Generation (HAID-adapted)

**Logic**: Instead of classifying sentences, generate what a "well-informed analyst" would have written given only public information, then measure the gap.

**How it works**:
1. For each analyst report, collect the public information set available before its publication date: the company's latest earnings announcement (from CNINFO), prior analyst reports on the same firm, recent news
2. Feed all of this to the LLM with a prompt like: "Based on the following publicly available information, write an analyst report covering [company] for [period]"
3. Compute semantic similarity (BERT/cosine) between the LLM-generated report and the actual report, at both the report level and section level
4. The dissimilarity score = new information content

**Strengths**: Sidesteps temporal verification entirely. Produces a clean continuous measure for regressions. Directly parallels Bai et al., so methodologically defensible.

**Weaknesses**: Requires assembling the pre-report information set for each report, which means programmatic access to CNINFO announcements and prior reports. The LLM-generated report is a "generic" benchmark, so it may miss firm-specific context that was actually public.

**Best for**: Large-sample empirical tests (does new information in analyst reports predict returns, volume, forecast revisions?).

---

## Option 2: Two-Stage Classification (LLM + Database Verification)

**Logic**: Let the LLM do what it's good at (structural decomposition and type classification), and use external databases for temporal verification.

**How it works**:
1. **Stage 1 (LLM)**: Decompose report into atomic sentences. Classify each sentence's information type: hard fact, causal interpretation, forecast/opinion, boilerplate. This does NOT require temporal knowledge.
2. **Stage 2 (Database check)**: For sentences classified as "hard fact," check against CNINFO/Wind whether the underlying data was publicly disclosed before the report date. For example, if the sentence says "H1 revenue was 58.22 billion," check if the mid-year report was filed before or after the analyst report.
3. **Stage 3 (Final label)**: Combine the two stages. Hard facts that were pre-disclosed = Old. Hard facts not yet disclosed = New. Interpretations/forecasts = Soft Information (never "New" regardless of novelty, per the three-tier framework).

**Three-tier framework** (from co-author's GPT experiment):
- **Hard New Information**: new facts (new orders, new tech, newly disclosed data)
- **Soft Information / Interpretation**: forecasts, ratings, causal explanations. These are f(existing information set), not new facts.
- **Pure Old Information**: financial data restated from filings, boilerplate

**Strengths**: Most precise sentence-level classification. Clear theoretical grounding (the "does it change the information set?" criterion). The hardest part (type classification) is what LLMs do well. The verification is mechanical and auditable.

**Weaknesses**: Requires structured access to CNINFO filing dates and potentially Wind data. Only works for verifiable facts; some "new" information (e.g., channel checks, proprietary data) may not appear in any database.

**Best for**: Building a gold-standard labeled dataset. Understanding the composition of new vs. old information across report types.

---

## Option 3: Prior-Report Delta (No External Database Needed)

**Logic**: New information is what appears in this report that was NOT in any prior report about the same company. Use text comparison across sequential reports.

**How it works**:
1. Collect all analyst reports for a given company in chronological order
2. For each report, compute sentence-level embeddings (using a Chinese BERT model like chinese-roberta-wwm-ext)
3. For each sentence in the focal report, find its maximum similarity to any sentence in prior reports (within a lookback window, e.g., 90 days)
4. Sentences with low maximum similarity to the prior report corpus = potentially new information
5. Aggregate to a report-level novelty score

**Strengths**: Purely mechanical, fully reproducible, no LLM judgment needed. No database access required beyond the reports themselves. Scales easily.

**Weaknesses**: "New to the analyst report corpus" is not the same as "new to the market." A fact could be well-known from news/announcements but never mentioned in prior analyst reports. Also, rephrased old information might score as "new" (though BERT embeddings handle paraphrasing reasonably well).

**Best for**: A clean, scalable first pass. Good as a complementary/robustness measure alongside one of the other approaches.

---

## Option 4: LLM-as-Informed-Reader

**Logic**: Adapt Bai et al.'s core insight for a written report setting. Instead of simulating an investor in a live call, simulate an informed reader who has already absorbed all public information. The LLM reads the report and flags what surprises it given its knowledge.

**How it works**:
1. Set the LLM's knowledge cutoff to the report's publication date (via system prompt)
2. Feed the LLM the report's metadata (company, date, report type) and all preceding public context you can gather (even just the prior quarter's earnings announcement)
3. Ask the LLM: "For each sentence, rate on a 1-5 scale how surprising/unexpected this information is, given everything a well-informed market participant would know before [date]. Explain your reasoning."
4. Instead of binary Old/New classification, collect a continuous "surprise" score
5. Validate: do reports with higher aggregate surprise scores predict stronger market reactions?

**Strengths**: Leverages the LLM's training knowledge as a proxy for "what the market knows" (which is exactly what Bai et al. argue). The continuous scale avoids the hard classification problem. The reasoning traces can be audited.

**Weaknesses**: Still subject to the temporal knowledge limitation, though framing it as "surprise" rather than "old/new" makes the LLM's errors less systematic. The knowledge cutoff instruction may not be perfectly enforced.

**Best for**: A pragmatic middle ground. Works well with the co-author's existing prompt framework with modest modifications. Can pilot quickly.

---

## Recommended Combination

For a publishable paper, combine **Options 1 and 2**:

- **Option 1 (HAID-style)** for the main empirical analysis: a scalable, continuous measure of information novelty to regress against market outcomes. This is the "big test" establishing that Chinese analyst reports contain measurable new information.
- **Option 2 (sentence-level classification)** for mechanism/descriptive analysis: showing where the new information lives (which sections, which report types), and validating the three-tier framework. This is the conceptual contribution.
- **Option 3 (prior-report delta)** as a robustness check: requires no LLM judgment at all, purely mechanical.

---

## Key References

- Bai, J., Boyson, N., Cao, Y., Liu, M., & Wan, C. (2025). Executives vs. Chatbots: Unmasking Insights through Human-AI Differences in Earnings Conference Q&A. SSRN 4480056.
- Co-author pilot experiments: Claude test on industry deep-dive (low-orbit satellites), GPT-5-3 test on earnings review (航天电子)
