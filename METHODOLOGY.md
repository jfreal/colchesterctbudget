# Colchester, CT Budget Analysis — Methodology

## Overview

This analysis examines 18 years of adopted budget data for the Town of Colchester, Connecticut (FY 2007-08 through FY 2024-25) to assess whether spending has been fiscally responsible. It covers the overall town budget, town government operations, and the Board of Education (BOE).

## Data Sources

### Primary Sources

1. **Colchester Adopted Budget PDFs (17 files)**
   - Source: https://www.colchesterct.gov/finance-department/pages/adopted-budget
   - Files: `BudgetPdfs/07-08adoptedbudget.pdf` through `BudgetPdfs/fy_24-25_adopted_budget.pdf`
   - Note: FY 2023-24 budget PDF was not available; some values for that year are interpolated
   - Extraction method: Python `pdfplumber` library for text extraction from budget detail pages

2. **CT EdSight FTE Staffing CSV Reports (19 files)**
   - Source: https://public-edsight.ct.gov/educators/fte-staffing
   - Files: `StaffLevels/FTEStaffing.csv` through `StaffLevels/FTEStaffing (18).csv`
   - Coverage: FY 2001-02 through FY 2025-26 (though only FY 2008-09 onward used in charts)
   - Format: First line is "FTE Staffing Report for YYYY-YY", then CSV with columns: District, School, Assignment Category, Educator Type, FTE Count
   - Important: These are school-level only — central office administrators (superintendent, directors) are NOT included in EdSight data

3. **U.S. Bureau of Labor Statistics — Consumer Price Index (CPI)**
   - Source: https://www.bls.gov/cpi/
   - Used: Annual average CPI-U (All Urban Consumers, U.S. City Average)
   - Annual CPI rates used:
     ```
     2007: 2.85%, 2008: 3.84%, 2009: -0.36%, 2010: 1.64%, 2011: 3.16%,
     2012: 2.07%, 2013: 1.46%, 2014: 1.62%, 2015: 0.12%, 2016: 1.26%,
     2017: 2.13%, 2018: 2.44%, 2019: 1.81%, 2020: 1.23%, 2021: 4.70%,
     2022: 8.00%, 2023: 4.12%, 2024: 2.94%
     ```
   - Cumulative CPI inflation FY 2007-08 to FY 2024-25: 51.3%

4. **CT Open Data Portal — Mill Rates**
   - Source: https://data.ct.gov/Local-Government/Mill-Rates-for-FY-2014-2026/emyx-j53e
   - Used for: Colchester and neighboring/peer town mill rate comparisons

5. **Population Data**
   - Source: CT Department of Public Health / U.S. Census Bureau Population Estimates Program
   - URL: https://portal.ct.gov/dph/health-information-systems--reporting/population/annual-town-and-county-population-for-connecticut
   - Colchester values (some years interpolated):
     ```
     2007: 15,495  2008: 15,578  2009: 15,838  2010: 16,068  2011: 16,087
     2012: 16,087  2013: 16,050  2014: 16,000  2015: 15,950  2016: 15,900
     2017: 15,850  2018: 15,800  2019: 15,861  2020: 15,555  2021: 15,501
     2022: 15,600  2023: 15,690  2024: 15,752
     ```

### Secondary/Verification Sources

- **NCES Common Core of Data** — Colchester School District (ID 0900840): https://nces.ed.gov/ccd/districtsearch/district_detail.asp?ID2=0900840
- **Ballotpedia** — Colchester Public Schools: https://ballotpedia.org/Colchester_Public_Schools,_Connecticut
- **GovSalaries.com** — Colchester BOE employee salary data (244 employees, 2024)
- **Niche.com** — Average teacher salary ($82,554), student-teacher ratio
- **ConnecticutTeach.org** — CT statewide average teacher salary ($86,511)
- **CT SDE Press Release PR-102** — Statewide staffing increase data: https://portal.ct.gov/sde/press-room/press-releases/2023/pr-102-increase-school-staffing
- **CT Mirror** — ESSER funding cliff reporting: https://ctmirror.org/2024/06/02/ct-arpa-esser-school-funding-end/
- **NCTQ** — National paraprofessional staffing trends: https://www.nctq.org/research-insights/paraprofessional-and-educator-support-role-staffing-has-increased-to-the-benefit-of-students-and-teachers-but-will-it-last/
- **East Haddam, Hebron, Ellington, Lebanon** town websites — Historical mill rates
- **East Hampton, Ellington, Tolland, Ledyard** — Peer town budget documents

## Data Extraction Methods

### Budget Data (from PDFs)

Budget figures were extracted using Python's `pdfplumber` library:

1. Each PDF was opened and text extracted page by page
2. Budget summary pages were identified by keywords: "TOTAL BUDGET", "BOARD OF EDUCATION", "BUDGET SUMMARY BY FUNCTION"
3. Dollar amounts parsed using regex for formats like `$1,234,567` or `1,234,567`
4. FY 07-08 and 08-09 budgets contained detailed BOE breakdowns (Salaries, Employee Benefits, Transportation, etc.); later budgets show BOE as a lump sum
5. Town insurance extracted from budget code 41211 (Health Insurance) and "Total Legal & Insurances" lines
6. Multi-year comparison tables in budget narratives used to fill gaps in health insurance data

Extraction scripts used:
- `extract_benefits.py` — All PDFs searched for benefits/insurance keywords (1,915 matches)
- `extract_boe_benefits.py` — BOE employee benefits totals
- `search_paras.py` — Paraprofessional references
- `search_asst_supt.py` — Assistant superintendent references

