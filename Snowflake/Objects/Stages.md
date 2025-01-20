- Logical objects that abstract cloud filesystems so they can be used in a standard manner to load data into Snowflake
- Two types of stages:
	- External
		- Can be created on top of a cloud location outside Snowflake
		- Used to load data from external source systems
		- Supported cloud storage
			- S3 buckets
			- GCS buckets
			- Azure containers
		![[Pasted image 20241024201857.png]]
	- Internal
		- Created within snowflake account
		- Uses the storage type of the hosting provider.
		- Used to stage files that originate from within the Snowflake account and cannot be used to load external data.
  
- Can store any type of file and variety of file types. However file format must be specified when accessing data from a stage for subsequent loading into a snowflake table.


## Tables

- Data in snowflake is stored in tables.
- Types:
	- Permanent
	- Transient
	- Temporary
	- Hybrid Unistore 