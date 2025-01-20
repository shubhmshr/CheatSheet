## Optimising schema inference for reading data in Apache Spark

- It is critical for improving job startup time, especially when dealing with large datasets or numerous small files. By default, schema inference can be an expensive operation as Spark scans a portion of the dataset to deduce the schema. Below are strategies to optimise schema inference

##### **Avoid Schema Inference (Explicit Schema Definition)**

Manually specifying the schema is the most effective way to avoid the overhead of schema inference.

##### ** Benefits** :

- Eliminates the need for Spark to infer the schema.
- Ensures consistent data types regardless of source changes.

```
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

# Define the schema
schema = StructType([
    StructField("id", IntegerType(), True),
    StructField("name", StringType(), True),
    StructField("age", IntegerType(), True)
])

# Read data with a predefined schema
df = spark.read.csv("path/to/file.csv", schema=schema, header=True)
```

##### **Parallelised File Reading**

Large datasets with numerous small files can slow down schema inference. Use Spark configurations to parallelised file listing and scanning:

```
spark.conf.set("spark.sql.files.maxPartitionBytes", "134217728")  # Reduce partition size
spark.conf.set("spark.sql.files.openCostInBytes", "4194304")     # Optimize file handling
```

##### **Use Data Skipping Techniques**

When reading from formats like Parquet or ORC, Spark can leverage column pruning or predicate push-down to reduce schema inference work.

```
spark.conf.set("spark.sql.parquet.filterPushdown", "true")
spark.conf.set("spark.sql.orc.filterPushdown", "true")

```

##### **Enable Push-down Optimisations**

For formats like `CSV` and `JSON`, enabling push-down filters can help Spark infer schema more efficiently by reducing the amount of data it reads.

```
spark.conf.set("spark.sql.pushdown.inferSchema", "true")

```