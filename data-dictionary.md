# Colchester, CT Budget Analysis — Data Dictionary

## Overview

This document defines every data array used in the interactive HTML chart dashboard (`src/index.html`). All arrays are defined in the `<script>` section and power 19 Chart.js visualizations. Values are listed in full for reproducibility.

---

## Time Periods

### `labels` — Fiscal Year Labels (18 values)
- **Used by:** Charts 1–6, 15b
- **Coverage:** FY 2007-08 through FY 2024-25
- **Source:** Colchester adopted budget PDFs
```
["FY 07-08","FY 08-09","FY 09-10","FY 10-11","FY 11-12","FY 12-13","FY 13-14","FY 14-15","FY 15-16","FY 16-17","FY 17-18","FY 18-19","FY 19-20","FY 20-21","FY 21-22","FY 22-23","FY 23-24","FY 24-25"]
```

### `staffLabels` — Staffing Year Labels (17 values)
- **Used by:** Charts 13, 13b, 14, 14b, 15, 16
- **Coverage:** FY 2008-09 through FY 2024-25 (one fewer year because EdSight data starts FY 2008-09)
- **Source:** CT EdSight FTE Staffing CSVs
```
["08-09","09-10","10-11","11-12","12-13","13-14","14-15","15-16","16-17","17-18","18-19","19-20","20-21","21-22","22-23","23-24","24-25"]
```

### `ecsLabels` — ECS Year Labels (18 values)
- **Used by:** Charts 11, 12
```
["07-08","08-09","09-10","10-11","11-12","12-13","13-14","14-15","15-16","16-17","17-18","18-19","19-20","20-21","21-22","22-23","23-24","24-25"]
```

---

## Budget Data (18 values each, FY 2007-08 through FY 2024-25)

### `totalBudgets` — Total Adopted Budget ($)
- **Source:** Budget PDF summary pages, regex-extracted dollar amounts
- **Unit:** Nominal U.S. dollars
- **Notes:** FY 23-24 value interpolated (PDF not available)
```
[46940735, 47634370, 48172704, 47618651, 50501287, 50281526, 52225904, 52995877, 53558796, 54094776, 55344488, 55370654, 56392987, 56472475, 57532017, 57648602, 59639491, 62678131]
```

### `townBudgets` — Town Operating Budget ($)
- **Source:** Budget PDF summary pages (Total Budget minus BOE)
- **Unit:** Nominal U.S. dollars
```
[13636350, 13338957, 13344980, 13569651, 13679697, 12757366, 13149850, 13334082, 13763426, 14389712, 14708083, 14821310, 15155865, 15155865, 15704962, 15622901, 15660140, 17102056]
```

### `boeBudgets` — Board of Education Budget ($)
- **Source:** Budget PDF summary pages
- **Unit:** Nominal U.S. dollars
```
[33304385, 34295413, 34827724, 34049000, 36821590, 37524160, 39076054, 39661795, 39795370, 39705064, 40636405, 40549344, 41237122, 41316610, 41827055, 42025701, 43979351, 45576075]
```

---

## Indexed Series (18 values each, base = FY 2007-08 = 100)

### `totalIndexed` — Total Budget Growth Index
- **Derivation:** `totalBudgets[i] / totalBudgets[0] * 100`
```
[100.0, 101.5, 102.6, 101.4, 107.6, 107.1, 111.3, 112.9, 114.1, 115.2, 117.9, 118.0, 120.1, 120.3, 122.6, 122.8, 127.1, 133.5]
```

### `cpiIndexed` — CPI Inflation Index
- **Source:** BLS CPI-U annual averages, compounded from 2007
- **Derivation:** Cumulative CPI growth indexed to 100
```
[100, 103.8, 103.5, 105.2, 108.5, 110.7, 112.3, 114.2, 114.3, 115.7, 118.2, 121.1, 123.3, 124.8, 130.7, 141.1, 146.9, 151.3]
```

