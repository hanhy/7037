# PS2


## Q1
Earnings of companies are typically announced outside of market hours. This practice allows investors time to digest the information and react in an orderly manner when the market reopens, reducing the risk of extreme volatility during trading hours. Stock prices for famous firms may often surge before post-close earnings announcements but then drop after the announcement, according to the research paper published by Harvard Business School in 2023. 

In contrast, macro announcements, such as non-farm payrolls, are usually released during the trading day. These announcements often have broad implications for the entire economy and financial markets, so releasing them during market hours ensures immediate price discovery and liquidity in the markets. These macro announcements can influence stock prices by affecting investor sentiment and expectations. 

For instance, if non-farm payrolls exceed expectations, stock prices may rise due to increased optimism about economic growth based on strong job growth. Similarly, decisions by the Federal Reserve, such as raising or lowering interest rates, can directly impact stock prices; a rate hike might cause prices to fall, while a rate cut could lead to a rise. A rate hike can reduce stock prices as borrowing becomes more expensive and future earnings are discounted at a higher rate. A rate cut can boost stock prices by lowering borrowing costs and increasing the present value of future earnings. Additionally, inflation rates play a crucial role—higher-than-expected inflation may lead to concerns about rising costs and tighter monetary policy, causing stock prices to fall. Some macro announcements disproportionately affect specific sectors. Rising interest rates may hurt interest-sensitive sectors like real estate or utilities. These examples illustrate how both corporate earnings and macroeconomic indicators can significantly shape stock market dynamics.

