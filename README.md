<h1 align="center">🚀 Startup Funding & Bankruptcy Analysis</h1>
<p align="center">A data-driven view of which industries, regions, and investors produce durable startups — and which don't.</p>

<p align="center">
  <img src="https://img.shields.io/badge/SQL-MySQL_8-12395B?style=flat-square&logo=mysql&logoColor=white" />
  <img src="https://img.shields.io/badge/Rows-56,740+-12395B?style=flat-square" />
  <img src="https://img.shields.io/badge/Tables-5-12395B?style=flat-square" />
  <img src="https://img.shields.io/badge/Status-Complete-C08A2E?style=flat-square" />
</p>

---

## 📋 Executive Summary

This project analyzes a global dataset of **10,500 startups**, their full funding history, and their eventual outcomes, to answer a question investors and founder-support teams ask constantly: **which parts of the startup ecosystem actually produce durable companies, and which combinations of industry, funding, and investor tend to end in failure?**

The dataset shows a healthier picture than headlines usually suggest. Roughly **three in four startups reach a durable outcome** — staying active, being acquired, or going public. But that resilience isn't evenly spread: capital is concentrated in a handful of industries, and a specific subset of investors carries repeat exposure to companies that later went bankrupt.

| | | |
|---|---|---|
| **75%** | Startup survival rate | Active, Acquired, or IPO |
| **$1.94T+** | Total capital tracked | Across all funding rounds |
| **1,625** | Bankruptcy cases | Each traceable to its investors |

**Bottom line:** the data supports weighting new capital toward industries that combine strong funding with strong survival, and treating repeat investor exposure to bankrupt startups as a standing due-diligence flag rather than a one-off observation.

---

## 🎯 Business Problem

Investors, accelerators, and founder-support teams face the same recurring question: which industries, regions, and investor types actually produce durable startups — and which combinations tend to end in failure? Without a structured way to connect funding history to outcomes, that insight stays anecdotal. Capital gets allocated on hype cycles rather than evidence, and investors have no systematic way to flag their own exposure to high-risk bets.

This project treats that gap as a data problem: model the full lifecycle of a startup — founding, every funding round it raises, and its eventual outcome — in a relational database, then answer the questions a VC analyst or founder-support team would actually ask.

### Objective

- Quantify total capital deployed and break it down by industry to see where the market is placing its bets.
- Measure the startup survival rate against the closure (bankruptcy/inactive) rate as a headline health KPI.
- Identify the top-funded startups within each industry to spot capital concentration.
- Trace which investors were exposed to startups that later went bankrupt — a risk signal for due diligence.
- Track cumulative and round-over-round funding growth per startup to model momentum over time.

---

## 🗂️ The Dataset

The analysis is built on a 5-table relational model covering 10,500 startups founded between 2005 and 2022, across 15 industries and 15 countries. Every funding round, every investor, and every closure is linked back to the startup it belongs to, so any question about outcomes can be traced back to the capital and investors behind it.

| Table | Rows | What it holds |
|---|---|---|
| `startups` | 10,500 | Industry, country, region, city, founded year, status |
| `investors` | 500 | Investor name, type, and country |
| `funding` | 33,615 | Every funding round raised, with date, round type, and amount |
| `bankruptcy` | 1,625 | Bankruptcy date, reason, and funding raised before closure |
| `closure_reason` | 10,500 | The reason behind every startup's current status |

**Coverage:** 15 industries · 15 countries · 8 global regions · founded 2005–2022 · $1.94T+ in total funding tracked

### Data Model

`startups` sits at the center of the model. `funding` connects startups and investors; `bankruptcy` and `closure_reason` both extend `startups` to capture why a company left the active pool.

<p align="center">
  <img src="images/er-diagram.png" alt="Entity Relationship Diagram" width="700"/>
</p>
<p align="center"><i>Figure 1 — Database schema</i></p>

---

## 📊 Key Insight — Where the Capital Goes

<p align="center">
  <img src="images/funding-by-industry.png" alt="Total funding by industry" width="650"/>
</p>
<p align="center"><i>Figure 2 — Total funding raised by industry</i></p>

CleanTech, PropTech, and BioTech lead total funding raised — each pulling in well over $130B across the dataset, ahead of more "hyped" categories such as AI/ML. This is a signal for where the market has historically placed conviction bets, rather than where attention alone has gone.

## 📊 Key Insight — Survival vs. Closure

<p align="center">
  <img src="images/status-distribution.png" alt="Startup status distribution" width="480"/>
</p>
<p align="center"><i>Figure 3 — Startup status distribution (n = 10,500)</i></p>

Roughly three in four tracked startups reach a durable outcome — Active (45%), Acquired (~20%), or IPO'd (~10%). The remaining quarter closes down: about 15% file for bankruptcy and 10% go quietly inactive. That closure rate is where investor risk and industry weakness concentrate, and it's the part of the picture worth watching most closely.

### Key Insight — Investor Exposure

Connecting `investors` → `funding` → `startups` → `bankruptcy` surfaces exactly which investors backed a company that later filed for bankruptcy. This turns a vague reputational hunch into a concrete, repeatable check that can be re-run on every new funding round.

---

## ✅ Conclusion

The data supports a fairly reassuring headline: the majority of tracked startups reach a durable outcome rather than failing outright. But survival is not evenly distributed. Capital concentration is heaviest in CleanTech, PropTech, and BioTech, and an identifiable subset of investors carries disproportionate exposure to startups that eventually went bankrupt.

In practical terms, this analysis turns a static, one-time question — "how are our startups doing?" — into something that can be recomputed on demand: total funding, survival rate, closure rate, top performers per industry, and investor risk exposure, all traceable back to the underlying data as new rounds and outcomes are recorded.

## 💡 Recommendations

- Weight new capital allocation toward industries showing both high funding density and high survival — not most-funded alone.
- Flag investors with repeat exposure to bankrupt startups for extra due diligence on new deals.
- Track round-over-round funding growth as an early warning signal — a startup with flattening or negative growth is a candidate for closer monitoring.
- Extend the model with post-bankruptcy outcome data, such as founder re-entry or asset acquisition, to move from a lagging report toward a predictive one.

---

## ▶️ How to Run This Project

```bash
# 1. Clone the repo
git clone https://github.com/RaxitPansuriya03/startup-funding-bankruptcy-analysis.git
cd startup-funding-bankruptcy-analysis

# 2. Run the analysis queries against your own `startup` database
#    (schema: startups, investors, funding, bankruptcy, closure_reason
#     — see the ER diagram above for structure)
mysql -u root -p startup < analysis_queries.sql
```

Open `analysis_queries.sql` directly in **MySQL Workbench** / **DBeaver** to run the queries step by step.

*(The full synthetic dataset — 56,740+ rows across 5 tables — is kept private and isn't included in this repo. Only the schema structure, queries, and result-level insights are shared here.)*

---

## 📁 Repository Structure

```
startup-funding-bankruptcy-analysis/
├── README.md                  ← you are here
├── analysis_queries.sql       ← all 11 analytical queries, cleaned & commented
└── images/
    ├── er-diagram.png
    ├── funding-by-industry.png
    └── status-distribution.png
```

---

## 👤 Author

**Raxit Pansuriya**
Data Analyst | SQL · Python · Power BI · Tableau
🔗 [GitHub](https://github.com/RaxitPansuriya03) · [LinkedIn](https://linkedin.com/in/raxitpansuriya)

---

<p align="center"><i>⭐ If this project helped you understand window functions or KPI-style SQL analysis, consider starring the repo!</i></p>
