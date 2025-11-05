# IPL Data Analysis — Apache Spark on Databricks

End-to-end **IPL analytics** project built with **Apache Spark (PySpark)** on **Databricks**.  
It performs distributed data ingestion, cleaning, feature engineering, **Spark SQL** analysis with **window functions**, and produces visual insights using **Matplotlib/Seaborn**. Data is read directly from **Amazon S3**.

> Tech: Apache Spark, Databricks, PySpark (DataFrames + SQL + Window), AWS S3, Matplotlib, Seaborn

---

## 📌 Project Goals

- Ingest raw IPL datasets at scale (ball-by-ball, matches, players, player-match, teams)  
- Build typed **schemas** and create **Temp Views** for SQL exploration  
- Analyze performance trends (top batters per season, powerplay efficiency, team win patterns, toss impact, etc.)  
- Use **Spark SQL** with **window functions** for season ranking and aggregated KPIs  
- Produce clear plots for leadership-ready insights

---

## 🗂️ Data Sources (from S3)

The notebook reads CSVs from S3 (you can change paths if needed):

- `s3://ipl-data-analysis-project/Ball_By_Ball.csv`  
- `s3://ipl-data-analysis-project/Match.csv`  
- `s3://ipl-data-analysis-project/Player.csv`  
- `s3://ipl-data-analysis-project/Player_match.csv`  
- `s3://ipl-data-analysis-project/Team.csv`

> Temp views created in Spark:
`ball_by_ball`, `match`, `player`, `player_match`, `team`

---

## 🧱 Project Structure

- `IPL-DataAnalysis-ApacheSpark.ipynb` — main Databricks/Colab-compatible PySpark notebook  
  - Creates a `SparkSession`
  - Defines **StructType** schemas
  - Loads CSVs from S3 with headers
  - Registers **Temp Views**
  - Runs **Spark SQL** queries (with **window**/ranking)
  - Generates visualizations (Matplotlib/Seaborn)

---

## ⚙️ Run on Databricks (Recommended)

1. **Create/Attach a Cluster**
   - Databricks Runtime: **12.x–14.x** (includes Spark + Python)
   - Libraries typically available by default; if not, install `matplotlib` and `seaborn` via cluster Libraries or `%pip install`.

2. **AWS S3 Access**
   - If your workspace has an **instance profile** / **Unity Catalog** set up for S3, you can read directly.  
   - Otherwise, set credentials at the cluster or notebook level (e.g., secret scope) and use `spark.conf.set(...)` or Databricks mounts as per your org’s policy.

3. **Import the Notebook**
   - `Workspace → Import → Upload` `IPL-DataAnalysis-ApacheSpark.ipynb`.

4. **Run**
   - Attach the cluster and run all cells.
   - Verify the S3 paths (update if your bucket or folder names differ).

---

## 🧪 Key Transformations & Queries (Examples)

**Typed Schemas:** `StructType` for robust CSV reads (dates, integers, booleans).

**Filters:**  
Exclude non-legal deliveries for certain analyses:
```python
from pyspark.sql.functions import col
ball_by_ball_df = ball_by_ball_df.filter((col("wides") == 0) & (col("noballs") == 0))