### Staffing Data (from EdSight CSVs)

Parsed using Python's `csv` module via `parse_edsight.py`:

1. Each CSV's first line contains "FTE Staffing Report for YYYY-YY" — year extracted via regex
2. FTE counts aggregated by Assignment Category across all schools
3. Schools included: Bacon Academy, Colchester Elementary, Jack Jackter Intermediate, William J. Johnston Middle, plus alternative/transition programs in some years
4. Special education analysis via `sped_analysis.py` broke out sped teachers, sped paras, gen ed teachers separately

### EdSight Assignment Categories

| EdSight Category | Our Label | Notes |
|---|---|---|
| General Education - Teachers and Instructors | Gen Ed Teachers | Classroom teachers |
| Special Education - Teachers and Instructors | Sped Teachers | Special ed teachers |
| Administrators Coordinators and Department Chairs - School Level | School Admin | Principals, APs, dept chairs |
| Instructional Specialists Who Support Teachers | Instr Specialists | Coaches, curriculum coordinators |
| Counselors Social Workers and School Psychologists | Counselors/Psych | |
| School Nurses | Nurses | |
| General Education - Paraprofessional Instructional Assistants | Gen Ed Paras | Major ESSER jump FY 24-25 |
| Special Education - Paraprofessional Instructional Assistants | Sped Paras | Grew 30% |
| Library/Media - Specialists (Certified) + Support Staff | Library/Media | Combined |
| Other Staff Providing Non-Instructional Services/Support | Other Non-Instr | Custodial, food service, etc. |

### Central Office Administration

EdSight only reports school-level data. Central office administrators (superintendent, directors) estimated at 2.8-4.0 FTE based on budget org charts, GovSalaries.com records, and known superintendent names from budget documents.

## Analysis Methods

### Inflation Adjustment

CPI factors relative to FY 2007-08 (base = 1.000):
```
[1.000, 1.038, 1.035, 1.052, 1.085, 1.107, 1.123, 1.142, 1.143, 1.157, 1.182, 1.211, 1.233, 1.248, 1.307, 1.411, 1.469, 1.513]
```

### Indexed Comparisons

Base year = 100, allowing comparison of growth rates across different magnitudes on the same axis.

### Per Capita Calculations

Real per-capita spending = (Total Budget / Population) / CPI Factor

### Peer Town Selection

Two comparison groups:
1. **Neighbors** (geographic): Hebron, East Haddam, East Hampton, Marlborough, Lebanon, Salem, Bozrah
2. **Demographic peers** (statewide): Population 12K-17K, median income $100K-$135K — Ellington, East Hampton, Ledyard, Tolland, Suffield

### Mill Rate Caveats

CT towns revalue property every 5 years on staggered schedules. A revaluation raises the grand list, allowing a lower mill rate for the same revenue. Always note revaluation years when comparing.

## Key Corrections Made During Analysis

1. **Staffing estimates vs. actuals**: Initial staffing was estimated from budget documents. When actual EdSight CSVs were provided, significant corrections were needed — teachers were overestimated by 5-16 FTE, and instructional specialists were already at 14.6 FTE in FY 08-09 (not growing from 6 as initially estimated). The "specialists tripling" narrative was incorrect.

2. **Gen Ed Paraprofessional spike (FY 24-25)**: A 170% single-year jump (33 to 89 FTE) was investigated. "Other Non-Instructional" did NOT decline, confirming these were real new positions (not reclassification). Attributed to federal ESSER/ARPA COVID relief funding based on: statewide 12% Gen Ed Para increase (CT SDE PR-102), ESSER III obligation deadline of Sept 30 2024 aligning with FY 24-25, national NCTQ data on ESSER-funded paraprofessional hiring, and CT Mirror reporting on the ESSER cliff.

3. **FY 23-24 data gaps**: Budget PDF not available. Insurance and some budget values interpolated. Staffing data available from EdSight CSVs.

## Reproducibility

To reproduce from scratch:

1. Download budget PDFs from https://www.colchesterct.gov/finance-department/pages/adopted-budget
2. Download FTE Staffing CSVs from https://public-edsight.ct.gov/educators/fte-staffing (filter for Colchester, download each year)
3. Download CPI data from https://www.bls.gov/cpi/data.htm (annual averages, CPI-U)
4. Download mill rates from https://data.ct.gov/Local-Government/Mill-Rates-for-FY-2014-2026/emyx-j53e
5. Run `parse_edsight.py` to extract staffing data
6. Run `sped_analysis.py` for special education analysis
7. Use `pdfplumber` to extract budget figures from PDFs
8. Build Excel with `openpyxl`, verify with LibreOffice recalculation
9. Build HTML with Chart.js 4.4.1

## Tools Used

- Python 3 with `pdfplumber`, `openpyxl`, `csv`
- Chart.js 4.4.1 (CDN) for interactive charts
- LibreOffice Calc for Excel formula verification
- Claude AI (Anthropic) for data analysis, chart design, and narrative

## Files Produced

| File | Description |
|---|---|
| `src/index.html` | Primary deliverable — 19 interactive charts in 3 sections |
| `Colchester_Budget_Charts.html` | Backup copy of HTML |
| `Colchester_CT_Budget_Analysis.xlsx` | Excel workbook, 6 sections, 1,193 formulas |
| `StaffLevels/*.csv` | Raw EdSight CSV downloads (19 files) |
| `BudgetPdfs/*.pdf` | Raw budget PDFs (17 files) |
| `methodology.md` | This file |
| `data-dictionary.md` | All data arrays, definitions, sources |
| `findings-summary.md` | Key findings and narrative |
