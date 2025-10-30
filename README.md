**Welcome to DataLand!**
This repository is your go-to resource for comprehensive questions covering a wide range of topics in **data engineering**. Dive deep into various concepts, challenges, and insights to enhance your understanding and skills in the field. Whether you're a beginner or an experienced data engineer, you'll find valuable content here. Stay tuned for **frequent updates** as we continue to expand our collection of questions and insights. 
Explore, learn, and excel in data engineering with us on **DataLand**!"
Thank You!

| **Basic Transformations** | `select`, `withColumn`, `drop`, `filter`, `distinct` | ```python
from pyspark.sql.functions import col, upper
df = df.withColumn("CITY", upper(col("city"))).filter(col("amount") > 1000)
``` |
| **Aggregations** | `groupBy().agg()`, `count()`, `sum()`, `avg()` | ```python
from pyspark.sql.functions import sum, avg
df.groupBy("region").agg(sum("sales").alias("total_sales"), avg("sales").alias("avg_sales"))
``` |
| **Window Functions** | `row_number`, `rank`, `lag`, `lead`, `avg().over()` | ```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number, lag, avg
w = Window.partitionBy("category").orderBy("month")
df.withColumn("prev_sales", lag("sales", 1).over(w)) \
  .withColumn("rolling_avg", avg("sales").over(w.rowsBetween(-2, 0)))
``` |
| **Joins** | `inner`, `left`, `right`, `full`, `semi`, `anti` | ```python
df.join(df2, on="id", how="left")
``` |
| **Union & Append** | `union`, `unionByName` | ```python
df_final = df1.unionByName(df2)
``` |
| **Null Handling** | `fillna`, `dropna`, `na.replace()` | ```python
df.fillna({'amount': 0}).dropna(subset=['id'])
``` |
| **UDFs** | `udf()`, `pandas_udf()` | ```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType
@udf(StringType())
def greet(name): return f"Hello {name}"
df.withColumn("greeting", greet(col("name")))
``` |

⏱ *Time: 25 min to explain + code hands-on (if practiced live).*

---

## 🚀 3️⃣ Interview-Level Coding Scenarios (Real Questions)

### ✅ Q1. **Find top 2 products by revenue per category**
```python
from pyspark.sql.window import Window
from pyspark.sql.functions import sum, rank

w = Window.partitionBy("category").orderBy(col("total_revenue").desc())

df.groupBy("category", "product") \
  .agg(sum("revenue").alias("total_revenue")) \
  .withColumn("rnk", rank().over(w)) \
  .filter(col("rnk") <= 2) \
  .show()

