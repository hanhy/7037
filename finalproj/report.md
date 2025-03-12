## Data and Factors


## Factor Momentum Strategy Construction
### Defination
Factor momentum is a cross-sectional momentum strategy where factors are ranked by their past returns over a formation period (L), and long/short positions are taken in the best/worst performers for a holding period (H).
### Formation and Holding Periods:
Two combinations are tested: L=1, H=1 (1-month formation, 1-month holding) and L=6, H=6 (6-month formation, 6-month holding).
### Portfolio Selection
The number of factors on the long and short sides is determined by the formula:
$n = \max \left\{ \text{round} \left(\frac{3}{20} \times N \right), 1 \right\}, N = 62.$  

- $N=$ , $n = \max \left\{ \text{round} \left(\frac{3}{20} \times 50 \right), 1 \right\} = \max \left\{ \text{round} (7.5), 1 \right\} = 8$  
- ​Thus, each strategy takes long positions in the top 8 factors and short positions in the bottom 8 factors based on prior returns.
- Equal amounts are invested in each factor on the long and short sides

Handling Overlaps: For the 6-month holding period (H=6), returns overlap across months. The Jegadeesh and Titman (1993) methodology is used to address this:

- Each month, a new strategy is formed based on the prior 6-month returns.
- The holding period return is the average of six sub-strategies (formed in the current and prior five months), effectively rebalancing 1/6 of the portfolio monthly and avoiding overlapping observations in statistical tests.


