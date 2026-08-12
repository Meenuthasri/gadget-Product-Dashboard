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
