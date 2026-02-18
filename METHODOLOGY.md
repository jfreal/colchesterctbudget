# Methodology

## Data Sources

### 1. Colchester Budget Data (Primary Source)
- **Source**: 17 adopted budget PDFs downloaded from https://www.colchesterct.gov/finance-department/pages/adopted-budget
- **Coverage**: FY 2007-08 through FY 2024-25 (18 fiscal years)
- **Extraction Method**: Python `pdfplumber` library used to extract text and tables from specific pages:
  - "Budget In Brief" pages — for total budget, BOE/Town breakdown, mill rate
  - "Budget Summary & Mill Rate Calculation" pages — for mill rate verification
  - "Budget Summary by Function" pages — for department-level expenditure breakdowns
- **Key Pages Targeted**: Each PDF was manually inspected to identify the correct pages containing summary data, as page numbers varied across years

### 2. CPI Inflation Data
- **Source**: U.S. Bureau of Labor Statistics (BLS) — Consumer Price Index for All Urban Consumers (CPI-U)
- **URL**: https://www.bls.gov/cpi/
- **Data Used**: Annual average CPI rates, 2007–2024
- **Values Used**:
  ```
  2007: 2.85%, 2008: 3.84%, 2009: -0.36%, 2010: 1.64%, 2011: 3.16%,
  2012: 2.07%, 2013: 1.46%, 2014: 1.62%, 2015: 0.12%, 2016: 1.26%,
  2017: 2.13%, 2018: 2.44%, 2019: 1.81%, 2020: 1.23%, 2021: 4.70%,
  2022: 8.00%, 2023: 4.12%, 2024: 2.94%
  ```

### 3. Population Data
- **Source**: CT Department of Public Health / U.S. Census Bureau Population Estimates Program
- **URL**: https://portal.ct.gov/dph/health-information-systems--reporting/population/annual-town-and-county-population-for-connecticut
- **Data Used**: Annual town population estimates for Colchester and peer towns, 2007–2024
- **Colchester Values**:
  ```
  2007: 15,495  2008: 15,578  2009: 15,838  2010: 16,068  2011: 16,087
  2012: 16,087  2013: 16,050  2014: 16,000  2015: 15,950  2016: 15,900
  2017: 15,850  2018: 15,800  2019: 15,861  2020: 15,555  2021: 15,501
  2022: 15,600  2023: 15,690  2024: 15,752
  ```
- **Note**: 2013–2018 and 2022–2023 values are interpolated estimates between Census/ACS anchor points

### 4. Mill Rate Data (Neighboring & Peer Towns)
- **Primary Source**: CT Open Data Portal — Mill Rates for FY 2014-2026 (https://data.ct.gov/Local-Government/Mill-Rates-for-FY-2014-2026/emyx-j53e)
- **Secondary Sources**: Individual town websites for historical mill rates:
  - East Haddam: https://www.easthaddam.org/departments/tax_collector/mill-rate
  - Hebron: https://hebronct.com/town-departments/tax-collectors-office/mill-rates/
  - Ellington: https://www.ellington-ct.gov/departments-and-services/finance/mill-rate-history
  - Lebanon: https://www.lebanonct.gov/tax-office/pages/grand-list-mill-rates
- **CT OPM**: https://portal.ct.gov/opm/igpp/publications/mill-rates

### 5. Peer Town Budget & Demographic Data
- **Demographics**: U.S. Census Bureau QuickFacts, Connecticut-Demographics.com, Data USA
- **Budgets**: Individual town budget documents and news coverage of budget referendums
  - East Hampton: https://www.easthamptonct.gov/finance/pages/annual-budgets (~$56.7M)
  - Ellington: Budget referendum coverage ($71.2M)
  - Tolland: Town budget information page ($63.7M)
  - Ledyard: Estimated from FY 25-26 proposal ($71.1M ÷ 1.0539 ≈ $67.4M)

## Data Extraction Process

### Step 1: PDF Download
All 17 budget PDFs were downloaded from the Colchester town website into `BudgetPdfs/`.

### Step 2: Manual Targeted Extraction
Each PDF was processed individually using `pdfplumber` to extract:
- **Total Budget** (Education + Town Operating + Debt Service + Transfers)
- **BOE Budget** (Board of Education)
- **Town Budget** (Town Operating, calculated as Total minus BOE)
- **Mill Rate** (from Budget Summary & Mill Rate Calculation pages)
- **Department Expenditures** (from Budget Summary by Function tables):
  - General Government
  - Public Safety
  - Public Works
  - Community & Human Services (combined "Human Services" + "Civic & Cultural" for pre-FY 12-13)
  - Debt Service
  - Transfers/Capital

### Step 3: Cross-Verification
- Mill rates were cross-verified between "Budget In Brief" and "Mill Rate Calculation" pages
- Budget totals were verified against prior-year references in subsequent budgets
- FY 2023-24 BOE budget was calculated from: Total ($59,639,491) minus Town ($15,660,140) = $43,979,351

### Step 4: Excel Construction
- Built using Python `openpyxl` library
- All calculations use Excel formulas (not hardcoded values) for auditability
- Blue font = hardcoded inputs; Black font = formulas (financial modeling standard)
- Recalculated using LibreOffice to verify all formulas evaluate correctly

### Step 5: Chart Generation
- HTML page built with Chart.js 4.4.1 (CDN)
- All data embedded directly in JavaScript (no external dependencies)
- Charts are interactive (hover tooltips, responsive sizing)

## Known Limitations & Caveats

1. **Mill Rate Comparisons**: Connecticut towns undergo property revaluations every 5 years on staggered schedules. A revaluation increases the grand list (total assessed value), which allows for a lower mill rate even if the budget stays the same. Comparing mill rates across towns requires noting each town's last revaluation year.

2. **Population Estimates (2013–2018)**: Some years between Census anchor points use interpolated estimates. These are close approximations but not exact counts.

3. **Peer Town Budget Data**: Some peer town budgets (Wolcott, Suffield, Plainfield) were not publicly available in searchable form. Per capita comparisons include only towns with confirmed budget figures.

4. **Department Category Changes**: Colchester reorganized department categories around FY 2012-13. "Human Services" and "Civic & Cultural" were combined into "Community & Human Services." Pre-FY 12-13 data combines these categories for consistency.

5. **FY 2010-11 and FY 2011-12 Special Notes**: These years included federal grants ($1.9M State Stabilization and $550K Jobs Bill) that were sometimes excluded from BOE cross-references in budget documents.

6. **East Haddam Mill Rate Mapping**: East Haddam's website lists mill rates by Grand List year (October 1 date). The mapping to fiscal year is: GL 10/1/XXXX → FY (XXXX+1)-(XXXX+2). For example, GL 10/1/2022 → FY 2023-24.

## Reproducibility

To regenerate this analysis from scratch:

1. Download budget PDFs from https://www.colchesterct.gov/finance-department/pages/adopted-budget
2. Extract data using `pdfplumber` targeting "Budget In Brief" and "Budget Summary by Function" pages
3. Compile CPI data from https://www.bls.gov/cpi/
4. Compile population data from CT DPH annual estimates
5. Build Excel workbook with `openpyxl` and verify with LibreOffice recalculation
6. Generate HTML charts using Chart.js with embedded data
