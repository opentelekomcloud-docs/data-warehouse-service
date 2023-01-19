:original_name: dws_06_0160.html

.. _dws_06_0160:

CREATE FOREIGN TABLE (for OBS Import and Export)
================================================

Function
--------

**CREATE FOREIGN TABLE** creates a foreign table in the current database for parallel data import and export of OBS data. The server used is **gsmpp_server**, which is created by the database by default.

.. note::

   The hybrid data warehouse (standalone) does not support OBS foreign table import and export.

Precautions
-----------

-  Only the data in text and CSV formats is supported, and the OBS connection should be configured. ORC and CarbonData data on OBS is not applicable. For details, see :ref:`CREATE FOREIGN TABLE (SQL on OBS or Hadoop) <dws_06_0161>`.
-  An OBS foreign table can be set to **READ ONLY** or **WRITE ONLY**. The default value is **READ ONLY**. To import data to the cluster, use **READ ONLY** for the foreign table. To export data, use **WRITE ONLY**.
-  The foreign table is owned by the user who runs the command.
-  The distribution mode of an OBS foreign table does not need to be explicitly specified. The default mode is **ROUNDROBIN**.
-  Only constraints in :ref:`Informational Constraint <en-us_topic_0000001145830873__s0b7a85d0acff48e79ada2f91d1e79a0f>` take effect for an OBS foreign table.
-  Ensure no Chinese characters are contained in paths used for importing data to or exporting data from OBS.

.. table:: **Table 1** Read and write formats supported by OBS foreign tables

   ========== ================= ========== ============ ==========
   Data Type  User-built Server            gsmpp_server
   ========== ================= ========== ============ ==========
   ``-``      READ ONLY         WRITE ONLY READ ONLY    WRITE ONLY
   ORC        Y                 Y          x            x
   CARBONDATA Y                 x          x            x
   TEXT       Y                 x          Y            Y
   CSV        Y                 x          Y            Y
   JSON       Y                 x          x            x
   ========== ================= ========== ============ ==========

Syntax
------

.. code-block::

   CREATE FOREIGN TABLE [ IF NOT EXISTS  ] table_name
   ( { column_name type_name [column_constraint ]
       | LIKE source_table | table_constraint [, ...]} [, ...] )
   SERVER gsmpp_server
   OPTIONS (  { option_name ' value '  }  [, ...] )
   [  { WRITE ONLY  |  READ ONLY  }]
   [ WITH error_table_name | LOG INTO error_table_name]
   [PER NODE REJECT LIMIT 'value']  ;

-  **column_constraint** is as follows:

   .. code-block::

      [CONSTRAINT constraint_name]
      {PRIMARY KEY | UNIQUE}
      [NOT ENFORCED [ENABLE QUERY OPTIMIZATION | DISABLE QUERY OPTIMIZATION] | ENFORCED]

-  **table_constraint** is as follows:

   .. code-block::

      [CONSTRAINT constraint_name]
      {PRIMARY KEY | UNIQUE} (column_name)
      [NOT ENFORCED [ENABLE QUERY OPTIMIZATION | DISABLE QUERY OPTIMIZATION] | ENFORCED]

Parameter Overview
------------------

**CREATE FOREIGN TABLE** provides multiple parameters, which are classified as follows:

-  Mandatory parameters

   -  :ref:`table_name <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l1e116805692646b8a2ca3d93aef5b958>`
   -  :ref:`column_name <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2518be16d19e4cafbe13a99ccaf99af0>`
   -  :ref:`type_name <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l176fa84ebc0a4aa9a13d121a21f08851>`
   -  :ref:`SERVER gsmpp_server <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l476b88f42a094b16bd42e78b93c6c5d3>`
   -  :ref:`access_key <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l91fc6d1438d74165809df29852adb50d>`
   -  :ref:`secret_access_key <dws_06_0160>`

