# Prompts & Instructions Used to Generate This Analysis

This document records the user prompts and Claude's approach for each stage of the analysis, so the work can be reviewed, reproduced, or extended.

---

## Session Overview

- **Tool**: Claude (Anthropic) via Cowork mode
- **Date**: February 18, 2026
- **Model**: claude-opus-4-6
- **Total conversation turns**: ~30+ across multiple context windows

---

## Prompt 1: Initial Budget Analysis

**User prompt:**
> I want to analyze all the budgets for the town of Colchester, CT. The budgets are located here: https://www.colchesterct.gov/finance-department/pages/adopted-budget

**Clarifying questions asked (via AskUserQuestion tool):**
- Analysis type → User selected: "Year-over-year trends"
- Time range → User selected: "All available years"
- Output format → User selected: "Excel spreadsheet (Recommended)"

**What Claude did:**
1. Attempted to fetch the webpage directly (403 error). Used WebSearch to find budget PDF links.
2. Downloaded 17 budget PDFs (FY 2007-08 through FY 2024-25) into `BudgetPdfs/`.
3. Extracted budget summary data from each PDF using `pdfplumber` (Python), targeting "Budget In Brief" and "Budget Summary & Mill Rate Calculation" pages.
4. Built the initial Excel workbook with sheets: Budget Summary, Year-over-Year Trends, FY 2024-25 Detail, Charts Data.
5. Verified all formulas with LibreOffice recalculation (zero errors).

**Key technical decisions:**
- Used targeted page extraction rather than full-PDF parsing for accuracy
- Combined "Human Services" + "Civic & Cultural" into "Community & Human Services" for pre-FY 12-13 consistency
- Calculated FY 2023-24 BOE budget by subtraction: Total - Town = BOE

---

## Prompt 2: Department Growth Research

**User prompt:**
> can you research which departments or areas grew the most?

**What Claude did:**
1. Extracted department-level expenditure data from all 17 PDFs using "Budget Summary by Function" tables.
2. Compiled data for 6 departments: General Government, Public Safety, Public Works, Community & Human Services, Debt Service, Transfers/Capital, plus BOE.
3. Added two new Excel sheets: "Department Trends" and "Town vs BOE Growth".
4. Calculated CAGR, total % change, and YoY % change for each department.

**Key findings:**
- Public Works: 4.7% CAGR (+117.7% total) — fastest growing
- Debt Service: -4.5% CAGR (-54.6%) — declined significantly
- BOE: 1.9% CAGR — largest dollar increase ($12.3M)

---

## Prompt 3: Charts & Inflation/Population Comparison

**User prompt:**
> can you create some charts to explain the trends in spending? my goal is to paint a picture that spending is pretty normal and undercontrol. can you also compare the spending rate to inflation and possibly population growth or decline?

**What Claude did:**
1. Researched CPI inflation data (BLS annual averages 2007-2024) via WebSearch.
2. Researched Colchester population data (CT DPH estimates, Census data) via WebSearch.
3. Computed indexed growth comparisons (all indexed to 100 in FY 07-08):
   - Budget: 100 → 133.5 (+33.5%)
   - CPI: 100 → 151.3 (+51.3%)
   - Population: 100 → 101.7 (+1.7%)
   - Real per-capita spending: 100 → 86.9 (-13.1%)
4. Created interactive HTML page (`Colchester_Budget_Charts.html`) with 6 Chart.js charts.
5. Added "Inflation & Population" sheet to Excel workbook.

**Narrative framing (per user's goal):**
- Budget grew LESS than inflation (33.5% vs 51.3%)
- Real per-capita spending DECLINED 13.1%
- Annual growth rate of 1.7% is well below CPI average of 2.7%
- In 13 of 17 years, budget increase was at or below inflation

---

## Prompt 4: Neighboring Town Comparison

**User prompt:**
> can you also research the mill rates and budget growth from other surrounding towns for similar time periods? I want to know if colchester stands out from it's neighbors

**What Claude did:**
1. Identified Colchester's 6 neighboring towns: East Hampton, Hebron, Salem, Marlborough, East Haddam, Lebanon.
2. Searched for FY 2024-25 mill rates for each town via WebSearch (CT Open Data Portal, town websites, news sources).
3. Searched for 2024 population estimates from CT DPH.
4. Compiled per capita budget data where available.
5. Added "Regional Comparison" sheet to Excel and Charts 7-8 to HTML.

**Mill rates found (FY 2024-25):**
- Lebanon: 21.00, East Haddam: 26.76, Colchester: 28.67, Salem: 30.20, Hebron: 34.50, Marlborough: 36.29, East Hampton: 38.04

**Key finding:** Colchester is 3rd lowest among 7 neighbors, 8% below neighbor average.

---

## Prompt 5: Demographic Peer Comparison

**User prompt:**
> what about CT towns with similar demographics

**What Claude did:**
1. Researched Colchester's demographic profile: pop ~15,752, median HH income $118,839, suburban-rural character.
2. Identified peer towns with similar population (12K-18K) and income ($100K-$135K): Ellington, Wolcott, Tolland, Ledyard, Suffield, East Hampton, Plainfield.
3. Searched for mill rates and budget totals for each peer.
4. Compiled per capita spending comparison.
5. Added "Demographic Peers" sheet to Excel and Chart 9 to HTML.

**Key finding:** Colchester has the lowest per capita spending ($3,979) among all peers with available data (peer avg: $4,334).

---

## Prompt 6: Per Capita Peer Spending & Sources

**User prompt:**
> what about spending per capita per peer towns, also can you list all your sources at the bottom of the html document

**What Claude did:**
1. Searched for total budget figures for all peer towns.
2. Found confirmed budgets for: Ellington ($71.2M), East Hampton ($56.7M), Ledyard (~$67.4M), Tolland ($63.7M).
3. Calculated per capita spending for each.
4. Added Chart 10 (per capita peer comparison with peer average line) to HTML.
5. Added comprehensive "Sources" section to HTML with categorized, linked citations.
6. Updated Excel "Demographic Peers" sheet with per capita comparison table.

---

## Prompt 7: Documentation for Source Control

**User prompt:**
> can you write any instructions and prompts used to generate this report into the folder so I can check it into source control

**What Claude did:**
1. Created `README.md` — project overview, file descriptions, key findings.
2. Created `METHODOLOGY.md` — data sources, extraction process, known limitations, reproducibility notes.
3. Created `PROMPTS.md` (this file) — full record of user prompts and Claude's approach at each stage.

---

## Tools & Libraries Used

- **Python libraries**: `pdfplumber` (PDF text/table extraction), `openpyxl` (Excel creation/editing), `json` (data serialization)
- **JavaScript library**: Chart.js 4.4.1 (interactive charts via CDN)
- **LibreOffice**: Formula recalculation and verification (via headless mode)
- **Web research**: WebSearch and WebFetch tools for CPI, population, mill rate, and peer town data

## Data Verification Steps

1. Every PDF extraction was cross-verified against prior-year references in subsequent budget documents
2. Mill rates were verified against both "Budget In Brief" and "Mill Rate Calculation" pages
3. All Excel formulas were recalculated via LibreOffice at each stage (final: 754 formulas, 0 errors)
4. Per capita calculations were independently verified via Python
5. CPI indexed values were verified: base year = 100, each subsequent year = prior × (1 + rate/100)
