# Databricks Documentation

## Overview
The Databricks environment is utilized for transforming the Tokyo Olympic data. This document outlines the setup, the transformations performed, and how to execute the notebook.

## Environment Setup

### Cluster Configuration
- **Node Type**: Standard_D4ds_v5
- **Number of Nodes**: 1 Driver
- **Memory**: 16 GB
- **Cores**: 4
- **Runtime**: Databricks Runtime 14.3.x-scala2.12
- **Photon Enabled**: Yes
- **Unity Catalog**: Enabled
- **Cost**: 2 DBU/h

### Libraries and Dependencies
The following libraries and functions are used in the project:
- **PySpark Functions**:
  - `from pyspark.sql.functions import col, sum` - For column operations and aggregations.
- **PySpark Data Types**:
  - `from pyspark.sql.types import IntegerType, DoubleType, BooleanType, DateType` - For schema definition.

## Notebooks

### Notebook Overview
- **Tokyo_Olympics_Data.ipynb**: This notebook performs the following actions:
  - Imports data from the raw data folder.
  - Transforms the data, specifically converting columns in `entriesgender` to integer types.
  - Exports the transformed data to the transformed data folder.

### Notebook File
The notebook file can be accessed [here](https://github.com/HannibalGh/Azure-DE-Project-Tokyo-Olympic-Data-Analytics/blob/main/Databricks/Notebooks/Tokyo_Olympics_Data.ipynb).

### Code Explanation
The notebook performs data type conversion on the `entriesgender` DataFrame. It imports CSV files, transforms the data types, and saves the transformed data.

## Data Transformation

### Process Overview
- **Data Import**: Data is read from the `raw-data` folder in the data lake.
- **Data Transformation**: The `entriesgender` DataFrame's columns are converted to integer types.
- **Data Export**: The transformed data is saved to the `transformed-data` folder in the data lake.

### Transformation Details
- **Data Cleansing**: No cleansing was performed; only data type conversion.
- **Feature Engineering**: No additional features were created; only type casting was done.

### Code Examples
```python
# Import necessary functions and types from PySpark
from pyspark.sql.functions import col, sum
from pyspark.sql.types import IntegerType, DoubleType, BooleanType, DateType

# Load the data
entriesgender = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/mnt/tokyoolympics/raw-data/entriesgender.csv")

# Convert columns to IntegerType
entriesgender = entriesgender.withColumn("Female", col("Female").cast(IntegerType())) \
                             .withColumn("Male", col("Male").cast(IntegerType())) \
                             .withColumn("Total", col("Total").cast(IntegerType()))

# Save the transformed data
entriesgender.repartition(1).write.mode("overwrite").option("header", "true").csv("/mnt/tokyoolympics/transformed-data/entriesgender")