### `townIndexed` — Town Operating Budget Index
- **Derivation:** `townBudgets[i] / townBudgets[0] * 100`
```
[100.0, 97.8, 97.9, 99.5, 100.3, 93.6, 96.4, 97.8, 100.9, 105.5, 107.9, 108.7, 111.1, 111.1, 115.2, 114.6, 114.8, 125.4]
```

### `boeIndexed` — BOE Budget Index
- **Derivation:** `boeBudgets[i] / boeBudgets[0] * 100`
```
[100.0, 103.0, 104.6, 102.2, 110.6, 112.7, 117.3, 119.1, 119.5, 119.2, 122.0, 121.8, 123.8, 124.1, 125.6, 126.2, 132.1, 136.8]
```

### `popIndexed` — Population Index
- **Source:** CT DPH / U.S. Census Bureau population estimates
- **Derivation:** `population[i] / population[0] * 100`
```
[100.0, 100.5, 102.2, 103.7, 103.8, 103.8, 103.6, 103.3, 102.9, 102.6, 102.3, 102.0, 102.4, 100.4, 100.0, 100.7, 101.3, 101.7]
```

### `pcRealIndexed` — Real Per-Capita Spending Index
- **Derivation:** `pcReal[i] / pcReal[0] * 100`
```
[100.0, 97.2, 97.1, 93.0, 95.5, 93.2, 95.6, 95.8, 97.0, 97.0, 97.5, 95.5, 95.2, 96.0, 93.8, 86.5, 85.4, 86.9]
```

---

## Per-Capita and Year-over-Year Data

### `pcReal` — Real Per-Capita Spending (Constant FY 07-08 $)
- **Derivation:** `(totalBudgets[i] / population[i]) / cpiFactors[i]`
- **Unit:** Inflation-adjusted dollars per resident
```
[3029, 2945, 2940, 2818, 2894, 2823, 2896, 2901, 2938, 2939, 2954, 2894, 2884, 2909, 2840, 2619, 2587, 2631]
```

### `yoyBudget` — Year-over-Year Budget Change (%)
- **Derivation:** `(totalBudgets[i] - totalBudgets[i-1]) / totalBudgets[i-1] * 100`
```
[null, 1.48, 1.13, -1.15, 6.05, -0.44, 3.87, 1.47, 1.06, 1.0, 2.31, 0.05, 1.85, 0.14, 1.88, 0.2, 3.45, 5.1]
```

### `yoyCpi` — Year-over-Year CPI Change (%)
- **Source:** BLS CPI-U annual average inflation rates
```
[null, 3.84, -0.36, 1.64, 3.16, 2.07, 1.46, 1.62, 0.12, 1.26, 2.13, 2.44, 1.81, 1.23, 4.7, 8.0, 4.12, 2.94]
```

---

## ECS (Education Cost Sharing) Data (18 values each)

### `ecsActual` — Actual ECS Revenue ($)
- **Source:** Revenue pages of each adopted budget PDF
- **Unit:** Nominal U.S. dollars
- **Notes:** FY 23-24 assumed same as FY 22-23 per hold-harmless provision
```
[12976438, 13547231, 13547231, 11614515, 13547231, 13723859, 13773810, 13761528, 13761528, 13591055, 13503310, 12670601, 12359179, 12040218, 12040218, 12040218, 12040218, 12040218]
```

### `ecsInflAdj` — Inflation-Adjusted ECS Baseline ($)
- **Derivation:** FY 07-08 ECS × cumulative CPI factor for each year
- **Purpose:** Shows what ECS would be if it had kept pace with inflation
```
[12976438, 12924532, 13145132, 13560378, 13832883, 14040506, 14274082, 14287058, 14468728, 14767186, 15130527, 15403032, 15597678, 16337335, 17634979, 18361660, 18828812, 19205128]
```

### `ecsPctBOE` — ECS as Percentage of BOE Budget
- **Derivation:** `ecsActual[i] / boeBudgets[i] * 100`
```
[36.8, 37.3, 36.8, 31.7, 36.8, 36.6, 35.2, 34.7, 34.6, 34.2, 33.2, 31.3, 30.0, 29.1, 28.7, 27.9, 27.1, 26.4]
```

---

## Staffing Data (17 values each, FY 2008-09 through FY 2024-25)

