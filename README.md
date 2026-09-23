# 📊 Online Retail Data Analytics

A complete **Data Analytics project** built using the **UCI Online Retail Dataset** to analyze retail transactions, clean raw data, engineer meaningful features, perform business analysis, and visualize insights using Python.

The project follows a practical data analytics workflow using **NumPy, Pandas, Matplotlib, and Seaborn**.

---

## 🚀 Project Overview

This project analyzes transactional data from an online retail business to understand:

* Product sales performance
* Customer spending behavior
* Country-wise revenue
* Monthly revenue trends
* Day-wise revenue patterns
* Revenue distribution
* Top-selling products

The project starts with a raw dataset and goes through the complete analytics pipeline:

**Raw Data → Data Cleaning → Feature Engineering → Data Analysis → Visualization → Business Insights**

---

## 🛠️ Technologies & Libraries

| Technology       | Purpose                            |
| ---------------- | ---------------------------------- |
| Python           | Core programming language          |
| NumPy            | Numerical/statistical calculations |
| Pandas           | Data manipulation and analysis     |
| Matplotlib       | Data visualization                 |
| Seaborn          | Statistical visualization          |
| Jupyter Notebook | Development environment            |

---

## 📁 Project Structure

```text
DataAnalytics/
│
├── Online Retail.csv
├── Online_Retail_Cleaned.csv
│
├── 01_NumPy.ipynb
├── 02_Pandas_Basics.ipynb
├── 03_Data_Cleaning.ipynb
├── 04_Feature_Engineering.ipynb
├── 05_Data_Analysis.ipynb
└── 07_Seaborn.ipynb
```

### Dataset Files

**`Online Retail.csv`**

The original raw dataset. It is preserved separately and remains untouched.

**`Online_Retail_Cleaned.csv`**

The processed version of the dataset after data cleaning.

---

# 🔄 Project Workflow

```text
                 Raw Dataset
                      │
                      ▼
              Data Exploration
                      │
                      ▼
                Data Cleaning
                      │
                      ▼
             Feature Engineering
                      │
                      ▼
                Data Analysis
                      │
                      ▼
             Data Visualization
                │           │
                ▼           ▼
           Matplotlib     Seaborn
                │           │
                └─────┬─────┘
                      ▼
               Business Insights
```

---

# 1️⃣ NumPy

NumPy was used for numerical and statistical calculations.

### Main Operations

```python
np.mean()
np.max()
np.min()
np.std()
```

### Mean

The average revenue was calculated using:

```text
Mean = Σx / n
```

Python:

```python
np.mean(revenue)
```

### Maximum

```python
np.max(revenue)
```

Returns the highest revenue value.

### Minimum

```python
np.min(revenue)
```

Returns the lowest revenue value.

### Standard Deviation

```python
np.std(revenue)
```

Standard deviation measures how widely revenue values are distributed around the mean.

---

# 2️⃣ Pandas Basics

Pandas was the primary library used for working with the dataset.

### Dataset Loading

```python
df = pd.read_csv("Online Retail.csv")
```

### Dataset Inspection

```python
df.head()
df.tail()
df.shape
df.columns
df.info()
df.describe()
```

### Important Pandas Methods

The project used methods such as:

```python
head()
tail()
shape
columns
info()
describe()
isnull()
sum()
duplicated()
drop_duplicates()
groupby()
sort_values()
reset_index()
```

These methods were used to inspect, clean, transform, and analyze the dataset.

---

# 3️⃣ Data Cleaning

Data cleaning was performed before analysis to improve data quality.

### Missing Values

Missing values were identified using:

```python
df.isnull().sum()
```

Total missing values were also checked with:

```python
df.isnull().sum().sum()
```

Important columns checked included:

* `Description`
* `CustomerID`

### Duplicate Records

Duplicate rows were detected using:

```python
df.duplicated().sum()
```

Duplicates were removed using:

```python
df = df.drop_duplicates()
```

### Date Conversion

The `InvoiceDate` column was initially stored as text.

It was converted into a datetime format:

```python
df["InvoiceDate"] = pd.to_datetime(df["InvoiceDate"])
```

This allowed extraction of useful time-based features.

### Date Features

