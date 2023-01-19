:original_name: dws_04_0194.html

.. _dws_04_0194:

Creating a GDS Foreign Table
============================

The source data information and GDS access information are configured in a foreign table. Then, GaussDB(DWS) can import data from a data server to a database table based on the configuration in the foreign table.

Procedure
---------

#. .. _en-us_topic_0000001098654836__li175507108553:

   Collect source data information and GDS access information.

   You need to collect the following source data information:

   -  **format**: format of the data to be imported. Only data in CSV, TEXT, or FIXED format can be imported using GDS in parallel.
   -  **header**: whether a source data file has a header. This parameter is set only for files in CSV or FIXED format.
   -  **delimiter**: delimiter in the source data file. For example, it can be a comma (,).
   -  **encoding**: encoding format of the data source file. Assume that the encoding format is UTF-8.
   -  **eol**: line break character in the data file. It can be a default character, such as 0x0D0A or 0X0A, or a customized line break character, such as a string: **!@#**. This parameter can be set only for TEXT import.
   -  For details about more source data information configured in a foreign table, see data format parameters.

   You need to collect the following GDS access information:

   **location**: GDS URL. GDS information in :ref:`Installing, Configuring, and Starting GDS <dws_04_0193>` is used as an example. In non-SSL mode, **location** is set to **gsfs**://*192.168.0.90:5000*/*/input_data/*. In SSL mode, **location** is set to **gsfss**://*192.168.0.90:5000*/*/input_data/*. **192.168.0.90:5000** indicates the IP address and port number of GDS. **input_data** indicates the path of data source files managed by GDS. Replace the values as required.

#. .. _en-us_topic_0000001098654836__la571bf23a4b24288b5dce0d83a176a56:

   Design an error tolerance mechanism for data import.

   GaussDB(DWS) supports the following error tolerance in data import:

   -  **fill_missing_fields**: This parameter specifies whether to report an error when the last column in a row of the source data file is empty, or to fill the column with **NULL**.

   -  **ignore_extra_data**: This parameter specifies whether to report an error when the number of columns in the source data file is greater than that specified in the foreign table, or to ignore the extra columns.

   -  **per node reject_limit**: This parameter specifies the number of data format errors allowed on each DN. If the number of errors recorded in the error table on a DN exceeds the specified value, the import will fail and an error message will be reported. You can also set it to **unlimited**.

   -  **compatible_illegal_chars**: This parameter specifies whether to report an error when an illegal character is encountered, or to convert it and proceed with the import.

      The following describes the rules for converting an illegal character:

      -  **\\0** is converted to a space.
      -  Other illegal characters are converted to question marks.
      -  If **NULL**, **DELIMITER**, **QUOTE**, or **ESCAPE** is also set to a space or question mark, an error message such as "illegal chars conversion may confuse COPY escape 0x20" is displayed, prompting you to modify parameter settings that may cause import errors.

   -  **error_table_name**: This parameter specifies the name of the table that records data format errors. After the parallel import, you can query this table for error details.

   -  **remote log 'name'**: This parameter specifies whether to store data format errors in files on the GDS server. **name** is the prefix of the error data file.

   -  For details about more error tolerance parameters, see error tolerance parameters.

#. After connecting to the database using **gsql** or Data Studio, create a GDS foreign table based on the collected and design information.

   For example:

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      )
       SERVER gsmpp_server
       OPTIONS
      (
      LOCATION 'gsfs://192.168.0.90:5000/input_data | gsfs://192.168.0.91:5000/input_data',
      FORMAT 'CSV' ,
      DELIMITER ',',
      ENCODING 'utf8',
      HEADER 'false',
      FILL_MISSING_FIELDS 'true',
      IGNORE_EXTRA_DATA 'true'
      )
      LOG INTO product_info_err
      PER NODE REJECT LIMIT 'unlimited';

   The following describes information in the preceding command:

   -  The columns specified in the foreign table must be the same as those in the target table.
   -  Retain the value **gsmpp_server** for **SERVER**.
   -  Set **location** based on the GDS access information collected in :ref:`1 <en-us_topic_0000001098654836__li175507108553>`. If SSL is used, replace **gsfs** with **gsfss**.
   -  Set **FORMAT**, **DELIMITER**, **ENCODING**, and **HEADER** based on the source data information collected in :ref:`1 <en-us_topic_0000001098654836__li175507108553>`.
   -  Set **FILL_MISSING_FIELDS**, **IGNORE_EXTRA_DATA**, **LOG INTO**, and **PER NODE REJECT LIMIT** based on the error tolerance mechanism designed in :ref:`2 <en-us_topic_0000001098654836__la571bf23a4b24288b5dce0d83a176a56>`. **LOG INTO** specifies the name of the error table.

   For details about the CREATE FOREIGN TABLE syntax, see CREATE FOREIGN TABLE (for GDS Import and Export).

Example
-------

For more examples, see :ref:`Example of Importing Data Using GDS <dws_04_0198>`.

-  Example 1: Create a GDS foreign table named **foreign_tpcds_reasons**. The data format is CSV.

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      )
       SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/* | gsfs://192.168.0.91:5000/*', FORMAT 'CSV',MODE 'Normal', ENCODING 'utf8', DELIMITER E'\x08', QUOTE E'\x1b', NULL '');

-  Example 2: Create a GDS foreign table named **foreign_tpcds_reasons_SSL**. SSL is used and the data format is CSV.

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons_SSL
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      )
       SERVER gsmpp_server OPTIONS (location 'gsfss://192.168.0.90:5000/* | gsfss://192.168.0.91:5000/*', FORMAT 'CSV',MODE 'Normal', ENCODING 'utf8', DELIMITER E'\x08', QUOTE E'\x1b', NULL '');

-  Example 3: Create a GDS foreign table named **foreign_tpcds_reasons**. The data format is TEXT.

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/* | gsfs://192.168.0.91:5000/*', FORMAT 'TEXT', delimiter E'\x08',  null '',reject_limit '2',EOL '0x0D') WITH err_foreign_tpcds_reasons;
