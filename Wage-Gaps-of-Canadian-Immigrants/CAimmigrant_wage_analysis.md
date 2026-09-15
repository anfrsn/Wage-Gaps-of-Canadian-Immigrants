lfs_wage_analysis
================
Andrew Park
2026-09-15

``` r
library(sandwich)
```

    ## Warning: package 'sandwich' was built under R version 4.6.1

``` r
library(lmtest)
```

    ## Warning: package 'lmtest' was built under R version 4.6.1

    ## Loading required package: zoo

    ## Warning: package 'zoo' was built under R version 4.6.1

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

``` r
lfs_raw_data <- read.csv("pub0626.csv")

str(lfs_raw_data) # Checks for strings
```

    ## 'data.frame':    114034 obs. of  60 variables:
    ##  $ REC_NUM : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ SURVYEAR: int  2026 2026 2026 2026 2026 2026 2026 2026 2026 2026 ...
    ##  $ SURVMNTH: int  6 6 6 6 6 6 6 6 6 6 ...
    ##  $ LFSSTAT : int  1 1 4 4 1 4 1 1 1 4 ...
    ##  $ PROV    : int  13 48 24 35 35 35 13 35 48 10 ...
    ##  $ CMA     : int  0 8 0 0 0 0 0 4 7 0 ...
    ##  $ AGE_12  : int  1 7 12 12 5 9 4 4 1 12 ...
    ##  $ AGE_6   : int  2 NA NA NA NA NA NA NA 2 NA ...
    ##  $ GENDER  : int  2 1 1 1 1 1 2 2 2 1 ...
    ##  $ MARSTAT : int  6 2 1 1 1 1 6 2 6 1 ...
    ##  $ EDUC    : int  2 6 6 1 1 5 6 5 3 4 ...
    ##  $ MJH     : int  1 1 NA NA 1 NA 2 1 1 NA ...
    ##  $ EVERWORK: int  NA NA 2 2 NA 1 NA NA NA 2 ...
    ##  $ FTPTLAST: int  NA NA NA NA NA 1 NA NA NA NA ...
    ##  $ COWMAIN : int  2 1 NA NA 3 2 2 2 2 NA ...
    ##  $ IMMIG   : int  3 3 3 3 3 3 3 1 3 3 ...
    ##  $ NAICS_21: int  18 21 NA NA 6 12 7 16 19 NA ...
    ##  $ NOC_10  : int  6 5 NA NA 8 3 8 6 7 NA ...
    ##  $ NOC_43  : int  29 19 NA NA 35 11 38 27 34 NA ...
    ##  $ YABSENT : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ WKSAWAY : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ PAYAWAY : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ UHRSMAIN: int  350 380 NA NA 400 NA 400 70 300 NA ...
    ##  $ AHRSMAIN: int  350 260 NA NA 400 NA 400 70 300 NA ...
    ##  $ FTPTMAIN: int  1 1 NA NA 1 NA 1 2 1 NA ...
    ##  $ UTOTHRS : int  350 380 NA NA 400 NA 750 70 300 NA ...
    ##  $ ATOTHRS : int  350 260 NA NA 400 NA 720 70 300 NA ...
    ##  $ HRSAWAY : int  0 150 NA NA NA NA 0 0 0 NA ...
    ##  $ YAWAY   : int  NA 3 NA NA NA NA NA NA NA NA ...
    ##  $ PAIDOT  : int  0 0 NA NA NA NA 0 0 0 NA ...
    ##  $ UNPAIDOT: int  0 30 NA NA NA NA 0 0 0 NA ...
    ##  $ XTRAHRS : int  0 30 NA NA NA NA 0 0 0 NA ...
    ##  $ WHYPT   : int  NA NA NA NA NA NA NA 5 NA NA ...
    ##  $ TENURE  : int  24 124 NA NA 173 NA 10 22 5 NA ...
    ##  $ PREVTEN : int  NA NA NA NA NA 240 NA NA NA NA ...
    ##  $ HRLYEARN: int  1690 12045 NA NA NA NA 1700 4000 1500 NA ...
    ##  $ UNION   : int  3 3 NA NA NA NA 3 3 3 NA ...
    ##  $ PERMTEMP: int  2 1 NA NA NA NA 4 1 1 NA ...
    ##  $ ESTSIZE : int  1 2 NA NA NA NA 2 1 2 NA ...
    ##  $ FIRMSIZE: int  1 3 NA NA NA NA 2 2 4 NA ...
    ##  $ DURUNEMP: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ FLOWUNEM: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ UNEMFTPT: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ WHYLEFTO: int  NA NA NA NA NA 5 NA NA NA NA ...
    ##  $ WHYLEFTN: int  NA NA NA NA NA 7 NA NA NA NA ...
    ##  $ DURJLESS: int  NA NA 198 55 NA 2 NA NA NA 240 ...
    ##  $ AVAILABL: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LKPUBAG : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LKEMPLOY: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LKRELS  : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LKATADS : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LKANSADS: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LKOTHERN: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ PRIORACT: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ YNOLOOK : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ TLOLOOK : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ SCHOOLN : int  1 1 NA NA 1 1 1 1 1 NA ...
    ##  $ EFAMTYPE: int  6 2 11 11 3 11 3 2 4 11 ...
    ##  $ AGYOWNK : int  NA NA NA NA 2 NA NA NA NA NA ...
    ##  $ FINALWT : int  167 874 280 186 163 150 101 1099 742 61 ...

