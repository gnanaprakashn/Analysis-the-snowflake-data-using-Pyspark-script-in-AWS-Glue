
# ❄️ Snowflake Data Analysis using PySpark in AWS Glue

This project demonstrates how to integrate **Snowflake** with **Apache Spark (PySpark)** inside **AWS Glue** for performing data extraction, transformation, and loading (ETL).  
It uses JDBC and Spark-Snowflake connectors to enable bidirectional data movement between Snowflake and Glue.

This setup is commonly used by data engineering teams to process Snowflake data at scale using distributed Spark jobs.

---

## 🚀 What This Pipeline Does (Simple Explanation)

1. AWS Glue job runs a **PySpark script**  
2. Spark uses:
   - **Snowflake JDBC driver**  
   - **Snowflake Spark Connector**  
3. Reads data from Snowflake tables  
4. Runs transformations / SQL queries using Spark  
5. Writes processed results back into Snowflake  

This enables scalable Snowflake analytics using Spark clusters.

---

## 📁 Repository Structure

```

Snowflake-Data-Spark-Integration/
│
├── gluejobforsnowflake.py                 # Main PySpark ETL script for Glue
├── snowflake-jdbc-3.13.15.jar             # JDBC connector for Snowflake
├── spark-snowflake_2.12-2.9.2-spark_3.1.jar # Spark-Snowflake connector
├── read me..txt                           # Notes / reference
└── README.md

````

---

## 🔧 Components Used

### **1️⃣ Snowflake JDBC Connector**
- File: `snowflake-jdbc-3.13.15.jar`
- Purpose:  
  Enables low-level JDBC connectivity between Spark and Snowflake.

### **2️⃣ Snowflake Spark Connector**
- File: `spark-snowflake_2.12-2.9.2-spark_3.1.jar`
- Purpose:  
  Allows Spark to read/write Snowflake tables using Spark DataFrames efficiently.

### **3️⃣ PySpark Script (AWS Glue Job)**
- File: `gluejobforsnowflake.py`
- Purpose:  
  Executes Spark job that interacts with Snowflake:
  - Reads tables  
  - Runs SQL  
  - Writes results back  

---

# 🧠 How the ETL Works

### **Step 1 — Initialize Spark Session**
AWS Glue starts a distributed PySpark session.

### **Step 2 — Load Snowflake Options**
Includes:
- URL  
- User  
- Password  
- Database  
- Schema  
- Warehouse  

### **Step 3 — Read Data from Snowflake**
Using:
```python
df = spark.read.format("net.snowflake.spark.snowflake")
````

### **Step 4 — Run Transformations or SQL**

Spark runs your logic, filters, transformations, aggregations, or SQL queries.

### **Step 5 — Write Back to Snowflake**

Example:

```python
df.write.mode("overwrite").format("snowflake")
```

---

# 📜 PySpark Code (Main Logic Overview)

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Snowflake Integration").getOrCreate()

SNOWFLAKE_SOURCE_NAME = "net.snowflake.spark.snowflake"

snowflake_database = ""
snowflake_schema = ""
snowflake_table_name = ""

snowflake_option = {
    "sfUrl": "",
    "sfUser": "",
    "sfPassword": "",
    "sfdatabase": snowflake_database,
    "sfschema": snowflake_schema,
    "sfWarehouse": ""
}

# Read table
df = spark.read.format(SNOWFLAKE_SOURCE_NAME) \
    .option(**snowflake_option) \
    .option("dbtable", snowflake_table_name) \
    .load()

# Perform SQL or transformations
sql_query = """ 
SELECT * FROM table 
"""

# Write back
df.write.format("snowflake") \
    .option(**snowflake_option) \
    .option("dbtable", "output") \
    .mode("overwrite") \
    .save()

spark.stop()
```

---

# ⚙️ How to Run This in AWS Glue

### **1️⃣ Upload Connectors**

Upload the following to S3 and attach to Glue job:

* `snowflake-jdbc-3.13.15.jar`
* `spark-snowflake_2.12-2.9.2-spark_3.1.jar`

Glue → Job → “Job Parameters / Libraries”

### **2️⃣ Create AWS Glue Job**

Set:

* Script location: `gluejobforsnowflake.py`
* Worker type: Standard or G.1X
* Glue version: Spark 3.x compatible
* IAM Role: S3 + Snowflake permission

### **3️⃣ Configure Snowflake Connection Inside Script**

Fill:

* URL
* USER
* PASSWORD
* DATABASE
* SCHEMA
* WAREHOUSE

### **4️⃣ Run the Glue Job**

It will:

* Connect to Snowflake
* Read from the table
* Execute transformations
* Write results back

---

# 🧪 Testing & Validation

### ✔ Confirm JDBC & Spark connectors load

Check Glue job logs for connector load success.

### ✔ Validate Snowflake read

Run:

```sql
SELECT * FROM MY_TABLE;
```

### ✔ Validate outputs

Check new table or updated rows in Snowflake.

### ✔ Review Glue logs

Look for:

* Connection success
* DataFrame load
* Write confirmation

---

# 🎯 Skills Demonstrated

* Spark + Snowflake integration
* Distributed ETL processing
* Using JDBC & Spark connectors
* AWS Glue PySpark development
* External database connectivity
* Data ingestion + writeback patterns
* Cloud-based analytics integration

This level of hands-on work represents typical tasks for real-world ETL engineers.

---

# 📄 Resume Bullet Points

* Developed ETL pipeline using **AWS Glue PySpark** to integrate with **Snowflake**, leveraging JDBC and Spark-Snowflake connectors for high-performance data transfer.
* Designed and executed distributed queries, table writes, and analytics workflows directly from Spark to Snowflake.
* Implemented secure Snowflake connectivity using Glue job parameters, external connectors, and optimized Spark session configuration.

---

# 👤 Author

**Gnana Prakash N**
Data Engineer
GitHub: [gnanaprakashn](https://github.com/gnanaprakashn)

---

# 📜 License

MIT © 2025

