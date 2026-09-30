## Fact Table Grain and Measure Additivity

<img width="865" height="932" alt="image" src="https://github.com/user-attachments/assets/aad38380-d2bc-4429-9f7c-393b06bd448b" />


### `orders_fact`

The `orders_fact` table is a **transaction-level fact table** whose grain is:

> **One row represents one product/order line within an order.**

An order can contain multiple products. Therefore, a single `orderID` can appear in multiple rows of the fact table, with each row representing a different product included in that order.

For example, if Order `1001` contains three products, the fact table will contain three rows:

| orderID | product_key | quantity |
|---------|-------------|----------|
| 1001 | 101 | 2 |
| 1001 | 205 | 1 |
| 1001 | 310 | 5 |

The `orderID` identifies the order, while the `product_key` identifies the product associated with each order line.

This grain is important when interpreting aggregations. For example:

- `COUNT(*)` counts **order lines**, not orders.
- `COUNT(DISTINCT orderID)` counts **orders**.
- `COUNT(DISTINCT customer_key)` counts **customers**.
- `SUM(quantity)` calculates the **total number of units sold**.

### Measures and Additivity

The fact table contains the following measures:

| Measure | Description | Additivity |
|---------|-------------|------------|
| `quantity` | Number of units sold for the product on the order line | **Additive** |
| `unit_price` | Selling price per unit on the specific order line | **Non-additive** |
| `discount` | Discount applied to the order line; if represented as a percentage/rate, it should not be summed | **Non-additive** |
| `actual_cost` | Cost associated with the order line | **Additive**, assuming it represents a total monetary amount for the line |
| `freight` | Shipping/freight cost associated with the order | **Not safely additive at the order-line grain** if the same order-level freight value is repeated for every product line |

#### Additive Measures

`quantity` is additive because it can be summed across the dimensions represented in the fact table.

For example:

```text
Product A: 2 units
Product B: 3 units
```

Similarly, `actual_cost` can be summed when it represents the total cost associated with each order line.

#### Non-additive Measures

`unit_price` is non-additive because it represents the price of one unit. Adding prices across different products or order lines does not produce a meaningful business metric.

For example:

```text
Product A: $10
Product B: $20
```
SUM(unit_price) = $30
Product C: 5 units

Total Quantity = 2 + 3 + 5 = 10 units
