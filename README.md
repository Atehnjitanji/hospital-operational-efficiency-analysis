# Hospital Operational Efficiency Across U.S. Regions

### Emergency department performance, hospital capacity, and population aging

**Team 10 · Group data analytics project**  
**Tools:** Python · pandas · Matplotlib · Seaborn · Plotly · Google Colab

## Overview

How does emergency department (ED) performance vary across the United States, and how do those differences relate to hospital capacity and an aging population?

This project combines hospital quality measures, facility characteristics, Medicare enrollment, population estimates, and rural–urban classifications to explore regional differences in healthcare operations. We compare the Northeast, Midwest, South, and West, with additional analyses by state, hospital size, ownership, and rural–urban setting.

The project demonstrates an end-to-end analytical workflow: cleaning public datasets, resolving inconsistent identifiers, validating joins, calculating comparable metrics, and communicating findings through visualizations.

> **Scope:** Older adults motivate the research, but the ED performance measures cover patients of all ages. Population and Medicare data provide demographic context; the analysis does not directly measure ED outcomes for patients aged 65 and older.

**[Explore the full analysis notebook](A10-Hospital-Operational-Efficiency-for-Elderly-Across-U.S.-Regions%20%282%29.ipynb)**

## Research Questions

- How do ED visit duration and the percentage of patients leaving without being seen differ across regions and rural–urban settings?
- How has regional ED performance changed over the available reporting periods?
- Do states with larger older-population shares have longer ED visits?
- How do hospital availability and bed supply compare relative to population?
- How do ED performance and hospital size vary by region and ownership?

## Key Findings

- **The Northeast had longer ED visits, including within comparable hospital-size groups.** Among matched hospitals with fewer than 100 beds, the median reported OP_18b score was **140 minutes** in the Northeast and **117 minutes** in the Midwest. For hospitals with at least 300 beds, the corresponding values were **219** and **189 minutes**.
- **Urban hospitals showed longer ED visits than rural hospitals** across the reporting years examined. The urban group also had higher standardized scores for patients leaving without being seen.
- **Hospital counts and bed supply tell different stories.** The Northeast had the fewest reporting hospitals per resident, while the West had the lowest reported beds per resident.
- **Population aging alone did not show a clear relationship with ED visit duration** in the state-level scatterplots. This is a descriptive finding, rather than a causal or statistical significance claim.
- **The population aged 65+ and Medicare enrollment aged 65+ increased in every region from 2020 to 2025.** The South had the largest counts in both series.

### Regional Capacity Snapshot

| Region | Reporting hospitals per 100,000 residents | Reported beds per 1,000 residents | Population aged 65+ |
|---|---:|---:|---:|
| Northeast | 0.93 | 3.36 | 20.11% |
| Midwest | 1.95 | 3.68 | 19.56% |
| South | 1.31 | 3.33 | 18.61% |
| West | 1.19 | 2.59 | 18.25% |

*Figures come from the notebook's saved outputs. Population denominators use 2025 civilian population estimates. Hospital counts use the 2026 Timely and Effective Care release; beds use the Q2 2026 Provider of Services snapshot. The hospital lists differ, and reported beds do not measure staffed ED capacity. Regional bed rates are calculated from total regional beds divided by total regional population, rather than an unweighted average of state rates.*

## Data Sources

