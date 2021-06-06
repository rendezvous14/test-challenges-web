<p align="center">
<img src="https://zipmex.com/static/ce5d44c15e491bcae66cc036bd4d8f2d/aa8de/zipmex-logo.webp"/>
</p>
<p align="center">
  <a href='https://zipmex.com/en'>
    <img src="https://zipmex.com/static/46668043c8bae4d941d2699f233078f1/THB.png" width="300" />
  </a>
</p>
<br />

## Test challenge for Zipmex QA Engineer

There are 2 sections to be completed:

- [Test Strategies](#section-1-test-strategies)
- [Test Automation](#section-2-test-automation)


## Section 1: Test Strategies

### Scenario
The Food Delivery services can take the order online. 
The `delivery_charge` will be calculated depends on number of `boxes`, `box_price`, `distance`,`vehicle_type` to deliver 
and `service_charges`. The customer will get the `discount` on the `delivery_charge` for first 15 kilometers, 
if user order food (`box_price`) more than $200. `delivery_charge` for the remaining distance above 15 kilometers will be calculated as usual.

### Vehicle Type Index Formula
`vehicle_type` to deliver will be selected automatically by calculating index of the `boxes` count.

- Select 'BIKE' if total box index < 40

- Select 'CAR' if total box index >= 40

    When
    
- `Meal box` index = 1.3 per box

- `Dessert box` index = 1 per box


**Examples**
1. Ordered 20 x `Meal Box` and 10 x `Dessert Box`

    Index = (20 * 1.3) + 10 = 36

    Then select **'BIKE'** for delivery

2. Ordered 20 x `Meal Box` and 14 x `Dessert Box`

    Index = (20 * 1.3) + 20 = 40

    Then select **'CAR'** for delivery

3. Ordered 15 x `Meal Box` and 20 x `Dessert Box`

    Index = (15 * 1.3) + 20 = 39.5 then ROUNDUP to 40

    Then select **'CAR'** for delivery


### Delivery Charge Formula
1. BIKE
   - starts at $10
   - charge for distance between 0 - 30 Kilometers = $7.2 / Kilometers
   - charge for istance above 31 Kilometers = $14 / Kilometers
   - includes 10% `service_charge` 


2. CAR
   - starts at $20
   - charge for any distance (1 rate) = $12 / Kilometers
   - includes 10% `service_charge`

### Discount
1. If customer orders with total `box_price` >= $200
   - `discount` will deduct the `delivery_charge` for `distance` between 0 - 15 kilometers
   - `delivery_charge` for the remaining `distance` above 15 kilometers will be calculated as usual
   - `discount` includes 10% `service_charge` before deduction (yes!)

2. If customer orders with total `box_price` < $200
   - `delivery_charge` will be calculated as usual without discount offer

Note that the total `box_price` can be based on assumption, you do not need to worry about how different ordered food boxes sum up to certain price.

### How Service Works

**Example #1**

- Ordered 20 x `Meal Box` and 14 x `Dessert Box`
- The total `box_price` = $210 
- `distance` from restaurant to destination = 20 kilometers

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

**Example #2**

- Ordered 20 x `Meal Box` and 14 x `Dessert Box`
- The total `box_price` = $500
- `distance` from restaurant to destination = 20 kilometers

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

**Example #3**

- Ordered 38 x `Dessert Box`
- The total `box_price` = $199
- `distance` from restaurant to destination = 32 kilometers

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

### Mission
You are a QA who has contracts to test **Food Delivery Charge**
- [ ] Create a test plan with as much detail as possible. (Prefer Excel or google sheet format). Think of various scenarios that need testing.
- [ ] Estimate the time required to test your scenarios (for cases you think cannot be automated).
- [ ] Explain how to test, the approach, or the technique you use, to come up with these test scenarios in brief.
- [ ] From your test scenarios, propose test automation plan. You can answer based on the following key ideas: 
  - what are the cases you want to automate, and what kind of test will you call it,
  - where (or at which points) in SDLC/CICD do you want to automate,
  - what is your test environment and/or dependencies you think needs to be controlled.

## Section 2: Test Automation

### Scenario
https://trade.zipmex.com/trade/BTCUSD is the crypto-currencies Exchange platform for crypto traders. 
We provide the 24x7 services to operate orders matching engine. 
We have been implementing 10++ pairs of currencies such as BTCUSD, ZMTUSD, USDTUSD and etc.. to serve the traders' purpose.

QA Engineer team need to ensure the quality by manual testing and convert them to automation test scripts,
because there are a lot of features and new pairs of currencies to be released weekly.

#### Background on Zipmex Trade Website
For **Web UI test case**, See the [Guidline](test-automation/test_1.md) to walkthrough Zipmex Exchange platform 
(This is the only UI feature we are going to cover in this "Section 2: Test Automation")

### Test Case #1 Web UI
1. Select a Instrument `USDT/USD`
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

### Test Case #2 RESTful API
1. Create an automation test scripts to get the value from this API endpoint:

    https://public-api.zipmex.net/api/v1.0/summary

2. Then write the value to log file, or log to console, as Markdown format.

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
- [ ] Write automation scripts and make sure the test cases execute correctly.
- [ ] Compose the README of the required set up and how to run your test automation scripts (depends on tools/framework you submitted). 
  Let's say we would like to share it to anyone who does not have background.

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
