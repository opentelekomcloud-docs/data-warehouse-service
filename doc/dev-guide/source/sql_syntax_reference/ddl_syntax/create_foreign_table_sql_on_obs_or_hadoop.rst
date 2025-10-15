:original_name: dws_06_0161.html

.. _dws_06_0161:

CREATE FOREIGN TABLE (SQL on OBS or Hadoop)
===========================================

Function
--------

**CREATE FOREIGN TABLE** creates an HDFS or OBS foreign table in the current database to access structured data stored on HDFS or OBS. You can also export data in ORC or PARQUET format to HDFS or OBS.

Data stored in OBS: Data storage is decoupled from compute. The cluster storage cost is low, and storage capacity is not limited. Clusters can be deleted at any time. However, the computing performance depends on the OBS access performance and is lower than that of HDFS. OBS is recommended for applications that do not demand a lot of computation.

Data stored in HDFS: Data storage is not decoupled from compute. The cluster storage cost is high, and storage capacity is limited. The computing performance is high. You must export data before you delete clusters. HDFS is recommended for computing-intensive scenarios.

.. note::

   -  The hybrid data warehouse (standalone) does not support HDFS foreign tables.
   -  The hybrid data warehouse (standalone) 8.2.0.100 and later versions support OBS foreign tables, but you need to set the foreign data wrapper of the server to **DFS_FDW**.
   -  The following read and write foreign tables are supported only in the storage-compute decoupling 3.0 version.

Precautions
-----------

-  The HDFS foreign table and OBS foreign table are classified into read-only, write-only, and read-write foreign tables. Read-only foreign tables are used for query. Write-only foreign tables can be used to export data from GaussDB(DWS) to the distributed file system. Read-write foreign tables can be used for query and data export. Only 9.1.0.100 and later versions support read-write foreign tables.
-  This mode allows you to import and query data in a variety of formats including ORC, TEXT, CSV, CARBONDATA, PARQUET, and JSON. If you are dealing with OBS foreign tables, you can export them in the ORC format or the PARQUET format (only supported in the storage-compute decoupling 3.0 version). HDFS foreign tables can be exported in ORC or PARQUET format.
-  In this mode, you need to manually create a foreign server. For details, see :ref:`CREATE SERVER <dws_06_0175>`.
-  If the foreign data wrapper is set to **HDFS_FDW** or **DFS_FDW** when you manually create a server, you need to specify the distribution mode in the **DISTRIBUTE BY** clause when creating a read-only foreign table.

.. table:: **Table 1** Read and write formats supported by OBS foreign tables

   ========== ================ ========== ==========
   Data Type  DFS_FDW/HDFS_FDW
   ========== ================ ========== ==========
   ``-``      READ ONLY        WRITE ONLY READ WRITE
   ORC        Y                Y          Y
   PARQUET    Y                Y          Y
   CARBONDATA Y                x          x
   TEXT       Y                x          x
   CSV        Y                x          x
   JSON       Y                x          x
   ========== ================ ========== ==========

Syntax
------

Create an HDFS foreign table.

::

   CREATE FOREIGN TABLE [ IF NOT EXISTS ] table_name
   ( [ { column_name type_name
       [ { [CONSTRAINT constraint_name] NULL |
       [CONSTRAINT constraint_name] NOT NULL |
         column_constraint [...]} ] |
         table_constraint [, ...]} [, ...] ] )
       SERVER server_name
       OPTIONS ( { option_name ' value ' } [, ...] )
       [ {WRITE ONLY | READ ONLY | READ WRITE} ]
       DISTRIBUTE BY {ROUNDROBIN | REPLICATION}

       [ PARTITION BY ( column_name ) [ AUTOMAPPED ] ] ;

-  **column_constraint** is as follows:

   ::

      [CONSTRAINT constraint_name]
      {PRIMARY KEY | UNIQUE}
      [NOT ENFORCED [ENABLE QUERY OPTIMIZATION | DISABLE QUERY OPTIMIZATION] | ENFORCED]

-  **table_constraint** is as follows:

   ::

      [CONSTRAINT constraint_name]
      {PRIMARY KEY | UNIQUE} (column_name)
      [NOT ENFORCED [ENABLE QUERY OPTIMIZATION | DISABLE QUERY OPTIMIZATION] | ENFORCED]

.. _en-us_topic_0000001811634745__s755e54aa01f04a4bb44806bedcebdab4:

Parameter Description
---------------------

-  **IF NOT EXISTS**

   Does not throw an error if a table with the same name exists. A notice is issued in this case.