```python
df["Year"] = df["InvoiceDate"].dt.year
df["Month"] = df["InvoiceDate"].dt.month
df["Day"] = df["InvoiceDate"].dt.day
df["Hour"] = df["InvoiceDate"].dt.hour
```

### Data Validation

The cleaned dataset was validated using:

```python
df.isnull().sum()
df.duplicated().sum()
df.shape
df.dtypes
```

Finally, the cleaned dataset was saved:

```python
df.to_csv("Online_Retail_Cleaned.csv", index=False)
```

---

# 4️⃣ Feature Engineering

Feature engineering was used to create new analytical variables from existing data.

## 💰 Revenue

The most important feature created was `Revenue`.

### Formula

```text
Revenue = Quantity × UnitPrice
```

Python:

```python
df["Revenue"] = df["Quantity"] * df["UnitPrice"]
```

Example:

```text
Quantity  = 6
UnitPrice = 2.55

Revenue = 6 × 2.55
        = 15.30
```

---

## 📅 Year-Month

A monthly time feature was created:

```python
df["YearMonth"] = df["InvoiceDate"].dt.to_period("M")
```

This was later used for monthly revenue analysis.

---

## 🗓️ Day Name

The weekday was extracted from the invoice date:

```python
df["DayName"] = df["InvoiceDate"].dt.day_name()
```

Example:

```text
Monday
Tuesday
Wednesday
Thursday
Friday
```

---

## 🧾 Invoice Total

Revenue was aggregated at invoice level:

```python
invoice_totals = df.groupby("InvoiceNo")["Revenue"].sum()
```

Conceptually:

```text
Invoice Total = Σ Revenue for all products in an invoice
```

---

## 👤 Customer Total

Customer-level revenue was calculated using:

```python
customer_totals = df.groupby("CustomerID")["Revenue"].sum()
```

Formula:

```text
Customer Total = Σ Revenue generated by the customer
```

---

# 5️⃣ Data Analysis

After cleaning and feature engineering, the dataset was analyzed to extract business insights.

## 🛍️ Top-Selling Products

```python
df.groupby("Description")["Quantity"] \
  .sum() \
  .sort_values(ascending=False) \
  .head(10)
```

This identifies the products with the highest total quantity sold.

---

## 👤 Top Customers

```python
df.groupby("CustomerID")["Revenue"] \
  .sum() \
  .sort_values(ascending=False) \
  .head(10)
```

This identifies customers generating the highest total revenue.

---

## 🌍 Country-wise Revenue

```python
df.groupby("Country")["Revenue"] \
  .sum() \
  .sort_values(ascending=False) \
  .head(10)
```

This identifies the countries contributing the most revenue.

---

## 📅 Monthly Revenue

```python
df.groupby(
    df["InvoiceDate"].dt.to_period("M")
)["Revenue"].sum()
```

This helps identify revenue trends over time.

---

## 🗓️ Day-wise Revenue

```python
df.groupby(
    df["InvoiceDate"].dt.day_name()
)["Revenue"].sum().sort_values(ascending=False)
```

This compares revenue across days of the week.

---

## 💵 Total Revenue

```python
df["Revenue"].sum()
```

Formula:

```text
Total Revenue = Σ Revenue
```

---

# 6️⃣ Matplotlib Visualization

Matplotlib was used to create analytical charts.

### Monthly Revenue

```python
monthly_revenue.plot(kind="line")

plt.title("Monthly Revenue")
plt.xlabel("Month")
plt.ylabel("Revenue")
plt.show()
```

### Top 10 Products

```python
top_products.plot(kind="bar")

plt.title("Top 10 Products by Quantity Sold")
plt.xlabel("Product")
plt.ylabel("Quantity")
plt.show()
```

### Country-wise Revenue

```python
country_revenue.plot(kind="bar")

plt.title("Top 10 Countries by Revenue")
plt.xlabel("Country")
plt.ylabel("Revenue")
plt.show()
```

### Day-wise Revenue

```python
day_revenue.plot(kind="bar")

plt.title("Revenue by Day")
plt.xlabel("Day")
plt.ylabel("Revenue")
plt.show()
```

---

# 7️⃣ Seaborn Visualization

Seaborn was used for more statistical and visually polished charts.

