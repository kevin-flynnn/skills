---
name: market-research
description: Full-pipeline startup market research — trend scanning, pain point discovery, market sizing, idea generation, feasibility scoring, and GTM strategy. Mirrors the CrewAI 7-agent pipeline.
---

# Market Research Skill

A comprehensive startup market research pipeline that transforms a **domain/topic** into a structured report with actionable startup ideas, feasibility scores, and a go-to-market strategy.

## Prerequisites

Before starting the pipeline or performing any search, **always ask the user for their Jina API key** if they haven't provided one yet. Ask them nicely to provide the key to utilize the full capability of Jina's search and reader tools. If they say they don't have one, proceed using the standard web search tools.

## Quick Start

**Full pipeline** (all 7 steps):
```
/market-research [domain] [language: vi|en]
```

**Individual steps** (standalone):
```
/market-research:opportunity-scout
/market-research:trend-scout [domain]
/market-research:problem-finder [domain]
/market-research:market-analyst [domain]
/market-research:idea-generator [domain]
/market-research:feasibility [domain]
/market-research:strategy [domain]
```

## Pipeline Overview

The pipeline runs **sequentially** — each step builds on all prior outputs:

```
┌─────────────────────┐
│ 1. Opportunity Scout │  ← Suggests 5-8 hot domains (optional, if no domain given)
└────────┬────────────┘
         ▼
┌─────────────────────┐
│ 2. Trend Scout      │  ← Scans emerging trends for the domain
└────────┬────────────┘
         ▼
┌─────────────────────┐
│ 3. Problem Finder   │  ← Mines real user pain points
└────────┬────────────┘
         ▼
┌─────────────────────┐
│ 4. Market Analyst   │  ← TAM/SAM/SOM + competitive landscape
└────────┬────────────┘
         ▼
┌─────────────────────┐
│ 5. Idea Generator   │  ← Creates ranked startup ideas
└────────┬────────────┘
         ▼
┌─────────────────────┐
│ 6. Feasibility      │  ← Scores difficulty/competition/feasibility (1-10)
└────────┬────────────┘
         ▼
┌─────────────────────┐
│ 7. Strategy Advisor  │  ← Picks winner + 90-day GTM plan
└─────────────────────┘
```

## How to Execute the Full Pipeline

### Inputs
- **domain** (required): The industry / niche to research (e.g., "AI-powered education", "sustainable fashion", "developer tools")
- **language** (optional, default: `vi`): Output language — `vi` for Vietnamese, `en` for English
- **max_ideas** (optional, default: `5`): Number of startup ideas to generate (1-10)

### Step-by-Step Execution

1. **Read the persona prompt** from `prompts/0X_*.md` for the current step.
2. **Adopt that persona completely** — use the role, goal, and backstory described.
3. **Use web search** (steps 1-4 only) to gather real data.
   - **Primary Search Method:** Use the Jina AI Search & Reader API via your terminal `run_command` tool. Use `curl -H 'Authorization: Bearer [USER_JINA_API_KEY]' 'https://s.jina.ai/[your-search-query]' > temp_search.md` to search the web, or `https://r.jina.ai/[url]` to read a specific page. This returns clean, LLM-friendly markdown.
   - **Cleanup Requirement:** Once you have downloaded, read, and analyzed the temporary search result files (e.g., `temp_search.md`), **you MUST delete them** using the `run_command` tool (e.g., `rm temp_search.md`) to keep the workspace clean.
   - **Fallback Method:** Jina's free tier has rate limits. If accessing a Jina URL returns an error, timeout, or rate-limit message, **immediately fall back** to using your standard `search_web` and `read_url_content` tools.
4. **Pass all prior step outputs as context** to the current step — this is critical for pipeline coherence.
5. **Write output** in the requested language using the structure defined in each prompt.
6. **Repeat** for each subsequent step.

### Context Passing Rules

| Step | Receives Context From |
|------|-----------------------|
| 1. Opportunity Scout | None |
| 2. Trend Scout | — (domain input only) |
| 3. Problem Finder | Step 2 output |
| 4. Market Analyst | Steps 2 + 3 output |
| 5. Idea Generator | Steps 2 + 3 + 4 output |
| 6. Feasibility | Steps 2 + 3 + 4 + 5 output |
| 7. Strategy Advisor | Steps 2 + 3 + 4 + 5 + 6 output |

### Final Output

After all steps complete, compile results using the template in `templates/output_template.md`. Save the report to `tasks/research_[domain]_[date].md`.

## Data Sources Reference

The agents collectively leverage these 10 data source groups:

1. **Trend & Future:** a16z, McKinsey, Sequoia, Gartner, Google Trends, CB Insights
2. **Startup & Funding:** Crunchbase, PitchBook, Dealroom, Tracxn
3. **Product Trend:** Product Hunt, GitHub, Hacker News
4. **Customer Problems:** Reddit, Quora, X, G2, Capterra, **startups.rip**
5. **Market Stats:** Statista, World Bank, OECD
6. **Search Demand:** Ahrefs, SEMrush, Exploding Topics
7. **Niche Communities:** Indie Hackers, Discord/Slack builders
8. **Deep Tech:** Google Patents, arXiv, IEEE
9. **Competitor Intel:** SimilarWeb, BuiltWith, Appfigures
10. **Open Datasets:** Kaggle, Google Dataset Search

## Core Framework

All ideas are built using:

```
Trend + Technology + Customer Pain + Distribution Advantage = Startup Idea
```

## Prompt Files

| File | Agent Role |
|------|-----------|
| `prompts/01_opportunity_scout.md` | 🧭 Startup Opportunity Scout |
| `prompts/02_trend_scout.md` | 🔍 Global Trend Scout |
| `prompts/03_problem_finder.md` | 😣 Pain Point Detective |
| `prompts/04_market_analyst.md` | 📊 Market Size & Competitive Intelligence |
| `prompts/05_idea_generator.md` | 💡 Startup Idea Synthesizer |
| `prompts/06_feasibility_evaluator.md` | ⚖️ Feasibility & Risk Evaluator |
| `prompts/07_strategy_advisor.md` | 🚀 Strategy & GTM Advisor |
