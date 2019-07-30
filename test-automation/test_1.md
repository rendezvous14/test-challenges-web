## Test Case 1 - Web UI testing

### Test Steps

1. Go to https://trade.go.exchange page

2. Then user will see the trading page and there are many sections. I will only highlight the sections we will tested as described in this picture:
![Alt text](img/test-challenges-1.png?raw=true "Title")
    
3. Click on the Markets Selector dropdown to select each pairs of currencies:
![Alt text](img/test-challenges-2.png?raw=true "Title")

4. For example, when select a BCH-USDC pair, the Order Book will be populated from selected market. There are active orders which are waiting for user to buy / sell.
![Alt text](img/test-challenges-3.png?raw=true "Title")

5. Order Form - Buy, if user click on on price on Order Book (any side), the price will be automatically updated on Order Form - Price. Basically if users would like to buy, they will select the best price for buying on 'Top orderbook on sell side' (the bottom order of sell side).
![Alt text](img/test-challenges-4.png?raw=true "Title")

6. User can select the Order Form - Sell side to sell their own coins as well. Users will select the best price for selling on 'Top orderbook on buy side' (the top order of buy side).
![Alt text](img/test-challenges-8.png?raw=true "Title")

7. Try to make an order eventhough there is 0 balance Basically the amount and total will be calculated automatically and there will be warning message if the amount or total is above the balance.
![Alt text](img/test-challenges-5.png?raw=true "Title")

8. If user attempt to make an order. There will be notification message about you are not logged in yet.
![Alt text](img/test-challenges-7.png?raw=true "Title")