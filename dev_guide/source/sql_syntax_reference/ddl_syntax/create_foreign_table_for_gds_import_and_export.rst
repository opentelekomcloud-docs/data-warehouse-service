:original_name: dws_06_0159.html

.. _dws_06_0159:

CREATE FOREIGN TABLE (for GDS Import and Export)
================================================

**CREATE FOREIGN TABLE** creates a GDS foreign table.

Function
--------

**CREATE FOREIGN TABLE** creates a GDS foreign table in the current database for concurrent data import and export. The GDS foreign table can be read-only or write-only, used for concurrent data import and export, respectively. The OBS foreign table is read-only by default.

Precautions
-----------

-  The foreign table is owned by the user who runs the command.
-  The distribution mode of a GDS foreign table does not need to be explicitly specified. The default is **ROUNDROBIN**.
-  All constraints (including column and row constraints) are invalid to the GDS foreign table.
-  GDS supports the following file formats: TEXT, CSV, and FIXED.

Syntax
------

::

   CREATE FOREIGN TABLE [ IF NOT EXISTS  ] table_name
       ( [  { column_name type_name POSITION(offset,length) | LIKE source_table } [, ...]  ] )
       SERVER gsmpp_server
       OPTIONS (  { option_name ' value '  }  [, ...] )
       [  { WRITE ONLY  |  READ ONLY  }]
       [ WITH error_table_name | LOG INTO error_table_name]
       [REMOTE LOG 'name']
       [PER NODE REJECT LIMIT 'value']
       [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ];

Parameter Overview
------------------

**CREATE FOREIGN TABLE** provides multiple parameters, which are classified as follows:

-  Mandatory parameters

   -  :ref:`table_name <en-us_topic_0000001188589008__l20903b2b22b741bb839421d382d07c32>`
   -  :ref:`column_name <en-us_topic_0000001188589008__l06b8baaf06ba44589e22b8c4f701c016>`
   -  :ref:`type_name <en-us_topic_0000001188589008__l6722985a800f446c8860743139d762e4>`
   -  :ref:`SERVER gsmpp_server <en-us_topic_0000001188589008__l56e9c7be738d42448d9ea638e32fe05e>`
   -  :ref:`OPTIONS <en-us_topic_0000001188589008__l2631a88728a14f43857bb4c04b2d9104>`

      -  Data source location parameter for foreign tables: :ref:`location <en-us_topic_0000001188589008__l6a4cdf99a0364e289119f03b114b8b62>`
      -  Data format parameters

         -  :ref:`format <en-us_topic_0000001188589008__l251896c64542407a912f06640ef49f5a>`
         -  :ref:`header <en-us_topic_0000001188589008__l4509784974094c3a8e2e196463bba140>` (only for CSV and FIXED source data files)
         -  :ref:`fileheader <en-us_topic_0000001188589008__l233844e8314043318e05730e92485ab7>` (only for CSV and FIXED source data files)
         -  :ref:`out_filename_prefix <en-us_topic_0000001188589008__li02461414114117>`
         -  :ref:`delimiter <en-us_topic_0000001188589008__en-us_topic_0059778310_li99825131592>`
         -  :ref:`quote <en-us_topic_0000001188589008__l97b17ee10c314710a461ba967e9d0b8c>` (only for CSV source data files)
         -  :ref:`escape <en-us_topic_0000001188589008__en-us_topic_0059778310_li74427391592>` (only for CSV source data files)
         -  :ref:`null <en-us_topic_0000001188589008__l23f5b49658fe4a77b126c30aee563507>`
         -  :ref:`noescaping <en-us_topic_0000001188589008__ldb9cda16bf2849678b91826adb3b4c96>` (only for TEXT source data files)
         -  :ref:`encoding <en-us_topic_0000001188589008__l88460d19d60945e99eccf5f6429762b7>`
         -  :ref:`eol <en-us_topic_0000001188589008__en-us_topic_0059778310_li62201592>`
         -  :ref:`conflict_delimiter <en-us_topic_0000001188589008__li718215784217>`
         -  :ref:`file_type <en-us_topic_0000001188589008__li148341029172620>`
         -  :ref:`auto_create_pipe <en-us_topic_0000001188589008__li4151040112612>`
         -  :ref:`del_pipe <en-us_topic_0000001188589008__li20789241193611>`

      -  Error-tolerance parameters

         -  :ref:`fill_missing_fields <en-us_topic_0000001188589008__l4217c10dcb944cc3a68346ad11014331>`
         -  :ref:`ignore_extra_data <en-us_topic_0000001188589008__l6a63450436114055b9ea51a0174a1886>`
         -  :ref:`reject_limit <en-us_topic_0000001188589008__lff1a3b7e86664932b1bb2f44bb740455>`
         -  :ref:`compatible_illegal_chars <en-us_topic_0000001188589008__l1355aef8984145488d8b1e213302bf55>`

      -  Performance parameter

         -  :ref:`file_sequence <en-us_topic_0000001188589008__li1155623884317>`

