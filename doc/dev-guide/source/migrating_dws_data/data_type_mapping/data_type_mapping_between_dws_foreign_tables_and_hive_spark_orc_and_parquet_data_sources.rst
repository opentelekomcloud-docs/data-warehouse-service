:original_name: dws_04_1453.html

.. _dws_04_1453:

Data Type Mapping Between DWS Foreign Tables and Hive/Spark, ORC, and Parquet Data Sources
==========================================================================================

When data is read from Hive/Spark or third-party components' ORC/Parquet to DWS foreign tables, data incompatibility may occur due to different data types on different platforms. This section describes the data type mapping between DWS foreign tables and these data sources. By configuring correct data types for foreign table columns, you can ensure that data can be accurately identified, processed, and queried in DWS, and that no data is lost.

For details about the foreign table syntax, see "CREATE FOREIGN TABLE (SQL on OBS or Hadoop)" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

Data Type Mapping Between DWS Foreign Tables and Hive/Spark Data Sources
------------------------------------------------------------------------

:ref:`Table 1 <en-us_topic_0000002464333144__en-us_topic_0000002490497941_table15192148192820>` is based on the types supported by DWS foreign tables. Some types supported by Hive/Spark, such as INTERVAL and UNION, are not supported by DWS foreign tables.

.. _en-us_topic_0000002464333144__en-us_topic_0000002490497941_table15192148192820:

.. table:: **Table 1** Data type mapping between DWS foreign tables and Hive/Spark data sources

   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Type                            | DWS **Foreign Table Type**   | Hive Type       | Spark Type               |
   +=================================+==============================+=================+==========================+
   | 1-byte integer                  | -  TINYINT (not recommended) | TINYINT         | BYTE, TINYINT            |
   |                                 | -  SMALLINT (recommended)    |                 |                          |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | 2-byte integer                  | SMALLINT                     | SMALLINT        | SHORT, SMALLINT          |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | 4-byte integer                  | INTEGER                      | INT, INTEGER    | INT, INTEGER             |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | 8-byte integer                  | BIGINT                       | BIGINT          | LONG, BIGINT             |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Single-precision floating point | FLOAT, REAL                  | FLOAT           | FLOAT, REAL              |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Double-precision floating point | FLOAT8, DOUBLE               | DOUBLE          | DOUBLE                   |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Scientific data                 | DECIMAL[p (,s)]              | DECIMAL         | DECIMAL                  |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Date type                       | DATE                         | DATE            | DATE                     |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Time type                       | TIMESTAMP WITHOUT TIME ZONE  | TIMESTAMP       | TIMESTAMP_NTZ            |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   |                                 | TIMESTAMP WITH TIME ZONE     |                 | TIMESTAMP, TIMESTAMP_LTZ |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Boolean                         | BOOLEAN                      | BOOLEAN         | BOOLEAN                  |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Char                            | CHAR(n)                      | CHAR            | CHAR(length)             |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Varchar                         | VARCHAR(n)                   | VARCHAR         | VARCHAR(length)          |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   |                                 | NVARCHAR2                    |                 |                          |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | text                            | TEXT(CLOB)                   | STRING          | STRING                   |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Binary                          | BYTEA                        | BINARY          | BINARY                   |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   | Complex type                    | JSON                         | ARRAY           | ARRAY                    |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   |                                 |                              | MAP             | MAP                      |
   +---------------------------------+------------------------------+-----------------+--------------------------+
   |                                 |                              | STRUCT          | STRUCT                   |
   +---------------------------------+------------------------------+-----------------+--------------------------+

Data Type Mapping Between DWS Foreign Tables and ORC Data Sources
-----------------------------------------------------------------

-  ORC types are internal types defined in the ORC format specifications. Generally, you do not need to pay special attention to these types. They can be used as a reference for creating DWS foreign tables if the Hive or Spark types are not clearly defined.
-  When DWS foreign tables are written to ORC files, data types are mapped one by one. However, in the **(read)** scenario, in addition to the recommended mapping of types, some other mapping scenarios are supported. :ref:`Table 2 <en-us_topic_0000002464333144__en-us_topic_0000002490497941_table109232044122815>` lists these scenarios.

.. _en-us_topic_0000002464333144__en-us_topic_0000002490497941_table109232044122815:

