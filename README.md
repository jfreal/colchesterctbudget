# Colchester, CT Budget Trends Analysis

## Overview

This repository contains an 18-year budget analysis for the Town of Colchester, CT (FY 2007-08 through FY 2024-25), including comparisons to inflation, population growth, neighboring towns, and demographic peer towns across Connecticut.

**This research is source-controlled and designed to be publishable.** The `src/` folder contains a self-contained static website (`index.html`) that can be deployed to any web host, GitHub Pages, Netlify, or similar service for public sharing.

## Publishing

The `src/index.html` file is a fully self-contained interactive report — no build step, no dependencies to install. To publish:

- **GitHub Pages**: Push this repo, go to Settings > Pages, set source to `main` branch and `/src` folder.
- **Netlify / Vercel**: Point the publish directory to `src/`.
- **Any web server**: Just serve the `src/` folder.
- **Local preview**: Open `src/index.html` directly in a browser.

## Repository Structure

```
├── README.md                              Project overview (this file)
├── METHODOLOGY.md                         Data sources, extraction process, limitations
├── PROMPTS.md                             Prompts & instructions used to generate this analysis
├── .gitignore                             Files excluded from version control
│
├── src/                                   Publishable web report
│   └── index.html                         Interactive charts & analysis (10 Chart.js charts, sources)
│
├── Colchester_CT_Budget_Analysis.xlsx     Excel workbook (9 sheets, 754 formulas, 0 errors)
│
└── BudgetPdfs/                            Source data — 17 adopted budget PDFs
    ├── 07-08adoptedbudget.pdf
    ├── 08-09adoptedbudget.pdf
    ├── ...
    └── fy_24-25_adopted_budget.pdf
```

## Excel Workbook Sheets

1. **Budget Summary** — 18 years of budget totals (BOE, Town, Total, Mill Rate) with YoY formulas
2. **Year-over-Year Trends** — Percentage changes with revaluation years highlighted
3. **FY 2024-25 Detail** — Department expenditure and revenue breakdown
4. **Charts Data** — Pre-formatted data for charting
5. **Department Trends** — Department-level expenditure history, CAGR, and growth rankings
6. **Town vs BOE Growth** — Side-by-side Town Operating vs Board of Education comparison
7. **Inflation & Population** — Budget indexed vs CPI, population, and real per-capita spending
8. **Regional Comparison** — Mill rates and per capita spending for 7 neighboring towns
9. **Demographic Peers** — Comparison to CT towns with similar population, income, and character

## Key Findings

- **Budget growth (33.5%) is well below inflation (51.3%)** over 18 years
- **Real per-capita spending has declined 13.1%** since FY 2007-08
- **Mill rate (28.67) is 3rd lowest** among 7 neighboring towns
- **Lowest per capita spending ($3,979)** among demographic peer towns (peer avg: $4,334)
- **Annual growth rate (CAGR) of just 1.7%** vs average CPI of 2.7%

## Source Control Guidelines

This analysis should always be kept under version control:

- **Commit the source PDFs** (`BudgetPdfs/`) so the raw data is always available for verification
- **Commit the Excel workbook** as the primary data artifact — it contains all formulas for auditability
- **Commit `src/index.html`** as the publishable deliverable
- **Update `METHODOLOGY.md`** and `PROMPTS.md`** whenever data is refreshed or the analysis is extended
- When new fiscal year budgets are released, add the PDF to `BudgetPdfs/`, update the Excel, and regenerate `src/index.html`
