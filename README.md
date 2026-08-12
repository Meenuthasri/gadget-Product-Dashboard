%pip install pandas

import pandas as pd

df = pd.read_csv("C:\\Users\\Meenakshi S\\Downloads\\uncleaned_sales_100_rows.csv")
df
df.isnull().sum()
df[df.isnull().any(axis=1)]

df["Quantity"] = df["Quantity"].fillna(df["Quantity"].median())
df["Quantity"].isnull().sum()
df["Discount_Percent"] = df["Discount_Percent"].fillna(0)
df["Customer"] = df["Customer"].fillna("Unknown")
df["Sales"] = df["Sales"].fillna(
    df["Quantity"] * df["Unit_Price"] * (1 - df["Discount_Percent"] / 100)
)
df.dtypes
df["Order_Date"] = pd.to_datetime(
    df["Order_Date"],
    dayfirst=True,
    errors="coerce"
)
df["Order_Date"].dtype
df["Quantity"] = df["Quantity"].astype(int)
df["Order_ID"] = df["Order_ID"].astype(str)
df["Product"] = df["Product"].str.strip().str.title()

df["Category"] = df["Category"].str.strip().str.title()

df["City"] = df["City"].str.strip().str.title()

df["Customer"] = df["Customer"].str.strip().str.title()
df.duplicated().sum()
df[df.duplicated()]
df.isnull().sum()
df.info()
df.describe()
df.head()
%pip install pandas seaborn matplotlib
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
total_sales = df["Sales"].sum()
total_profit = df["Profit"].sum()
total_orders = df["Order_ID"].nunique()
average_order_value = df["Sales"].mean()

print(total_sales)
print(total_profit)
print(total_orders)
print(average_order_value)
monthly_sales = df.groupby(
    df["Order_Date"].dt.to_period("M")
)["Sales"].sum()

monthly_sales.plot(kind="line", marker="o")

plt.title("Monthly Sales Trend")
plt.xlabel("Month")
plt.ylabel("Sales")
plt.xticks(rotation=45)
plt.show()
sns.barplot(
    data=df,
    x="Category",
    y="Sales"
)

plt.title("Sales by Category")
plt.xticks(rotation=45)
plt.show()
city_sales = df.groupby("City", as_index=False)["Sales"].sum()

sns.barplot(
    data=city_sales,
    x="Sales",
    y="City"
)

plt.title("Sales by City")
plt.show()
top_products = (
    df.groupby("Product", as_index=False)["Sales"]
      .sum()
      .sort_values("Sales", ascending=False)
      .head(5)
)

sns.barplot(
    data=top_products,
    x="Sales",
    y="Product"
)

plt.title("Top 5 Products by Sales")
plt.show()
sns.scatterplot(
    data=df,
    x="Sales",
    y="Profit",
    hue="Category",
    size="Quantity"
)

plt.title("Sales vs Profit")
plt.show()



# 📊 Sales Performance Dashboard – Business Insight Report

## 1. Executive Summary

The **Sales Performance Dashboard** provides a consolidated view of sales, profitability, order volume, product performance, city-level performance, and the relationship between sales and profit.

The dashboard is designed to help management:

* Monitor overall sales performance
* Evaluate profitability
* Identify high-performing products and categories
* Understand regional/city-level sales performance
* Analyze sales trends over time
* Identify opportunities for revenue and profit improvement
* Support data-driven business decisions

---

# 2. KPI Overview

The dashboard contains four key KPIs:

* **Total Sales**
* **Total Profit**
* **Total Orders**
* **Average Order Value (AOV)**

### Business Perspective

These KPIs provide management with a quick snapshot of the organization's overall commercial performance.

### Total Sales

Total Sales represents the overall revenue generated during the reporting period.

**Management Insight:**

* Helps management understand the overall revenue scale.
* Can be compared with previous periods or business targets.
* A decline in sales may indicate reduced demand, pricing issues, or weaker product performance.
* Continuous growth in sales indicates positive revenue momentum.

