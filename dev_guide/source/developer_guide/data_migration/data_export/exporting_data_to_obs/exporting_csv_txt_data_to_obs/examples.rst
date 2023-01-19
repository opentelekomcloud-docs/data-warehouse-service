:original_name: dws_04_0255.html

.. _dws_04_0255:

Examples
========

Exporting a Table
-----------------

Create two foreign tables and use them to export tables from a database to two buckets in OBS.

#. Log in to the OBS data server through the management console. On the OBS server, create the buckets **/input-data1** and **/input-data2** for storing data files, and create data directories **/input-data1/data** and **/input-data2/data**, respectively, in the two buckets.

#. On the GaussDB(DWS) database, create the foreign tables **tpcds.customer_address_ext1** and **tpcds.customer_address_ext2** for the OBS data server to receive data exported from the database.

   OBS and the database are in the same region. The example GaussDB(DWS) table to be exported is **tpcds.customer_address**.

   Export information is set as follows:

   -  The source data file directories are **/input-data1/data/** and **/input-data2/data/**, so **location** of **tpcds.customer_address_ext1** and **tpcds.customer_address_ext2** is set to **obs://input-data1/data/** and **obs://input-data2/data/**, respectively.

   Information about data formats is set based on the detailed data format parameters specified during data export from a database. The parameter settings are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  Configure **encrypt**. Its default value is **off**.
   -  **access_key** is set to the AK you have obtained. (mandatory)
   -  **secret_access_key** is set to the SK you have obtained. (mandatory)

      .. note::

         **access_key** and **secret_access_key** have been obtained during user creation. Replace the italic part with the actual keys.

   Based on the preceding settings, the foreign table is created using the following statements:

   ::

      CREATE FOREIGN TABLE tpcds.customer_address_ext1
      (
      ca_address_sk             integer                       ,
      ca_address_id             char(16)                      ,
      ca_street_number          char(10)                      ,
      ca_street_name            varchar(60)                   ,
      ca_street_type            char(15)                      ,
      ca_suite_number           char(10)                      ,
      ca_city                   varchar(60)                   ,
      ca_county                 varchar(30)                   ,
      ca_state                  char(2)                       ,
      ca_zip                    char(10)                      ,
      ca_country                varchar(20)                   ,
      ca_gmt_offset             decimal(5,2)                  ,
      ca_location_type          char(20)
      )
      SERVER gsmpp_server
      OPTIONS(LOCATION 'obs://input-data1/data/',
      FORMAT 'CSV',
      ENCODING 'utf8',
      DELIMITER E'\x08',
      ENCRYPT 'off',
      ACCESS_KEY 'access_key_value_to_be_replaced',
      SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced'
      )Write Only;

   ::

      CREATE FOREIGN TABLE tpcds.customer_address_ext2
      (
      ca_address_sk             integer                       ,
      ca_address_id             char(16)                      ,
      ca_street_number          char(10)                      ,
      ca_street_name            varchar(60)                   ,
      ca_street_type            char(15)                      ,
      ca_suite_number           char(10)                      ,
      ca_city                   varchar(60)                   ,
      ca_county                 varchar(30)                   ,
      ca_state                  char(2)                       ,
      ca_zip                    char(10)                      ,
      ca_country                varchar(20)                   ,
      ca_gmt_offset             decimal(5,2)                  ,
      ca_location_type          char(20)
      )
      SERVER gsmpp_server
      OPTIONS(LOCATION 'obs://input-data2/data/',
      FORMAT 'CSV',
      ENCODING 'utf8',
      DELIMITER E'\x08',
      ENCRYPT 'off',
      ACCESS_KEY 'access_key_value_to_be_replaced',
      SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced'
      )Write Only;

#. In GaussDB(DWS), export the data table **tpcds.customer_address** to the foreign tables **tpcds.customer_address_ext1** and **tpcds.customer_address_ext2** concurrently.

   ::

      INSERT INTO tpcds.customer_address_ext1 SELECT * FROM tpcds.customer_address;

   ::

      INSERT INTO tpcds.customer_address_ext2 SELECT * FROM tpcds.customer_address;

   .. note::

      The design of OBS foreign tables does not allow exporting files to a non-empty path. However, in concurrent export scenarios, multiple files are exported to the same path, causing an error.

      Assume that a user concurrently exports data from the same table to the same OBS foreign table, and that one SQL statement is executed to export data when another SQL statement is being executed and has not generated any file on the OBS server. In this case, certain data is overwritten although both SQL statements are successfully executed. Therefore, you are advised not to concurrently export data to the same OBS foreign table.

Concurrently Exporting Tables
-----------------------------

Use the two foreign tables to export tables from the database to two buckets in OBS.

#. Log in to the OBS data server through the management console. On the OBS server, create the buckets **/input-data1** and **/input-data2** for storing data files, and create data directories **/input-data1/data** and **/input-data2/data**, respectively, in the two buckets.