## Revenue Distribution

```python
sns.histplot(df["Revenue"], bins=50)

plt.title("Revenue Distribution")
plt.xlabel("Revenue")
plt.ylabel("Frequency")
plt.show()
```

This visualizes the distribution of transaction-level revenue.

---

## Monthly Revenue

```python
monthly = df.groupby(
    df["InvoiceDate"].dt.to_period("M")
)["Revenue"].sum().reset_index()

monthly["InvoiceDate"] = monthly["InvoiceDate"].astype(str)

sns.lineplot(
    data=monthly,
    x="InvoiceDate",
    y="Revenue"
)

plt.title("Monthly Revenue")
plt.xlabel("Month")
plt.ylabel("Revenue")
plt.xticks(rotation=45)
plt.show()
```

---

## Top 10 Products

```python
top_products = (
    df.groupby("Description")["Quantity"]
    .sum()
    .sort_values(ascending=False)
    .head(10)
    .reset_index()
)

sns.barplot(
    data=top_products,
    x="Quantity",
    y="Description"
)

plt.title("Top 10 Products by Quantity Sold")
plt.xlabel("Quantity")
plt.ylabel("Product")
plt.show()
```

---

# 📐 Important Formulas

### Revenue

```text
Revenue = Quantity × Unit Price
```

### Total Revenue

```text
Total Revenue = Σ Revenue
```

### Average Revenue

```text
Average Revenue = Total Revenue / Number of Transactions
```

### Invoice Total

```text
Invoice Total = Σ Revenue per Invoice
```

### Customer Total

```text
Customer Total = Σ Revenue per Customer
```

### Mean

```text
Mean = Σx / n
```

### Standard Deviation

```text
σ = √(Σ(x - μ)² / N)
```

---

# 📊 Key Analytical Questions

This project was designed around practical business questions:

1. Which products sell the most?
2. Which customers generate the most revenue?
3. Which countries contribute the most revenue?
4. How does revenue change month by month?
5. Which days generate more revenue?
6. What does the transaction revenue distribution look like?
7. What is the overall revenue generated by the dataset?

---

# 🎯 Skills Demonstrated

Through this project, the following practical skills were developed:

### Python

* Variables
* Expressions
* Functions
* Data processing
* Numerical calculations

### NumPy

* Mean
* Minimum
* Maximum
* Standard deviation
* Array-based calculations

### Pandas

* CSV data loading
* Data inspection
* Missing-value handling
* Duplicate handling
* Datetime conversion
* Feature creation
* GroupBy
* Aggregation
* Sorting
* Data transformation

### Data Cleaning

* Missing-value detection
* Duplicate detection
* Data type conversion
* Data validation
* Clean dataset generation

### Data Visualization

* Line charts
* Bar charts
* Histograms
* Statistical visualization
* Trend analysis
* Categorical comparison

### Data Analytics

* Product analysis
* Customer analysis
* Geographic analysis
* Time-series analysis
* Revenue analysis
* Business insight generation

---

# 💡 Project Outcome

This project demonstrates a complete beginner-to-intermediate **Data Analytics workflow** using a real-world retail transaction dataset.

Instead of only learning individual Python libraries, the project applies them together in a practical workflow:

```text
Python
   ↓
NumPy
   ↓
Pandas
   ↓
Data Cleaning
   ↓
Feature Engineering
   ↓
Data Analysis
   ↓
Matplotlib
   ↓
Seaborn
   ↓
Business Insights
```

---

# 📌 Dataset

Dataset used:

**UCI Online Retail Dataset**

The dataset contains transactional information including:

* Invoice number
* Product code
* Product description
* Quantity
* Invoice date
* Unit price
* Customer ID
* Country

---

# 👨‍💻 Author

**Abdullah Asim**

### Role

**Full Stack Developer | Data Analytics Learner | AI Enthusiast**

### Profiles

* LinkedIn: `https://www.linkedin.com/in/abdullah-asim-dev`
* GitHub: `https://github.com/abdullah-asim-dev/`

---

## ⭐ Project Status

**Completed ✅**

This project was built as a practical learning project to develop hands-on experience with Python-based data analytics, data cleaning, feature engineering, analysis, and visualization.
