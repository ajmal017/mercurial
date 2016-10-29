# mercurial

Mercurial is a framework that allows you to:

1. Get access to market data from yahoo
2. Plug-in strategies
3. Consolidate output of multiple strategies based on their weighting (not finished yet)
4. Execute trades via Interactive Brokers
5. Analyze your portfolio and compare to benchmark (S&P)

There are multiple components to Mercurial:

1. Strategy
  * This script consolidates output of multiple strategies and stores the data in a mysql table
  * For example, currently, there are two strategies: strategy_MA and strategy_coin_flip.
2. Trade

3. Analysis