#. In GaussDB(DWS), create foreign tables **tpcds.customer_address_ext1** and **tpcds.customer_address_ext2** for the OBS server to receive exported data.

   OBS and the database are in the same region. Tables to be exported are **tpcds.customer_address** and **tpcds.customer_demographics**.

   Export information is set as follows:

   -  The source data file directories are **/input-data1/data/** and **/input-data2/data/**, so **location** of **tpcds.customer_address_ext1** and **tpcds.customer_address_ext2** is set to **obs://input-data1/data/** and **obs://input-data2/data/**, respectively.

   Information about data formats is set based on the detailed data format parameters specified during data export from GaussDB(DWS). The parameter settings are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  Configure **encrypt**. Its default value is **off**.
   -  **access_key** is set to the AK you have obtained. (mandatory)
   -  **secret_access_key** is set to the SK you have obtained. (mandatory)

      .. note::

         **access_key** and **secret_access_key** have been obtained during user creation. Replace the italic part with the actual keys.

   Based on the preceding settings, the foreign table is created using the following statements:

   ::

      CREATE FOREIGN TABLE tpcds.customer_address_ext1
      (
      ca_address_sk             integer               ,
      ca_address_id             char(16)              ,
      ca_street_number          char(10)                      ,
      ca_street_name            varchar(60)                   ,
      ca_street_type            char(15)                      ,
      ca_suite_number           char(10)                      ,
      ca_city                   varchar(60)                   ,
      ca_county                 varchar(30)                   ,
      ca_state                  char(2)                       ,
      ca_zip                    char(10)                      ,
      ca_country                varchar(20)                   ,
      ca_gmt_offset             decimal(5,2)                  ,
      ca_location_type          char(20)
      )
      SERVER gsmpp_server
      OPTIONS(LOCATION 'obs://input-data1/data/',
      FORMAT 'CSV',
      ENCODING 'utf8',
      DELIMITER E'\x08',
      ENCRYPT 'off',
      ACCESS_KEY 'access_key_value_to_be_replaced',
      SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced'
      )Write Only;

   ::

      CREATE FOREIGN TABLE tpcds.customer_address_ext2
      (
      ca_address_sk             integer               ,
      ca_address_id             char(16)              ,
      ca_address_name           varchar(20)           ,
      ca_address_code           integer               ,
      ca_street_number          char(10)                      ,
      ca_street_name            varchar(60)                   ,
      ca_street_type            char(15)                      ,
      ca_suite_number           char(10)                      ,
      ca_city                   varchar(60)                   ,
      ca_county                 varchar(30)                   ,
      ca_state                  char(2)                       ,
      ca_zip                    char(10)                      ,
      ca_country                varchar(20)                   ,
      ca_gmt_offset             decimal(5,2)
      )
      SERVER gsmpp_server
      OPTIONS(LOCATION 'obs://input_data2/data/',
      FORMAT 'CSV',
      ENCODING 'utf8',
      DELIMITER E'\x08',
      QUOTE E'\x1b',
      ENCRYPT 'off',
      ACCESS_KEY 'access_key_value_to_be_replaced',
      SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced'
      )Write Only;

#. In GaussDB(DWS), export the data tables **tpcds.customer_address** and **tpcds.warehouse** in parallel to the foreign tables **tpcds.customer_address_ext1** and **tpcds.customer_address_ext2**, respectively.

   ::

      INSERT INTO tpcds.customer_address_ext1 SELECT * FROM tpcds.customer_address;

   ::

      INSERT INTO tpcds.customer_address_ext2 SELECT * FROM tpcds.warehouse;
