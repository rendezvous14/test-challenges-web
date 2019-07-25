<p align="center">
  <a href='https://www.omise.co'>
    <img src="https://cdn.omise.co/assets/omise-logo/omise-wordmark.png" width="300" />
  </a>
</p>
<br />

## Test challenge for GO.Exchange QA Engineer

## Section 1: Test Strategies

### Scenario
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


### How logic works

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
    Then select BIKE to delivery (BIKE has starter fee at $10)

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

### Mission#1
You are a QA who has contracts to test **Food Delivery Charge**
- [ ] Create a test plan with as much detail as possible. Think of various scenarios that need testing.
- [ ] Estimation time to test your test scenaiors 
- [ ] Explain how to test or technique you use to create the test scenarios in brief
- [ ] Propose the test cases from your test plan that should be covered by Automation Testing and why you selected those test cases

