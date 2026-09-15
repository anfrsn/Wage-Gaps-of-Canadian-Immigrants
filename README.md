# Wage-Gaps-of-Canadian-Immigrants

Immigrant Wage Gap In Canada's Major Cities

Research Question:
Is there a wage gap between immigrants and non-immigrants for those who are currently employed in Canada?
Does this gap differ in Canada's 9 largest metro areas (Québec, Montréal, Ottawa–Gatineau (Ontario part), Toronto, Hamilton, Winnipeg, Calgary, Edmonton, Vancouver) compared to everywhere else?


DATA
Source: Statistics Canada, Labour Force Survey (LFS), Public Use Microdata File (PUMF)
Time period: June 2026
Sample Size: 58,516 employed employees

Adapted from Statistics Canada, Labour Force Survey PUMF (This project is NOT endorsed by Statistics Canada).

Method Used:
Outcome: log(hourly wage) - 'HRLYEARN' was converted from raw units to dollars (this is because the PUMF reports wages with two implied decimals)
Predictors:
'Immigrant' - 1 if landed as an immigrant (any amount of time), 0 otherwise
'Urban' - 1 if employee is living in one of the 9 largest metro areas, 0 otherwise
 Age group was also included as a control, since wages naturally rise and fall across different age brackets

The model was weighted using Statistics Canada's survey weight (FINALWT), which the LFS User Guide requires for any analysis of this data. Standard errors were also used to compensate for the fact that the wage data doesn't have constant variance.


IMPORTANT FINDINGS
Immigrants on average earn 8.9% less than non-immigrants, holding all other variables (region and age) constant
Wages are roughly 8.6% higher in large metro areas regardless of immigrant status
In major cities, the immigrant wage gap was found to be significantly wider: about 7.1% outside major cities compared to around 9.7% within them (p-value = 0.0019)
Immigrants are also more than twice as likely to live in a major metro area than non-immigrants (55.1% vs. 23.5%), meaning immigrants are concentrated in exactly the regions where the wage gap is widest

The boxplot shows a pattern, where major cities generally have higher wages, which are slightly lower for immigrants within each region. However, it's unable to show that the immigrant wage gap is about 2.5 percentage points wider inside major cities than outside them. That difference only shows up once the regression model accounts for other variables/factors like age, which a simple chart of raw wages cannot show. 

LIMITATIONS
'Urban' only includes the 9 largest metro areas on the PUMF
Smaller sample size: Only the month of June 2026 was included within the project.
The models used do not account for education, occupation, industry, or years since immigration. These variables could most likely explain parts of the wage gap.


FILES
CAimmigrant_wage_analysis.md - Knitted output will ALL results, tables, and visible boxplot \n
pub2026 - dataset provided by Statistics Canada \n
2026-06-CSV - original zip file \n
CAimmigrant_wage_analysis_files - Includes boxplot \n
