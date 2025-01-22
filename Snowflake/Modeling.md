## How data is stored
- Data is stored in a *columnar format* using continuous storage units called **micro-partitions**.
- Micro-partitions are immutable.
- DML operations are more efficient and always create new partitions.
- Databases divide large tables into *physical partitions* to improve *performance* and *scalability*. However, physical partitions *require maintenance* and result in *data-skew*.
- snowflake escapes above limitation by keeping micro-partitions small(50-500 MB uncompressed) and managing them through Service layer.
- Due to smaller size, granular statistics can be kept for individual columns within a micro-partition and each one compresses optimally based on its data type.
- Micro-partitions can overlap in their value ranges but a single record can never be split across multiple partitions.
- These are created by *natural ordering* of data as it is loaded into table i.e how the table is being sorted.
- Micro-partitions allows snowflake optimizer to *prune* user queries which helps in avoiding search through micro-partitions where filter values are known not to exist.

## Clustering

- As table data is broken up in micro-partitions, column values naturally group themselves into data segments. The *degree of variance of column values and their overlap* across micro-partitions is known as *clustering*.
- Lesser the overlap between partitions, higher the degree of query and column pruning that the optimizer can provide.

	![Pasted%20image%2020241103164632.png](https://github.com/shubhmshr/CheatSheet/blob/main/Pasted%20image%2020241103164632.png))
- Snowflake services layer also stores clustering statistics:
	- Total no of micro-partitions in each table.
	- No of micro-partitions containing values that overlap with each other.
	- The depth of overlapping micro-partitions.
- Poor query performance could be assessed by:
	- High table scan percentage
	- Lack of pruning
- To fix above, we do *reclustering* of the table
- Reclustering can be performed by:
	- Manually recreating the table with required sorting
		- It is costly on large table
	- Automatically by Snowflake
		- It is done through natural loading pattern of the incoming data
		- To avoid costly manual re-sorting, snowflake uses a *clustering key*
			- It is one/more columns that are used to automatically sort the data in a table and transparently re-sort it when necessary.
			- It improves query performance through pruning
			- Better column compression and reduced storage costs.
			- Maintenance free automated reclustering performed.
			- Preferable where performance is prioritised over cost 
			- When to use:
				- Table contains multiple TBs of data
				- Queries need to read only small percentage of rows or sort the data
				- High percentage of queries can benefit from same clustering key
		- It has the same processing cost as manually.
		