### `enrollments` — Student Enrollment
- **Source:** Budget PDF town profile sections
- **Notes:** FY 2012-13 through FY 2015-16 interpolated
```
[3267, 3265, 3223, 3210, 3150, 3100, 2975, 2850, 2727, 2488, 2488, 2452, 2321, 2321, 2235, 2161, 2235]
```

### `teacherFTE` — Total Teachers (Gen Ed + Sped)
- **Source:** EdSight CSVs, sum of "General Education - Teachers and Instructors" + "Special Education - Teachers and Instructors"
- **Unit:** Full-Time Equivalent positions
```
[231.6, 228.3, 220.8, 213.0, 205.7, 207.1, 198.8, 195.2, 190.3, 184.4, 188.7, 187.3, 186.8, 186.4, 187.0, 186.4, 189.5]
```

### `adminSchool` — School-Level Administrators
- **Source:** EdSight CSVs, "Administrators Coordinators and Department Chairs - School Level"
- **Includes:** Principals, assistant principals, department chairs
```
[10.0, 9.0, 9.0, 9.0, 9.0, 9.0, 9.0, 9.0, 10.0, 9.8, 9.6, 9.6, 9.6, 9.7, 9.0, 10.0, 9.0]
```

### `centralOffice` — Central Office Administrators
- **Source:** EdSight district-level reports + budget org charts + GovSalaries.com
- **Includes:** Superintendent, assistant superintendent, directors
- **Important:** NOT included in school-level EdSight CSVs; estimated at ~4.0 pre-2019, verified at 4.0 (FY 19-20) and 2.8 (FY 22-23 onward)
```
[4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 3.5, 3.0, 2.8, 2.8, 2.8]
```

### `adminNarrow` — Total Administrators (School + Central)
- **Derivation:** `adminSchool[i] + centralOffice[i]`
```
[14.0, 13.0, 13.0, 13.0, 13.0, 13.0, 13.0, 13.0, 14.0, 13.8, 13.6, 13.6, 13.1, 12.7, 11.8, 12.8, 11.8]
```

### `instrSpec` — Instructional Specialists Who Support Teachers
- **Source:** EdSight CSVs, "Instructional Specialists Who Support Teachers"
- **Includes:** Coaches, curriculum coordinators, instructional technology specialists
```
[14.6, 14.8, 13.1, 14.4, 14.2, 11.6, 12.0, 14.0, 13.0, 14.0, 13.4, 11.6, 13.0, 14.0, 15.0, 16.6, 16.7]
```

### `adminBroad` — Broad Administrative Category
- **Derivation:** `adminNarrow[i] + instrSpec[i]`
- **Purpose:** Matches the broader "administrative" definition used by some reporting agencies
```
[28.6, 27.8, 26.1, 27.4, 27.2, 24.6, 25.0, 27.0, 27.0, 27.8, 27.0, 25.2, 26.1, 26.7, 26.8, 29.4, 28.5]
```

### `adminNCES` — NCES/Federal Classification of Administrators
- **Source:** NCES Common Core of Data + Ballotpedia verification
- **Notes:** Different from EdSight; includes LEA admins + school admins + instructional coordinators. Verified for FY 19-20 (15.8), FY 22-23 (15.8), FY 23-24 (17.6)
```
[16.0, 16.0, 15.5, 15.5, 15.0, 15.0, 15.0, 15.0, 15.0, 15.5, 15.5, 15.8, 16.0, 15.9, 15.8, 17.6, 17.8]
```

---

## Special Education Staffing (17 values each)

### `spedTeach` — Special Education Teachers
- **Source:** EdSight CSVs, "Special Education - Teachers and Instructors"
```
[26.5, 26.5, 26.5, 26.6, 24.3, 23.2, 23.0, 24.0, 24.0, 24.0, 27.0, 26.0, 26.0, 25.0, 26.0, 24.0, 28.0]
```

### `spedPara` — Special Education Paraprofessionals
- **Source:** EdSight CSVs, "Special Education - Paraprofessional Instructional Assistants"
```
[50.0, 50.0, 47.0, 54.5, 51.0, 53.5, 55.5, 60.5, 57.0, 52.3, 62.3, 62.3, 62.0, 63.0, 61.0, 57.0, 65.0]
```

