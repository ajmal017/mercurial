# mercurial

Mercurial is a framework that allows you to:

1. Get access to market data from yahoo
2. Plug-in strategies
3. Consolidate output of multiple strategies based on their weighting (not finished yet)
4. Execute trades via Interactive Brokers
5. Analyze your portfolio and compare to benchmark (S&P)

There are multiple components to Mercurial:

1. Strategy
  * This script consolidates output of multiple strategies and stores the data in a mysql table called orders. For example, currently, there are two strategies: strategy_MA and strategy_coin_flip.
2. Trade
  * This script reads strategy output from 'orders' table and executes the orders via Interactive Brokers. As orders are sent to Interactive Brokers, messages are received and stored in 'orders' table to reflect latest status of each order. For example, as an order is sent, it's status changes to 'opened' and as it is executed, it changes to 'filed'.
3. Analysis
  * As orders are executed, their executed price is stored in the same table. This transaction data is analyzed in the Analysis.py script and is compared to a benchmark (S&P).


## Set up

1. First, you need to install mysql and create a table to store all the orders. The schema of the table is:

 ```
 mysql> describe orders;
 +------------+-------------+------+-----+---------+----------------+
 | Field      | Type        | Null | Key | Default | Extra          |
 +------------+-------------+------+-----+---------+----------------+
 | order_id   | int(11)     | NO   | PRI | NULL    | auto_increment |
 | date       | datetime    | YES  |     | NULL    |                |
 | strategy   | varchar(50) | YES  |     | NULL    |                |
 | security   | varchar(20) | YES  |     | NULL    |                |
 | action     | varchar(20) | YES  |     | NULL    |                |
 | size       | int(11)     | YES  |     | NULL    |                |
 | ask_price  | varchar(20) | YES  |     | NULL    |                |
 | exec_price | varchar(20) | YES  |     | NULL    |                |
 | status     | varchar(50) | YES  |     | NULL    |                |
 +------------+-------------+------+-----+---------+----------------+

 ```

 You will also need to create another table called 'positions' to hold your positions. Here is the schema:
 ```
 mysql> describe positions;
 +-------------+-------------+------+-----+---------+-------+
 | Field       | Type        | Null | Key | Default | Extra |
 +-------------+-------------+------+-----+---------+-------+
 | security    | varchar(50) | YES  |     | NULL    |       |
 | avg_cost    | varchar(20) | YES  |     | NULL    |       |
 | total_value | varchar(20) | YES  |     | NULL    |       |
 | quantity    | int(11)     | YES  |     | NULL    |       |
 +-------------+-------------+------+-----+---------+-------+
 ```

2. Fill in your table/db details in the config file (config.yaml)

3. Download Interactive Brokers demo tool - Trader Workstation, and fill in your port and client_id in config.yaml

4. Change universe details for each strategy in config.yaml to whatever you like.

5. Run

 ```
 python strategy.py
 ```

6. Check 'orders' table to make sure strategy has output orders into the table.

7. Execute the orders by running:

 ```
 python trade.py
 ```

8. Check 'orders' table to see if status of your orders has changed and confirm on Trader Workstation GUI that orders have been placed.

9. Run 
 ```
 python analysis.py
 ```
to analyze your portfolio.
