# Project 4 - Colorado and Texas Risk Comparison

## Summary

This project analyzes FEMA National Risk Index (NRI) census tract data for Colorado and Texas and compares published NRI hazard risk outputs against custom risk models.

The notebook computes and compares:

- Flood risk (RFLD): NRI score & ranking vs custom score & ranking
- Wildfire risk (WFIR): NRI score & ranking vs custom score & ranking
- Top 10 highest-risk tracts by state using side-by-side bar charts

Primary notebook:

- CIVE_202_Project4_FINALCODE.ipynb

---

## Data Inputs

The notebook expects these files in the same folder:

- NRI_Shapefile_CensusTracts.shp (+ .dbf, .shx, .prj, .cpg, .sbn, .sbx)
  - This is a Canvas upload due to file size limitations
- NRI_Table_CensusTracts_Colorado.csv
- NRI_Table_CensusTracts_Texas.csv
- Colorado_SVI.csv
- Texas_SVI.csv

---

## Python Libraries

- pandas
- numpy
- matplotlib
- seaborn
- geopandas
- mapclassify
- IPython.display

Install any missing packages before running.

---

## Workflow Within Notebbook

### Data loading

1. Load the national NRI tract shapefile
2. Filter to Colorado and Texas
3. Define hazard groups: wildfire (WFIR) and flood (RFLD)

### SVI and NRI table preparation

1. Read state SVI and NRI tables
2. Build normalized 11-digit GEOID keys
3. Merge SVI + NRI attributes by GEOID
4. Fill missing numeric values with per-column medians

### Normalization and custom risk equations

1. Normalize key inputs to 0-1 with min-max scaling
2. Compute transformed terms (including log1p for AFREQ/EALT)
3. Build custom weighted hazard scores using variability-based weights

### Classification

1. Apply Jenks natural breaks (k=5)
2. Assign ordered labels:
   1. Very Low, Relatively Low, Relatively Moderate, Relatively High, Very High

### Visualization and comparisons

1. Flood:
   1. Colorado: Score and Ranking maps
   2. Texas: Score and Ranking maps
2. Wildfire:
   1. Colorado: Score and Ranking maps
   2. Texas: Score and Ranking maps
3. Summary comparison charts:
   1. Top 10 highest wildfire tracts (CO and TX)
   2. Top 10 highest flood tracts (CO and TX)

---

## Custom Risk Equation

The notebook uses this custom equation:

$$
{Risk} = w_1 \cdot \text{AFREQ}_{\text{norm}} + w_2 \cdot \log(1 + \text{EAL}_{\text{norm}}) + w_3 \cdot \text{SVI}_{\text{norm}} + w_4 \cdot (1 - \text{RESL}_{\text{norm}})^2
$$

Where:

- AFREQ = annualized frequency term
- EAL = expected annual loss term
- SVI = social vulnerability indicator
- RESL = community resilience indicator
- Weights (w1, w2, w3, w4) are based on term variability (standard deviation normalization)
  - Briefly: each term's standard deviation is computed, then divided by the sum of all term standard deviations so the weights add to 1. We use the assumption that terms with more variation across tracts receive larger weights.

---

## Expected Outputs

When run top-to-bottom, the notebook produces:

- Side-by-side state maps comparing NRI vs custom flood scores for both states
- Side-by-side state maps comparing NRI vs custom flood rankings for both states
- Side-by-side state maps comparing NRI vs custom wildfire scores for both states
- Side-by-side state maps comparing NRI vs custom wildfire rankings for both states
- Two summary bar-chart panels for top 10 tracts (wildfire and flood) for both states

---

## Run Instructions

1. Open CIVE_202_Project4_FINALCODE.ipynb
2. Confirm all required data files are in the same parent folder
3. Run cells from top to bottom
4. Review generated maps and bar charts in order
