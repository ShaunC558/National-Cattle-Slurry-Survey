Analysis script: combined_analysis_2023_2024.R

R script used for the statistical analysis and figures in the Teagasc Signpost Programme National Cattle Slurry Characteristics Survey 2023-2024. It cleans the laboratory results, applies quality control, compares Irish cattle slurry with Berry et al. (2013) and Nitrates Action Programme (NAP) values, fits the regression models, and produces the report tables and figures.

What it does
Step	Section in script	Description
1	Load and clean 2023 and 2024 data	Reads the two laboratory workbooks, standardises enterprise names, extracts county (2023 only), converts laboratory units to kg per tonne (units per 1,000 gal divided by 9; 1 unit = 0.5 kg, 1,000 gal = 4.5 t)
2	Combine and prepare	Merges years, assigns species and province (province is "Unknown" when county is missing)
3	Quality control (cattle)	Removes samples with DM above 16%, then removes IQR outliers (1.5 x IQR) on DM, available N, P and K. Pig and poultry rows with missing pH are dropped
4	Year comparison	Wilcoxon rank-sum tests (2023 vs 2024) and Type II ANOVA with year as a factor
5	Combined cattle analysis	Summary statistics (mean, SD, SE, 95% CI, median, range), beef vs dairy tests, comparison with Berry (2013) and NAP values, DM-normalised comparison, estimated Total N under different N-fraction assumptions
6	GLM	Linear models (DM + pH + enterprise + province + year) with Type II ANOVA, Shapiro-Wilk and Breusch-Pagan checks, forest plot
7	Regression and DM lookup	Nutrient ~ DM regressions and the DM lookup table (DM 2 to 14%)
8	Figures	Boxplots, DM scatter plots, Berry comparisons, percentage change, DM distribution, residual diagnostics
9	Map	Sample counts by county (2023 only)
10	pH analysis	Simple regressions of nutrients on pH
11	Pig and poultry	Descriptive summaries only (very small samples)
Inputs

The script expects two laboratory workbooks (Southern Scientific Services results, 2023 sheet "Data" and 2024 sheet "Sheet1"). These contain farm and adviser details and are not distributed in this repository. The anonymised outcome of this script is the public dataset in data/.

Set the paths at the top of the script before running (file_2023, file_2024, out_dir).

Requirements

R 4.2 or later with: readxl, ggplot2, dplyr (1.1.0 or later, for case_match), tidyr, gridExtra, scales, broom, car, lmtest, sf, maps.

r
install.packages(c("readxl","ggplot2","dplyr","tidyr","gridExtra","scales",
                   "broom","car","lmtest","sf","maps"))
Running
r
source("analysis/combined_analysis_2023_2024.R")

Outputs are written to output/.

Outputs
File	Content
Table1_Summary_Stats.csv	Overall cattle summary statistics
Table2_By_Enterprise.csv	Summary by beef and dairy
Table3_Comparison.csv	Comparison with Berry (2013), pooled literature and NAP
Table4_Normalised.csv	Nutrients per % DM vs Berry (2013)
Table5_TotalN_Range.csv	Estimated Total N under 40, 50, 58.3 and 65% N-fraction assumptions
Table8_DM_Lookup.csv	Predicted nutrient content by DM
Table_Year_Tests.csv	2023 vs 2024 Wilcoxon tests
Table_Pig_Summary.csv, Table_Poultry_Summary.csv	Pig and poultry descriptive statistics
cattle_combined_clean.csv, pig_clean.csv, poultry_clean.csv	Cleaned data after QC
fig_*.png, fig1 to fig7	Figures (300 dpi)
Key settings and reference values (edit at top of script)
Unit conversion divisor: CONV = 9
Cattle DM cap: 16%; outlier rule: 1.5 x IQR
Berry et al. (2013): DM 6.30%, N 1.40 (available), P 0.50, K 3.50 kg/t; Total N 2.4 kg/t
NAP: Total P 0.8 kg/t; Total N 5.0 kg/t (cattle); pig Total N 4.2, Total P 0.8 kg/t
Limitations
County is available for 2023 samples only, so province-level terms rely on a subset.
Convenience sample, not probability-based; storage type, storage duration and cover are not recorded.
Pig (n = 6) and poultry (n = 2) results are descriptive only.
Available N is a laboratory-derived value (about 40% of Total N), not measured NH4-N.
