# Problem Description

We are tasked with finding the average selling price for each product based on the data from two tables: `Prices` and `UnitsSold`. The average price for a product is calculated by dividing the total revenue (units sold multiplied by the price) by the total units sold. If a product does not have any sold units, its average selling price is assumed to be `0`. The result should be rounded to two decimal places.

# Solution

To solve the problem:
1. Join the `Prices` and `UnitsSold` tables on the `product_id` column and ensure that the `purchase_date` from the `UnitsSold` table falls between the `start_date` and `end_date` in the `Prices` table.
2. Calculate the total revenue for each product as the sum of `(units * price)`.
3. Calculate the total units sold for each product.
4. Compute the average selling price as `total revenue / total units sold`. If no units are sold, the average price is `0`.
5. Round the result to two decimal places.

Here is the SQL solution:

```sql
SELECT 
    prices.product_id, 
    ROUND(
        IFNULL(
            SUM(unitssold.units * prices.price) / SUM(unitssold.units), 
            0
        ), 
        2
    ) AS average_price
FROM 
    prices
LEFT JOIN 
    unitssold 
ON 
    prices.product_id = unitssold.product_id
    AND unitssold.purchase_date BETWEEN prices.start_date AND prices.end_date
GROUP BY 
    prices.product_id;
