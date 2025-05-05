:original_name: dws_01_0007.html

.. _dws_01_0007:

Advantages
==========

GaussDB(DWS) supports ANSI/ISO SQL-92, SQL-99, and SQL-2003 syntax, as well as the PostgreSQL, Oracle, Teradata, and MySQL database ecosystems. It offers powerful solutions for analyzing massive amounts of data in different industries, even at the petabyte scale.

Unlike conventional data warehouses, GaussDB(DWS) excels in mass data processing and general platform management with the following features:

**Ease of use**

-  Visualized one-stop management

   GaussDB(DWS) simplifies the entire process from project conception to production deployment. The GaussDB(DWS) console allows you to quickly set up a high-performance and highly available enterprise-level data warehouse cluster in just a few minutes, without requiring any data warehouse software or servers.

   With just a few clicks, you can easily connect applications to the data warehouse, back up data, restore data, and monitor data warehouse resources and performance.

-  Seamless integration with big data

   Without the need to migrate data, you can use standard SQL statements to directly query data on HDFS and OBS.

-  Heterogeneous database migration tools

   GaussDB(DWS) provides various migration tools to migrate SQL scripts of Oracle and Teradata to GaussDB(DWS).

**High performance**

-  Cloud-based distributed architecture

   GaussDB(DWS) adopts the MPP-based database so that service data is separately stored on numerous nodes. Data analysis tasks are executed in parallel on the nodes where data is stored. The massively parallel data processing significantly improves response speed.

-  Query response to trillions of data records within seconds

   GaussDB(DWS) improves data query performance by executing multi-thread operators in parallel, running commands in registers in parallel with the vectorized computing engine, and reducing redundant judgment conditions using LLVM.

   GaussDB(DWS) provides you with a better data compression ratio (column-store), better indexing (column-store), and higher point update and query (row-store) performance.

-  Fast data loading

   GDS is a tool that helps you with high-speed massively parallel data loading.

-  Data Compression in Column Storage

   To compress old and inactive data to save space and reduce procurement and O&M costs.

   In GaussDB(DWS), data can be compressed using the Delta Value Encoding, Dictionary, RLE, LZ4, and ZLIB algorithms. The system automatically selects a compression algorithm based on data characteristics. The average compression ratio is 7:1. Compressed data can be directly accessed and is transparent to services, greatly reducing the preparation time before accessing historical data.

**High scalability**

-  On-demand scale-out: With the shared-nothing open architecture, nodes can be added at any time to enhance the data storage, query, and analysis capabilities of the system.
-  Enhanced linear performance after scale-out: The capacity and performance increase linearly with the cluster scale. The linear rate is 0.8.
-  Service continuity: During scale-out, data can be added, deleted, modified, and queried, and DDL operations (**DROP**/**TRUNCATE**/**ALTER TABLE**) can be performed. Table-level scale-out ensures service continuity.
-  Online upgrade: Upgrading major versions online from 8.1.1 and performing online patch upgrades from 8.1.3 and later versions is now possible without interrupting your services. Any interruptions will only last a few seconds.

**Robust reliability**

-  Transaction management

   -  Transaction blocks are supported. You can run **start transaction** to explicitly start a transaction block.
   -  Single-statement transactions are supported. If you do not explicitly start a transaction, a single statement is processed as a transaction.
   -  Distributed transaction management and global transaction information management are supported. This includes gxid, snapshot, timestamp management, distributed transaction status management, and gxid overflow processing.
   -  The atomicity, consistency, isolation, and durability (ACID) feature is supported, which ensures strong data consistency for distributed transactions.
   -  Deadlocks are prevented in the distributed system. A transaction will be unlocked immediately after a deadlock (if any).

-  Comprehensive HA design

   All software processes of GaussDB(DWS) are in active/standby mode. Logical components such as the CNs and DNs of each cluster also work in active/standby mode. This ensures data reliability and consistency when any single point of failure (SPOF) occurs.

-  High security

   GaussDB(DWS) supports transparent data encryption and can interconnect with the Database Security Service (DBSS) to better protect user privacy and data security with network isolation and security group rule setting options. In addition, GaussDB(DWS) supports automatic full and incremental backup of data, improving data reliability.