-  :ref:`OPTIONS parameters <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l9e47719322234105b24a0882253c15fe>`

   -  Data source location parameter in foreign tables: :ref:`location <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2721bcdfcf8a4391ae5148dd06067e3b>`
   -  Data format parameters

      -  :ref:`format <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l02cd20d09e064a269bf43102e1ca1437>`
      -  :ref:`header <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2e762d0f0291481b978b0acbd1521e3d>` (Only CSV and FIXED formats are supported.)
      -  :ref:`delimiter <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lcc2eb777e6164c60a35d88181ac54d20>`
      -  :ref:`quote <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l50b8f261d3c449e989662626550b7068>` (Only the CSV format is supported.)
      -  :ref:`escape <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l89f3a864abe54befb9b98234f2bd34dc>` (Only the CSV format is supported.)
      -  :ref:`null <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2fd004690cb34662b0b07ed5493be39c>`
      -  :ref:`noescaping <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lc2550e9054ba426996765e851a0f555b>` (Only the TEXT format is supported.)
      -  :ref:`encoding <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l5b46e2d544f84265a5116ad03d6cdcff>`
      -  :ref:`eol <en-us_topic_0000001099344764__li163076215411>`

   -  Error-tolerance parameters

      -  :ref:`fill_missing_fields <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lef2faac1a54446c59d3ff99a28cc7192>`
      -  :ref:`ignore_extra_data <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lf62d1cf82f1a4ee6bf1c497f19e0caef>`
      -  :ref:`compatible_illegal_chars <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l89bb7bce1f364bbdba8116aabe0a818d>`
      -  :ref:`PER NODE REJECT LIMIT 'val... <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lb3d7bb6ade724417b2a19bd41c30bc90>`
      -  :ref:`LOG INTO error_table_name <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_ld7597049cd774e1b95cf9133139f6051>`
      -  :ref:`WITH error_table_name <en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lc83138913ec84fab81c7e1a0fe62218e>`

Parameter Description
---------------------