### Total Profit

Total Profit measures the amount generated after considering the cost associated with sales.

**Management Insight:**

* Sales growth should always be evaluated together with profit growth.
* High sales with low profit may indicate excessive discounts, high costs, or low-margin products.
* Management should focus on products and categories that provide sustainable profitability.

### Total Orders

Total Orders represents the volume of customer transactions.

**Management Insight:**

* Increasing orders indicate stronger customer demand.
* If orders increase but sales remain flat, the average transaction value may be declining.
* If orders decrease while AOV increases, customers may be purchasing fewer but higher-value products.

### Average Order Value (AOV)

AOV indicates the average revenue generated per order.

**Business Formula:**

```text
AOV = Total Sales / Total Orders
```

**Management Insight:**

* A higher AOV indicates stronger value per transaction.
* Management can improve AOV through cross-selling, upselling, product bundles, and premium product recommendations.

---

# 3. Monthly Sales Trend

### Visual

**Line Chart – Monthly Sales Trend**

### Business Objective

The monthly sales trend visual helps management understand how revenue changes over time.

### Business Insights

The visual can be used to identify:

* Increasing or decreasing sales trends
* High-performing months
* Low-performing months
* Seasonal demand patterns
* Sudden increases or decreases in revenue

### Management Interpretation

If sales consistently increase over time, the business may be experiencing positive market demand.

If sales fluctuate significantly, management should investigate:

* Seasonal demand
* Promotional campaigns
* Product availability
* Pricing changes
* Customer behavior
* Market conditions

### Recommended Action

Management should compare monthly performance against:

* Previous month
* Previous year
* Sales target
* Budget

This will help identify whether sales growth is sustainable or driven by temporary factors.

---

# 4. Sales by Category

### Visual

**Bar Chart – Sales by Category**

### Business Objective

This visual compares revenue contribution across different product categories.

### Business Insights

The chart helps identify:

* Highest-performing categories
* Lowest-performing categories
* Categories contributing significantly to revenue
* Categories requiring improvement

### Management Interpretation

A category generating high sales should not automatically be considered the most profitable category.

Management should evaluate both:

```text
Category Sales
        +
Category Profit
        =
Overall Category Performance
```

A high-sales category with low profit may require pricing or cost optimization.

### Recommended Action

Management can:

* Increase inventory for high-demand categories
* Promote profitable categories
* Review pricing for low-margin categories
* Reduce unnecessary discounts
* Investigate weak-performing categories

---

# 5. Sales by City

### Visual

**Horizontal Bar Chart – Sales by City**

### Business Objective

This visual evaluates sales performance across different geographical locations.

### Business Insights

The visual helps management identify:

* Highest-performing cities
* Lowest-performing cities
* Revenue concentration
* Regional growth opportunities

### Management Interpretation

Cities with high sales represent strong existing markets.

Cities with relatively low sales may represent:

* Low customer penetration
* Limited marketing activity
* Distribution challenges
* Lower product demand
* Potential expansion opportunities

### Recommended Action

Management can consider:

* Increasing marketing in low-performing cities
* Improving distribution coverage
* Running location-specific promotions
* Studying customer preferences by city
* Allocating inventory according to regional demand

---

# 6. Top 5 Products by Sales

### Visual

**Horizontal Bar Chart – Top 5 Products**

### Business Objective

The visual identifies products contributing the highest sales revenue.

### Business Insights

The Top 5 Products analysis helps management understand which products are driving revenue.

High-performing products can be prioritized for:

* Inventory planning
* Marketing campaigns
* Promotional activities
* Cross-selling
* Customer retention

### Management Interpretation

Products with consistently high sales should have sufficient inventory availability.

However, management should also compare sales with profit because a high-sales product may not necessarily be a high-profit product.

### Recommended Action