(Reference: Chen, Yixin and Cohen, Randolph B. and Wang, Zixuan (Kevin), Famous Firms, Earnings Clusters, and the Stock Market (September 5, 2020). Available at SSRN: https://ssrn.com/abstract=3685452 )

## Data Pre-processing
After reading the parquet file, we processed the data. The `date` column is then converted to a datetime format to ensure proper handling of time-based operations. The dataset is filtered to include only records from January 1, 1993, onwards. Next, the data is further cleaned by removing rows with missing values in key columns such as `ret`, `intraday_ret_month`, `overnight_ret_month`, `mom`, `mom_intraday`, `mom_overnight`, and `mcap_lag1`, while also ensuring that the absolute value of `prc_lag1` is at least 1. Additionally, the dataset is filtered to include only those `permno` identifiers that have at least 8 observations, ensuring sufficient data for analysis. The index is reset to maintain a clean, sequential structure, and the data is sorted by the `date` column to ensure chronological order. The resulting cleaned and filtered dataset is stored in the variable `hw2_data`.

## Q2
The correlation of intraday, overnight momentum and regular momentum is shown in the following table:

<img width="799" alt="Correlation table" src="https://github.com/user-attachments/assets/92e6cd9d-fcfc-492e-9c1f-b746dfd46a39" />

_Table: Correlation of intraday, overnight momentum and regular momentum_

The correlation matrix shows that intraday momentum has a positive correlation with regular momentum (0.5818) and a negative correlation with overnight momentum (-0.6637). Regular momentum and overnight momentum also exhibit a positive correlation of 0.1546. 

## Q3

## Q4
We used `apply_quantiles` and `produce_table` functions provided in the starter code. Before we apply quantiles, we further processed the data. We first filtered out stocks in the bottom 20% of market capitalization and those with price less than 5. Then, we constructed 3 portfolios (each containing 10 deciles) by sorting data based on `mom`, `mom_intraday`, and `mom_overnight` respectively. To produce the required tables, we need to calculate portfolios and pnl (for long-short portfolio), so we define a function. 

The function `calculate_portfolios_and_pnl` is designed to compute portfolio returns and PNL (Profit and Loss) for a given dataset. It takes two inputs: data, which contains financial data, and bin_column. The function first calculates equal-weighted (EW) and value-weighted (VW) returns for the portfolios, including total returns, intraday returns, and overnight returns. The value-weighted returns are computed using the lagged market capitalization (mcap_lag1) as weights. Next, the function calculates the PNL for a long-short portfolio by subtracting the returns of the first portfolio (e.g., lowest decile) from the returns of the last portfolio (e.g., highest decile) for each date. The function returns a dictionary containing two DataFrames: bin, which holds the portfolio returns, and pnl, which contains the long-short PNL results. 

After calculating portfolios and PNL for each portfolio, we produced 3 tables for portfolio sorted by regular momentum, intraday momentum and overnight momentum respectively. Here are the results:

<img width="987" alt="Table for portfolio sorted by regular momentum" src="https://github.com/user-attachments/assets/5a750f17-5e2f-4edf-a1e2-900e3ac9b56d" />

_Table for portfolio sorted by regular momentum_

<img width="1107" alt="Table for portfolio sorted by intraday momentum" src="https://github.com/user-attachments/assets/131196f8-cc54-4063-b29c-ae5ce4963291" />

_Table for portfolio sorted by intraday momentum_

<img width="1113" alt="Table for portfolio sorted by overnight momentum" src="https://github.com/user-attachments/assets/cf2d1112-815c-42ea-850e-33212c6b3d15" />

_Table for portfolio sorted by overnight momentum_

Based on the tables produced, the intraday momentum predicts the intraday future returns more than the overnight returns. A hedge portfolio based on past one-month intraday returns earns an average EW intraday return of 4.939% per month with an associated t-statistic of 16.75. However, hedge portfolio based on past one-month intraday returns earns an average EW overnight return of -3.94% per month with an associated t-statistic of -21.658. The intraday momentum also predicts the total returns more than the overnight returns, but less than the intraday future returns.

The overnight momentum predicts the overnight returns more than the intraday future returns. A hedge portfolio based on past one-month overnight returns earns an average EW intraday return of -5.758% per month with an associated t-statistic of -21.75. However, hedge portfolio based on past one-month overnight returns earns an average EW overnight return of 5.733% per month with an associated t-statistic of 26.55. The overnight momentum also predicts the total returns more than the intraday future returns, but less than the overnight returns.

## Q4 Extra Credit Answer
We first read the Fama French factor file named `h1_factors.parquet`. We converted the `dt` column to datetime format, and sorted the data chronologically. The market return (`Mkt`) is computed as the sum of the market risk premium (`mkt_rf`) and the risk-free rate (`rf`), and all return values are converted to decimal format by dividing by 100. Missing data is removed to ensure consistency. Finally, the daily market returns are aggregated to monthly frequency by computing the cumulative monthly return. The resulting monthly market returns are stored in a new DataFrame, `ff_data_mkt`, which is reset to a standard DataFrame (not indexed by `dt`). This step ensures alignment with monthly momentum data for further analysis.

For regular momentum portfolio data, we first made a copy of the momentum portfolio data (`portfolios_mom['bin']`) and adjusts the date column to align with the end of the month using `MonthEnd(0)`.  The data is then sorted chronologically and merged with the monthly Fama French market return data (`ff_data_mkt`) on the date column. Next, the code filters the data to include only the top portfolio (bin 10) and creates a winner column to identify months where the equal-weighted return (`ew`) of the portfolio exceeds the market return (`Mkt`). We then calculated the monthly fraction of winners in the top portfolio (bin 10) that outperform the market return (`Mkt`) and then computes the average fraction of winners across all months. The average fraction of winners for portfolio 10 that outperforms the market return is 56.3%.  

Besides, we also try to see the probability of other portfolios (bins 1 to 9) beating the market return. From the result, all bins from 1 to 9 are having lower probability of winning the market return than bin 10. Also, the average fraction of winner of all bins across all months is 49.46%, which is lower than bin 10. It shows that the portfolio with the highest momentum has a higher chance of beating the market return than other lower momentum's portfolios.

<img width="122" alt="Bin1to9" src="https://github.com/user-attachments/assets/ba70ed70-ca0a-41e2-a0f7-7f47f6d3763c" />

_Table: Average fraction of winner from bin 1 to 9_

The portfolios are necessary in our quant trading strategies because the portfolios allow us to diversify our investments and reduce the risk of investing in a single stock. If we just pick one stock, we are exposed to the risk of that stock. The chance of a single stock beating the market return is not consistent, sometimes surge to a very high level, but sometimes very low. By investing in a portfolio of stocks, we can reduce the risk of investing in a single stock and have higher overall chance of beating the market return.

## Q5

## Q6

## Q7

## Q8



## Q9
