# E-commerce Sales Performance Analysis

## Project Background

As a data analyst for this e-commerce platform, this project was initiated to conduct a comprehensive review of our sales performance from July 2016 through August 2018. Our company is a direct-to-consumer online retailer with a diverse product catalog, operating for several years in a competitive market. Our business model relies on driving high-volume sales across multiple categories, with key business metrics including Gross Merchandise Volume (GMV), Average Order Value (AOV), customer retention, and order cancellation/return rates.
This analysis aims to uncover key trends and provide actionable insights to guide strategic decisions for the marketing, sales, and operations teams.

Insights and recommendations are provided on the following key areas:

* **Category 1**: Overall Sales Performance & Revenue Trends
* **Category 2**: Order Fulfillment and Cancellation Analysis
* **Category 3**: Payment Method Insights & Customer Behavior
* **Category 4**: Discount Strategy & Pricing Analysis

## Data Structure & Initial Checks

The analysis was performed on a single, denormalized sales transaction table exported from our primary sales database. This table contains item-level data for all orders placed within the specified timeframe.

The dataset consists of 1048575 records. A description of the key tables used in the analysis is as follows:

**Table 1**: SalesTransactions

A single flat table containing all transaction-level data. Key columns include item_id, increment_id (Order ID), created_at, status, price, qty_ordered, grand_total, discount_amount, payment_method, and category_name_1. Several data quality issues were identified and cleaned, including handling of \N values, correcting data types for numeric columns, and addressing #REF! errors in the BI Status column. Trailing empty columns were also removed.

(Note: For this project, a single table was used. In a production environment, this data would likely be normalized across tables for customers, orders, order_items, and products.)

## Executive Summary

### Overview of Findings

This analysis reveals a business with strong seasonal sales peaks but facing significant operational challenges. The three key takeaways for stakeholders are: 
1. A critically high order cancellation rate of 34.4% severely impacts realized revenue, with the "Mobiles & Tablets" category being a major contributor to both high revenue and high cancellations.
2. "Cash on Delivery" (COD) is the overwhelmingly dominant payment method, but it is also correlated with the highest volume of cancellations, presenting a significant operational risk.
3. Our discount strategy appears effective, as discounted orders have a substantially higher Average Order Value (AOV) than non-discounted ones, suggesting discounts are successfully driving larger purchases rather than just clearing low-value stock.

### Insights Deep Dive

**Category 1**: Sales Performance and Revenue Trends

* The business experiences strong seasonality, with sales revenue consistently peaking in the fourth quarter (October-November) of each year, as seen in the monthly revenue trend line. The peak in late 2017 was particularly strong, exceeding 800M (Rs). This indicates a heavy reliance on holiday season or event-driven sales.
* "Mobiles & Tablets" is the single most dominant category, generating over 2.5B (Rs) in revenue. This is more than three times the revenue of the next closest category, "Appliances," making the business's overall performance highly sensitive to this one category.
* Following "Mobiles & Tablets" and "Appliances," the categories of "Entertainment," "Women's Fashion," and "Men's Fashion" form the next tier of significant revenue drivers.
* The correlation matrix shows an expected strong positive relationship (0.76) between qty_ordered and grand_total, confirming basic data integrity after cleaning.