``` r
colnames(lfs_raw_data) #Looking at Col names (update this later)
```

    ##  [1] "REC_NUM"  "SURVYEAR" "SURVMNTH" "LFSSTAT"  "PROV"     "CMA"     
    ##  [7] "AGE_12"   "AGE_6"    "GENDER"   "MARSTAT"  "EDUC"     "MJH"     
    ## [13] "EVERWORK" "FTPTLAST" "COWMAIN"  "IMMIG"    "NAICS_21" "NOC_10"  
    ## [19] "NOC_43"   "YABSENT"  "WKSAWAY"  "PAYAWAY"  "UHRSMAIN" "AHRSMAIN"
    ## [25] "FTPTMAIN" "UTOTHRS"  "ATOTHRS"  "HRSAWAY"  "YAWAY"    "PAIDOT"  
    ## [31] "UNPAIDOT" "XTRAHRS"  "WHYPT"    "TENURE"   "PREVTEN"  "HRLYEARN"
    ## [37] "UNION"    "PERMTEMP" "ESTSIZE"  "FIRMSIZE" "DURUNEMP" "FLOWUNEM"
    ## [43] "UNEMFTPT" "WHYLEFTO" "WHYLEFTN" "DURJLESS" "AVAILABL" "LKPUBAG" 
    ## [49] "LKEMPLOY" "LKRELS"   "LKATADS"  "LKANSADS" "LKOTHERN" "PRIORACT"
    ## [55] "YNOLOOK"  "TLOLOOK"  "SCHOOLN"  "EFAMTYPE" "AGYOWNK"  "FINALWT"

``` r
cols_of_interest <- c("PROV", "LFSSTAT", "CMA", "AGE_12", "GENDER", "EDUC", "IMMIG", "COWMAIN", "HRLYEARN", "FINALWT")
lfs <- lfs_raw_data[, cols_of_interest]
```

# 

# PROV - province code (10=NL, 11=PE, 12=NS, 13=NB, 24=QC,

# 35=ON, 46=MB, 47=SK, 48=AB, 59=BC)

# LFSSTAT - labour force status (1/2=employed, 3=unemployed, 4=NILF)

# CMA - census metro area, limited to the 9 largest on the PUMF

# AGE_12 - age group with 12 categories

# GENDER - 1=Men+, 2=Women+

# EDUC - educational attainment

# IMMIG - immigrant status

# COWMAIN - class of worker 1/2 = employees

# HRLYEARN - hourly earnings (2 implied decimals)

# FINALWT - survey weight

``` r
# Cleaning the data
lfs_clean_data <- lfs[lfs$LFSSTAT %in% c(1,2) & lfs$COWMAIN %in% c(1,2) & !is.na(lfs$HRLYEARN) & lfs$HRLYEARN > 0 &
                        !is.na(lfs$IMMIG), ]
lfs_clean_data$HRLYEARN <- lfs_clean_data$HRLYEARN / 100 # 2 decimal points

lfs_clean_data$Immigrant <- ifelse(lfs_clean_data$IMMIG %in% c(1, 2), 1, 0)
lfs_clean_data$Urban    <- ifelse(lfs_clean_data$CMA != 0, 1, 0)
lfs_clean_data$log_wage <- log(lfs_clean_data$HRLYEARN)

# Sanity Check
table(lfs_clean_data$Immigrant)
```

    ## 
    ##     0     1 
    ## 45289 13227

``` r
table(lfs_clean_data$Urban)
```

    ## 
    ##     0     1 
    ## 40569 17947

``` r
table(lfs_clean_data$Immigrant, lfs_clean_data$Urban)
```

    ##    
    ##         0     1
    ##   0 34634 10655
    ##   1  5935  7292

``` r
# Regression Models

lfs_clean_data$log_wage <- log(lfs_clean_data$HRLYEARN)
str(lfs_clean_data$log_wage)
```

    ##  num [1:58516] 2.83 4.79 2.83 3.69 2.71 ...

