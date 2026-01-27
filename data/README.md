# Data Dictionary

This document describes the data files used in the Nexus Dashboard v.2, their structure, sources, and usage.

---

## Main Indicator Dataset

### `nexus.parquet`

**Description:** The primary indicator dataset containing all indicator values for countries and years.

**Schema:**
- `indicator_label` (string): Unique identifier for the indicator (e.g., "PEFA: PI-1 Aggregate expenditure out-turn")
- `country_or_area` (string): Country name (e.g., "Kenya", "Nigeria")
- `year` (integer): Year of the observation
- `value` (float): Indicator value
- `iso3` (string, optional): ISO3 country code

**Data Source:** Generated from the OSAA data pipeline repositories:
- **[osaa-pipeline](https://github.com/mirianlima/osaa-pipeline)**: Refactored OSAA data pipeline for policy analysis (current working repository)
- **[nexus-pipeline](https://github.com/UN-OSAA/nexus-pipeline)**: Original nexus-pipeline repository

The pipeline integrates data from multiple sources:
- World Bank PEFA Assessments
- IMF ISORA Database
- Global Financial Integrity (GFI)
- UNODC Crime Data
- World Justice Project
- And other international data sources

**Usage in Codebase:**
- Loaded via `scripts.universal_viz.load_main_data()` function
- Used throughout all topic pages for indicator visualizations
- Filtered by country, year, and indicator_label in visualization functions

**Data Lineage:**
1. Raw data collected from various international sources
2. Processed and standardized in the [nexus-pipeline](https://github.com/UN-OSAA/nexus-pipeline) repository
3. Exported to `nexus.parquet` format
4. Loaded into dashboard for visualization

---

## Reference Data Files

### `iso3_country_reference.csv`

**Description:** Comprehensive country reference data with geographic and classification information.

**Schema:**
- `Global Code` (integer): Global region code
- `Global Name` (string): "World"
- `Region Code` (integer): Region code (e.g., 2 for Africa)
- `Region Name` (string): Region name (e.g., "Africa")
- `Sub-region Code` (integer): Sub-region code
- `Sub-region Name` (string): Sub-region name
- `Intermediate Region Code` (integer): Intermediate region code
- `Intermediate Region Name` (string): Intermediate region name (used for filtering)
- `Country or Area` (string): Full country name
- `m49` (integer): UN M49 numeric code
- `iso2` (string): ISO 3166-1 alpha-2 code
- `iso3` (string): ISO 3166-1 alpha-3 code
- `Least Developed Countries (LDC)` (string): LDC classification
- `Land Locked Developing Countries (LLDC)` (string): LLDC classification
- `Small Island Developing States (SIDS)` (string): SIDS classification
- `Arab states` (string): Arab states classification
- `Fragile and Conflict-Affected Situations` (string): FCAS classification
- `HIPC` (string): Heavily Indebted Poor Countries classification
- `High income`, `Low income`, `Lower middle income`, `Upper middle income` (string): Income classifications
- `OECD member` (string): OECD membership
- `Small State` (string): Small state classification
- `LDC Transit countries` (string): LDC transit classification
- `Oil exporting countries` (string): Oil exporting classification

**Data Source:** UN Statistics Division / UN M49 standard

**Usage in Codebase:**
- Loaded via `scripts.universal_viz.load_country_reference_data()`
- Used for:
  - Country filtering (especially Africa-only filtering)
  - Region-based filtering (using `Intermediate Region Name`)
  - Mapping country names to ISO3 codes
  - Geographic classifications

**Data Lineage:**
1. UN Statistics Division maintains M49 standard
2. Exported to CSV format
3. Used directly in dashboard for reference and filtering

---

### `countries_codes_and_coordinates.csv`

**Description:** Country codes with geographic coordinates for map visualizations.

**Schema:**
- `Country` (string): Country name
- `Alpha-2 code` (string): ISO 3166-1 alpha-2 code
- `Alpha-3 code` (string): ISO 3166-1 alpha-3 code
- `Numeric code` (integer): ISO 3166-1 numeric code
- `Latitude (average)` (float): Average latitude
- `Longitude (average)` (float): Average longitude

**Data Source:** ISO 3166-1 standard with coordinate data

**Usage in Codebase:**
- Used for map visualizations (choropleth maps)
- Provides geographic coordinates for country mapping
- Links country names to ISO codes

**Data Lineage:**
1. ISO 3166-1 standard country codes
2. Coordinate data added from geographic databases
3. Combined into single CSV file

---

## Specialized Data Files

### `Pension_Fund_Asset_Allocation_by_Country.csv`

**Description:** Pension fund asset allocation data by country for Topic 4.3.3 (Institutional Investors).

**Schema:**
- `Country` or `Country or Area` (string): Country name
- `Domestic_Equities (%)` (float): Percentage allocated to domestic equities
- `Domestic_Bonds (%)` (float): Percentage allocated to domestic bonds
- `Real_Estate (%)` (float): Percentage allocated to real estate
- `Private_Equity (%)` (float): Percentage allocated to private equity
- `Cash & Deposits (%)` (float): Percentage allocated to cash and deposits
- `Foreign_Assets (%)` (float): Percentage allocated to foreign assets

**Data Source:** 
- RisCura Bright Africa Pension Industry Report
- National pension fund reports (GEPF, PenCom, NSSF, RSSB, SSNIT)
- Country-specific pension regulatory data

**Usage in Codebase:**
- Loaded directly in `special_pages/tab_4_4_1.py` (if used) or `pages/5_topic_4_3.py`
- Used for Topic 4.3.3 visualizations showing pension fund asset allocation
- Displayed in stacked bar charts by country

**Data Lineage:**
1. Data collected from national pension fund reports
2. Aggregated and standardized into percentage allocations
3. Combined into single CSV file
4. Used for Topic 4.3.3 visualizations

---

### `Viz specification matrix.xlsx`

**Description:** Excel file containing visualization specifications for indicators.

**Purpose:** Defines chart types, visualization goals, and specifications for each indicator.

**Usage:** Reference document for developers implementing new indicators. Not directly loaded into the codebase but used as a specification guide.

**Note:** This file is used during development to determine the appropriate visualization type for each indicator.

---

## Data Loading Patterns

### Standard Data Loading

Most pages follow this pattern:

```python
import scripts.universal_viz as uv

# Load reference data
ref_data = uv.load_country_reference_data()

# Load main indicator data
df_main = uv.load_main_data("data/nexus.parquet")

# Apply filters
filters = uv.setup_sidebar_filters(ref_data, df_main, key_prefix="topic_X")
df_filtered = uv.filter_dataframe_by_selections(df_main, filters, ref_data)
```

### Specialized Data Loading

Some pages load additional data files:

```python
# Example: Pension fund data
df_pension = pd.read_csv('data/Pension_Fund_Asset_Allocation_by_Country.csv')
```

---

## Data Transformations

### Filtering

All data is filtered to Africa-only countries using the reference data:

```python
africa_ref_data = ref_data[ref_data['Region Name'] == 'Africa']
```

### Indicator Matching

Indicators are matched by `indicator_label`:

```python
indicator_data = df_main[df_main['indicator_label'] == 'Indicator Name']
```

### Year Filtering

Data can be filtered by year:

```python
filtered_data = indicator_data[indicator_data['year'] == selected_year]
```

---

## Data Quality Notes

- **Missing Values:** Some indicators may have missing values for certain countries/years. The dashboard handles this gracefully.
- **Data Coverage:** Coverage varies by indicator. Some indicators have data for many countries and years, others are more limited.
- **Data Updates:** Data is updated periodically through the [osaa-pipeline](https://github.com/mirianlima/osaa-pipeline) repository (refactored pipeline) or the [nexus-pipeline](https://github.com/UN-OSAA/nexus-pipeline) repository. Check the pipeline repositories for update schedules.

---

## Adding New Data Files

When adding new data files:

1. Place the file in the `data/` directory
2. Document the file in this README with:
   - Description
   - Schema (column names and types)
   - Data source
   - Usage in codebase
   - Data lineage
3. Update any loading functions if needed
4. Ensure the file is included in version control (if appropriate)

---

**Last Updated:** January 2026