![monthylrevenue](https://github.com/user-attachments/assets/1e78777a-88bc-4f4b-95e8-f4d21a6913d6)

![top10categories](https://github.com/user-attachments/assets/b44255db-736c-4c31-aee3-cc5bcce4c866)


**Category 2**: Order Fulfillment and Cancellation Analysis

* The order cancellation rate is alarmingly high at 34.4% of all item statuses. Combined with a 10.1% refund rate, nearly 45% of all orders are not successfully completed and retained, representing a massive loss in potential revenue and operational efficiency.
* The "Mobiles & Tablets" category, while being the highest revenue generator, also has the highest absolute number of canceled orders (over 53,000). This suggests a significant problem with either inventory management, payment failures for high-ticket items, or customer purchasing behavior in this category.
* "Men's Fashion" and "Beauty & Grooming" have the highest number of complete orders, indicating strong fulfillment performance in these areas relative to other categories like "Mobiles & Tablets."
* "Cash on Delivery" (COD) is the payment method associated with the highest volume of both complete (over 147k) and canceled (over 21k) orders. While its high completion volume is due to its popularity, the high cancellation volume points to it being a high-risk payment channel.

![top5orderstatus](https://github.com/user-attachments/assets/87db823b-4655-49da-b6e9-471d6f91b52b)

![category-status-corr](https://github.com/user-attachments/assets/167b77a9-15c7-4cd5-b634-4adb3d2b8cf6)


**Category 3**: Payment Method Insights & Customer Behavior

* "Cash on Delivery" (COD) is the preferred payment method by a vast margin, accounting for a significantly larger portion of transactions than all other digital payment methods combined. This indicates that the customer base is either less willing or less able to use digital payments.
* Digital payment methods like Payaxis, Easypay, and jazzwallet form the second tier of payment options but are dwarfed by the volume of COD.
* Payment methods like ublcreditcard and bankalfalah have a relatively high proportion of cancellations compared to their successful transactions, which could indicate issues with payment gateways or fraud checks.
* The customercredit payment method shows a very high ratio of complete to canceled orders, suggesting customers using store credit are highly likely to complete their purchases.

![paymentmethods](https://github.com/user-attachments/assets/c7923b05-eb96-4e2f-8298-3c692657347f)

![payment-method-status-corr](https://github.com/user-attachments/assets/c0cb7614-4bae-40ad-9d4f-c49071db429c)


**Category 4**: Discount Strategy & Pricing Analysis

* Discounted orders result in a significantly higher Average Order Value (AOV) of approximately 14,000 (Rs) compared to roughly 11,300 (Rs) for non-discounted orders. This strongly suggests that discounts are not being used on low-value items but are instead encouraging customers to buy more or higher-priced items.
* The correlation matrix confirms this, showing a moderate positive correlation (0.46) between price and discount_amount. This provides statistical evidence that higher-priced items are more likely to receive a discount.
* The scatter plot of "Discount Impact" shows that while most transactions have low discounts, there is a wide spread. There isn't a simple linear relationship, indicating a varied and potentially targeted discount strategy across different categories and price points.

![aov](https://github.com/user-attachments/assets/f007b5f2-73c6-47a1-b1ec-e1e3344c8aba)

![heatmap](https://github.com/user-attachments/assets/6b9e74a4-84d4-4fb3-b6ae-5c7be2086d5c)

![discountimpact](https://github.com/user-attachments/assets/b7fac59b-563e-49fe-8352-4ebbf71ee7b5)


## Recommendations:

Based on the insights and findings above, I would recommend the following actions:

### For the Operations & Sales Teams:

* **Observation**: The 34.4% cancellation rate, especially in the high-value "Mobiles & Tablets" category, is the most critical issue.
* **Recommendation**: Immediately form a task force to investigate the root causes of cancellations. Analyze cancellation reasons (if available), inventory availability at time of order, and follow-up with customers who canceled high-value orders. Consider implementing partial pre-payment requirements for high-value items to reduce non-serious orders.

### For the Finance & Marketing Teams:

* **Observation**: Cash on Delivery (COD) is a necessary but high-risk channel.
* **Recommendation**: Develop and promote incentives for customers to switch to prepaid digital payment methods. A small, exclusive discount for using Easypay or credit cards could reduce the operational burden and risk of COD cancellations.

### For the Marketing & Merchandising Teams:

* **Observation**: The current discount strategy, which appears to target higher-priced items and results in a higher AOV, is working effectively.
* **Recommendation**: Continue and expand this targeted, strategic approach. Analyze which specific high-margin products respond best to discounts to maximize profitability, rather than implementing broad, margin-eroding sales events.

### For the Marketing & Inventory Teams:

* **Observation**: Sales show strong seasonality with a major peak in Q4 (Oct-Nov).
* **Recommendation**: Align marketing campaigns and inventory planning to capitalize on this predictable peak. Begin campaigns earlier in Q3 to build momentum and ensure stock levels, especially for top categories, are sufficient to meet the surge in demand.

### For the Data Governance Team:

* **Observation**: The dataset contained data quality issues such as #REF! errors in BI Status and \N values.
* **Recommendation**: Implement stricter data validation rules at the source of data entry or during the ETL process to ensure higher data quality for future analysis.

## Assumptions and Caveats:

* **Data Cleaning**: \N values in sales_commission_code were treated as null. Values like #REF! in BI Status were also treated as null, indicating data quality issues at the source.
* **Column Interpretation**: grand_total was assumed to be the final price for the item line after discounts were applied, based on observed calculations within the data.
* **Incomplete Data**: The provided dataset contained several trailing empty columns which were removed. The analysis is based solely on the columns with data.
* **Scope**: This analysis focuses on trends within the provided transaction data and does not include external factors like marketing spend, competitor actions, or macroeconomic trends.