-  **table_name**

   Specifies the name of the foreign table to be created.

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Specifies the name of a column in the foreign table. Columns are separated by commas (,).

   Value range: a string. It must comply with the naming convention.

   .. note::

      A JSON object consists of nested or parallel name-value pairs, which are irrelevant to the sequence. When data in JSON format is imported, the mapping between fields and values is determined based on the automatic mapping between field names and names of name-value pairs. You need to define proper field names. Otherwise, you may not get the expected result. The rules for automatic mapping between field names and names of name-value pairs are as follows:

      -  If there are no nesting or arrays, the field names must be the same as the names of name-value pairs, case insensitive.
      -  Use underscores (_) to concatenate two names to identify the nesting relationship.
      -  A field name uses the number sign (#) and a decimal non-negative integer (n) to identify the nth element (starting from 0) of an array.

      For example, to import each element of the **{"A": "simple", "B": {"C": "nesting"}, "D": ["array", 2, {"E": "complicated"}]}** object, the field names in the foreign table must be defined as **a**, **b**, **b_c**, **d**, **d#0**, **d#1**, **d#2** and **d#2_e**. The sequence in which the fields are defined does not affect the import result.

-  **type_name**

   Specifies the data type of the column.

-  **constraint_name**

   Specifies the name of a constraint for the foreign table.

-  **{ NULL \| NOT NULL }**

   Specifies whether the column allows **NULL**.

   When you create a table, whether the data in HDFS is **NULL** or **NOT NULL** cannot be guaranteed. The consistency of data is guaranteed by users. Users must decide whether the column is **NULL** or **NOT NULL**. (The optimizer optimizes the **NULL/NOT NULL** and generates a better plan.)

-  **SERVER server_name**

   Specifies the server name of the foreign table. Users can customize its name.

   Value range: a string indicating an existing server. It must comply with the naming convention.

-  **OPTIONS ( { option_name ' value ' } [, ...] )**

   Specifies the following parameters for a foreign table:

   -  header

      Specifies whether a data file contains a table header. **header** is available only for CSV files.

      If **header** is **on**, the first row of the data file will be identified as the header and ignored during export. If **header** is **off**, the first row will be identified as a data row.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

   -  quote

      Specifies the quotation mark for the CSV format. The default value is a double quotation mark (").

      .. note::

         The **quote** value cannot be the same as the **delimiter** or **null** value.

         The **quote** value must be a single-byte character.

         Invisible characters are recommended as **quote** values, such as 0x07, 0x08, and 0x1b.

   -  escape

      Specifies an escape character for a CSV file. The value must be a single-byte character.

      The default value is a double quotation mark ("). If the value is the same as the **quote** value, it will be replaced with **\\0**.

   -  location

      Specifies the file path on OBS. This is an OBS foreign table parameter. The data sources of multiple buckets are separated by vertical bars (|), for example, **LOCATION 'obs://bucket1/folder/ \| obs://bucket2/'**. The database scans all objects in the specified folders.

      When accessing a DLI multi-version table, you do not need to specify the **location** parameter.

   -  **format**: format of the data source file in the foreign table.

      The HDFS foreign table can read data in ORC, TEXT, JSON, CSV, and Parquet file formats when in READ ONLY mode. But if you are using the WRITE ONLY/READ WRITE foreign table, you can only work with data in ORC and PARQUET file formats.

      OBS foreign tables allow you to read data in ORC, TEXT, JSON, CSV, CarbonData, Parquet, and Hudi file formats in read-only mode. But if you are using write-only mode, you can only work with data in ORC and Parquet file formats.

      .. note::

         -  Only JSON objects (embraced in {}) can be imported. JSON arrays (embraced in []) cannot be imported. However, arrays inside a JSON object can be imported.
         -  Apache Hudi is a transactional data lake platform. In the current version, you can set the **format** parameter of an OBS foreign table to **Hudi** to access the corresponding Hudi table data sets. This version also provides the method of accessing the incremental or real-time view of the Hudi table.

   -  **foldername**: The directory of the data source file in the foreign table, that is, the corresponding file directory in HDFS or on OBS. This is mandatory for WRITE ONLY and READ WRITE writable foreign tables and optional for READ ONLY foreign tables.

      When accessing a DLI multi-version table, you do not need to specify the **foldername** parameter.

   -  **encoding**: encoding of data source files in foreign tables. The default value is **utf8**. This parameter is optional.

   -  **totalrows**: (Optional) estimated number of rows in a table. This parameter is used only for OBS foreign tables. Because OBS may store many files, it is slow to analyze data. This parameter allows you to set an estimated value so that the optimizer can estimate the table size according to the value. Generally, query efficiency is high when the estimated value is close to the actual value.

   -  **filenames**: data source files specified in the foreign table. Multiple files are separated by commas (,).

      .. note::

         -  You are advised to use the **foldername** parameter to specify the location of the data source. For a read-only foreign table, either **filenames** or **foldername** must be specified. For a write-only foreign table or read-write foreign table, only **foldername** can be specified.
         -  If **foldername** is an absolute directory, it should be enclosed by slashes (/). Multiple paths are separated by commas (,).
         -  When you query a partitioned table, data is pruned based on partition information, and data files that meet the requirement are queried. Pruning involves scanning HDFS directory contents many times. Therefore, do not use columns with low repetition as partition column.
         -  OBS read-only foreign tables and read/write foreign tables are not supported.

   -  delimiter

      Specifies the column delimiter of data, and uses the default delimiter if it is not set. The default delimiter of TEXT is a tab.

      .. note::

         -  A delimiter cannot be \\r or \\n.
         -  A delimiter cannot be the same as the null parameter.
         -  A separator cannot contain (\\), (.), digits, or letters.
         -  The data length of a single row should be less than 1 GB. A row that has many columns using long delimiters cannot contain much valid data.
         -  You are advised to use a multi-character, such as the combination of the dollar sign ($), caret (^), ampersand (&), or invisible characters, such as 0x07, 0x08, and 0x1b as the delimiter.
         -  **delimiter** is available only for TEXT and CSV source data files.

      Valid value:

      The value of **delimiter** can be a multi-character delimiter whose length is less than or equal to 10 bytes.

   -  eol

      Specifies the newline character style of the imported data file.

      Value range: multi-character newline characters within 10 bytes. Common newline characters include **\\r** (0x0D), **\\n** (0x0A), and **\\r\\n** (0x0D0A). Special newline characters include **$** and **#**.

      .. note::

         -  The **eol** parameter applies only to TEXT files.
         -  The value of the **eol** parameter cannot be the same as that of **DELIMITER** or **NULL**.
         -  The value of the **eol** parameter cannot contain digits, letters, or periods (.).

   -  null

      Specifies the string that represents a null value.

      .. note::

         -  The null value cannot be \\r or \\n. The maximum length is 100 characters.
         -  The **null** parameter cannot be the same as the delimiter.
         -  **null** is available only for TEXT and CSV source data files.

      Valid value:

      The default value is **\\N** for the TEXT format.

   -  noescaping

      Specifies in TEXT format, whether to escape the backslash (\\) and its following characters.

      .. note::

         **noescaping** is available only for TEXT source data files.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

   -  fill_missing_fields

      Specifies whether to generate an error message when the last column in a row in the source file is lost during data loading.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on** and the last column of a data row in a data source file is lost, the column is replaced with **NULL** and no error message will be generated.

      -  If this parameter is set to **false** or **off** and the last column is missing, the following error information will be displayed:

         .. code-block::

            missing data for column "tt"

      .. note::

         -  Because **SELECT COUNT(*)** does not parse columns in TEXT format, it does not report missing columns.
         -  **fill_missing_fields** is available only for TEXT and CSV source data files.

   -  ignore_extra_data

      Specifies whether to ignore excessive columns when the number of data source files exceeds the number of foreign table columns. This parameter is available only during data importing.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on** and the number of data source files exceeds the number of foreign table columns, excessive columns will be ignored.

      -  If this parameter is set to **false** or **off** and the number of data source files exceeds the number of foreign table columns, the following error information will be displayed:

         .. code-block::

            extra data after last expected column

      .. important::

         -  If the newline character at the end of the row is lost, setting the parameter to **true** will ignore data in the next row.
         -  Because **SELECT COUNT(*)** does not parse columns in TEXT format, it does not report missing columns.
         -  **ignore_extra_data** is available only for TEXT and CSV source data files.

   -  date_format

      Specifies the DATE format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid DATE value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         -  If ORACLE is specified as the compatible database, the DATE format is TIMESTAMP. For details, see **timestamp_format** below.
         -  **date_format** is available only for TEXT and CSV source data files.

   -  time_format

      Specifies the TIME format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: a valid TIME value. Time zones cannot be used. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         **time_format** is available only for TEXT and CSV source data files.

   -  timestamp_format

      Specifies the TIMESTAMP format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid TIMESTAMP value. Time zones are not supported. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         **timestamp_format** is available only for TEXT and CSV source data files.

   -  smalldatetime_format

      Specifies the SMALLDATETIME format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: a valid SMALLDATETIME value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         **smalldatetime_format** is available only for TEXT and CSV source data files.

   -  dataencoding

      This parameter specifies the data code of the data table to be exported when the database code is different from the data code of the data table. For example, the database code is Latin-1, but the data in the exported data table is in UTF-8 format. This parameter is optional. If this parameter is not specified, the database encoding format is used by default. This syntax is valid only for the WRITE ONLY/READ WRITE foreign table of HDFS.

      Value range: data code types supported by the database encoding

      .. note::

         The **dataencoding** parameter is available only for HDFS foreign tables in ORC format with WRITE ONLY or READ WRITE.

   -  filesize

      Specifies the file size of a write-only foreign table. This parameter is optional. If this parameter is not specified, the file size in the distributed file system configuration is used by default. This syntax is valid only for WRITE ONLY/READ WRITE writable foreign tables.

      Value range: an integer ranging from 1 to 1024

      .. note::

         The **filesize** parameter is available only for HDFS foreign tables in ORC format with WRITE ONLY or READ WRITE.

   -  compression

      Specifies the file compression mode. This parameter is optional and is supported only by cluster versions 8.2.0 and later.

      For a write only/read-write foreign table, it specifies the compression mode for ORC files.

      For a READ ONLY foreign table, it specifies the compression mode for TEXT, CSV, or JSON files.

      Value range:

      -  For ORC files: the value can be **zlib** (default), **snappy**, and **lz4**.
      -  For Parquet files: **zlib**, **snappy** (default), **lz4**, **lz4_hadoop**.
      -  For TEXT, CSV, and JSON files: the value is **gzip**.

         .. caution::

            -  In versions earlier than 8.2.0, the parameter cannot be specified for READ ONLY foreign tables.
            -  The **lz4_hadoop** compression option is designed to work with the Hive LZ4 algorithm. By setting the compression format for a parquet foreign table, both the parquet foreign table and Hive can seamlessly exchange data.

   -  version

      Specifies the ORC version number. This parameter is optional. This syntax is available only for WRITE ONLY/READ WRITE foreign tables.

      Value range: Only **0.12** is supported. The default value is **0.12**.

   -  dli_project_id

      Specifies the project ID corresponding to DLI. You can obtain the project ID from the management console. This parameter is available only when the server type is DLI. This parameter is supported only in 8.1.1 or later.

   -  dli_database_name

      Specifies the name of the database where the DLI multi-version table to be accessed is located. This parameter is available only when the server type is DLI. This parameter is supported only in 8.1.1 or later.

   -  dli_table_name

      Specifies the name of the DLI multi-version table to be accessed. This parameter is available only when the server type is DLI. This parameter is supported only in 8.1.1 or later.

   -  cache_policy

      Specifies the disk cache policy for foreign tables. This parameter is supported only in the storage-compute decoupling 3.0 version.

      Value range: For foreign tables, this parameter can be set to **ALL** or **NONE**. **ALL** indicates that the hot cache in the disk cache is used, and **NONE** indicates that the cold cache is used. Hot cache occupies more space than cold cache and uses more complex replacement algorithms.

   -  checkencoding

      Specifies whether to check the character encoding.

      Value range: **no**, **low**, and **high** The default value is **low**.

      .. note::

         In TEXT format, the rule of error tolerance for invalid characters imported is as follows:

         -  **\\0** is converted to a space.
         -  Other invalid characters are converted to question marks.
         -  Setting **checkencoding** to **low** enables invalid characters toleration. If **NULL** and **DELIMITER** are set to spaces or question marks (?), errors like "illegal chars conversion may confuse null 0x20" will be displayed, prompting you to modify parameters that may cause confusion and preventing importing errors.

         In ORC format, the rule of error tolerance for invalid characters imported is as follows:

         -  If **checkencoding** is set to **no**, invalid characters are not checked during data import. If you confirm that there are no invalid characters, this option can shorten the import time.

         -  If **checkencoding** is **low**, an imported field containing invalid characters will be replaced with a quotation mark string of the same length.
         -  If **checkencoding** is **high**, data import stops when an invalid character is detected.

   -  force_mapping

      Indicates the handling method used when no correct name-value pairs are matched for the foreign table columns in JSON format.

      The value can be **true** or **false**. Default value: **true**

      -  If **force_mapping** is **true**, null is entered in the corresponding column. The meaning of null is the same as that defined in JSON.
      -  If **force_mapping** is **false**, an error is reported, indicating that the column does not exist.

      .. note::

         There are no restrictions on JSON objects. While the definition of foreign table fields must comply with GaussDB(DWS) identifier specifications (such as length and character restrictions). Therefore, this import method may cause exceptions. For example, a JSON name cannot be correctly identified or a field is repeatedly defined. You are advised to use the fault tolerance option **force_mapping** or JSON operators (for details, see :ref:`JSON/JSONB Functions and Operators <dws_06_0041>`).

         For JSON format, **SELECT COUNT(*)** does not parse specific fields. Therefore, no error is reported when a field is missing or the format is incorrect.

   -  auth_server

      Specifies a server, which is used for user identity authentication when KMS is used to encrypt data in the Hudi foreign table. This parameter can be specified only when **format** is set to **Hudi**. This parameter is supported only by clusters of version 8.3.0 or later.

      Value range: name of an available HDFS server.

      .. note::

         MRS provides the column-level encryption capability for Hudi data. RangerKMS is used to manage users and key IDs and control encryption and decryption permissions. You can specify the **auth_server** parameter when creating a foreign table to bind user **RangerKMS** to the table. In this way, you can be authorized and obtain keys to read encrypted data.

   -  kms_url

      Specifies the IP address and port number of the KMS service. This parameter can be specified only when **format** is set to **hudi** and **auth_server** is specified. This parameter is supported only by clusters of version 8.3.0 or later.

      Value range: a valid string in **ip**:**port** format.

      .. note::

         Generally, if **auth_server** is specified, the IP address and port number of the KMS service can be automatically parsed. If this parameter is specified, the address is forcibly used to access the KMS service.

   -  cow_improve

      Optimization parameter for the **COPY_ON_WRITE** table. When this parameter is enabled, DNs replace the dws-bigdata component to directly read data from OBS. This parameter can be specified only when **format** is set to **Hudi** and cannot be specified together with **auth_server**. This parameter is supported only by clusters of version 8.3.0 or later.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      .. note::

         This parameter is invalid for MERGE_ON_READ tables.

   -  julian_adjust

      Specifies whether to correct the Julian day in the data. This parameter is optional and is supported only by cluster versions 8.3.0 and later.

      The value can be **true** or **false**. Default value: **true**

      .. note::

         Spark 2.X uses the Julian day, and Spark 3.0 and later versions use the Proleptic Gregorian calendar. As a result, some dates before parquet 1582 are different. Correct the dates based on the site requirements.

   .. table:: **Table 2** OBS foreign table options supported by Text, CSV, JSON, ORC, CarbonData, and Parquet formats

      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | Parameter            | OBS       |           |           |           |            |            |            |           |            |            |           |
      +======================+===========+===========+===========+===========+============+============+============+===========+============+============+===========+
      | ``-``                | TEXT      | CSV       | JSON      | ORC       |            |            | CARBONDATA | PARQUET   |            |            | HUDI      |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      |                      | READ ONLY | READ ONLY | READ ONLY | READ ONLY | WRITE ONLY | READ WRITE | READ ONLY  | READ ONLY | WRITE ONLY | READ WRITE | READ ONLY |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | location             | Y         | Y         | Y         | Y         | x          | x          | Y          | Y         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | format               | Y         | Y         | Y         | Y         | Y          | Y          | Y          | Y         | Y          | Y          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | header               | x         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | delimiter            | Y         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | quote                | x         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | escape               | x         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | null                 | Y         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | noescaping           | Y         | x         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | encoding             | Y         | Y         | Y         | Y         | Y          | Y          | Y          | Y         | Y          | Y          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | fill_missing_fields  | Y         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | ignore_extra_data    | Y         | Y         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | date_format          | Y         | Y         | Y         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | time_format          | Y         | Y         | Y         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | timestamp_format     | Y         | Y         | Y         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | smalldatetime_format | Y         | Y         | Y         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | chunksize            | Y         | Y         | Y         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | filenames            | x         | x         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | foldername           | Y         | Y         | Y         | Y         | Y          | Y          | Y          | Y         | Y          | Y          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | dataencoding         | x         | x         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | filesize             | x         | x         | x         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | compression          | Y         | Y         | Y         | x         | Y          | Y          | x          | x         | Y          | Y          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | version              | x         | x         | x         | x         | Y          | Y          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | checkencoding        | Y         | Y         | Y         | Y         | x          | Y          | Y          | Y         | Y          | Y          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | totalrows            | Y         | Y         | Y         | Y         | x          | Y          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | force_mapping        | x         | x         | Y         | x         | x          | x          | x          | x         | x          | x          | x         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | auth_server          | x         | x         | x         | x         | x          | x          | x          | x         | x          | x          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | kms_url              | x         | x         | x         | x         | x          | x          | x          | x         | x          | x          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | cow_improve          | x         | x         | x         | x         | x          | x          | x          | x         | x          | x          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+
      | julian_adjust        | x         | x         | x         | x         | x          | x          | x          | Y         | x          | x          | Y         |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+------------+-----------+------------+------------+-----------+

   .. table:: **Table 3** HDFS foreign table options supported by Text, CSV, JSON, ORC, and Parquet formats

      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | Parameter            | HDFS      |           |           |           |            |            |           |            |            |
      +======================+===========+===========+===========+===========+============+============+===========+============+============+
      | ``-``                | TEXT      | CSV       | JSON      | ORC       |            |            | PARQUET   |            |            |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      |                      | READ ONLY | READ ONLY | READ ONLY | READ ONLY | WRITE ONLY | READ WRITE | READ ONLY | WRITE ONLY | READ WRITE |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | location             | x         | x         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | format               | Y         | Y         | Y         | Y         | Y          | Y          | Y         | Y          | Y          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | header               | x         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | delimiter            | Y         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | quote                | x         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | escape               | x         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | null                 | Y         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | noescaping           | Y         | x         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | encoding             | Y         | Y         | Y         | Y         | Y          | Y          | Y         | Y          | Y          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | fill_missing_fields  | Y         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | ignore_extra_data    | Y         | Y         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | date_format          | Y         | Y         | Y         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | time_format          | Y         | Y         | Y         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | timestamp_format     | Y         | Y         | Y         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | smalldatetime_format | Y         | Y         | Y         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | chunksize            | Y         | Y         | Y         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | filenames            | Y         | Y         | Y         | Y         | x          | x          | Y         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | foldername           | Y         | Y         | Y         | Y         | Y          | Y          | Y         | Y          | Y          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | dataencoding         | x         | x         | x         | x         | Y          | Y          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | filesize             | x         | x         | x         | x         | Y          | Y          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | compression          | Y         | Y         | Y         | x         | Y          | Y          | x         | Y          | Y          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | version              | x         | x         | x         | x         | Y          | Y          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | checkencoding        | Y         | Y         | Y         | Y         | Y          | Y          | Y         | Y          | Y          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | totalrows            | x         | x         | x         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | force_mapping        | x         | x         | Y         | x         | x          | x          | x         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+
      | julian_adjust        | x         | x         | x         | x         | x          | x          | Y         | x          | x          |
      +----------------------+-----------+-----------+-----------+-----------+------------+------------+-----------+------------+------------+

   .. note::

      **chunksize** indicates the cache size of each OBS reading thread on a DN. The format can be text, CSV, or JSON. The default size is 4 MB.

-  WRITE ONLY \| READ ONLY \| READ WRITE

   **WRITE ONLY** creates a write-only HDFS/OBS foreign table.

   **READ ONLY** creates a read-only HDFS/OBS foreign table.

   **READ WRITE** creates a read/write HDFS/OBS foreign table.

   If the foreign table type is not specified, a read-only foreign table is created by default.

-  **DISTRIBUTE BY ROUNDROBIN**

   Specifies **ROUNDROBIN** as the distribution mode for the HDFS/OBS foreign table.

-  **DISTRIBUTE BY REPLICATION**

   Specifies **REPLICATION** as the distribution mode for the HDFS/OBS foreign table.

-  **PARTITION BY ( column_name ) AUTOMAPPED**

   **column_name** specifies the partition column. **AUTOMAPPED** means the partition column specified by the HDFS partitioned foreign table is automatically mapped with the partition directory information in HDFS. The prerequisite is that the sequences of partition columns specified in the HDFS foreign table and in the directory are the same. This function is applicable only to read-only foreign tables.

   .. note::

      -  A write-only partitioned foreign table can have up to four levels of partitions, meaning that you can specify a maximum of four columns as partition columns. At least one column must be reserved as a non-partition column.
      -  HDFS read-only foreign tables support partition tables in text, CSV, CarbonData, ORC, and Parquet formats.
      -  HDFS write-only foreign tables support ORC and Parquet partitioned tables.
      -  HDFS read and write foreign tables support ORC and Parquet partitioned tables.
      -  OBS read-only, write-only, and read-write foreign tables support ORC and Parquet partitioned tables.
      -  Columns of the floating point or Boolean type cannot be used as partition columns.
      -  The maximum length of a partition field can be specified by the GUC parameter **dfs_partition_directory_length**.
      -  A partition directory name is in the format *Partition column name*\ **=**\ *Partition column value*. Special characters in the name will be escaped. To ensure that the total length of the name after escaping does not exceed **dfs_partition_directory_length**, it is advisable to keep the name length before escaping less than or equal to (**dfs_partition_directory_length** + 1)/3.
      -  Do not use a column containing too many Chinese characters as a partition column. You may encounter errors when calculating the final partition directory name's length due to the different space requirements of Chinese and English characters. This is particularly true when the partition directory name exceeds the **dfs_partition_directory_length** length limit.

-  **CONSTRAINT constraint_name**

   Specifies the name of informational constraint of the foreign table.

   Value range: a string. It must comply with the naming convention.

-  **PRIMARY KEY**

   The primary key constraint specifies that one or more columns of a table must contain unique (non-duplicate) and non-null values. Only one primary key can be specified for a table.

-  **UNIQUE**

   Specifies that a group of one or more columns of a table must contain unique values. For the purpose of a unique constraint, **NULL** is not considered equal.

-  **NOT ENFORCED**

   Specifies the constraint to be an informational constraint. This constraint is guaranteed by the user instead of the database.

-  **ENFORCED**

   The default value is **ENFORCED**. **ENFORCED** is a reserved parameter and is currently not supported.

-  **PRIMARY KEY (column_name)**

   Specifies the informational constraint on **column_name**.

   Value range: a string. It must comply with the naming convention, and the value of **column_name** must exist.

-  **ENABLE QUERY OPTIMIZATION**

   Optimizes an execution plan using an informational constraint.

-  **DISABLE QUERY OPTIMIZATION**

   Disables the optimization of an execution plan using an informational constraint.

.. _en-us_topic_0000001811634745__s0b7a85d0acff48e79ada2f91d1e79a0f:

Informational Constraints
-------------------------

In GaussDB(DWS), the use of data constraints depend on users. If users can make data sources strictly comply with certain constraints, the query on data with such constraints can be accelerated. Foreign tables do not support Index. Informational constraint is used for optimizing query plans.

The constraints of creating informational constraints for an HDFS foreign table are as follows:

-  You can create an informational constraint only if the values in a NOT NULL column in your table are unique. Otherwise, the query result will be different from expected.
-  Currently, the informational constraint of GaussDB(DWS) supports only PRIMARY KEY and UNIQUE constraints.
-  The informational constraints of GaussDB(DWS) support the NOT ENFORCED attribute.
-  UNIQUE informational constraints can be created for multiple columns in a table, but only one PRIMARY KEY constraint can be created in a table.
-  Multiple informational constraints can be established in a column of a table (because the function that establishing a column or multiple constraints in a column is the same.) Therefore, you are not advised to set up multiple informational constraints in a column, and only one Primary Key type can be set up.
-  Table-level COMMENT is not supported.
-  Multi-column combination constraints are not supported.
-  Different CNs in the same cluster cannot concurrently export data to the same write-only ORC foreign table.
-  The catalog of a write-only foreign table in ORC format can only be used as the export catalog of a single foreign table of GaussDB(DWS). It cannot be used for multiple foreign tables, and other components cannot write other files to this catalog.

.. _en-us_topic_0000001811634745__s07885a43a93744359b82400d0a8873b3:

Example 1
---------

In HDFS, import the TPC-H benchmark test tables **part** and **region** using Hive. The path of the **part** table is **/user/hive/warehouse/partition.db/part_4**, and that of the **region** table is **/user/hive/warehouse/mppdb.db/region_orc11_64stripe/**.

#. .. _en-us_topic_0000001811634745__li390413437714:

   Establish HDFS_Server, with HDFS_FDW or DFS_FDW as the foreign data wrapper.

   ::

      CREATE SERVER hdfs_server FOREIGN DATA WRAPPER HDFS_FDW OPTIONS (address '10.10.0.100:25000,10.10.0.101:25000',hdfscfgpath '/opt/hadoop_client/HDFS/hadoop/etc/hadoop',type'HDFS');

   .. note::

      -  The IP addresses and port numbers of HDFS NameNodes are specified in **OPTIONS**. For details about the port number, search for **dfs.namenode.rpc.port** in the MRS-HDFS service configuration. In this example the port number is 25000.
      -  **10.10.0.100:25000,10.10.0.101:25000** indicates the IP addresses and port numbers of the primary and standby HDFS NameNodes. It is the recommended format. Two groups of parameters are separated by commas (,).

#. Create an HDFS foreign table. The HDFS server associated with the table is **hdfs_server**, the corresponding file format of the **ft_region** table on the HDFS server is **'orc'**, and the file directory in the HDFS file system is **'/user/hive/warehouse/mppdb. db/region_orc11_64stripe/'**.

   -  Create an HDFS foreign table without partition keys.

      ::

         CREATE FOREIGN TABLE ft_region
         (
             R_REGIONKEY INT4,
             R_NAME TEXT,
             R_COMMENT TEXT
         )
         SERVER
             hdfs_server
         OPTIONS
         (
             FORMAT 'orc',
             encoding 'utf8',
             FOLDERNAME '/user/hive/warehouse/mppdb.db/region_orc11_64stripe/'
         )
         DISTRIBUTE BY
              roundrobin;

   -  Create an HDFS foreign table with partition keys.

      ::

         CREATE FOREIGN TABLE ft_part
         (
              p_partkey int,
              p_name text,
              p_mfgr text,
              p_brand text,
              p_type text,
              p_size int,
              p_container text,
              p_retailprice float8,
              p_comment text
         )
         SERVER
              hdfs_server
         OPTIONS
         (
              FORMAT 'orc',
              encoding 'utf8',
              FOLDERNAME '/user/hive/warehouse/partition.db/part_4'
         )
         DISTRIBUTE BY
              roundrobin
         PARTITION BY
              (p_mfgr) AUTOMAPPED;

      .. note::

         GaussDB(DWS) allows you to specify files using the keyword **filenames** or **foldername**. The latter is recommended. The key word **distribute** specifies the storage distribution mode of the **ft_region** table.

#. View the created server and foreign table.

   ::

      SELECT * FROM pg_foreign_table WHERE ftrelid='ft_region'::regclass;
       ftrelid | ftserver | ftwriteonly |                                  ftoptions
      ---------+----------+-------------+------------------------------------------------------------------------------
         16510 |    16509 | f           | {format=orc,foldername=/user/hive/warehouse/mppdb.db/region_orc11_64stripe/}
      (1 row)

      select * from pg_foreign_table where ftrelid='ft_part'::regclass;
       ftrelid | ftserver | ftwriteonly |                            ftoptions
      ---------+----------+-------------+------------------------------------------------------------------
         16513 |    16509 | f           | {format=orc,foldername=/user/hive/warehouse/partition.db/part_4}
      (1 row)

Example 2
---------

Export data from the TPC-H benchmark test table **region** to the **/user/hive/warehouse/mppdb.db/regin_orc/** directory of the HDFS file system through the HDFS write-only foreign table.

#. Create an HDFS foreign table. The corresponding foreign data wrapper is **HDFS_FDW** or **DFS_FDW**, which is the same as that in :ref:`Example 1 <en-us_topic_0000001811634745__li390413437714>`.

#. Create a write-only HDFS foreign table.

   ::

      CREATE FOREIGN TABLE ft_wo_region
      (
          R_REGIONKEY INT4,
          R_NAME TEXT,
          R_COMMENT TEXT
      )
      SERVER
          hdfs_server
      OPTIONS
      (
          FORMAT 'orc',
          encoding 'utf8',
          FOLDERNAME '/user/hive/warehouse/mppdb.db/regin_orc/'
      )
      WRITE ONLY;

#. Writes data to the HDFS file system through a write-only foreign table.

   ::

      INSERT INTO ft_wo_region SELECT * FROM region;

Example 3 (Only for the Storage-Compute Decoupling 3.0 Version)
---------------------------------------------------------------

Export data from the TPC-H benchmark test table region table to the **/user/hive/warehouse/mppdb.db/region_orc/** directory of the HDFS file system through the write-only, multi-level partitioned HDFS foreign table.

#. Create an HDFS foreign table. The corresponding foreign data wrapper is **HDFS_FDW** or **DFS_FDW**, which is the same as that in :ref:`Example 1 <en-us_topic_0000001811634745__s07885a43a93744359b82400d0a8873b3>`.

#. Create a write-only foreign table for HDFS multi-level partitions.

   ::

      CREATE FOREIGN TABLE ft_wo_region_partition
      (
          R_REGIONKEY INT4,
          R_NAME TEXT,
          R_COMMENT TEXT
      )
      SERVER
          hdfs_server
      OPTIONS
      (
          FORMAT 'orc',
          encoding 'utf8',
          FOLDERNAME '/user/hive/warehouse/mppdb.db/region_orc/'
      )
      WRITE ONLY
      PARTITION BY (R_REGIONKEY, R_NAME);

#. Writes data to the HDFS file system through a write-only foreign table.

   ::

      INSERT INTO ft_wo_region_partition SELECT * FROM region;

Example 4 (Only for the Storage-Compute Decoupling 3.0 Version)
---------------------------------------------------------------

Read data from the OBS file system through the foreign table **partsupp**.

#. Create an OBS server.

   .. important::

      -  Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.
      -  For details about the syntax for creating a server, see :ref:`CREATE SERVER <dws_06_0175>`.

   ::

      CREATE SERVER obs_srv FOREIGN DATA WRAPPER dfs_fdw OPTIONS (
          address 'obs.example.com',
          type 'obs',
          access_key 'xxx',
          secret_access_key 'xxx'
      );

#. Create an OBS foreign table **partsupp** and use the cold cache in the disk cache.

   ::

      CREATE FOREIGN TABLE PARTSUPP
      (
         PS_PARTKEY     BIGINT NOT NULL,
         PS_SUPPKEY     BIGINT NOT NULL,
         PS_AVAILQTY    BIGINT NOT NULL,
         PS_SUPPLYCOST  DECIMAL(15,2)  NOT NULL,
         PS_COMMENT     VARCHAR(199) NOT NULL
      ) SERVER obs_srv options (
          encoding     'utf-8',
          format       'parquet',
          foldername   'xxx',
          cache_policy 'NONE'
      ) READ ONLY DISTRIBUTE BY ROUNDROBIN;

#. Read data from OBS through the foreign table **partsupp**.

   ::

      SELECT * FROM PARTSUPP;

Example 5
---------

Perform operations on an HDFS foreign table that includes informational constraints.

-  Create an HDFS foreign table with informational constraints.

   ::

      CREATE FOREIGN TABLE ft_region  (
       R_REGIONKEY  int,
       R_NAME TEXT,
       R_COMMENT TEXT
        , primary key (R_REGIONKEY) not enforced)
      SERVER hdfs_server
      OPTIONS(format 'orc',
          encoding 'utf8',
       foldername '/user/hive/warehouse/mppdb.db/region_orc11_64stripe')
      DISTRIBUTE BY roundrobin;

-  Check whether the region table has an informational constraint index:

   ::

      SELECT relname,relhasindex FROM pg_class WHERE oid='ft_region'::regclass;
              relname         | relhasindex
      ------------------------+-------------
              ft_region       | f
      (1 row)

      SELECT conname, contype, consoft, conopt, conindid, conkey FROM pg_constraint WHERE conname ='ft_region_pkey';
         conname      | contype | consoft | conopt | conindid | conkey
      ----------------+---------+---------+--------+----------+--------
       ft_region_pkey | p       | t       | t      |        0 | {1}
      (1 row)

-  Delete the informational constraint:

   ::

      ALTER FOREIGN TABLE ft_region DROP CONSTRAINT ft_region_pkey RESTRICT;

      SELECT conname, contype, consoft, conindid, conkey FROM pg_constraint WHERE conname ='ft_region_pkey';
       conname | contype | consoft | conindid | conkey
      ---------+---------+---------+----------+--------
      (0 rows)

-  Add a unique informational constraint for the foreign table:

   ::

      ALTER FOREIGN TABLE ft_region ADD CONSTRAINT constr_unique UNIQUE(R_REGIONKEY) NOT ENFORCED;

   Delete the unique informational constraint:

   ::

      ALTER FOREIGN TABLE ft_region DROP CONSTRAINT constr_unique RESTRICT;

      SELECT conname, contype, consoft, conindid, conkey FROM pg_constraint WHERE conname ='constr_unique';
       conname | contype | consoft | conindid | conkey
      ---------+---------+---------+----------+--------
      (0 rows)

-  Add a unique informational constraint for the foreign table:

   ::

      ALTER FOREIGN TABLE ft_region ADD CONSTRAINT constr_unique UNIQUE(R_REGIONKEY) NOT ENFORCED disable query optimization;

      SELECT relname,relhasindex FROM pg_class WHERE oid='ft_region'::regclass;
              relname         | relhasindex
      ------------------------+-------------
              ft_region       | f
      (1 row)

   Delete the unique informational constraint:

   ::

      ALTER FOREIGN TABLE ft_region DROP CONSTRAINT constr_unique CASCADE;

Example 6
---------

Read json data stored in OBS using a foreign table.

#. The following JSON files are on OBS. The JSON objects contain nesting and arrays. Some objects have lost columns, and some object names are duplicate.

   .. code-block::

      {"A" : "simple1", "B" : {"C" : "nesting1"}, "D" : ["array", 2, {"E" : "complicated"}]}
      {"A" : "simple2", "D" : ["array", 2, {"E" : "complicated"}]}
      {"A" : "simple3", "B" : {"C" : "nesting3"}, "D" : ["array", 2, {"E" : "complicated3"}]}
      {"B" : {"C" : "nesting4"},"A" : "simple4",  "D" : ["array", 2, {"E" : "complicated4"}]}
      {"A" : "simple5", "B" : {"C" : "nesting5"}, "D" : ["array", 2, {"E" : "complicated5"}]}

#. Create **obs_server**, with **DFS_FDW** as the foreign data wrapper.

   ::

      CREATE SERVER obs_server FOREIGN DATA WRAPPER DFS_FDW OPTIONS (
        ADDRESS 'obs.example.com',
        ACCESS_KEY 'xxxxxxxxx',
        SECRET_ACCESS_KEY 'yyyyyyyyyyyyy',
        TYPE 'OBS'
      );

   .. note::

      -  **ADDRESS** is the endpoint of OBS. Replace it with the actual endpoint. You can find the domain name by searching for the value of **regionCode** in the **region_map** file.
      -  **ACCESS_KEY** and **SECRET_ACCESS_KEY** are access keys for the cloud account system. Replace the values as needed.
      -  **TYPE** indicates the server type. Retain the value **OBS**.

#. Create the OBS foreign table **json_f** and define the column names. For example, **d#2_e** indicates that the column is object **e** nested in the **2**\ nd element of array **d**. The OBS server associated with the table is **obs_server**. **foldername** indicates the data source directory of the foreign table, that is, the OBS directory.

   .. important::

      // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

   ::

      CREATE FOREIGN TABLE json_f (
        a VARCHAR(10),
        b_c TEXT,
        d#1 INTEGER,
        d#2_e VARCHAR(30)
      )SERVER obs_server OPTIONS (
          foldername '/xxx/xxx/',
          format 'json',
          encoding 'utf8',
          force_mapping 'true'
      )distribute by roundrobin;

#. Query the foreign table **json_f**. The fault tolerance parameter force_mapping is enabled by default. If a column is missing in a JSON object, NULL is filled in. If a JSON object name is duplicate, the last name prevails.

   ::

      SELECT * FROM json_f;
          a    |   b_c    | d#1 |    d#2_e
      ---------+----------+-----+--------------
       simple1 | nesting1 |   2 | complicated1
       simple2 |          |   2 | complicated2
       simple3 | nesting3 |   2 | complicated3
       simple4 | nesting4 |   2 | complicated4
       repeat  | nesting5 |   2 | complicated5
      (5 rows)

Example 7
---------

Read a DLI multi-version foreign table using a foreign table. Only DLI 8.1.1 and later support the multi-version foreign table example.

#. Create **dli_server**, with **DFS_FDW** as the foreign data wrapper.

   ::

      CREATE SERVER dli_server FOREIGN DATA WRAPPER DFS_FDW OPTIONS (
        ADDRESS 'obs.example.com',
        ACCESS_KEY 'xxxxxxxxx',
        SECRET_ACCESS_KEY 'yyyyyyyyyyyyy',
        TYPE 'DLI',
        DLI_ADDRESS 'dli.example.com',
        DLI_ACCESS_KEY 'xxxxxxxxx',
        DLI_SECRET_ACCESS_KEY 'yyyyyyyyyyyyy'
      );

   .. note::

      -  **ADDRESS** is the endpoint of OBS. **DLI_ADDRESS** is the endpoint of DLI. Replace it with the actual endpoint.
      -  **ACCESS_KEY** and **SECRET_ACCESS_KEY** are access keys for the cloud account system to access OBS. Use the actual value.
      -  **DLI_ACCESS_KEY** and **DLI_SECRET_ACCESS_KEY** are access keys for the cloud account system to access DLI. Use the actual value.
      -  **TYPE** indicates the server type. Retain the value **DLI**.

#. Create the OBS foreign table **customer_address** for accessing DLI. The table does not contain partition columns, and the DLI server associated with the table is **dli_server**. Where, the **project_id** is *xxxxxxxxxxxxxxx*, the **database_name** on DLI is **database123**, and the **table_name** of the table to be accessed is **table456**. Replace them based on the actual requirements.

   .. important::

      // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

   ::

      CREATE FOREIGN TABLE customer_address
      (
          ca_address_sk             integer               not null,
          ca_address_id             char(16)              not null,
          ca_street_number          char(10)                      ,
          ca_street_name            varchar(60)                   ,
          ca_street_type            char(15)                      ,
          ca_suite_number           char(10)                      ,
          ca_city                   varchar(60)                   ,
          ca_county                 varchar(30)                   ,
          ca_state                  char(2)                       ,
          ca_zip                    char(10)                      ,
          ca_country                varchar(20)                   ,
          ca_gmt_offset             decimal(36,33)                  ,
          ca_location_type          char(20)
      )
      SERVER dli_server OPTIONS (
          FORMAT 'ORC',
          ENCODING 'utf8',
          DLI_PROJECT_ID 'xxxxxxxxxxxxxxx',
          DLI_DATABASE_NAME 'database123',
          DLI_TABLE_NAME 'table456'
      )
      DISTRIBUTE BY roundrobin;

#. Query data in a DLI multi-version table using a foreign table.

   ::

      SELECT COUNT(*) FROM customer_address;
       count
      -------
          20
      (1 row)

Helpful Links
-------------

:ref:`ALTER FOREIGN TABLE (for HDFS or OBS) <dws_06_0124>`, :ref:`DROP FOREIGN TABLE <dws_06_0192>`
