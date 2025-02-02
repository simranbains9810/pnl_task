# Profit and Loss Pipeline 
This project presents a pipeline that takes PnL data and processes, cleans, and calculates Value at Risk (VaR) using the historical method at different confidence levels. The results are stored following a Medallion Architecture (Bronze, Silver and Gold) to logically organise data into a lakehouse, with the goal of improving structure and quality of data as it flows through each layer of the architecture. 

![Alt text](https://lh5.googleusercontent.com/proxy/yltwNDMMXGvhUPvZKsBspX8xR5X6bQR9HdqS_IU2GGNyFm2G8L8VKucLoj4PveCfZVU0qYqNjz7Xpfxhisi2T55E27gOGf2hECTZAHpKsUC3MVdLN9yPVhkCf2-osC52q8PvPjAbR2LIQWVPC2ZTvPRXVRFRIvB6DlYYjOeSfWSMbDZbM4qi53p4GGNDusU)

## Getting Started

The code is written using Python 3.12 and requires the following packages and tools to be installed:

- `numpy`
- `pandas`
- `matplotlib`
- `seaborn`
- `datetime`
- `os`
- `google.colab.files`

## Approach 
The pipeline follows the standard ETL approach which ingests, cleans, transforms and analyses the PnL data. 
1. Data Ingestion: PnL data is loaded from the CSV.
2. Parsing, Cleaning & Transformations: The dataset is cleaned by handling missing values, renaming and standardising columns, and adding data processing timestamps.
3. Value at Risk (VaR) Calculation: Computes Value at Risk (VaR) at different confidence levels (95%, 97.5& and 99%) per trade and book.
4. Outputs and Data Storage: The Medallion Pattern approach is used to store cleaned data, results and insights in different layers (Bronze, Silver and Gold).

## Assumptions Made
1. The format of the trade data follows a consistent structure with `TradeID`, `PortfolioId`, `businessDate`, and `PnL` columns. This suggests each trade has a valid identity and values are in a parseable format and converted to `YYYY-MM-DD`.
2. Missing values have been left blank and any outliers or extreme values have been considered in the distribution to reflect the normal behaviour of PnL trends. 
3. The historical simulation approach has been used, which relies a lot on historical data of the returns for which the VaR is being calculated. It is founf to provide superior unconditional coverage among a wide variety of alternate methods ranging from the simple variance covariance approach to the sophisticated GARCH. The VaR calculations treat trades independently without considering interdependencies between books or trades. 
4. The computed values under VaR reflect percentiles from historical data without assuming normality.
5. Various confidence levels were used to assess VaR. Interpretations could imply:

    a. 95% Confidence Level (Per Trade): Indicates expected loss per trade under normal conditions. Some trades show significant losses, while others remain unaffected.

    b. 97.5% Confidence Level (Per Trade): A stricter threshold than 95%, showing higher potential losses in extreme cases.

    c. 99% Confidence Level (Per Book): Aggregates risk at the book level, helping to identify which books carry the highest volatility.
    
    d. 95% Confidence Level (Across All Trades): Reflects overall portfolio exposure. A high negative value indicates significant risk across trades, whereas a smaller value suggests diversification benefits.