.. table:: **Table 2** Data type mapping between DWS foreign tables and ORC data sources

   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | Type            | ORC Type                       | DWS **Foreign Table Type (Read)** | DWS **Foreign Table Type (Write)** |
   +=================+================================+===================================+====================================+
   | Integer         | boolean (1 bit)                | BOOLEAN                           | BOOLEAN                            |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | tinyint (8 bit)                | -  TINYINT (not recommended)      | TINYINT (not recommended)          |
   |                 |                                | -  SMALLINT (recommended)         |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | smallint (16 bit)              | SMALLINT                          | SMALLINT                           |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | int (32 bit)                   | INTEGER                           | INTEGER                            |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | bigint (64 bit)                | BIGINT                            | BIGINT                             |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | Floating point  | float                          | FLOAT4                            | FLOAT4                             |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | double                         | FLOAT8                            | FLOAT8                             |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | String          | string                         | -  TEXT (recommended)             | -  TEXT                            |
   |                 |                                | -  VARCHAR (n)                    | -  DECIMAL[p (,s)]                 |
   |                 |                                | -  CHAR (n)                       | -  CHAR (n)                        |
   |                 |                                | -  DECIMAL[p (,s)]                | -  VARCHAR (n)                     |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | char                           | -  CHAR (n) (recommended)         | CHAR (n)                           |
   |                 |                                | -  VARCHAR (n)                    |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | varchar                        | VARCHAR(n)                        | VARCHAR (n)                        |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | Binary          | binary                         | BYTEA                             | BYTEA                              |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | Scientific data | decimal                        | DECIMAL[p (,s)]                   | DECIMAL[p (,s)]                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | Time            | timestamp                      | -  TIMESTAMP WITHOUT TIME ZONE    | -  TIMESTAMP WITHOUT TIME ZONE     |
   |                 |                                | -  TIMESTAMP WITH TIME ZONE       | -  TIMESTAMP WITH TIME ZONE        |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | timestamp with local time zone | -  TIMESTAMP WITHOUT TIME ZONE    | Not supported                      |
   |                 |                                | -  TIMESTAMP WITH TIME ZONE       |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | date                           | -  DATE                           | DATE                               |
   |                 |                                | -  TIMESTAMP WITHOUT TIME ZONE    |                                    |
   |                 |                                | -  TIMESTAMP WITH TIME ZONE       |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   | Complex type    | struct                         | -  JSON                           | Not supported                      |
   |                 |                                | -  JSONB                          |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | list                           | -  JSON                           | Not supported                      |
   |                 |                                | -  JSONB                          |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | map                            | -  JSON                           | Not supported                      |
   |                 |                                | -  JSONB                          |                                    |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+
   |                 | union                          | Not supported                     | Not supported                      |
   +-----------------+--------------------------------+-----------------------------------+------------------------------------+

Data type mapping between DWS foreign tables and PARQUET data sources
---------------------------------------------------------------------

PARQUET types are the internal storage types defined in the PARQUET format specifications. PARQUET uses **converted_type** (deprecated) or **logical_type** to further describe type behavior. When DWS data is **written** to PARQUET, only **logical_type** is used. When data is **read** from PARQUET, **converted_type** is compatible.

.. table:: **Table 3** Data type mapping between DWS foreign tables and PARQUET data sources

   +-----------------------+---------------------------------------+------------------------------------+
   | Parquet Type          | DWS **Foreign Table Type (Read)**     | DWS **Foreign Table Type (Write)** |
   +=======================+=======================================+====================================+
   | BOOLEAN               | BOOLEAN                               | BOOLEAN                            |
   +-----------------------+---------------------------------------+------------------------------------+
   | INT32                 | -  TINYINT (not recommended)          | -  TINYINT (not recommended)       |
   |                       | -  SMALLINT                           | -  SMALLINT                        |
   |                       | -  INTEGER                            | -  INTEGER                         |
   |                       | -  DATE                               |                                    |
   |                       | -  TIMESTAMP[(p)]                     |                                    |
   |                       | -  DECIMAL[p (,s)]                    |                                    |
   +-----------------------+---------------------------------------+------------------------------------+
   | INT64                 | -  BIGINT                             | -  BIGINT                          |
   |                       | -  money                              | -  DECIMAL[p (,s)]                 |
   |                       | -  TIMESTAMP WITHOUT TIME ZONE]       | -  money                           |
   |                       | -  TIMESTAMP WITH TIME ZONE           | -  TIMESTAMP WITHOUT TIME ZONE     |
   |                       | -  DECIMAL[p (,s)]                    | -  TIMESTAMP WITH TIME ZONE        |
   +-----------------------+---------------------------------------+------------------------------------+
   | INT96                 | -  TIMESTAMP[(p)] [WITHOUT TIME ZONE] | Not supported                      |
   |                       | -  TIMESTAMP WITH TIME ZONE           |                                    |
   +-----------------------+---------------------------------------+------------------------------------+
   | FLOAT                 | FLOAT4                                | FLOAT4                             |
   +-----------------------+---------------------------------------+------------------------------------+
   | DOUBLE                | FLOAT8                                | FLOAT8                             |
   +-----------------------+---------------------------------------+------------------------------------+
   | BYTE_ARRAY            | -  CHAR (n)                           | -  DECIMAL[p (,s)]                 |
   |                       | -  VARCHAR (n)                        | -  CHAR (n)                        |
   |                       | -  TEXT                               | -  VARCHAR (n)                     |
   |                       | -  NVARCHAR2                          | -  TEXT                            |
   |                       | -  BYTEA                              | -  NVARCHAR2                       |
   |                       |                                       | -  BYTEA                           |
   +-----------------------+---------------------------------------+------------------------------------+
   | FIXED_LEN_BYTE_ARRAY  | DECIMAL[p (,s)]                       | DECIMAL[p (,s)]                    |
   +-----------------------+---------------------------------------+------------------------------------+
