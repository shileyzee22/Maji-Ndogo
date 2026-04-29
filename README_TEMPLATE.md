# Maji Ndogo Water Access Dashboard
> *Analysed water access across 27.6M people in five provinces of Maji Ndogo to identify infrastructure gaps, quantify improvement costs, and deliver a decision-ready Power BI report for national and provincial leadership.*
 
---
 
## ⚙️ Project Type
 
- [ ] Exploratory Data Analysis (EDA)
- [ ] Dashboard / Data Visualization
- [ ] Data Cleaning 

---
 
## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [visualize](#7-analysis--metrics)
9. [Key Insights](#9-key-insights)
10. [Recommendations](#10-recommendations)
11. [Assumptions & Limitations](#11-assumptions--limitations)
12. [Future Enhancements](#12-future-enhancements)
13. [Deliverables](#13-deliverables)
14. [Author](#14-author)
---
 
## 1. Project Overview
 
**Context:** Maji Ndogo is a fictional nation facing a widespread water access crisis. A national survey was conducted to assess water source types, queue times, water quality, and population distribution across five provinces — Akatsi, Amanzi, Hawassa, Kilimani, and Sokoto.
 
**Problem Statement:** National leadership (President Aziza Naledi) needed to understand the current state of water access across the country, how many people lacked basic water, and how much it would cost to resolve the crisis — at both national and provincial levels — in order to allocate budgets and mobilise ground teams.
 
**Approach:** Survey data was modelled in Power BI across multiple relational tables. DAX was used to classify water sources against UN standards, calculate adjusted improvement costs, and measure baseline versus post-project water access rates. National and provincial reports were built with user stories as the design blueprint.
 
**Outcome:** A multi-page Power BI report was delivered — including a national overview page, five provincial drill-through pages, and an interactive budget breakdown — showing that 34% of the population currently has access to basic water, and that a $147M investment across 25,398 improvements could bring that to 100%.
 
---
 
## 2. Objectives
 
- **Primary Objective:** Build an interactive Power BI dashboard that enables President Naledi and provincial leaders to understand water access status and make budget allocation decisions.
- **Secondary Objective 1:** Classify all water sources in Maji Ndogo as "Basic Access" or "Below Basic Access" using UN water quality standards.
- **Secondary Objective 2:** Calculate the total and province-level cost of all required infrastructure improvements, adjusting for rural vs. urban cost differences.
- **Secondary Objective 3:** Design province-specific drill-through pages so local leaders can explore data relevant only to their region.
> 💡 *Every analysis decision in this project traces back to one of these objectives.*
 
---
 
## 3. Project Scope & Tools
 
### Scope
 
| Dimension | Details |
|-----------|---------|
| **In Scope** | All five provinces (Akatsi, Amanzi, Hawassa, Kilimani, Sokoto); water source types, queue times, well pollution, population counts, infrastructure improvement costs |
| **Out of Scope** | Historical trend data beyond the survey period; individual household-level identifiers; external economic or climate data |
| **Time Period** | Based on a single national water survey dataset (point-in-time snapshot) |
| **Granularity** | Water source level; aggregated to town, province, and national level for reporting |
 
### Tools & Technologies
 
| Category | Tool(s) Used |
|----------|-------------|
| Data Storage | Power BI data model (imported tables) |
| Data Processing | Power Query (M), DAX calculated columns and measures |
| Analysis | DAX (CALCULATE, FILTER, IF, CONTAINSSTRING, AVERAGE) |
| Visualization | Power BI Desktop — bar charts, donut charts, maps, card visuals, tables, bookmarks |
| Version Control | Git / GitHub |
| Documentation | Markdown |
 
---
 
## 4. Repository Structure
 
```
maji-ndogo-water-access/
│
├── data/
│   ├── raw/                  # Original survey data files - never edited
│   └── processed/            # Cleaned tables used in the Power BI model
│
├── reports/                  # Published Power BI .pbix file
│
├── visuals/                  # Dashboard screenshots, page previews
│
├── docs/                     # Data dictionary, schema notes
│
└── README.md                 # You are here
```
 
---
 
## 5. Data Workflow
 
```
Survey Data (multiple tables: water_source, visits, well_pollution,
             location, project_progress, infrastructure_cost)
      ↓
Loaded into Power BI via Import mode
      ↓
Relationships established between tables on shared keys (source_id, location_id)
      ↓
DAX calculated columns: Average_queue_time, Basic_water_access,
                         Rural_adjusted_cost, Budgeted_improvement_cost,
                         Aggregated_improvements
      ↓
DAX measures: Basic_water_access %, Improvement %, Total Budget (USD)
      ↓
Multi-page Power BI report: National overview + 5 provincial drill-through pages
```
 
1. **Source:** Multi-table dataset from the Maji Ndogo national water survey — including water source records, visit logs with queue times, well pollution results, location data, improvement plans, and infrastructure cost estimates.
2. **Ingestion:** Tables imported into Power BI Desktop and connected via a star-schema-style data model.
3. **Cleaning:** Improvement categories were consolidated using DAX (`CONTAINSSTRING`) — e.g., all "Install N taps nearby" variants were aggregated into "Install public tap(s)*", and "Diagnose local infrastructure" was renamed to "Repair infrastructure" for clarity.
4. **Transformation:** Key DAX columns created: `Average_queue_time` (per source, averaged across multiple visits); `Basic_water_access` (UN-standard classification per source); `Rural_adjusted_cost` (unit cost × 1.5 for rural sources); `Budgeted_improvement_cost` (looks up rural/urban cost per improvement type).
5. **Analysis:** Descriptive aggregation by province, town, and rural/urban split; access rate calculation (people with basic water ÷ total population); budget breakdown by province and improvement type.
6. **Output:** Interactive Power BI report — national summary page with KPI cards, provincial drill-through pages, bookmark-toggled budget tables, and a province slicer linked to a map visual.
---
 
## 6. Data Model & Schema
 
### Dataset / Table: `water_source`
 
| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `source_id` | string | Unique identifier for each water source | `SRC-00482` |
| `type_of_water_source` | string | Category of water source | `shared_tap` |
| `number_of_people_served` | int | Population relying on this source | `2,340` |
| `Average_queue_time` | float (DAX) | Calculated average queue time across all visits to this source | `42.5` |
| `Basic_water_access` | string (DAX) | UN-standard classification: Basic Access or Below Basic Access | `Basic Access` |
 
### Dataset / Table: `visits`
 
| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `source_id` | string | Foreign key linking to `water_source` | `SRC-00482` |
| `time_in_queue` | int | Minutes spent queuing at this visit | `55` |
| `visit_count` | int | Number of times this source was surveyed | `3` |
 
### Dataset / Table: `well_pollution`
 
| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `source_id` | string | Foreign key linking to `water_source` | `SRC-00210` |
| `results` | string | Pollution test outcome | `Contaminated: Biological` |
 
### Dataset / Table: `project_progress`
 
| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `source_id` | string | Water source requiring improvement | `SRC-00482` |
| `improvement` | string | Type of upgrade planned | `Install RO filter` |
| `town` | string | Town where source is located | `Harare` |
| `province` | string | Province of the source | `Kilimani` |
| `Budgeted_improvement_cost` | float (DAX) | Calculated cost (rural-adjusted where applicable) | `5,625` |
 
### Dataset / Table: `infrastructure_cost`
 
| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `improvement` | string | Improvement type | `Drill well` |
| `unit_cost_USD` | float | Base cost per improvement in USD | `8,500` |
| `Rural_adjusted_cost` | float (DAX) | Cost × 1.5 for rural sources | `12,750` |
 
> **Total improvements planned:** 25,398
> **Total budgeted cost:** $146,737,375
> **Key relationships:** `visits.source_id` → `water_source.source_id` | `well_pollution.source_id` → `water_source.source_id` | `project_progress.source_id` → `water_source.source_id`
 
---
 
## 7. Analysis & Metrics
 
### Analytical Approach
 
This project followed a user-story-driven design approach. Rather than exploring the data freely, each visual was built to answer a specific question posed by the two primary users: President Aziza Naledi (national overview) and provincial leaders (localised decision-making). DAX calculations were developed to bridge gaps between raw survey data and the metrics needed to answer those questions.
 
### Key Metrics Defined
 
| Metric | Plain-Language Definition | Why It Matters |
|--------|--------------------------|----------------|
| `Basic_water_access %` | The percentage of the total population currently using a water source that meets UN basic access standards | Measures the baseline problem — only 34% of Maji Ndogo currently has basic water access |
| `Improvement %` | The percentage point increase in basic water access once all planned upgrades are complete | Quantifies the impact of the $147M investment — the goal is to reach 100% |
| `Budgeted_improvement_cost` | The estimated cost per water source improvement, adjusted by 50% for rural sources | Ensures budget estimates reflect the higher cost of remote, rural infrastructure work |
| `Total Budget (USD)` | Sum of all budgeted improvement costs across all 25,398 planned upgrades | Gives President Naledi the single national number she needs for capital allocation |
 
### Methods Used
 
- UN classification framework applied to categorise water sources (clean wells, shared taps with queue < 30 min, and home taps = Basic Access; rivers, broken taps, contaminated wells = Below Basic Access)
- DAX `CALCULATE` + `FILTER` to compute per-source average queue times from multi-visit data
- Conditional DAX logic (`IF`, `AND`, `CONTAINSSTRING`) for cost lookups and access classification
- Aggregation to provincial and national grain for KPI cards and budget tables
- Bookmark-based interactivity to toggle between Province and Improvements budget views
- Drill-through pages for each of the five provinces, filtered to local data only
---

## 4. Visualize
### Maji Ndogo Water Access report

![Maji Ndogo Water Access Dashboard](image_url)

*Above: Screenshot of the interactive Power BI dashboard.*
---

## 9. Key Insights
 
**Insight 1: Only 34% of Maji Ndogo's 27.6M people currently have basic water access**
The national baseline sits at just 34% — meaning roughly 18 million people are relying on unimproved or unsafe sources. This is the clearest indicator that the water crisis is not a localised issue but a national emergency requiring coordinated investment across all five provinces.
 
**Insight 2: Kilimani and Sokoto require the largest budgets ($39M and $40M respectively)**
Budget distribution is uneven across provinces. Kilimani leads in both population (6.58M) and planned improvement quantity (6,700 upgrades), while Sokoto's rural-heavy geography drives up costs through the 50% rural adjustment. Amanzi, by contrast, requires only $13M — the smallest share — due to lower population and fewer required upgrades.
 
**Insight 3: RO filter installation and well drilling account for the majority of the $147M budget**
Of the five major improvement types, Install RO filter (7,093 upgrades, ~$40M) and Drill well (3,379 upgrades, ~$38.9M) together represent over half the total spend. These two categories are the primary cost drivers and should be prioritised in procurement and contractor planning.
 
**Insight 4: Saturday queue times are three times longer than any weekday**
Queue time analysis shows Saturday averages 246 minutes — versus 42–60 minutes on weekdays. This suggests shared tap infrastructure is severely over-capacity during weekends, likely due to combined work and school schedule pressures. Improvements to shared taps should account for peak-day demand, not average daily usage.
 
**Insight 5: Water well contamination is split roughly evenly between chemical and biological sources**
Of wells tested, 40.8% were chemically contaminated, 30.9% biologically contaminated, and only 28.3% were clean. This means a majority of wells are currently unusable as basic water sources, and their improvement path (RO filter vs. UV+RO vs. repair) depends on contamination type — a nuance the improvement plan already accounts for.
 
---
 
## 10. Recommendations
 
| Priority | Recommendation | Based On | Suggested Owner |
|----------|---------------|----------|-----------------|
| High | Prioritise Kilimani and Sokoto in the first budget release — these provinces represent the largest gap between current access and the 100% target, and have the highest improvement counts | Insight 2 — provincial budget breakdown | President Naledi / National Treasury |
| High | Begin procurement planning for RO filter installations and well drilling contractors immediately — these two categories account for 54% of total spend and will have the longest lead times | Insight 3 — improvement type cost analysis | Infrastructure / Supply Chain team |
| Medium | Redesign shared tap scheduling or expand capacity at high-queue sources — Saturday peaks suggest current infrastructure cannot support weekend demand | Insight 4 — Saturday queue time anomaly | Provincial leaders, Local operations teams |
| Medium | Ensure contamination-type data is used to assign correct improvement type per well — not all contaminated wells require the same fix, and misassignment would waste budget | Insight 5 — well contamination results | Data team / Field survey verification |
| Low | After initial rollout, track `Basic_water_access %` monthly per province to measure progress against the 100% target and flag provinces that are falling behind | All insights — post-project monitoring | Dalila's data team / BI reporting function |
 
---
 
## 11. Assumptions & Limitations
 
### Assumptions
- Survey data is assumed to be complete and representative for all five provinces — no cross-validation against an external population register was performed.
- The 50% rural cost uplift applied to all rural improvements is a planning estimate provided by project managers — actual procurement costs may vary.
- Well contamination classifications (chemical vs. biological) are treated as accurate and final — no re-testing or margin of error has been incorporated.
- Queue times were averaged across multiple visits per source; this assumes visit timing was representative of typical usage patterns.
### Limitations
- The dataset is a point-in-time snapshot — it does not capture seasonal variation in water availability, queue length, or contamination levels.
- The analysis cannot distinguish between wells that are contaminated due to natural geology versus human activity (e.g., agricultural runoff), which may affect remediation strategy.
- Crime data was included in the dashboard but not deeply analysed in this version — the relationship between crime patterns and water collection behaviour (particularly for women and children) is noted but not quantified.
- Provincial cost totals assume uniform per-unit costs within each improvement type — economies of scale or local procurement differences are not modelled.
---
 
## 12. Future Enhancements
 
- [ ] Add a progress-tracking page to the report that updates `Basic_water_access %` as improvements are marked complete in `project_progress`, replacing the static snapshot with a live project tracker
- [ ] Incorporate seasonal water availability data to adjust queue time classifications — a source with < 30 min average queue may still be inadequate during dry season
- [ ] Analyse the crime dataset in depth to quantify safety risk by province and time of day, specifically as it affects water collection by women and children
- [ ] Add a cost vs. impact scatter plot to help leadership identify the highest-ROI improvements (most people served per dollar spent)
- [ ] Publish the report to Power BI Service and configure row-level security (RLS) so each provincial leader can only see their own province's data
---
 
## 13. Deliverables
 
| Deliverable | Description | Location |
|-------------|-------------|----------|
| Power BI Report (.pbix) | Multi-page interactive dashboard — national overview + 5 provincial pages | [`/reports/Maji Ndogo 2.pdf/`] |
| Dashboard Screenshots | Visual previews of key report pages | [`/visuals/`] |
| Data Dictionary | Field-level descriptions for all tables used in the model | [`/data/raw/Md_water_services_data.xlsx/`] |
| Docs | Descriptions for all instruction given by president | [`/docs/Part_3 (1).pdf/`] |
| README | Full project documentation (this file) | [`/README.md`] |
 
---
 
## 14. Author
 
**[Farinde Olasile Lateefat]**
Data Analyst
 
- 🔗 [https://www.linkedin.com/in/olasile/]
- 💼 [[GitHub Profile URL](https://github.com/shileyzee22)]
- 📧 [Email - olasileopeyemi3079@gmail.com]
---
 
*Last updated: April 2026*