Management should classify products into:

```text
High Sales + High Profit
→ Strategic Products

High Sales + Low Profit
→ Pricing/Cost Review

Low Sales + High Profit
→ Marketing Opportunity

Low Sales + Low Profit
→ Review / Rationalization
```

---

# 7. Sales vs Profit Analysis

### Visual

**Scatter Plot – Sales vs Profit**

### Business Objective

The scatter plot evaluates the relationship between revenue and profitability.

### Business Insights

The visual helps identify:

* High-sales/high-profit products
* High-sales/low-profit products
* Low-sales/high-profit opportunities
* Low-sales/low-profit products

### Management Interpretation

Products positioned toward higher sales and higher profit are generally strong business contributors.

Products with high sales but relatively low profit require further investigation.

Possible reasons include:

* High discounts
* High procurement costs
* High operational costs
* Low selling prices
* Low-margin product categories

### Recommended Action

Management should focus on improving the profitability of high-revenue but low-margin products.

Potential actions include:

* Optimizing pricing
* Reducing excessive discounts
* Negotiating supplier costs
* Improving operational efficiency
* Promoting higher-margin alternatives

---

# 8. Overall Business Insights

Based on the dashboard structure, management should focus on five major areas:

### 1. Revenue Growth

Monitor monthly sales trends and identify periods of strong and weak performance.

### 2. Profitability

Do not evaluate business performance using sales alone. Profit should be analyzed alongside revenue.

### 3. Product Strategy

Identify high-performing products and ensure adequate inventory and promotional support.

### 4. Regional Strategy

Use city-level performance to identify strong markets and potential expansion areas.

### 5. Customer Value

Monitor Average Order Value and order volume to understand customer purchasing behavior.

---

# 9. Management Recommendations

Based on the dashboard analysis framework, the following actions are recommended:

1. **Monitor monthly sales trends** to identify seasonal patterns and unexpected changes.

2. **Prioritize high-profit products** instead of focusing only on high-sales products.

3. **Review discount strategies** where sales are high but profitability is relatively low.

4. **Strengthen high-performing cities** by maintaining sufficient inventory and marketing support.

5. **Investigate low-performing cities** to identify opportunities for customer acquisition and market expansion.

6. **Improve Average Order Value** through bundles, cross-selling, and upselling strategies.

7. **Maintain inventory availability** for the highest-selling products to avoid lost sales opportunities.

8. **Regularly compare sales and profit** against business targets and previous periods.

---

# 10. Key Management Takeaway

> **The dashboard should not be used only to determine how much the business is selling, but also to understand where revenue is coming from, which products and locations are driving performance, and whether revenue is being converted into sustainable profit.**

The combination of **Sales, Profit, Orders, AOV, Sales Trend, Category Performance, City Performance, Top Products, and Sales vs Profit** provides management with a comprehensive view of business performance and supports better strategic decision-making.

---

# 11. Technology Used

* **Python**
* **Pandas** – Data cleaning and analysis
* **Seaborn** – Statistical data visualization
* **Matplotlib** – Data visualization
* **CSV / Excel** – Data source

---

# 12. Analytical Workflow

```text
Raw Sales Data
      ↓
Data Loading
      ↓
Data Quality Check
      ↓
Handle Missing Values
      ↓
Change Data Types
      ↓
Clean Text Values
      ↓
Remove Duplicate Records
      ↓
Exploratory Data Analysis
      ↓
KPI Calculation
      ↓
Seaborn / Matplotlib Visualizations
      ↓
Business Insights
      ↓
Management Recommendations
```

---

## Conclusion

The Sales Performance Dashboard transforms raw transaction data into meaningful business insights.

It enables management to move from **"What happened?"** to **"Why did it happen?"** and ultimately **"What action should we take?"**

The dashboard can therefore be used as a decision-support tool for **revenue growth, profitability improvement, product planning, regional strategy, and customer value optimization**.

