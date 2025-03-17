## Data preprocessing
### Time Period
- Original sample period: July 1963 to December 2016
- Update period(we want to analyze): January 2000 to December 2023
    - Sample: Jan 2000 to Dec 2016
    - Post: Jan 2017 to Dec 2023
### Predictors Chosen
 For the period from January 2000 to December 2023, a total of 62 predictors were utilized, compared to 50 predictors for the broader time frame spanning July 1963 to December 2023.
- Time: 
    - 07/1963-12/2023: 50 predictors
    - 01/2000-12/2023: 62 predictors
- ....
### Fama-Franch Data
- FF3/FF5/UMD all data from offical website

## Factor Momentum Strategy Construction(Table 2, Panel A)
### Defination
Factor momentum is a cross-sectional momentum strategy where factors are ranked by their past returns over a formation period (L), and long/short positions are taken in the best/worst performers for a holding period (H).
### Formation and Holding Periods:
Two combinations are tested: L=1, H=1 (1-month formation, 1-month holding) and L=6, H=6 (6-month formation, 6-month holding).
Two Long-Short Percentages: Long-Short top and bottom 20% & Long-Short top and bottom 20%.
### Portfolio Selection and Formation
The number of factors on the long and short sides is determined by the formula:
$n = \max \left\{ \text{round} \left(\frac{3}{20} \times N \right), 1 \right\}$  

- Forexampl: $N=62$ , $n = \max \left\{ \text{round} \left(\frac{3}{20} \times 62 \right), 1 \right\}  = 9$  
- ​Thus, each strategy takes long positions in the top 9 factors and short positions in the bottom 9 factors based on prior returns.
- Equal amounts are invested in each factor on the long and short sides

Handling Overlaps: For the 6-month holding period (H=6), returns overlap across months. The Jegadeesh and Titman (1993) methodology is used to address this:

- Each month, a new strategy is formed based on the prior 6-month returns.
- The holding period return is the average of six sub-strategies (formed in the current and prior five months), effectively rebalancing 1/6 of the portfolio monthly and avoiding overlapping observations in statistical tests.

### Annualized Return
- Annually return = Mean of the monthly return * 12
- Annually Std = Std of monthly return * sqr(12)
- T-Value: Newey-West T-value, lag month = H (holding time)

## Spanning Regressions (Table 2, Panel B)
- Dependent Variables: Use the monthly returns of the four strategies from Panel A (standard L=1 H=1, standard L=6 H=6, industry-adjusted L=1 H=1, industry-adjusted L=6 H=6).
- Independent Variables: Include MKTRF, SMB, HML, RMW, CMA, UMD from Fama-Franch
- Regression Setup: Run four time-series regressions:
    - Long-Short 40%, L=1,H=1
    - Long-Short 40%, L=6,H=6
    - Long-Short 20%, L=1,H=1
    - Long-Short 20%, L=6,H=6
- Outputs:
    - R^2
    - t-value
    - inercepts
    - factor loadings

## P&L curve
### Construct Momentum Strategies:
- Factor Momentum 
    - LS 40% (L=1, H=1), from Table 2
    - LS 20% (L=1, H=1), from Table 2
- Stock Momentum (UMD): Use the UMD factor from Ken French’s data library (based on prior 12-month returns skipping one month, held for one month).
- Market Factor : 
    - Utilize the market return data obtained from Ken French’s data library.
    - Employ the S&P 500 index as an additional benchmark.



    