### `spedTotal` — Total Special Education Staff
- **Derivation:** `spedTeach[i] + spedPara[i]`
```
[76.5, 76.5, 73.5, 81.1, 75.3, 76.7, 78.5, 84.5, 81.0, 76.3, 89.3, 88.3, 88.0, 88.0, 87.0, 81.0, 93.0]
```

### `genEdTeach` — General Education Teachers Only
- **Source:** EdSight CSVs, "General Education - Teachers and Instructors"
- **Notes:** This is the `teacherFTE` minus `spedTeach`
```
[205.1, 201.8, 194.3, 186.4, 181.4, 183.9, 175.8, 171.2, 166.3, 160.4, 161.7, 161.3, 160.8, 161.4, 161.0, 162.4, 161.5]
```

---

## Salary Data (17 values each, FY 2008-09 through FY 2024-25)

### `avgTeachSal` — Average Teacher Salary ($)
- **Source:** GovSalaries.com, Niche.com ($82,554 verified), ConnecticutTeach.org
- **Notes:** Some years estimated/interpolated between known benchmarks
```
[64500, 65500, 66500, 67500, 68500, 70000, 71500, 73000, 74500, 76000, 78804, 79500, 80500, 81500, 82554, 84000, 85500]
```

### `avgAdminSal` — Average Administrator Salary ($)
- **Source:** GovSalaries.com (2024 records for 244 BOE employees)
- **Notes:** Most years estimated/interpolated
```
[95000, 97000, 98000, 100000, 102000, 104000, 106000, 108000, 110000, 112000, 115000, 117000, 119000, 122000, 125000, 128000, 131000]
```

### `cpiFactors` — CPI Adjustment Factors (17 values, base = FY 2008-09 = 1.000)
- **Source:** BLS CPI-U annual averages
- **Notes:** These are relative to FY 2008-09 (not FY 2007-08), used specifically for salary charts
```
[1.000, 0.996, 1.013, 1.045, 1.066, 1.082, 1.100, 1.101, 1.115, 1.139, 1.166, 1.188, 1.202, 1.259, 1.359, 1.415, 1.451]
```

### `teachInflAdj` — CPI-Adjusted Teacher Salary Baseline
- **Derivation:** `64500 * cpiFactors[i]`, rounded
- **Purpose:** Shows what FY 08-09 teacher salary would be if it grew exactly with CPI

### `adminInflAdj` — CPI-Adjusted Administrator Salary Baseline
- **Derivation:** `95000 * cpiFactors[i]`, rounded

---

## Town Insurance Data (18 values each, FY 2007-08 through FY 2024-25)

### `townHealthIns` — Town Government Health Insurance Cost ($)
- **Source:** Budget PDF line item code 41211 (Health Insurance)
- **Notes:** FY 23-24 value ($1,087,399) from FY 24-25 comparison table. Town-side only; BOE health insurance not available as a separate line item after FY 08-09.
```
[729106, 714526, 782570, 1016477, 1045629, 1122201, 1002760, 941618, 782635, 1004860, 860562, 874951, 916343, 1045603, 1118393, 1096201, 1087399, 1123290]
```

### `townTotalIns` — Town Total Insurance (All Types) ($)
- **Source:** Budget PDF "Total Legal & Insurances" lines
- **Includes:** Health insurance, workers compensation, property/liability, unemployment insurance
- **Notes:** FY 23-24 interpolated ($1,933,000)
```
[1253249, 1343683, 1588585, 1696379, 1737763, 1350978, 1567296, 1516957, 1395486, 1657614, 1579325, 1642333, 1683540, 1721911, 1863938, 1842053, 1933000, 2023725]
```

---

## Peer/Regional Comparison Data (Single-Year Snapshots)

### Regional Mill Rates (FY 2024-25)
- **Source:** CT Open Data Portal, Mill Rates for FY 2014-2026
- **Towns and values:**
  - Lebanon: 21.00
  - East Haddam: 26.76
  - Colchester: 28.67
  - Salem: 30.20
  - Hebron: 34.50
  - Marlborough: 36.29
  - East Hampton: 38.04