``` r
base_model <- lm(log_wage ~ Immigrant + Urban + factor(AGE_12), data = lfs_clean_data, weights = FINALWT)
summary(base_model)
```

    ## 
    ## Call:
    ## lm(formula = log_wage ~ Immigrant + Urban + factor(AGE_12), data = lfs_clean_data, 
    ##     weights = FINALWT)
    ## 
    ## Weighted Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -48.012  -4.444  -0.687   3.630  65.578 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       2.889799   0.008227  351.26   <2e-16 ***
    ## Immigrant        -0.093207   0.004091  -22.78   <2e-16 ***
    ## Urban             0.082650   0.003679   22.46   <2e-16 ***
    ## factor(AGE_12)2   0.209566   0.009750   21.50   <2e-16 ***
    ## factor(AGE_12)3   0.481648   0.009588   50.23   <2e-16 ***
    ## factor(AGE_12)4   0.624091   0.009501   65.69   <2e-16 ***
    ## factor(AGE_12)5   0.717599   0.009595   74.79   <2e-16 ***
    ## factor(AGE_12)6   0.761274   0.009701   78.47   <2e-16 ***
    ## factor(AGE_12)7   0.744284   0.009868   75.43   <2e-16 ***
    ## factor(AGE_12)8   0.733302   0.009987   73.42   <2e-16 ***
    ## factor(AGE_12)9   0.719653   0.010233   70.33   <2e-16 ***
    ## factor(AGE_12)10  0.613327   0.010741   57.10   <2e-16 ***
    ## factor(AGE_12)11  0.570630   0.013169   43.33   <2e-16 ***
    ## factor(AGE_12)12  0.433841   0.016863   25.73   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 7.655 on 58502 degrees of freedom
    ## Multiple R-squared:  0.1973, Adjusted R-squared:  0.1971 
    ## F-statistic:  1106 on 13 and 58502 DF,  p-value: < 2.2e-16

``` r
# Regression Model 2
# Interacting Immigrant * Urban

interaction_model <- lm(log_wage ~ Immigrant * Urban + factor(AGE_12), data = lfs_clean_data, weights = FINALWT)
summary(interaction_model)
```

    ## 
    ## Call:
    ## lm(formula = log_wage ~ Immigrant * Urban + factor(AGE_12), data = lfs_clean_data, 
    ##     weights = FINALWT)
    ## 
    ## Weighted Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -48.120  -4.447  -0.678   3.632  65.778 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       2.886698   0.008286 348.363  < 2e-16 ***
    ## Immigrant        -0.074177   0.007356 -10.083  < 2e-16 ***
    ## Urban             0.088850   0.004183  21.238  < 2e-16 ***
    ## factor(AGE_12)2   0.209128   0.009750  21.449  < 2e-16 ***
    ## factor(AGE_12)3   0.481365   0.009588  50.206  < 2e-16 ***
    ## factor(AGE_12)4   0.624058   0.009500  65.687  < 2e-16 ***
    ## factor(AGE_12)5   0.717674   0.009594  74.802  < 2e-16 ***
    ## factor(AGE_12)6   0.761390   0.009700  78.492  < 2e-16 ***
    ## factor(AGE_12)7   0.744453   0.009867  75.449  < 2e-16 ***
    ## factor(AGE_12)8   0.733649   0.009987  73.459  < 2e-16 ***
    ## factor(AGE_12)9   0.720194   0.010234  70.373  < 2e-16 ***
    ## factor(AGE_12)10  0.614079   0.010743  57.163  < 2e-16 ***
    ## factor(AGE_12)11  0.571451   0.013171  43.387  < 2e-16 ***
    ## factor(AGE_12)12  0.434654   0.016863  25.775  < 2e-16 ***
    ## Immigrant:Urban  -0.027445   0.008818  -3.112  0.00186 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 7.654 on 58501 degrees of freedom
    ## Multiple R-squared:  0.1974, Adjusted R-squared:  0.1972 
    ## F-statistic:  1028 on 14 and 58501 DF,  p-value: < 2.2e-16

``` r
# F test: Does the Immigrant * Urban significantly improve the existing model?
anova(base_model, interaction_model)
```

    ## Analysis of Variance Table
    ## 
    ## Model 1: log_wage ~ Immigrant + Urban + factor(AGE_12)
    ## Model 2: log_wage ~ Immigrant * Urban + factor(AGE_12)
    ##   Res.Df     RSS Df Sum of Sq      F   Pr(>F)   
    ## 1  58502 3428063                                
    ## 2  58501 3427495  1    567.55 9.6871 0.001857 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# Visualizing the Data
boxplot(log_wage ~ Immigrant + Urban, data = lfs_clean_data,
        names = c("Non-Imm.\nNon-CMA", "Imm.\nNon-CMA", "Non-Imm.\nCMA", "Imm.\nCMA"),
        ylab = "Log Hourly Wage",
        main = "Log Wage by Immigrant Status and Urban/Rural Region")
```

![](CAimmigrant_wage_analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
