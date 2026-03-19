# Building-a-Power-BI-Dashboard-Using-PostgreSQL-and-Aiven-Cloud-Data

## Introduction

Business intelligence tools help organizations turn raw data into actionable insights. **Microsoft Power BI** is widely used to analyze business data and create interactive dashboards for monitoring performance and supporting decision-making.

Relational databases such as **PostgreSQL** store structured operational data like customers, products, sales transactions, and inventory records. By connecting Power BI to PostgreSQL databases, both **local databases** and **cloud-hosted databases such as Aiven**,analysts can retrieve data directly from the source, transform it, and build analytical dashboards.

This project demonstrates the complete workflow of:

* Connecting **Power BI to a local PostgreSQL database**
* Connecting **Power BI to an Aiven cloud PostgreSQL database**
* Loading and transforming raw data using **Power Query**
* Creating a **Date Dimension (DimDate) table** for time-based analysis
* Building a dashboard with key **business KPIs and insights**

---

# Connecting PostgreSQL and Cloud Databases to Power BI

## Connecting to a Local PostgreSQL Database

Power BI provides a built-in PostgreSQL connector that allows users to connect directly to a local PostgreSQL server.

Steps:

1. Open **Power BI Desktop**
2. Click **Get Data**
   [!get data](https://github.com/Damaa-C/Building-a-Power-BI-Dashboard-Using-PostgreSQL-and-Aiven-Cloud-Data/blob/main/getdata.png)
3. Select **PostgreSQL Database**
   [!postgresget](https://github.com/Damaa-C/Building-a-Power-BI-Dashboard-Using-PostgreSQL-and-Aiven-Cloud-Data/blob/main/betterpostgresdbchoose.png)
4. Enter the server connection:

```
localhost:5432
```

5. Enter the database name
   [!enterdb](https://github.com/Damaa-C/Building-a-Power-BI-Dashboard-Using-PostgreSQL-and-Aiven-Cloud-Data/blob/main/enter%20postgresdbdetails.png)
7. Provide PostgreSQL credentials
   [!password](https://github.com/Damaa-C/Building-a-Power-BI-Dashboard-Using-PostgreSQL-and-Aiven-Cloud-Data/blob/main/postgresdbuserpassword.png)
9. Select the tables to load (customers, products, sales, inventory)

[!load](https://github.com/Damaa-C/Building-a-Power-BI-Dashboard-Using-PostgreSQL-and-Aiven-Cloud-Data/blob/main/previewofpostgresdbbeforechoosing.png)
---

## Connecting to Aiven Cloud PostgreSQL

Cloud databases such as Aiven require secure connections.

Steps:

1. Obtain connection details from Aiven:

   * Host
   * Port
   * Database name
   * Username
   * Password
   * CA SSL Certificate

2. Install the **CA certificate** on your system.

**Windows**

* Right-click the certificate file
* Select **Install Certificate**
* Add it to **Trusted Root Certification Authorities**

3. In Power BI Desktop select **Get Data → PostgreSQL Database**

Enter the connection:

```
host:port
```

4. Enter the database credentials and load the required tables.

[!pgsql]()
[!aivendb]()
[!passwd]()
[!chooose]()
### When SSL Mode is Not Available

Some versions of Power BI do not display an SSL configuration option. In this case, a secure connection can be established using a **VPN or SSH tunnel**, allowing Power BI to connect through:

```
localhost:port
```

The secure tunnel handles encryption while Power BI connects locally.

---

# Loading and Transforming Data

After establishing the connections, the next step is loading the database tables into Power BI.

It is recommended to **load raw data first** before applying transformations. This allows the analyst to review the dataset and identify potential issues such as:

* Missing values
* Incorrect data types
* Unnecessary columns
* Inconsistent formats

Power BI’s **Power Query Editor** is then used to clean and transform the data.

Typical transformations include:

* Removing unnecessary columns
* Renaming columns for clarity
* Filtering invalid or test records
* Adjusting data types
* Handling missing values

These steps ensure the dataset is consistent and ready for analysis.

📷 **Screenshot:** Power Query Editor showing transformations.

---

# Data Model

## Before Adding the Date Table

Initially, the model contained four PostgreSQL tables:

* `customers`
* `products`
* `sales`
* `inventory`

The **sales table acts as the fact table**, containing transaction records.

Relationships:

* `sales.customer_id → customers.customer_id`
* `sales.product_id → products.product_id`
* `inventory.product_id → products.product_id`

📷 **Screenshot:** Power BI Model View before adding the Date table.

---

## After Adding the Date Table

To enable time-based analysis, a **Date Dimension table (DimDate)** was created in Power BI.

Relationship added:

```
sales.sale_date → DimDate.FullDate
```

This table enables filtering by:

* Year
* Month
* Quarter
* Day

It also supports time-based KPI calculations.

📷 **Screenshot:** Model View after adding the Date table.

---

## Schema Type

The final data model follows a **Star Schema**.

**Fact Table**

* sales

**Dimension Tables**

* customers
* products
* inventory
* DimDate

📷 **Screenshot:** Final model view showing the complete schema.

---

# Dashboard and Key Performance Indicators

After preparing the data model, interactive dashboards were created in Power BI.

## Sales Performance

KPIs include:

* **Total Sales**

```
Total Sales = SUM(sales[total_amount])
```

* **Year-to-Date Sales**

```
YTD Sales = TOTALYTD(SUM(sales[total_amount]), DimDate[FullDate])
```

* **Monthly Sales Trend**

📷 **Screenshot:** Sales performance dashboard.

---

## Product Performance

KPIs include:

* **Total Quantity Sold**

```
Total Quantity Sold = SUM(sales[quantity_sold])
```

* **Top Selling Products**

Visualized using a bar chart ranking products by quantity sold.

📷 **Screenshot:** Product performance dashboard.

---

## Customer Insights

KPIs include:

* **Total Customers**

```
Total Customers = DISTINCTCOUNT(customers[customer_id])
```

* **Top Customers by Revenue**

Customers ranked by total sales value.

📷 **Screenshot:** Customer insights dashboard.

---

## Inventory Insights

KPIs include:

* **Remaining Stock**

```
Stock Remaining = SUM(inventory[stock_quantity]) - SUM(sales[quantity_sold])
```

* **Low Stock Products**

Used to identify products that require restocking.

📷 **Screenshot:** Inventory insights dashboard.

---

# Conclusion

This project demonstrates how Power BI can be integrated with both **local PostgreSQL databases and cloud-hosted PostgreSQL databases on Aiven** to build a complete analytics workflow.

The process involved connecting to multiple data sources, loading raw data, transforming it using Power Query, and designing a **star schema data model with a Date Dimension table** to support time-based analysis. The resulting Power BI dashboard provides insights into **sales performance, product demand, customer behavior, and inventory levels**.

By combining PostgreSQL data management with Power BI visualization capabilities, analysts can transform operational data into meaningful insights that support better business decisions.
