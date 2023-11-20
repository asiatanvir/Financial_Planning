# Financial Planning¶

This coding file invloves using APIs as part of the technical solution by creating two financial analysis tool for the purpose of financial planning.

* The first tool  is personal finance planner that allow users to visualize their savings composed by investments in shares and cryptocurrencies to assess if they have enough money as an emergency fund.

* The second tool is a retirement planning tool that used Alpaca API to fetch historical closing prices for a retirement portfolio composed of stocks and bonds.Expected portfolio returns for a specific initial investment amount is projected through Monte Carlo simulations to project the portfolio performance at 30 years.

* The optional part of the challenge is adjusted to refelct expected portfolio returns for early retirement (retiring in 5 or 10 years instead of 30) with the increased investment amount. 

## Personal Finance Planner¶

In this section of the challenge, a personal finance planner application was created. To develop the personal finance planner prototype, following assumptions were taken into account:

* The average household income for each member of the credit union is $12,000.

* Every union member has a savings portfolio composed of cryptocurrencies, stocks and bonds:

    * Assume the following amount of crypto assets: `1.2` BTC and `5.3` ETH.

    * Assume the following amount of shares in stocks and bonds: `50` SPY (stocks) and `200` AGG (bonds).


Bitcoin and Ethereum prices were fetched from "The Alternative Free Crypto API" while "The Alpaca Markets API" was utilized to pull historical stocks and bonds information.The documentation for these APIs can be found via the following links:

* [Free Crypto API Documentation](https://alternative.me/crypto/api/)

* [AlpacaDOCS](https://alpaca.markets/docs/)

After fetching information from APIs following code was used to compute total value of crypto currencies and stocks/bonds portfolio respectively.  

```
        # Cryptos
        btc_price=btc_data["data"]["1"]["quotes"]["USD"]["price"]
        eth_price=eth_data["data"]["1027"]["quotes"]["USD"]["price"]

        # Compute current value of my crpto
        my_btc_value=btc_price * my_btc
        my_eth_value=eth_price * my_eth

        # Print current crypto wallet balance
        print(f"The current value of your {my_btc} BTC is ${my_btc_value:0.2f}")
        print(f"The current value of your {my_eth} ETH is ${my_eth_value:0.2f}")
        
       #output: The current value of your 1.2 BTC is $44734.80.The current value of your 5.3 ETH is $10626.76.
       
       
       # Stocks 
       agg_close_price=float(my_portfolio["AGG"]["close"])
       spy_close_price=float(my_portfolio["SPY"]["close"])
       # Compute the current value of shares
       my_agg_value=agg_close_price * my_agg
       my_spy_value=spy_close_price * my_spy
       
       # Print current value of shares
       print(f"The current value of your {my_spy} SPY shares is ${my_spy_value:0.2f}")
       print(f"The current value of your {my_agg} AGG shares is ${my_agg_value:0.2f}")


       # output:The current value of your 50 SPY shares is $14591.00. The current value of your 200 AGG shares is $21680.00


 ```
##### Visual output of the saving portfolio
 

![Financial Planner](/Images/My_portfolio.png)
   
#### Saving Health Analysis 

Through `if` conditional statements the current savings were validated for an emergency fund. An ideal emergency fund was supposed to be equal to three times monthly income.Per the above calculations, the value of portfolio was greater than the desired emergency fund by $55632.57.

## Part 2 - Retirement Planning

In this section,Alpaca API was used to fetch historical closing prices for a retirement portfolio and then Used the MCForecastTools toolkit to create Monte Carlo simulations to project the portfolio performance at `30` years. After setting the requisite parameteres through following coding Monte Carlo simulation were configured.

```
            #Set number of simulations
            num_sims = 500

            # Configure a Monte Carlo simulation to forecast 30 years daily returns
            mc_thirty_year = MCSimulation(
                portfolio_data = df_stock_data,
                weights=[0.4,.6],             # Agg=40%  and SPY=60%
                num_simulation = num_sims,
                num_trading_days = 252*30     # for 30 years @ 252 trading days per year
            )

 ```

 
![monte_carlo_simu](/Images/monte_Carlo.png)


![monte_carlo_dis](/Images/mc_histogram.png)


      * The expected portfolio return at the 95% lower and upper confidence intervals based on a `$20,000`
      over the next 30 years will end within in the range of $100435.05 and $1009418.76. 

### Optional Challenge - Early Retirement

The same above MCForecastTools toolkit was used to create Monte Carlo simulations to project the portfolio performance at `5 and 10` years with the increased initial investment to `$30,000`. The generated results showed,

        *There is a 95% chance that an initial investment of $30000.0 in the portfolio over the next 5 years will end 
        within in the range of $29388.16 and $78678.8.
        * There is a 95% chance that an initial investment of $30000.0 in the portfolio over the next 10 years will end 
        within in the range of $36533.39 and $139706.66.

        

    

