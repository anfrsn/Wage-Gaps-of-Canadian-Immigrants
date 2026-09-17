# Wage-Gaps-of-Canadian-Immigrants

Immigrant Wage Gap In Canada's Major Cities

## Research Question:
Is there a wage gap between immigrants and non-immigrants for those who are currently employed in Canada?
Does this gap differ in Canada's 9 largest metro areas (Québec, Montréal, Ottawa–Gatineau (Ontario part), Toronto, Hamilton, Winnipeg, Calgary, Edmonton, Vancouver) compared to everywhere else?


## Data
Source: Statistics Canada, Labour Force Survey (LFS), Public Use Microdata File (PUMF)<br>
Time period: June 2026<br>
Sample Size: 58,516 employed employees<br>

Adapted from Statistics Canada, Labour Force Survey PUMF (This project is NOT endorsed by Statistics Canada).

## Method 
Outcome: log(hourly wage) - 'HRLYEARN' was converted from raw units to dollars (this is because the PUMF reports wages with two implied decimals)<br>
Predictors:<br>
'Immigrant' - 1 if landed as an immigrant (any amount of time), 0 otherwise<br>
'Urban' - 1 if employee is living in one of the 9 largest metro areas, 0 otherwise<br>
 Age group was also included as a control, since wages naturally rise and fall across different age brackets<br>

The model was weighted using Statistics Canada's survey weight (FINALWT), which the LFS User Guide requires for any analysis of this data. Standard errors were also used to compensate for the fact that the wage data doesn't have constant variance.


## Important Findings
Immigrants on average earn 8.9% less than non-immigrants, holding all other variables (region and age) constant<br>
Wages are roughly 8.6% higher in large metro areas regardless of immigrant status<br>
In major cities, the immigrant wage gap was found to be significantly wider: about 7.1% outside major cities compared to around 9.7% within them (p-value = 0.0019)<br>
Immigrants are also more than twice as likely to live in a major metro area than non-immigrants (55.1% vs. 23.5%), meaning immigrants are concentrated in exactly the regions where the wage gap is widest<br>

The boxplot shows a pattern, where major cities generally have higher wages, which are slightly lower for immigrants within each region. However, it's unable to show that the immigrant wage gap is about 2.5 percentage points wider inside major cities than outside them. That difference only shows up once the regression model accounts for other variables/factors like age, which a simple chart of raw wages cannot show. 

## Limitations
'Urban' only includes the 9 largest metro areas on the PUMF<br>
Smaller sample size: Only the month of June 2026 was included within the project.<br>
The models used do not account for education, occupation, industry, or years since immigration. These variables could most likely explain parts of the wage gap.

## Files
**CAimmigrant_wage_analysis.md** - Knitted output will ALL results, tables, and visible boxplot<br>
**pub2026** - dataset provided by Statistics Canada<br>
**2026-06-CSV** - original zip file<br>
**CAimmigrant_wage_analysis_files** - Includes boxplot<br>
