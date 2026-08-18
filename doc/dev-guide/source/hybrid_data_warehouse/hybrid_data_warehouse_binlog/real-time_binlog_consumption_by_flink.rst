:original_name: dws_04_1212.html

.. _dws_04_1212:

Real-Time Binlog Consumption by Flink
=====================================

Precautions
-----------

-  Currently, only 8.3.0.100 and later versions support HStore and HStore Opt tables to record binlogs.
-  V3 HStore tables do not support binlogs, whereas only V3 HStore Opt tables offer binlog support. V3 is currently in trial commercial use and should be thoroughly evaluated before deployment.
-  The Binlog function is only supported for HStore and HStore Opt tables in DWS. These tables must have primary keys and one of parameters **enable_binlog** and **enable_binlog_timestamp** must be set to **on**.
-  The name of the consumed binlog table cannot contain special characters, such as periods (.) and double quotation marks (").
-  If multiple tasks consume binlog data of a single table, ensure that **binlogSlotName** of each task is unique.
-  For maximum consumption speed, match task concurrency with the number of DNs in your DWS cluster.
-  If a Flink exception occurs during full synchronization, you are advised to change the value of **binlogSlotName** and perform full synchronization again. Otherwise, data may be inconsistent.
-  When Flink is used for incremental synchronization, if it restarts due to an exception, it will extract incremental data from the last checkpoint. To prevent data loss, the same incremental data may be consumed again. To avoid this repeated consumption, ensure that the task status can be restored after a restart.
-  If you use the sink capability of "dws-connector-flink" in *Data Warehouse Service (DWS) Tool Guide* to write binlog data, pay attention to the following:

   -  To ensure the data write sequence on DNs, set **connectionSize** to **1**.
   -  If the primary key is updated on the source end or Flink is required for aggregation calculation, set **ignoreUpdateBefore** to **false**. Otherwise, you are not advised to set **ignoreUpdateBefore** to **false** (the default value is **true**).


Real-Time Binlog Consumption by Flink
-------------------------------------

Use DWS Connector to consume binlogs in real time. For details, see "DWS-Connector" in *Data Warehouse Service (DWS) Tool Guide*.

If full data has been synchronized to the target end using other synchronization tools, and only incremental synchronization is required, you can call the following system function to update the synchronization points.

::

   SELECT * FROM pg_catalog.pgxc_register_full_sync_point('table_name', 'slot_name');

Source Table DDL
----------------

The source autonomously assigns the appropriate Flink RowKind type (INSERT, DELETE, UPDATE_BEFORE, or UPDATE_AFTER) to each data row based on the operation type. This mechanism facilitates the synchronization of table data in a mirrored way, akin to the Change Data Capture (CDC) feature in MySQL and PostgreSQL databases.

::

   CREATE TABLE test_binlog_source (
      a int,
      b int,
      c int,
      primary key(a) NOT ENFORCED
   ) with (
      'connector' = 'dws',
      'url' = 'jdbc:gaussdb://ip:port/gaussdb',
      'binlog' = 'true',
      'tableName' = 'test_binlog_source',
      'binlogSlotName' = 'slot',
      'username'='xxx',
      'password'='xxx')

Binlog Parameters
-----------------

The following table describes the parameters involved in binlog consumption.

.. table:: **Table 1** Parameters

   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | Parameter                   | Description                                                                                                                                                                                                                                                             | Data Type       | Default Value                   |
   +=============================+=========================================================================================================================================================================================================================================================================+=================+=================================+
   | binlog                      | Whether to read binlog information.                                                                                                                                                                                                                                     | Boolean         | false                           |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogSlotName              | Slot, which serves as an identifier. Multiple Flink tasks can simultaneously consume binlog data of the same table, so each task's **binlogSlotName** must be unique.                                                                                                   | String          | Name of the Flink mapping table |
   |                             |                                                                                                                                                                                                                                                                         |                 |                                 |
   |                             | If binlog is specified but **binlogSlotName** is not specified, the table name of the Flink mapping table is used by default to ensure that incremental data is correctly read.                                                                                         |                 |                                 |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogBatchReadSize         | Rows of binlog data read in batches.                                                                                                                                                                                                                                    | Integer         | 5000                            |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | fullSyncBinlogBatchReadSize | Rows of binlog data fully read.                                                                                                                                                                                                                                         | Integer         | 50000                           |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogReadTimeout           | Timeout for incrementally consuming binlog data, in milliseconds.                                                                                                                                                                                                       | Integer         | 600000                          |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | fullSyncBinlogReadTimeout   | Timeout for fully consuming binlog data, in milliseconds.                                                                                                                                                                                                               | Long            | 1800000                         |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogSleepTime             | Sleep duration when no real-time binlog data is consumed, in milliseconds. The sleep duration with consecutive read failures is **binlogSleepTime** \* failures, up to **binlogMaxSleepTime**. The value is reset after successful data read.                           | Long            | 500                             |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogMaxSleepTime          | Maximum sleep duration when no real-time binlog data is consumed, in milliseconds.                                                                                                                                                                                      | Long            | 10000                           |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogMaxRetryTimes         | Maximum number of retries after a binlog data consumption error.                                                                                                                                                                                                        | Integer         | 1                               |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogRetryInterval         | Interval between retries after a binlog data consumption error, in milliseconds. Sleep duration during retry, which is calculated as **binlogRetryInterval** \* (1~\ **binlogMaxRetryTimes**) + Random(100). The unit is millisecond.                                   | Long            | 100                             |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogParallelNum           | Number of threads for consuming binlog data. This parameter is valid only when task concurrency is less than the number of DNs in the DWS cluster.                                                                                                                      | Integer         | 3                               |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | connectionPoolSize          | Number of connections in the JDBC connection pool.                                                                                                                                                                                                                      | Integer         | 5                               |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | needRedistribution          | Compatibility with expansion redistribution. To ensure compatibility, upgrade the kernel to the corresponding version. If the kernel is an older version, set this parameter to **false**. If set to **true**, **restart-strategy** of Flink cannot be set to **none**. | Boolean         | true                            |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | newSystemValue              | Whether to use the new system field when reading binlog data. (The kernel needs to be upgraded to the corresponding version. If the kernel is an older version, set this parameter to **false**.)                                                                       | Boolean         | true                            |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | checkNodeChangeInterval     | Interval for detecting node changes. This parameter is valid only when **needRedistribution** is set to **true**.                                                                                                                                                       | Long            | 10000                           |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | connectionSocketTimeout     | Timeout interval for connection processing, in milliseconds. It can also be considered as the timeout interval for executing SQL statements on the client. The default value is **0**, which means that the timeout interval is not set.                                | Integer         | 0                               |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogIgnoreUpdateBefore    | Whether to filter out **before_update** records in binlogs and whether to return only primary key information for **delete** records. This parameter is supported only by clusters of version 9.1.0.200 or later.                                                       | Boolean         | false                           |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogStartTime             | Time point from which binlogs are consumed can be set using the format **yyyy-MM-dd hh:mm:ss**. :ref:`enable_binlog_timestamp <en-us_topic_0000001764650772__li125481342133319>` must be enabled for the table.                                                         | String          | N/A                             |
   |                             |                                                                                                                                                                                                                                                                         |                 |                                 |
   |                             | This parameter is supported only by clusters of version 9.1.0.200 or later.                                                                                                                                                                                             |                 |                                 |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogSyncPointSize         | Size of the synchronization point range for incrementally reading binlogs. This can control data flushing if the data volume is too large.                                                                                                                              | Integer         | 5000                            |
   |                             |                                                                                                                                                                                                                                                                         |                 |                                 |
   |                             | This parameter is supported only by clusters of version 9.1.0.200 or later.                                                                                                                                                                                             |                 |                                 |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | executeDirectOn             | Optimization for binlog reading. During full synchronization, this parameter prevents CN result sets from being flushed to disks. During incremental synchronization, this parameter improves the synchronization performance by 3 to 4 times.                          | Boolean         | false                           |
   |                             |                                                                                                                                                                                                                                                                         |                 |                                 |
   |                             | This parameter is supported only by clusters of version 9.1.0.200 or later.                                                                                                                                                                                             |                 |                                 |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+
   | binlogQueueSize             | Queue length during incremental binlog reading. If the Flink compute and memory are overloaded, you can decrease the value of this parameter for throttling.                                                                                                            | Integer         | 0 (no limit)                    |
   |                             |                                                                                                                                                                                                                                                                         |                 |                                 |
   |                             | This parameter is supported only by clusters of version 9.1.0.200 or later.                                                                                                                                                                                             |                 |                                 |
   +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+---------------------------------+

Data Synchronization Example
----------------------------

-  DWS:

   .. note::

      When creating a binlog table, set :ref:`enable_hstore_binlog_table <en-us_topic_0000001764491796__section19418514112914>` to **true**. You can run the **show enable_hstore_binlog_table** command to query the binlog table.

   -- Source table (generating binlogs)

   ::

      CREATE TABLE test_binlog_source(a int, b int, c int, primary key(a)) with(orientation=column, enable_hstore_opt=on, enable_binlog=true);

   -- Target table

   ::

      CREATE TABLE test_binlog_sink(a int, b int, c int, primary key(a)) with(orientation=column, enable_hstore_opt=on);

-  On Flink:

   Run the following commands to perform complete data synchronization:

   ::

      -- Create a mapping table for the source table.
      CREATE TABLE test_binlog_source (
         a int,
         b int,
         c int,
         primary key(a) NOT ENFORCED
      ) with (
         'connector' = 'dws',
         'url' = 'jdbc:gaussdb://ip:port/gaussdb',
         'binlog' = 'true',
         'tableName' = 'test_binlog_source',
         'binlogSlotName' = 'slot',
         'username'='xxx',
         'password'='xxx');

      -- Create a mapping table for the target table:
      CREATE TABLE test_binlog_sink (
         a int,
         b int,
         c int,
         primary key(a) NOT ENFORCED
      ) with (
         'connector' = 'dws',
         'url' = 'jdbc:gaussdb://ip:port/gaussdb',
         'tableName' = 'test_binlog_sink',
         'ignoreUpdateBefore'='false',
         'connectionSize' = '1',
         'username'='xxx',
         'password'='xxx');
      ​
      INSERT INTO test_binlog_sink select * from test_binlog_source;

Example of Using a Java Program
-------------------------------

Create a source table and a target table.

::

   -- source
   create table binlog_test_source(a int, b int, c int, primary key(a)) with(orientation=column, enable_hstore_opt=on, enable_binlog=true);
   -- sink
   create table binlog_test_sink(a int, b int, c int, primary key(a)) with(orientation=column, enable_hstore_opt=on, enable_binlog=true);

Demo program:

::

   public class BinlogDemo {

       //Name of the binlog table
       private static final String BINLOG_TABLE_NAME = "binlog_test_source";

       //Slot name of the binlog table
       private static final String BINLOG_SLOT_NAME = "binlog_test_slot";

       //Name of the table to be written
       private static final String SINK_TABLE_NAME = "binlog_test_sink";

       public static void main(String[] args) throws Exception {
           DwsConfig dwsConfig = buildDwsConfig();
           DwsClient dwsClient = new DwsClient(dwsConfig);

           TableSchema sourceTableSchema = dwsClient.getTableSchema(TableName.valueOf(BINLOG_TABLE_NAME));
           TableSchema sinkTableSchema = dwsClient.getTableSchema(TableName.valueOf(SINK_TABLE_NAME));

           // Columns to be written
           List<String> sinkColumns = sinkTableSchema.getColumnNames();

           // Thread pool
           DwsConnectionPool dwsConnectionPool = new DwsConnectionPool(dwsConfig);
           //Queue for storing data
           BlockingQueue<BinlogRecord> queue = new LinkedBlockingQueue<>();
           //Columns to be synchronized
           List<String> sourceColumnNames = sourceTableSchema.getColumnNames();

           BinlogReader binlogReader = new BinlogReader(dwsConfig, queue, sourceColumnNames, dwsConnectionPool);

           //Start the read task.
           binlogReader.start();
           binlogReader.getRecords();

           while (binlogReader.isStart()) {
               try {
                   while (!queue.isEmpty() && !binlogReader.hasException()) {
                       // Read data.
                       BinlogRecord record = queue.poll();
                       if (Objects.isNull(record)) {
                           continue;
                       }
                       BinlogRecordType type = BinlogRecordType.toBinlogRecordType(record.getType());
                       List<Object> columnValues = record.getColumnValues();

                       // Write data.
                       if (BinlogRecordType.INSERT.equals(type) || BinlogRecordType.UPDATE_AFTER.equals(type)) {
                           Operate upsert = dwsClient.write(sinkTableSchema);
                           for (int i = 0; i < sinkColumns.size(); i++) {
                               upsert.setObject(i, columnValues.get(i), false);
                           }
                           upsert.commit();
                       } else if (BinlogRecordType.DELETE.equals(type) || BinlogRecordType.UPDATE_BEFORE.equals(type)) {
                           Operate delete = dwsClient.delete(sinkTableSchema);
                           for (int i = 0; i < sinkColumns.size(); i++) {
                               String field = sinkColumns.get(i);
                               if (!sinkTableSchema.isPrimaryKey(field)) {
                                   continue;
                               }
                               delete.setObject(i, columnValues.get(i), false);
                           }
                           delete.commit();
                       }
                   }
                   binlogReader.checkException();
               } catch (Exception e) {
                   throw new DwsClientException(ExceptionCode.GET_BINLOG_ERROR, "get binlog  has error", e);
               }
           }
       }

       private static DwsConfig buildDwsConfig() {
           //Initialize configuration information. (Only necessary parameters are listed. For more information about the configuration, see the section.)
           TableConfig tableConfig = new TableConfig().withBinlog(true)
                   .withNewSystemValue(true)
                   .withNeedRedistribution(false)
                   .withBinlogSlotName(BINLOG_SLOT_NAME);
           return DwsConfig.builder()
                   .withUrl("Link information")
                   .withUsername("Username")
                   .withPassword ("Password")
                   .withBinlogTableName(BINLOG_TABLE_NAME)
                   .withTableConfig(BINLOG_TABLE_NAME, tableConfig)
                   .build();
       }
   }
