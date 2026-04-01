#oldnewinfo

# Summary of Co-author's Pilot Experiment Documents

## Document 1: `Claude问答与行业专题试验.pdf` (74 pages)

**What it is:** An exported Claude conversation testing old/new information classification on a **commercial aerospace industry report** (行业专题, published 2025-08-10).

**Key content:**
- Designed a prompt system (System Prompt + User Prompt) to classify each sentence as **OLD / REINTERPRETED / NEW / UNCERTAIN**
- Tested on a full 32-page research report about low-orbit satellites and commercial rockets, covering sections: investment thesis, industry background, satellite constellations, rocket capacity gaps, individual stock analysis (铂力特, 九丰能源, 华曙高科, 高华科技), and risk warnings
- **Results:** ~141 sentences annotated. OLD 43%, REINTERPRETED 23%, NEW 20% (28 sentences), UNCERTAIN 14%
- **Key findings:**
  - NEW concentrates in individual stock analysis and capacity forecasting sections; background/technical sections are almost all OLD
  - Identified 6 NEW subtypes: real-time snapshot data, annual statistics, exclusive contracts, precise financials, scenario estimates, analyst aggregated estimates
  - Found 3 internal data contradictions (e.g., SpaceX 2024 launches cited as 134, 138, and 153 in different sections)
  - "根据我们测算" (per our estimates) is a reliable trigger word for NEW
- **Critical limitation discovered:** Claude has no web search capability -- it relies on training knowledge, so NEW/UNCERTAIN are **systematically overestimated** (some items labeled NEW were actually publicly available pre-report). The co-author concluded this is a fundamental design flaw of the web-based approach.

---

## Document 2: `GPT-业绩点评试验.pdf` (19 pages)

**What it is:** An exported ChatGPT (GPT-5-3) conversation testing classification on an **earnings review report** for 航天电子 (published 2025-08-28).

**Key content:**
- First, GPT split the report text into 70 minimum information unit sentences across 9 categories (financial facts, causal explanations, business structure changes, profitability metrics, cash flow, capabilities/tech, forecasts/ratings, risks)
- Then tested the classification prompt on each group
- **Results:** Financial data -> Unknown (can't confirm if mid-year report was pre-disclosed); Causal analysis -> Old_with_new_interpretation; Business structure data -> Unknown; Forecasts/ratings -> initially labeled New, then corrected
- **Critical insight:** Co-author challenged GPT: "Are forecasts always New?" GPT self-corrected and explained that **forecasts and ratings are NOT new information** -- they are `f(existing information set)`, i.e., analyst interpretations/opinions. Only when forecasts are based on undisclosed facts (e.g., from proprietary channel checks) do they qualify as New.
- Proposed a **three-tier framework** for the paper: (1) Hard New Information (new facts), (2) Soft Information/Interpretation (forecasts, ratings, causal explanations), (3) Pure Old Information

---

## Document 3: `4.1内容.docx`

**What it is:** A **methodology and planning document** summarizing the overall approach and all prompts.

**Key content:**
- **Task list** from the 3/26 discussion: classify report types, read 2023 ChatGPT literature, test AI prompts, manually verify, sync via WeChat
- **Full prompt designs** for both GPT and Claude (System Prompts, User Prompts, reinforcement instructions)
- **Detailed methodology pipeline:** text extraction -> sentence splitting (NLP) -> AI classification -> manual verification (5-10% sample) -> bias analysis
- **Key issues identified:**
  1. LLMs have no real "temporal database" -- they use training knowledge + reasoning, so NEW labels can be wrong
  2. Classification bias: "forecast language" does not equal new info; "specific-sounding" does not equal new info
  3. LLMs default to pattern-based judgment rather than knowledge-based judgment
  4. Time travel problem: LLM judgment crosses temporal boundaries
- **Pilot test findings:** Industry reports yield mostly OLD with some NEW in data/contracts; earnings reviews have lots of Unknown because financial data disclosure timing is ambiguous
- **Conclusion:** Web-based Claude/GPT cannot reliably do web search verification. The co-author plans to use **Claude Code with a Web Search Skill** to address this limitation.

---

## Overall Takeaway

The co-author has built a solid pilot testing framework, identified the core methodological challenges (no web search, forecast vs. fact distinction, temporal knowledge limitations), and is now looking to move from web-based manual testing to Claude Code with web search integration for more reliable classification.