-  **IF NOT EXISTS**

   Does not throw an error if a table with the same name exists. A notice is issued in this case.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l1e116805692646b8a2ca3d93aef5b958:

   **table_name**

   Specifies the name of the foreign table to be created.

   Value range: a string compliant with the naming convention.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2518be16d19e4cafbe13a99ccaf99af0:

   **column_name**

   Specifies the name of a column in the foreign table.

   Value range: a string compliant with the naming convention.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l176fa84ebc0a4aa9a13d121a21f08851:

   **type_name**

   Specifies the data type of the column.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l476b88f42a094b16bd42e78b93c6c5d3:

   **SERVER gsmpp_server**

   Specifies the server name of the foreign table. In the OBS foreign table, its server **gsmpp_server** is created by the initial database.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l9e47719322234105b24a0882253c15fe:

   **OPTIONS ( { option_name ' value ' } [, ...] )**

   Specifies parameters of foreign table data.

   -  encrypt

      Specifies whether HTTPS is enabled for data transfer. **on** enables HTTPS and **off** disables it (in this case, HTTP is used). The default value is **off**.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l91fc6d1438d74165809df29852adb50d:

      access_key

      Indicates the access key (AK, obtained from the user information on the console) used for the OBS access protocol. When you create a foreign table, its AK value is encrypted and saved to the metadata table of the database.

   -  secret_access_key:

      Indicates the secret access key (SK, obtained from the user information on the console) used for the OBS access protocol. When you create a foreign table, its SK value is encrypted and saved to the metadata table of the database.

   -  chunksize

      Specifies the cache read by each OBS thread on a DN. Its value range is 8 to 512 in the unit of MB. Its default value is **64**.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2721bcdfcf8a4391ae5148dd06067e3b:

      location

      Specifies the data source location of a foreign table. Currently, only URLs are allowed. Multiple URLs are separated using vertical bars (|).

      .. note::

         -  The URL of a read-only foreign table (the default permission is read-only) can end with the path prefix or the full path of the target object in the format of **obs://**\ *Bucket*\ **/**\ *Prefix*. *Prefix* indicates the prefix of an object path, for example, **obs://mybucket/tpch/nation/**.
         -  If the **region** parameter is explicitly specified in **obs://**\ *Bucket*\ **/**\ *Prefix*, the value of **region** will be read. If the **region** parameter is not specified, the value of **defaultRegion** will be read.
         -  The URL of a writable foreign table does not need to contain a file name. You can specify only one data source location for a foreign table. The directory corresponding to the location must be created before you specify the location.
         -  URLs specified for a read-only foreign table must be different.
         -  Specify **location** when inserting data to a foreign table.
         -  The **location** parameter supports prefixes **gsobs** and **obs**, which are identified as OBS information. **location** should be followed by **gsobs**, *OBS URL*, and *Bucket*, or by **obs** and *Bucket*.

      When importing and exporting data, you are advised to use the **location** parameter as follows:

      -  You are advised to specify a file name for **location** during data import. If you only specify an OBS bucket or directory, all text files in it will be imported. An error message will be reported if the data format is incorrect. If you set fault tolerance, a large amount of data may be imported to the fault-tolerant table.

      -  Multiple files in an OBS bucket can be imported at the same time. The matched files are imported based on the file name prefix.

         For example, you can identify and import the following two files after specifying the prefix **mybucket/input_data/product_info** in **location**:

         .. code-block::

            mybucket/input_data/product_info.0
            mybucket/input_data/product_info.1

      -  If you specify a file name, for example, **1.csv**, then other files (like **1.csv1** or **1.csv22**) starting with **1.csv** in the bucket or directory where **1.csv** resides will be automatically imported. That is, files, such as **1.csv1** and **1.csv22**, are automatically imported.

      -  To specify multiple URLs in OBS mode, separate URLs by using vertical bars (|). In gsobs mode, only one URL can be specified.

      -  During data export, a directory is generated for **location** by default. If you specify only a file name, the system automatically creates a directory whose name starts with the file name and then generates the file that stores the exported data. The file name is automatically generated by GaussDB(DWS).

      -  You can specify one path for **location** only during data export.

   -  region

      (Optional) Specifies the value of **regionCode**, which indicates the region information on the cloud.

      If the **region** parameter is explicitly specified, the value of **region** will be read. If the **region** parameter is not specified, the value of **defaultRegion** will be read.

      .. note::

         Note the following when setting parameters for importing or exporting OBS foreign tables in TEXT or CSV format:

         -  The **location** parameter is mandatory. The prefixes **gsobs** and **obs** indicate file locations on OBS. The **gsobs** prefix should be followed by *obs url*, *bucket*, and *prefix*. The **obs** prefix should be followed by *bucket* or *prefix*.
         -  The data sources of multiple buckets are separated by vertical bars (|), for example, **LOCATION 'obs://bucket1/folder/ \| obs://bucket2/'**. The database scans all objects in the specified folders.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l02cd20d09e064a269bf43102e1ca1437:

      format

      Specifies the format of the source data file in a foreign table.

      Valid value: **CSV** and **TEXT**. The default value is **TEXT**. GaussDB(DWS) only supports CSV and TEXT formats.

      -  CSV (comma-separated format):

         -  The CSV file can process linefeeds efficiently, but cannot process certain special characters very well.
         -  A CSV file is composed of records that are separated as columns by delimiters. Each record shares the same column sequence.

      -  TEXT (text format):

         -  Records are separated as columns by linefeed. The TEXT file can process special characters efficiently, but cannot process linefeeds well.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2e762d0f0291481b978b0acbd1521e3d:

      header

      Specifies whether a file contains a header with the names of each column in the file.

      When OBS exports data, this parameter cannot be set to **true**. Use the default value **false**, indicating that the first row of the exported data file is not the header.

      When data is imported, if **header** is **on**, the first row of the data file will be identified as the header and ignored. If **header** is **off**, the first row will be identified as a data row.

      Valid value: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lcc2eb777e6164c60a35d88181ac54d20:

      delimiter

      Specifies the column delimiter of data. Use the default delimiter if it is not set. The default delimiter of TEXT is a tab and that of CSV is a comma (,).

      .. note::

         -  The delimiter of TEXT cannot be **\\r** or **\\n**.
         -  A delimiter cannot be the same as the **null** value. The delimiter for the CSV format cannot be same as the **quote** value.
         -  The delimiter for the TEXT format data cannot contain backslash (\\), lowercase letters, digits, or dot (.).
         -  The data length of a single row should be less than 1 GB. A row that has many columns using long delimiters cannot contain much valid data.
         -  You are advised to use a multi-character string, such as the combination of the dollar sign ($), caret (^), and ampersand (&), or invisible characters, such as 0x07, 0x08, and 0x1b as the delimiter.

      Value range:

      The value of **delimiter** can be a multi-character delimiter whose length is less than or equal to 10 bytes.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l50b8f261d3c449e989662626550b7068:

      quote

      Specifies the quotation mark for the CSV format. The default value is a double quotation mark (").

      .. note::

         -  The **quote** value cannot be the same as the delimiter or **null** value.
         -  The **quote** value must be a single-byte character.
         -  Invisible characters are recommended as **quote** values, such as 0x07, 0x08, and 0x1b.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l89f3a864abe54befb9b98234f2bd34dc:

      escape

      Specifies an escape character for a CSV file. The value must be a single-byte character.

      The default value is a double quotation mark ("). If the value is the same as the **quote** value, it will be replaced with **\\0**.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l2fd004690cb34662b0b07ed5493be39c:

      null

      Specifies the string that represents a null value.

      .. note::

         -  The **null** value cannot be **\\r** or **\\n**. The maximum length is 100 characters.
         -  The **null** value cannot be the same as the delimiter or **quote** value.

      Value range:

      -  The default value is **\\N** for the TEXT format.
      -  The default value for the CSV format is an empty string without quotation marks.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lc2550e9054ba426996765e851a0f555b:

      noescaping

      Specifies whether to escape the backslash (\\) and its following characters in the TEXT format.

      .. note::

         **noescaping** is available only for the TEXT format.

      Valid value: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l5b46e2d544f84265a5116ad03d6cdcff:

      encoding

      Specifies the encoding of a data file, that is, the encoding used to parse, check, and generate a data file. Its default value is the default **client_encoding** value of the current database.

      Before you import foreign tables, it is recommended that you set **client_encoding** to the file encoding format, or a format matching the character set of the file. Otherwise, unnecessary parsing and check errors may occur, leading to import errors, rollback, or even invalid data import. Before exporting foreign tables, you are also advised to specify this parameter, because the export result using the default character set may not be what you expect.

      If this parameter is not specified when you create a foreign table, a warning message will be displayed on the client.

      .. note::

         Currently, OBS cannot parse a file using multiple character sets during foreign table import.

         Currently, OBS cannot write a file using multiple character sets during foreign table export.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lef2faac1a54446c59d3ff99a28cc7192:

      fill_missing_fields

      Specifies how to handle the problem that the last column of a row in the source file is lost during data import.

      Valid value: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on** and the last column of a data row in a source data file is lost, the column will be replaced with **null** and no error message will be generated.

      -  If this parameter is set to **false** or **off** and the last column of a data row in a source data file is lost, the following error information will be displayed:

         .. code-block::

            missing data for column "tt"

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lf62d1cf82f1a4ee6bf1c497f19e0caef:

      ignore_extra_data

      Specifies whether to ignore excessive columns when the number of columns in a source data file exceeds that defined in the foreign table. This parameter is available only for data import.

      Valid value: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on** and the number of source data files exceeds the number of foreign table columns, excessive columns will be ignored.

      -  If this parameter is set to **false** or **off** and the number of source data files exceeds the number of foreign table columns, the following error information will be displayed:

         .. code-block::

            extra data after last expected column

      .. important::

         If the linefeed at the end of a row is lost and this parameter is set to **true**, data in the next row will be ignored.

   -  reject_limit

      Specifies the maximum number of data format errors allowed during a data import task. If the number of errors does not reach the maximum number, the data import task can still be executed.

      .. important::

         You are advised to replace this syntax with **PER NODE REJECT LIMIT 'value'**.

         Examples of data format errors include the following: a column is lost, an extra column exists, a data type is incorrect, and encoding is incorrect. When a non-data format error occurs, the whole data import process is stopped.

      Value range: an integer and **unlimited**.

      The default value is **0**, indicating that error information is returned immediately.

   -  .. _en-us_topic_0000001099344764__li163076215411:

      eol

      Specifies the newline character style of the imported or exported data file.

      Value range: multi-character newline characters within 10 bytes. Common newline characters include **\\r** (0x0D), **\\n** (0x0A), and **\\r\\n** (0x0D0A). Special newline characters include **$** and **#**.

      .. note::

         -  The **eol** parameter supports only the TEXT format for data import.
         -  The value of the **eol** parameter cannot be the same as that of **delimiter** or **null**.
         -  The value of the **eol** parameter cannot contain digits, letters, or periods (.).

   -  date_format

      Specifies the DATE format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: a valid DATE value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         If Oracle is specified as the compatible database, the DATE format is TIMESTAMP. For details, see **timestamp_format** below.

   -  time_format

      Specifies the TIME format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid TIME value. Time zones cannot be used.

   -  timestamp_format

      Specifies the TIMESTAMP format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid TIMESTAMP value. Time zones cannot be used.

   -  smalldatetime_format

      Specifies the SMALLDATETIME format for data import. This syntax is available only for READ ONLY foreign tables.

      Value range: a valid SMALLDATETIME value.

   -  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_l89bb7bce1f364bbdba8116aabe0a818d:

      compatible_illegal_chars

      Specifies whether to enable fault tolerance on invalid characters during data import. This syntax is available only for READ ONLY foreign tables.

      Valid value: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on**, invalid characters are tolerated and imported to the database after conversion.
      -  If this parameter is set to **false** or **off** and an error occurs when there are invalid characters, the import will be interrupted.

      .. important::

         On a Windows platform, if OBS reads data files using the TEXT format, 0x1A will be treated as an EOF symbol and a parsing error will occur. It is the implementation constraint of the Windows platform. Since OBS on a Windows platform does not support BINARY read, the data can be read by OBS on a Linux platform.

      .. note::

         The rule of error tolerance for invalid characters imported is as follows:

         (1) **\\0** is converted to a space.

         (2) Other invalid characters are converted to question marks.

         (3) If **compatible_illegal_chars** is set to **true** or **on**, invalid characters are tolerated. If **null**, **delimiter**, **quote**, and **escape** are set to a spaces or question marks, errors like "illegal chars conversion may confuse COPY escape 0x20" will be displayed to prompt users to change parameter values that cause confusion, preventing import errors.

-  **READ ONLY**

   Specifies whether a foreign table is read-only. This parameter is available only for data import.

-  **WRITE ONLY**

   Specifies whether a foreign table is write-only. This parameter is available only for data import.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lc83138913ec84fab81c7e1a0fe62218e:

   **WITH error_table_name**

   Specifies the table where data format errors generated during parallel data import are recorded. You can query the error information table after data is imported to obtain error details. This parameter is available only after **reject_limit** is set.

   .. note::

      To be compatible with postgres open source interfaces, you are advised to replace this syntax with **LOG INTO**. When this parameter is specified, an error table is automatically created.

   Value range: a string compliant with the naming convention.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_ld7597049cd774e1b95cf9133139f6051:

   **LOG INTO error_table_name**

   Specifies the table where data format errors generated during parallel data import are recorded. You can query the error information table after data is imported to obtain error details.

   .. note::

      -  This parameter is available only after **PER NODE REJECT LIMIT** is set.
      -  When this parameter is specified, an error table is automatically created.

   Value range: a string compliant with the naming convention.

-  .. _en-us_topic_0000001099344764__en-us_topic_0000001098811114_en-us_topic_0117407717_lb3d7bb6ade724417b2a19bd41c30bc90:

   **PER NODE REJECT LIMIT 'value'**

   Specifies the maximum number of data format errors on each DN during data import. If the number of errors exceeds the specified value on any DN, data import fails, an error is reported, and the system exits data import.

   .. important::

      This syntax specifies the error tolerance of a single node.

      Examples of data format errors include the following: a column is lost, an extra column exists, a data type is incorrect, and encoding is incorrect. When a non-data format error occurs, the whole data scanning process is stopped.

   Valid value: an integer and **unlimited**. The default value is **0**, indicating that error information is returned immediately.

-  **NOT ENFORCED**

   Specifies the constraint to be an informational constraint. This constraint is guaranteed by the user instead of the database.

-  **ENFORCED**

   The default value is **ENFORCED**. **ENFORCED** is a reserved parameter and is currently not supported.

-  **PRIMARY KEY (column_name)**

   Specifies the informational constraint on **column_name**.

   Value range: a string. It must comply with the naming convention, and the value of **column_name** must exist.

-  **ENABLE QUERY OPTIMIZATION**

   Optimizes the query plan using an informational constraint.

-  **DISABLE QUERY OPTIMIZATION**

   Disables the optimization of the query plan using an informational constraint.

Examples
--------

Create a foreign table to import data in the .txt format from OBS to the **OBS_ft** table.

::

   DROP FOREIGN TABLE IF EXISTS OBS_ft;
   NOTICE:  foreign table "obs_ft" does not exist, skipping
   DROP FOREIGN TABLE

   CREATE FOREIGN TABLE OBS_ft( a int, b int)SERVER gsmpp_server OPTIONS (location 'obs://gaussdbcheck/obs_ddl/test_case_data/txt_obs_informatonal_test001',format 'text',encoding 'utf8',chunksize '32', encrypt 'on',ACCESS_KEY 'access_key_value_to_be_replaced',SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced',delimiter E'\x08') read only;
   CREATE FOREIGN TABLE

   DROP TABLE row_tbl;
   DROP TABLE

   CREATE TABLE row_tbl( a int, b int);
   NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using 'a' as the distribution column by default.
   HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
   CREATE TABLE

   INSERT INTO row_tbl SELECT * FROM OBS_ft;
   INSERT 0 3

Helpful Links
-------------

:ref:`ALTER FOREIGN TABLE (for HDFS or OBS) <dws_06_0124>`, :ref:`DROP FOREIGN TABLE <dws_06_0192>`