-  Optional parameters

   -  :ref:`WITH error_table_name <en-us_topic_0000001188589008__l38d1f5d8d31946d1ac878003337961a6>`
   -  :ref:`LOG INTO error_table_name <en-us_topic_0000001188589008__l0197538463034921bffa55634fa035d2>`
   -  :ref:`REMOTE LOG 'name' <en-us_topic_0000001188589008__leffe0ccd2877448f88dab7b30cea8b7d>`
   -  :ref:`PER NODE REJECT LIMIT 'value' <en-us_topic_0000001188589008__l858bbb2e7da849a8a52f3e80dd08ff74>`

.. _en-us_topic_0000001188589008__s949bbfb7d67e4891ac3744b6ecf3bb2a:

Parameter Description
---------------------

-  **IF NOT EXISTS**

   Does not throw an error if a table with the same name already exists. A notice is issued in this case.

-  .. _en-us_topic_0000001188589008__l20903b2b22b741bb839421d382d07c32:

   **table_name**

   Specifies the name of the foreign table to be created.

   Value range: a string. It must comply with the naming convention.

-  .. _en-us_topic_0000001188589008__l06b8baaf06ba44589e22b8c4f701c016:

   **column_name**

   Specifies the name of a column in the foreign table.

   Value range: a string. It must comply with the naming convention.

-  .. _en-us_topic_0000001188589008__l6722985a800f446c8860743139d762e4:

   **type_name**

   Specifies the data type of the column.

-  **POSITION(offset,length)**

   Defining the location of each column in the data file in fixed length mode.

   .. note::

      **offset** is the start of the column in the source file, and **length** is the length of the column.

   Value range: **offset** must be greater than 0 bytes, and its unit is byte.

   The length of each record must be less than or equal to 1 GB. By default, columns not in the file are replaced with null.

-  .. _en-us_topic_0000001188589008__l56e9c7be738d42448d9ea638e32fe05e:

   **SERVER gsmpp_server**

   Specifies the server name of the foreign table. For the GDS foreign table, its server is created by initial database, which is **gsmpp_server**.

