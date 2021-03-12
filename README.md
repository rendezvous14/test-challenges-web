<p align="center">
  <a href='https://zipmex.co.th'>
    <img src="https://zipmex.co.th/static/324e5b26abe432a5551b4af777f1fce7/47498/Banner-10-02.jpg" width="300" />
  </a>
</p>
<br />

## Test challenge for Zipmex QA Engineer

There are 2 sections to be completed:

- [Test Strategies](#section-1-test-strategies)
- [Test Automation](#section-2-test-automation)


## Section 1: Test Strategies

#### Scenario
The Food Delivery services can take the order online. The delivery charge will be calculated depends on number of boxes, total price, range, vehicle type to delivery and service chages. The customer will get the discount of delivery fee for first 15 kilometers if user order food more than $200 and more than 15 kilometers will be calculated as usual.

#### Index Formula
Delivered vehicle type will be selected by calculating with number of order.

- BIKE if Index < 40

- CAR if Index >= 40

    When
    
- Meal box = 1.3 per box

- Dessert box = 1 per box

**Examples**
1. Ordered 20 x Meal Boxes and 10 x Dessert Boxes

    Index = (20 * 1.3) + 10 = 36

    Then select **'BIKE'** for delivery

2. Ordered 20 x Meal Boxes and 14 x Dessert Boxes

    Index = (20 * 1.3) + 20 = 40

    Then select **'CAR'** for delivery

3. Ordered 15 x Meal Boxes and 20 x Dessert Boxes

    Index = (15 * 1.3) + 20 = 39.5 then ROUNDUP to 40

    Then select **'CAR'** for delivery

#### Delivery Charge Formula
1. BIKE

    Started at $10

    Distance between 0 - 30 Kilometers = $7.2 / Kilometers

    Distance above 31 Kilometers = $14 / Kilometers

    Delivery Charge will be included 10%


2. CAR

    Started at $20

    Every distance (1 rate) = $12 / Kilometers

    Delivery Charge will be included 10%

#### Discount
1. If customer orders + Delivery fee >= $200

    Deliver charge will be deducted from distance between 0 - 15 kilometers
    
    Deliver charge above 15 kilometers will be calculated as usual

2. If customer orders + Delivery fee < $200

    Deliver charge will be calculated as usual without discount offer


#### How logic works

**Example#1**

- Ordered 20 x Meal Boxes and 14 x Dessert Boxes
- The price of Meal boxes = $210 
- Distance from restaurant to destination = 20 kilometers

    ```
    1. Index = 20 * 1.3 = 36
    Then select BIKE to delivery (BIKE has the starter fee at $10)

    2. Distance for 0 - 30 kilometers (20 kilometers)
    Then 10 + (20 * 7.2) = $154
    Included service charged 10% = 154 * 1.10 = 169.4 ~ 170

    3. Discount if the price >= $200
    Then 10 + (15 * 7.2) = $118
    Included service charged 10% = 118 * 1.10 = 129.8 ~ 130

    Total Delivery charges = 170 - 130 = $40
    ```

**Example#2**

- Ordered 20 x Meal Boxes and 14 x Dessert Boxes
- The price of Meal and Dessert boxes = $500
- Distance from restaurant to destination = 20 kilometers

    ```
    1. Index = (20 * 1.3) + 14 = 40
    Then select CAR to delivery (CAR has starter fee at $20)

    2. Distance = 20 kilometers
    Then 20 + (20 * 12) = $260
    Included service charged 10% = 260 * 1.10 = 286

    3. Discount if the price >= $200
    Then 20 + (15 * 12) = $200
    Included service charged 10% = 200 * 1.10 = 210

    Total Delivery charges = 286 - 210 = $76
    ```

**Example#3**

- Ordered 38 x Dessert Boxes
- The price of Meal and Dessert boxes = $199
- Distance from restaurant to destination = 32 kilometers

    ```
    1. Index = 38
    Then select BIKE to delivery (BIKeE has starter fee at $10)

    2.1 Distance for 0 - 30 kilometers (30 kilometers)
    Then 30 * 7.2 = $216

    2.2 Distance for 31 - 32 kilometers (2 kilometers)
    Then 2 * 14 = $28

    2.3 Include started fee = 10
    Then 216 + 28 + 10 = $254

    2.4 Included service charged 10% 
    Then 254 * 1.10 = 279.4 ~ 275

    3. Discount if the price >= $200
    None

    Total Delivery charges = $275
    ```

#### Mission
You are a QA who has contracts to test **Food Delivery Charge**
- [ ] Create a test plan with as much detail as possible. (Prefer Excel or google sheet format). Think of various scenarios that need testing.
- [ ] Estimation time to test your test scenaiors 
- [ ] Explain how to test or technique you use to create the test scenarios in brief
- [ ] Propose the test cases from your test plan that should be covered by Automation Testing and why you selected those test cases

## Section 2: Test Automation

#### Scenario
https://trade.zipmex.com/trade/BTCUSD is the crypto-currencies Exchange platform for crypto traders. We provide the 24x7 services to operate orders matching engine. We have been implementing 10++ pairs of currencies such as BTCUSD, ZMTUSD, USDTUSD and etc.. to serve the traders's purpose.

QA Engineer team need to ensure the quality by manual testing and convert them to automation test scripts because there are a lot of features and new pairs of currencies will be released weekly.

#### Handicap
For **Web UI test case**, See the [Guidline](test-automation/test_1.md) to walkthrough Zipmex Exchange platform (only the feature we are going to cover the automation testing)

#### Test Case#1 Web UI
1. Select a Instrument `USDTUSD`
2. Get the best price to buy. Then adjust the price a bit 
    - new price = price + 0.1%

3. Input amount = 1.00
4. Get the Total (USD) value
5. Then save the value into a variable and print into the console

    **Expected Result** 
    ```
    Trade USDTUSD 
    with side = buy
    with price = xxx.xxx 
    with amount = 1.00
    Total (USD) = xxx.xxx
    ```

6. Switch to sell side. Get the best price to sell. Then adjust the price a bit
    - new price = price - 0.1%
7. Input amount = 1.00
8. Get the Total (USD) value
9. Then save the total value into a variable and print into the console

    **Expected Result** 
    ```
    Trade USDTUSD 
    with side = sell
    with price = xxx.xxx 
    with amount = 1.00
    Total (USD) = xxx.xxx
    ```

#### Test Case#2 RESTful API
1. Create a automation test scripts to get the value from this API endpoint:

    https://public-api.zipmex.net/api/v1.0/summary

2. Then write the value to log file or log to console as markdown format.
```
Zipmex market cap
| instrument | last_price | lowest_24hr | highest_24hr |
|:-----------|-----------:|------------:|-------------:|
| USD_BTC    |56713.76    |56575.22     |57805.27      |
| USD_ZMT    | 1.2568     | 1.2302      |1.2568        |
...
| USDT_GOLD  |   57.1     |           0 |            0 |
```
#### Mission
- [ ] Follow the requirement on both Web UI and API testing
- [ ] Write the automate test scripts the execution the test cases. 

🤖 **Feel free to use any language or scripts which is easy for you. 

recommended Javascript!** 🤖

---

### Surprise us!
Ready to surprise us what you have done? 
- Push the test plan and automation testing code to github repository and share us the link
- Explain how to setup the tools and how to run the test cases clearly

We are looking for your submission. We will arrange the face-to-face interview once you are passed the challenges.

**Let's rock!** 🤘

![Alt text](test-automation/img/robot1.png?raw=true "QA Engineer")

[![Gauge Badge](https://gauge.org/Gauge_Badge.svg)](https://gauge.org)


Image by <a href="https://pixabay.com/users/Tabble-989840/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2192617">Tabble</a> from <a href="https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2192617">Pixabay</a>
