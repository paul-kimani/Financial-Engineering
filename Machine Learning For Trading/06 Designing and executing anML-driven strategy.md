![[Screenshot 2026-08-15 at 09.29.07.png]]

### Sourcing and managing data
 The proliferating supply of data requires careful selection and management to uncover the potential value, including the following steps:
 1. Identify and evaluate market, fundamental, and alternative data sources containing alpha signals that do not decay too quickly.
 2. Deploy or access a cloud-based scalable data infrastructure and analytical tools like Hadoop or Spark to facilitate fast, flexible data access..
 3. Carefully manage and curate data to avoid look-ahead bias by adjusting it to the desired frequency on a point-in-time basis. This means that data should reflect only information available and known at the given time. ML algorithms trained on distorted historical data will almost certainly fail during live trading.
 
### From alpha factor research to portfolio management
 >Alpha factors are designed to extract signals from data to predict returns for a given investment universe over the trading horizon.

![[Screenshot 2026-08-15 at 09.41.26.png]]

#### 1. The research phase
The **research phase** includes the design and evaluation of aloha factors.

 A predictive factor captures some aspect of a systematic relationship between a data source and an important strategy input like asset returns.
Optimising the predictive power(better predictions) requires very creative methods of feature engineering in the form of effective data transfromations.

You wouldn't want to meet false discoveries during the data mining process so it requires very careful management cause of the risks.

One way of reducing the risk is to focus the search process by following the guidance of decades of academic research.

Many investors still prefer factors aligning with theories about financial markets and investor behaviour.


Validating the signal content of an alpha factor requires a robust estimate of its predictive power in a representative context.

There are numerous methodological and practical pitfalls that undermine a reliable estimate. In addition to data mining and the failure to correct for multiple testing bias, these pitfalls include the use of data contaminated by #survivorship-bias or #look-ahead-bias, not reflecting realistic Principal, Interest and Taxes (PIT) information.

#### 2. The execution phase
During the execution phase, alpha factors emit signals that lead to buy or sell orders. The resulting portfolio holdings, in turn, have specific risk profiles that interact and contribute to the aggregate portfolio risk. Portfolio management involves optimizing position sizes to achieve a balance of return and risk of the portfolio that aligns with the investment objectives.

#### 3. Strategy backtesting
When implementing an investment idea in a real life algorithmic trading environment, the risks involved require us to take a scientific approach.
By scientific approach I mean a process that involves a lot of empirical tests to try and dispute or reject the idea based on its performance in the market samples that are out of sample.

Sometimes testing could require us to simulate conditions that are maybe not present in the historical data but could potentially happen.

So in the case we have an idea, we would need a backtesting engine that tests this idea inn an un biased way, that is, it needs to simulate market occurrences in a very realistic manner!

Apart from the biases that could be introduced by the data or using statistics in a flawed manner, our backtesting engine also needs to be able to accurately represent the practical aspects of trade-signal evaluation, order placement, and execution in line with market conditions.


Later in these notes, we will show some examples and use cases of these engines, like backtrader or zipline.