| Dataset | Original source and context | Use in this project |
|---|---|---|
| Timely and Effective Care — Hospital and State | CMS hospital quality reporting data, obtained through the [NBER Hospital Compare archive](https://www.nber.org/research/data/centers-medicare-medicaid-services-cms-hospital-compare-data). The notebook uses hospital releases labeled 2020–2026, state releases labeled 2020–2025, and a 2026 rural emergency hospital file. | ED performance measures and trends. |
| [Medicare Monthly Enrollment](https://data.cms.gov/summary-statistics-on-beneficiary-enrollment/medicare-and-medicaid-reports/medicare-monthly-enrollment/data) | CMS administrative enrollment statistics, from the June 2026 file. The analysis selects annual state-level records for 2020–2025. | Enrollment totals and counts of beneficiaries aged 65+. |
| [State Population by Characteristics](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-detail.html) | U.S. Census Bureau civilian population estimates by state, age, and sex for 2020–2025. | Older-population shares and per-capita denominators. |
| [Rural–Urban Commuting Area Codes](https://www.ers.usda.gov/data-products/rural-urban-commuting-area-codes) | USDA Economic Research Service 2020 RUCA ZIP code classifications, based on settlement and commuting patterns. | ZIP-based rural–urban classification of hospitals. |
| [Provider of Services File](https://data.cms.gov/provider-characteristics/hospitals-and-other-facilities/provider-of-services-file-quality-improvement-and-evaluation-system) | CMS facility records, using the Q2 2026 file. The notebook filters to hospital providers with active status. | Hospital beds, ownership, and facility characteristics. |

**Release years and measurement years differ.** ED trend analyses derive the analysis year from each measure's reporting-period end date. The `start_date` and `end_date` fields should be checked before comparing measures or sources.

## Methods

1. **Clean and standardize:** harmonize column names, convert numeric and date fields, preserve leading zeros in ZIP codes and hospital identifiers, and handle missing or suppressed values.
2. **Define the analysis groups:** map states to four regions and classify hospital ZIP codes as urban or rural using primary RUCA labels. The four-region comparisons cover the 50 states; DC and territories are outside that grouping.
3. **Integrate the datasets:** join hospital records to provider information through CMS Certification Numbers (CCNs), link RUCA classifications through ZIP codes, and connect demographic data through state codes. Use merge validation to check expected relationships.
4. **Construct comparison metrics:** calculate hospitals per 100,000 residents, beds per 1,000 residents, population shares aged 65+, and Medicare enrollment measures.
5. **Explore and visualize:** compare grouped means and medians, reporting-year trends, hospital-size categories, ownership groups, and standardized measure scores.

### Primary ED Measures

| Measure | Interpretation | Unit |
|---|---|---|
| `OP_18b` | Reported median time from ED arrival to departure for discharged patients covered by the measure | Minutes |
| `OP_22` | Percentage of ED patients who leave without being seen | Percent |

Lower values indicate shorter visits or fewer patients leaving without being seen. OP_18b measures the full ED visit duration covered by the measure, rather than only the wait before seeing a clinician. Regional summaries of hospital scores are not patient-weighted regional visit times.

## Visualizations

The notebook includes regional availability bar charts, ED performance heatmaps, time-series comparisons, hospital-size scatterplots, ownership summaries, and interactive state choropleths for bed supply and older-population shares. It also contains the detailed data dictionary and written interpretation of each analysis.

## Running the Analysis

The analysis notebook was written for **Google Colab** and reads CSV files from a mounted Google Drive. The notebook alone is insufficient for a fresh run; the source CSV files are also required.

1. Open the notebook in Google Colab.
2. Obtain the source files listed above. Use the same archived releases to reproduce the saved results; newer releases may change the findings.
3. Place the CSV files in your Google Drive and update every path in the notebook's **“Import all datasets here”** cell to your folder.
4. Mount your Drive when prompted, then run the notebook cells from top to bottom.

The import cell lists the exact expected filenames, including the project's renamed provider file, `Hospital_and_other_DATA_Q2_2026_RENAMED.csv`. A newly downloaded provider file may need its column names aligned with those used in the notebook.

For a local Jupyter environment, install the analysis packages:

```bash
python -m pip install "pandas>=2.0" matplotlib "seaborn>=0.13" plotly jupyter
```

Then remove the `google.colab` import and Drive-mount call, update the CSV paths to local locations such as `data/...`, and open the notebook with Jupyter. The minimum versions above reflect syntax used in the notebook; the original environment was not version-pinned.

## Limitations

- **Age-specific outcomes are unavailable:** state demographics cannot establish how individual older patients experience ED care.
- **Reporting windows vary:** release dates and measure periods differ across sources, and OP_18b and OP_22 do not always describe matching periods.
- **Missing and suppressed scores affect coverage:** analyses use available eligible scores after the notebook's filtering rules. Reporting hospitals may differ from excluded hospitals.
- **Capacity measures are incomplete:** total reported beds and facility counts do not capture staffing, occupancy, patient volume, case mix, or available ED beds.
- **The analysis is descriptive:** observed associations and group differences do not establish causes of delays or provide a comprehensive hospital-efficiency ranking.

## Practical Implications and Next Steps

The findings identify where further operational investigation may be useful. Northeast hospitals warrant closer examination of staffing, patient volume, and patient flow. The West's lower reported bed supply supports further investigation of staffed beds, occupancy, and local access before considering capacity changes.

A next phase could align measurement windows, incorporate age-specific outcomes, add staffing and patient-volume data, and model ED visit duration while accounting for differences in hospital characteristics.

## Team and Attribution

This project was developed collaboratively by **Team 10**:

- Ateh Nji-Tanji
- Lujia Shao (Lydia)
- Sophia Lee
- Vicky Ye
- Muntira Samalapa (View)

The analysis and findings presented here reflect the group's shared work. Public datasets are credited to their original providers above.

### Generative AI Disclosure

Team members used generative AI tools for support with syntax, debugging, data-source discovery, visualization formatting, and written presentation. Individual disclosures are included in the notebook.