### Demographic Peer Mill Rates (FY 2024-25)
- **Selection criteria:** CT towns with population 12K-17K, median income $100K-$135K
- **Towns and values:**
  - Suffield (reval): 23.41
  - Tolland (reval): 27.19
  - Colchester: 28.67
  - Plainfield: 29.88
  - Wolcott (reval): 35.93
  - South Windsor: 35.61
  - Ellington: 37.10
  - Ledyard: 37.14
  - East Hampton: 38.04

### Per Capita Budget — Neighbors (FY 2024-25)
- **Source:** FY 24-25 adopted budgets ÷ 2024 Census population estimates
- **Values:** Colchester $3,979; Lebanon $4,136; East Hampton $4,326; Salem $4,287; Hebron $4,471

### Per Capita Budget — Demographic Peers (FY 2024-25)
- **Values:** Colchester $3,979; Ellington $4,295; East Hampton $4,326; Ledyard $4,328; Tolland $4,390
- **Peer Average:** $4,334

---

## Derived/Computed Arrays

These arrays are computed in JavaScript from the primary arrays above:

| Array | Derivation |
|---|---|
| `ecsActualM` / `ecsInflAdjM` | ECS values divided by 1,000,000 (for chart display in millions) |
| `certifiedTotal` | `teacherFTE + adminNarrow + instrSpec` (total certified staff) |
| `teachPct` / `adminPct` / `specPct` | Each category as % of `certifiedTotal` |
| `enrollPct` | `pctChange(enrollments)` — % change from FY 08-09 baseline |
| `teachPctChange` | `pctChange(teacherFTE)` |
| `adminSchoolPct` | `pctChange(adminSchool)` |
| `specPctChange` | `pctChange(instrSpec)` |
| `spedTeachPct` / `spedParaPct` / `spedTotalPct` / `genEdTeachPct` | `pctChange()` applied to each sped array |
| `hiK` / `totalInsK` / `hiCpiK` | Insurance values divided by 1,000 (for chart display in $K) |
| `hiCpiAdj` | `729106 * cpiFactors[i]` — CPI-adjusted health insurance baseline |
| `boeCostPerStudent` | BOE budget ÷ enrollment for each year |

### `pctChange()` Helper Function
```javascript
const pctChange = (arr) => arr.map(v => +((v - arr[0]) / arr[0] * 100).toFixed(1));
```
Converts any array to percentage change from its first value (baseline = 0%).

---

## Population Data (used in derivations, not directly in charts)

- **Source:** CT DPH / U.S. Census Bureau
- **Coverage:** 2007–2024 (some years interpolated)
```
2007: 15495, 2008: 15578, 2009: 15838, 2010: 16068, 2011: 16087,
2012: 16087, 2013: 16050, 2014: 16000, 2015: 15950, 2016: 15900,
2017: 15850, 2018: 15800, 2019: 15861, 2020: 15555, 2021: 15501,
2022: 15600, 2023: 15690, 2024: 15752
```

## CPI Annual Rates (used in derivations)

- **Source:** BLS CPI-U annual averages
```
2007: 2.85%, 2008: 3.84%, 2009: -0.36%, 2010: 1.64%, 2011: 3.16%,
2012: 2.07%, 2013: 1.46%, 2014: 1.62%, 2015: 0.12%, 2016: 1.26%,
2017: 2.13%, 2018: 2.44%, 2019: 1.81%, 2020: 1.23%, 2021: 4.70%,
2022: 8.00%, 2023: 4.12%, 2024: 2.94%
```

## CPI Factors (18 values, base = FY 2007-08 = 1.000)

Used for inflation-adjusted budget comparisons (distinct from the 17-value salary `cpiFactors`):
```
[1.000, 1.038, 1.035, 1.052, 1.085, 1.107, 1.123, 1.142, 1.143, 1.157, 1.182, 1.211, 1.233, 1.248, 1.307, 1.411, 1.469, 1.513]
```