-  .. _en-us_topic_0000001188589008__l2631a88728a14f43857bb4c04b2d9104:

   **OPTIONS ( { option_name ' value ' } [, ...] )**

   Specifies all types of parameters of foreign table data.

   -  .. _en-us_topic_0000001188589008__l6a4cdf99a0364e289119f03b114b8b62:

      location

      Specifies the data source location of the foreign table, which can be expressed through URLs. Separate URLs with vertical bars (|).

      Currently, GDS can automatically create a directory defined by a foreign table during data export. For example, when the foreign table **location** defines that **gsfs:// 192.168.0.91:5000/2019/09** executes an export task, if the **2019/09** subdirectory in the GDS data directory does not exist, the subdirectory is automatically created. You do not need to manually create the directory specified in the foreign table.

      .. note::

         -  For a read-only foreign table imported by GDS from a remote server in parallel, its URL must end with its corresponding schema or file name. (Read-only is the default file attribute.)

            For example: gsfs://192.168.0.90:5000/``*`\` or file:///data/data.txt or gsfs:// 192.168.0.90:5000/``*`\` \| gsfs:// 192.168.0.91:5000/``*``.

         -  For a writable foreign table used for GDS to export data to a remote server in parallel, file names are not required in URLs. If the data source location is a remote URL, for example, **gsfs:// 192.168.0.90:5000/**, multiple data sources can be specified. If the number of exported data file locations is less than or equal to the number of DNs, when you use the foreign table for export, data is evenly distributed to each data source location. If the number of exported data file locations is greater than the number of DNs, when you export data, the data is evenly distributed to data source locations corresponding to the DNs. Blank data files are created on the excess data source locations.

         -  For a foreign table used for GDS to import data from a remote server in parallel, the number of URLs must be less than the number of DNs, and URLs containing the same location cannot be used.

         -  If the URL begins with **gsfss://**, data is imported and exported in encryption mode, and DOP cannot exceed 10.

         -  During GDS export, the **2019/09** subdirectory in the **gsfs://127.0.0.1:7789/2019/09/** directory specified by the **location** table is automatically created.

         -  If **file_type** is set to **pipe**, GDS determines whether the target file to be imported or exported is a pipe file or a directory based on whether the last character in the URL is a slash (/). Example:

            -  In **gsfs://192.168.0.90:5000/a/b**, GDS identifies **b** as a pipe file.
            -  In **gsfs://192.168.0.90:5000/a/b/**, GDS identifies **b** as a directory and creates a pipe file in the directory.

   -  .. _en-us_topic_0000001188589008__l251896c64542407a912f06640ef49f5a:

      format

      Specifies the format of the data source file in a foreign table.

      Value range: **CSV**, **TEXT**. The default value is **TEXT**.

      -  In CSV files, escape sequences are processed as common strings. Therefore, linefeeds are processed as data.
      -  In TEXT files, escape sequences are processed as they are. Therefore, linefeeds are not processed as data.
      -  The FIXED file can process newline characters in data columns efficiently, but cannot process special characters very well.

      .. note::

         -  An escape sequence is a string starting with a backslash (\\), including **\\b** (backspace), **\\f** (formfeed page break), **\\n** (new line), **\\r** (carriage return), **\\t** (horizontal tab), \\v (vertical tab), **\\**\ *number* (octal number), and **\\x**\ *number* (hexadecimal number). In TEXT files, strings are processed as they are. In files of other formats, strings are processed as data.
         -  **FIXED** is defined as follows: (**POSITION** must be specified for each column when **FIXED** is used.)

            #. The column length of each record is the same.
            #. Spaces are used for column padding. Left padding is used for the numeric type and right padding is used for the char type.
            #. No delimiters are used between columns.

   -  .. _en-us_topic_0000001188589008__l4509784974094c3a8e2e196463bba140:

      header

      Specifies whether a data file contains a table header. header is available only for CSV and FIXED files.

      When data is imported, if **header** is **on**, the first row of the data file will be identified as title row and ignored. If header is **off**, the first row is identified as data.

      When data is exported, if **header** is **on**, :ref:`fileheader <en-us_topic_0000001188589008__l233844e8314043318e05730e92485ab7>` must be specified. **fileheader** is used to specify the export header file format. If header is **off**, the exported file does not include a title row.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

   -  .. _en-us_topic_0000001188589008__l233844e8314043318e05730e92485ab7:

      fileheader

      Specifies a file that defines the content in the header for exported data. The file contains one row of data description of each column.

      For example, to add a header in a file containing product information, define the file as follows:

      The information of products.\\n

      .. important::

         -  This parameter is available only when **header** is **on** or **true**. The file must be prepared in advance.
         -  In Remote mode, the definition file must be put to the working directory of GDS (the **-d** directory specified when starting the GDS).
         -  The definition file can contain only one row of title information, and end with a newline character. Excess rows will be discarded. (Title information cannot contain newline character).
         -  The length of the definition file including the newline character cannot exceed 1 MB.

   -  .. _en-us_topic_0000001188589008__li02461414114117:

      out_filename_prefix

      Specifies the name prefix of the exported data file exported using GDS from a write-only foreign table.

      If **file_type** is set to **pipe**, the pipe file **dbName_schemaName_foreignTableName.pipe** is generated.

      If both **out_filename_prefix** and **location** specify a pipe name, the pipe name specified in **location** is used.

      .. important::

         -  The prefix of the specified file name must be valid and compliant with the restrictions of the file system in the physical environment where the GDS is deployed. Otherwise, the file will fail to be created.

            -  The file name prefix can contain only lowercase letters, uppercase letters, digits, and underscores (_).

            -  The prefix of the specified export file name cannot contain feature fields reserved for the Windows and Linux OS, including but not limited to:

               "con","aux","nul","prn","com0","com1","com2","com3","com4","com5","com6","com7","com8","com9","lpt0","lpt1","lpt2","lpt3","lpt4","lpt5","lpt6","lpt7","lpt8","lpt9"

            -  The total length of the absolute path consisting of the exported file prefix, the path specified by **GDS -d**, **.dat**, or **.pipe** should be as required by the file system where GDS is deployed.

            -  It is required that the prefix can be correctly parsed and identified by the receiver (including but not limited to the original database where it was exported) of the data file. Identify and modify the option that causes the file name resolution problem (if any).

         -  To concurrently perform export jobs, do not use the same file name prefix for them. Otherwise, the exported files may overwrite each other or be lost in the OS or file system.

   -  .. _en-us_topic_0000001188589008__en-us_topic_0059778310_li99825131592:

      delimiter

      Specifies the column delimiter of data, and uses the default delimiter if it is not set. The default delimiter of TEXT is a tab and that of CSV is a comma (,). No delimiter is used in FIXED format.

      .. note::

         -  A delimiter cannot be \\r or \\n.
         -  A delimiter cannot be the same as the **null** value. The delimiter of CSV cannot be same as the **quote** value.
         -  The delimiter for the TEXT format data cannot contain any of the following characters: \\.abcdefghijklmnopqrstuvwxyz0123456789.
         -  The data length of a single row should be less than 1 GB. If the delimiters are too long and there are too many rows, the length of valid data will be affected.
         -  You are advised to use a multi-character, such as the combination of the dollar sign ($), caret (^), the ampersand (&), or invisible characters, such as 0x07, 0x08, and 0x1b as the delimiter.
         -  For a multi-character delimiter, do not use the same characters, for example, **---**.

      Valid value:

      The value of **delimiter** can be a multi-character delimiter whose length is less than or equal to 10 bytes.

   -  .. _en-us_topic_0000001188589008__l97b17ee10c314710a461ba967e9d0b8c:

      quote

      Specifies which characters in a CSV source data file will be identified as quotation marks. The default value is a double quotation mark (").

      .. note::

         -  The quote parameter cannot be the same as the delimiter or null parameter.
         -  The **quote** parameter must be a single-byte character.
         -  Invisible characters are recommended as **quote** values, such as 0x07, 0x08, and 0x1b.

   -  .. _en-us_topic_0000001188589008__en-us_topic_0059778310_li74427391592:

      escape

      Specifies which characters in a CSV source data file are escape characters. Escape characters can only be single-byte characters.

      Default value: the same as the value of QUOTE

   -  .. _en-us_topic_0000001188589008__l23f5b49658fe4a77b126c30aee563507:

      null

      Specifies the string that represents a null value.

      .. note::

         -  The null value cannot be \\r or \\n. The maximum length is 100 characters.
         -  The **null** value cannot be the same as the delimiter or **quote** parameter.

      Valid value:

      -  The default value is **\\n** for the TEXT format.
      -  The default value for the CSV format is an empty string without quotation marks.

   -  .. _en-us_topic_0000001188589008__ldb9cda16bf2849678b91826adb3b4c96:

      noescaping

      Specifies in TEXT format, whether to escape the backslash (\\) and its following characters.

      .. note::

         **noescaping** is available only for the TEXT format.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

   -  .. _en-us_topic_0000001188589008__l88460d19d60945e99eccf5f6429762b7:

      encoding

      Specifies the encoding of a data file, that is, the encoding used to parse, check, and generate a data file. Its default value is the default **client_encoding** value of the current database.

      Before you import foreign tables, it is recommended that you set **client_encoding** to the file encoding format, or a format matching the character set of the file. Otherwise, unnecessary parsing and check errors may occur, leading to import errors, rollback, or even invalid data import. Before you import foreign tables, you are also advised to specify this parameter, because the export result using the default character set may not be what you expected.

      If this parameter is not specified when you create a foreign table, a warning message will be displayed on the client.

      .. note::

         Currently, GDS cannot parse or write in a file using multiple encoding formats during foreign table import or export.

   -  .. _en-us_topic_0000001188589008__l4217c10dcb944cc3a68346ad11014331:

      fill_missing_fields

      Specifies whether to generate an error message when the last column in a row in the source file is lost during data import.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on** and the last column of a data row in a data source file is lost, the column will be replaced with **NULL** and no error message will be generated.

      -  If this parameter is set to **false** or **off** and the last column is missing, the following error information will be displayed:

         .. code-block::

            missing data for column "tt"

   -  .. _en-us_topic_0000001188589008__l6a63450436114055b9ea51a0174a1886:

      ignore_extra_data

      Specifies whether to ignore excessive columns when the number of data source files exceeds the number of foreign table columns. This parameter is available during data import.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on** and the number of data source files exceeds the number of foreign table columns, excessive columns will be ignored.

      -  If this parameter is set to **false** or **off** and the number of data source files exceeds the number of foreign table columns, the following error information will be displayed:

         .. code-block::

            extra data after last expected column

      .. important::

         If the newline character at the end of the row is lost, setting the parameter to **true** will ignore data in the next row.

   -  .. _en-us_topic_0000001188589008__lff1a3b7e86664932b1bb2f44bb740455:

      reject_limit

      Specifies the maximum number of data format errors allowed during a data import task. If the number of errors does not reach the maximum number, the data import task can still be executed.

      .. important::

         You are advised to replace this syntax with **PER NODE REJECT LIMIT 'value'**.

         Examples of data format errors include the following: a column is lost, an extra column exists, a data type is incorrect, and encoding is incorrect. Once a non-data format error occurs, the whole data import process is stopped.

      Value range: a positive integer or **unlimited**

      If this parameter is not specified, an error message is returned immediately.

      .. note::

         Enclose positive integer values with single quotation marks ('').

   -  mode

      Specifies the data import policy during a specific data import process. GaussDB(DWS) supports only the **Normal** mode.

      Valid value:

      -  **Normal** (default): supports all file types (CSV, TEXT, FIXED). Enabling Gauss data service to help data import.

   -  .. _en-us_topic_0000001188589008__en-us_topic_0059778310_li62201592:

      eol

      Specifies the newline character style of the imported or exported data file.

      Value range: multi-character newline characters within 10 bytes. Common newline characters include **\\r** (0x0D), **\\n** (0x0A), and **\\r\\n** (0x0D0A). Special newline characters include **$** and **#**.

      .. note::

         -  The **eol** parameter supports only the TEXT format for data import and export and does not support the CSV or FIXED format for data import. For forward compatibility, the **eol** parameter can be set to **0x0D** or **0x0D0A** for data export in the CSV and FIXED formats.
         -  The value of the **eol** parameter cannot be the same as that of **DELIMITER** or **NULL**.
         -  The value of the **eol** parameter cannot contain digits, letters, or periods (.).

   -  .. _en-us_topic_0000001188589008__li718215784217:

      conflict_delimiter

      This parameter is generally used with the :ref:`compatible_illegal_chars <en-us_topic_0000001188589008__l1355aef8984145488d8b1e213302bf55>` parameter. If a data file contains a truncated Chinese character, the truncated character and a delimiter will be encoded into another Chinese character due to inconsistent encoding between the foreign table and the database. As a result, the delimiter is masked and an error will be reported, indicating that there are missing fields.

      This parameter is used to avoid encoding a truncated character and a delimiter into another character.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If the parameter is set to **true** or **on**, encoding a truncated character and a delimiter into another character is allowed.
      -  If the parameter is set to **false** or **off**, encoding a truncated character and a delimiter into another character is not allowed.

      .. important::

         This parameter is disabled by default. It is recommended that you disable this parameter, because encoding a truncated character and a delimiter into another character is rarely required. If the parameter is enabled, the scenario may be incorrectly identified and thereby causing incorrect information imported to the table.

   -  .. _en-us_topic_0000001188589008__li148341029172620:

      file_type

      Specifies the type of the file to be imported or exported.

      Value options: **normal**, **pipe**. **normal** is the default value.

      -  If this parameter is set to **normal**, the file to be imported or exported is a common file.
      -  If this parameter is set to **pipe**, the file to be imported or exported is a named pipe.

   -  .. _en-us_topic_0000001188589008__li1155623884317:

      file_sequence

      Concurrently imports data in parallel through GDS foreign tables, to improve single-file import performance. This parameter is only used for data import.

      The parameter format is **file_sequence**'*total number of shards*\ ``-``\ *current shard*'. Example:

      **file_sequence '3-1'** indicates that the imported file is logically split into three shards and the data currently imported by the foreign table is the data on the first shard.

      **file_sequence '3-2'** indicates that the imported file is logically split into three shards and the data currently imported by the foreign table is the data on the second shard.

      **file_sequence '3-3'** indicates that the imported file is logically split into three shards and the data currently imported by the foreign table is the data on the third shard.

      This parameter has the following constraints:

      -  A file can be split to a maximum of 8 shards.
      -  The number of currently imported shard should be less than or equal to the total number of split shards.
      -  Only CSV and TXT files can be imported.

      .. note::

         When data is imported in parallel in CSV format, some shards fail to be imported in the following scenario because the CSV rules conflict with the GDS splitting logic:

         Scenario: A CSV file contains a newline character that is not escaped, the newline character is contained in the character specified by **quote**, and the data of this line is in the first row of the logical shard.

         For example, if you import the **big.csv** file in parallel, the following information is displayed:

         .. code-block::

            --id, username, address
            10001,"customer1 name","Rose District"
            10002,"customer2 name","
            23 Road Rose
            District NewCity"
            10003,"customer3 name","NewCity"

         After the file is split into two shards, the content of the first shard is as follows:

         .. code-block::

            10001,"customer1 name","Rose District"
            10002,"customer2 name","
            23

         The content of the second shard is as follows:

         .. code-block::

            Road Rose
            District NewCity"
            10003,"customer3 name","NewCity"

         The newline character after the first line of the second shard is contained between double quotation marks. As a result, GDS cannot determine whether the newline character is a newline character in the field or a separator in the line. Therefore, two data records on the first shard are successfully imported, but the second shard fails to be imported.

   -  .. _en-us_topic_0000001188589008__li4151040112612:

      auto_create_pipe

      This parameter specifies whether the GDS process automatically creates a named pipe.

      Value options: **true**, **on**, **false**, and **off**. The default value is **true**/**on**.

      -  If this parameter is set to **true** or **on**, the GDS process is allowed to automatically create a named pipe.
      -  If this parameter is set to **false** or **off**, you need to manually create a named pipe.

      .. important::

         -  When setting **auto_create_pipe**, set **file_type** to **pipe**. Otherwise, the foreign table cannot be created.
         -  If **auto_create_pipe** is set to **false** and no pipe is specified during data import and export, the *database name*\ \_\ *schema name*\ \_\ *foreign table name*\ **.pipe** file will be opened. If a pipe has been specified, the specified pipe in the location will be opened. If the named pipe is not written by other programs or is not opened in write mode within the period specified by the **pipe-timeout** parameter, an error message is displayed indicating that the import or export task times out. If the file is not a pipe, an error is reported when the import or export task is executed.
         -  If **auto_create_pipe** is set to **true** and no pipe file is specified during data import and export, the *database name*\ \_\ *schema name*\ \_\ *foreign table name*\ **.pipe** file will be opened. If the file is a common file, an error is reported when the file is imported or exported. If the file is a pipe, the system automatically deletes the file and re-creates the named pipe.
         -  You can use the :ref:`location <en-us_topic_0000001188589008__l6a4cdf99a0364e289119f03b114b8b62>` parameter to specify the pipe when exporting data, for example, **location'gsfs://127.0.0.1:7789/aa.pipe**. When **auto_create_pipe** is set to **true**, GDS automatically creates the **aa.pipe** file in the data directory.

   -  .. _en-us_topic_0000001188589008__li20789241193611:

      del_pipe

      This parameter specifies whether to automatically delete the pipe file after the import or export task is complete.

      Value options: **true** or **on**; **false** or **off**. The default value is **true** or **on**.

      -  If this parameter is set to **true** or **on**, the GDS process will automatically delete a named pipe file.
      -  If this parameter is set to **false** or **off**, the GDS process will not delete a named pipe file.

      .. important::

         When setting **del_pipe**, set **file_type** to **pipe**. Otherwise, the foreign table cannot be created.

   -  fix

      Specifies the length of fixed format data. The unit is byte. This syntax is available only for READ ONLY foreign tables.

      Value range: Less than **1 GB**, and greater than or equal to the total length specified by **POSITION** (The total length is the sum of **offset** and **length** in the last column of the table definition.)

   -  out_fix_alignment

      Specifies how the columns of the types BYTEAOID, CHAROID, NAMEOID, TEXTOID, BPCHAROID, VARCHAROID, NVARCHAR2OID, and CSTRINGOID are aligned during fixed-length export.

      Value range: **align_left**, **align_right**

      Default value: **align_right**

      .. important::

         The bytea data type must be in hexadecimal format (for example, \\XXXX) or octal format (for example, \\XXX\\XXX\\XXX). The data to be imported must be left-aligned (that is, the column data starts with either of the two formats instead of spaces). Therefore, if the exported file needs to be imported using a GDS foreign table and the file data length is less than that specified by the foreign table formatter, the exported file must be left aligned. Otherwise, an error is reported during the import.

   -  date_format

      Imports data of the DATE type. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid DATE value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         If ORACLE is specified as the compatible database, the DATE format is TIMESTAMP. For details, see **timestamp_format** below.

   -  time_format

      Imports data of the TIME type. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid TIME value. Time zones cannot be used. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  timestamp_format

      Imports data of the TIMESTAMP type. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid TIMESTAMP value. Time zones are not supported. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  smalldatetime_format

      Imports data of the SMALLDATETIME type. This syntax is available only for READ ONLY foreign tables.

      Value range: any valid SMALLDATETIME value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  .. _en-us_topic_0000001188589008__l1355aef8984145488d8b1e213302bf55:

      compatible_illegal_chars

      Enables or disables fault tolerance on invalid characters during data import. This syntax is available only for READ ONLY foreign tables.

      Value range: **true**, **on**, **false**, and **off**. The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on**, invalid characters are tolerated and imported to the database after conversion.
      -  If this parameter is set to **false** or **off** and an error occurs when there are invalid characters, the import will be interrupted.

      .. note::

         The rule of error tolerance when you import invalid characters is as follows:

         (1) **\\0** is converted to a space.

         (2) Other invalid characters are converted to question marks.

         (3) If **compatible_illegal_chars** is set to **true** or **on**, invalid characters are tolerated. If **NULL**, **DELIMITER**, **QUOTE**, and **ESCAPE** are set to a spaces or question marks. Errors like "illegal chars conversion may confuse COPY escape 0x20" will be displayed to prompt user to modify parameter values that cause confusion, preventing import errors.

-  **READ ONLY**

   Specifies whether a foreign table is read-only. This parameter is available only for data import.

-  **WRITE ONLY**

   Specifies whether a foreign table is write-only. This parameter is available only for data export.

-  .. _en-us_topic_0000001188589008__l38d1f5d8d31946d1ac878003337961a6:

   **WITH error_table_name**

   Specifies the table where data format errors generated during parallel data import are recorded. You can query the error information table after data is imported to obtain error details. This parameter is available only after **reject_limit** is set.

   .. note::

      To be compatible with PostgreSQL open source interfaces, you are advised to replace this syntax with **LOG INTO**.

   Value range: a string. It must comply with the naming convention.

-  .. _en-us_topic_0000001188589008__l0197538463034921bffa55634fa035d2:

   **LOG INTO error_table_name**

   Specifies the table where data format errors generated during parallel data import are recorded. You can query the error information table after data is imported to obtain error details.

   .. note::

      This parameter is available only after **PER NODE REJECT LIMIT** is set.

   Value range: a string. It must comply with the naming convention.

-  .. _en-us_topic_0000001188589008__leffe0ccd2877448f88dab7b30cea8b7d:

   **REMOTE LOG 'name'**

   The data format error information is saved as files in GDS. **name** is the prefix of the error data file.

-  .. _en-us_topic_0000001188589008__l858bbb2e7da849a8a52f3e80dd08ff74:

   **PER NODE REJECT LIMIT 'value'**

   This parameter specifies the allowed number of data format errors on each DN during data import. If the number of errors exceeds the specified value on any DN, data import fails, an error is reported, and the system exits data import.

   .. important::

      This syntax specifies the error tolerance of a single node.

      Examples of data format errors include the following: a column is lost, an extra column exists, a data type is incorrect, and encoding is incorrect. When a non-data format error occurs, the whole data import process stops.

   Value range: integer, unlimited. If this parameter is not specified, an error information is returned immediately.

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   Currently, **TO GROUP** cannot be used. **TO NODE** is used for internal scale-out tools.

Examples
--------

Create a foreign table\ **customer_ft** to import data from GDS server 10.10.123.234 in TEXT format:

::

   CREATE FOREIGN TABLE customer_ft
   (
       c_customer_sk             integer               ,
       c_customer_id             char(16)              ,
       c_current_cdemo_sk        integer               ,
       c_current_hdemo_sk        integer               ,
       c_current_addr_sk         integer               ,
       c_first_shipto_date_sk    integer               ,
       c_first_sales_date_sk     integer               ,
       c_salutation              char(10)              ,
       c_first_name              char(20)              ,
       c_last_name               char(30)              ,
       c_preferred_cust_flag     char(1)               ,
       c_birth_day               integer               ,
       c_birth_month             integer               ,
       c_birth_year              integer                       ,
       c_birth_country           varchar(20)                   ,
       c_login                   char(13)                      ,
       c_email_address           char(50)                      ,
       c_last_review_date        char(10)
   )
       SERVER gsmpp_server
       OPTIONS
   (
       location 'gsfs://10.10.123.234:5000/customer1*.dat',
       FORMAT 'TEXT' ,
       DELIMITER '|',
       encoding 'utf8',
       mode 'Normal')
   READ ONLY;

Create a foreign table to import data from GDS servers 192.168.0.90 and 192.168.0.91 in TEXT format. Record errors that occur during data import in **foreign_HR_staffS_ft**. A maximum of two data format errors are allowed during the data import.

::

   CREATE FOREIGN TABLE foreign_HR_staffS_ft
   (
     staff_ID       NUMBER(6) ,
     FIRST_NAME     VARCHAR2(20),
     LAST_NAME      VARCHAR2(25),
     EMAIL          VARCHAR2(25),
     PHONE_NUMBER   VARCHAR2(20),
     HIRE_DATE      DATE,
     employment_ID  VARCHAR2(10),
     SALARY         NUMBER(8,2),
     COMMISSION_PCT NUMBER(2,2),
     MANAGER_ID     NUMBER(6),
     section_ID  NUMBER(4)
   ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/* | gsfs://192.168.0.91:5000/*', format 'TEXT', delimiter E'\x08',  null '',reject_limit '2') WITH err_HR_staffS_ft;

Create a foreign table to import all files in the **input_data** directory in CSV format.

::

   CREATE FOREIGN TABLE foreign_HR_staffS_ft1
   (
     staff_ID       NUMBER(6) ,
     FIRST_NAME     VARCHAR2(20),
     LAST_NAME      VARCHAR2(25),
     EMAIL          VARCHAR2(25),
     PHONE_NUMBER   VARCHAR2(20),
     HIRE_DATE      DATE,
     employment_ID  VARCHAR2(10),
     SALARY         NUMBER(8,2),
     COMMISSION_PCT NUMBER(2,2),
     MANAGER_ID     NUMBER(6),
     section_ID     NUMBER(4)
   ) SERVER gsmpp_server OPTIONS (location 'file:///input_data/*', format 'csv', quote E'\x08', mode 'private', delimiter ',') WITH err_HR_staffS_ft1;

Create a foreign table to export data to the **output_data** directory in CSV format.

::

   CREATE FOREIGN TABLE foreign_HR_staffS_ft2
   (
     staff_ID       NUMBER(6) ,
     FIRST_NAME     VARCHAR2(20),
     LAST_NAME      VARCHAR2(25),
     EMAIL          VARCHAR2(25),
     PHONE_NUMBER   VARCHAR2(20),
     HIRE_DATE      DATE,
     employment_ID  VARCHAR2(10),
     SALARY         NUMBER(8,2),
     COMMISSION_PCT NUMBER(2,2),
     MANAGER_ID     NUMBER(6),
     section_ID  NUMBER(4)
   ) SERVER gsmpp_server OPTIONS (location 'file:///output_data/', format 'csv', quote E'\x08', delimiter '|', header 'on') WRITE ONLY;

Helpful Links
-------------

:ref:`ALTER FOREIGN TABLE (GDS Import and Export) <dws_06_0123>`, :ref:`DROP FOREIGN TABLE <dws_06_0192>`
