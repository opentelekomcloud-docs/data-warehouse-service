:original_name: dws_04_0194.html

.. _dws_04_0194:

Creating a GDS Foreign Table
============================

The source data information and GDS access information are configured in a foreign table. Then, GaussDB(DWS) can import data from a data server to a database table based on the configuration in the foreign table.

Procedure
---------

#. .. _en-us_topic_0000001764896593__en-us_topic_0000001188323596_li175507108553:

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

#. .. _en-us_topic_0000001764896593__en-us_topic_0000001188323596_la571bf23a4b24288b5dce0d83a176a56:

   Design an error tolerance mechanism for data import.

   GaussDB(DWS) supports the following error tolerance in data import:

   -  **fill_missing_fields**: When the last column in a row of the source data file is empty, this parameter specifies whether to report an error or set this field in the row to **NULL**.

      .. note::

         Valid value: **true**, **on**, **false**, and **off**.

         -  If this parameter is set to **true** or **on** and the last column of a data row in a source data file is lost, the column will be replaced with **null** and no error message will be generated.

         -  If this parameter is set to **false** or **off** and the last column of a data row in a source data file is lost, the following error information will be displayed:

            .. code-block::

               missing data for column "tt"

         Default value: **false** or **off**

   -  **ignore_extra_data**: When the number of columns in the source data file is greater than that specified in the foreign table, this parameter specifies whether to report an error or ignore the extra columns.

      .. note::

         Value range: true/on, false/off.

         -  When this parameter is **true** or **on** and the number of data source files exceeds the number of foreign table columns, excessive columns will be ignored.

         -  If the parameter is set to **false** or **off**, and the number of data source files exceeds the number of foreign table columns, the following error information will be displayed:

            ::

               extra data after last expected column

         Default value: **false** or **off**

   -  **per node reject_limit**: This parameter specifies the number of data format errors allowed on each DN. If the number of errors recorded in the error table on a DN exceeds the specified value, the import will fail and an error message will be reported. You can also set it to **unlimited**.

   -  compatible_illegal_chars

      Enables or disables fault tolerance on invalid characters during their import and export. This syntax takes effect for read-only and write-only foreign tables.

      Only clusters of version 8.1.3.331 or later support export fault tolerance.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If the parameter is **true** or **on**, invalid characters are tolerated and imported to or exported from the database after being converted.
      -  If the parameter is **false** or **off**, and an error occurs when there are invalid characters, the import or export will be interrupted.

      .. note::

         The rule of error tolerance when you import or export invalid characters is as follows:

         -  **\\0** is converted to a space.
         -  Other invalid characters are converted to question marks.
         -  If **compatible_illegal_chars** is set to **true** or **on**, the database will convert and accept the invalid characters. If **NULL**, **DELIMITER**, **QUOTE**, and **ESCAPE** are set to a spaces or question marks. Errors like "illegal chars conversion may confuse COPY escape 0x20" will be displayed to prompt user to modify parameter values that cause confusion, preventing import and export errors.
         -  Enabling error tolerance for foreign table export will result in invalid characters being exported as question marks (?), which can lead to inconsistencies between the exported and original data when imported back into the GaussDB(DWS) database.

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
      LOCATION 'gsfs://192.168.0.90:5000/* | gsfs://192.168.0.91:5000/*',
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
   -  Set **location** based on the GDS access information collected in :ref:`1 <en-us_topic_0000001764896593__en-us_topic_0000001188323596_li175507108553>`. If SSL is used, replace **gsfs** with **gsfss**.
   -  Set **FORMAT**, **DELIMITER**, **ENCODING**, and **HEADER** based on the source data information collected in :ref:`1 <en-us_topic_0000001764896593__en-us_topic_0000001188323596_li175507108553>`.
   -  Set **FILL_MISSING_FIELDS**, **IGNORE_EXTRA_DATA**, **LOG INTO**, and **PER NODE REJECT LIMIT** based on the error tolerance mechanism designed in :ref:`2 <en-us_topic_0000001764896593__en-us_topic_0000001188323596_la571bf23a4b24288b5dce0d83a176a56>`. **LOG INTO** specifies the name of the error table.

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

-  Example 4: Create a GDS foreign table named **foreign_tpcds_reasons**. The data format is FIXED.

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons
      (
        r_reason_sk       integer      position(1,2),
        r_reason_id       char(16)     position(3,16),
        r_reason_desc     char(100)    position(19,100)
      ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/*', FORMAT 'FIXED', ENCODING 'utf8',FIX '119');
