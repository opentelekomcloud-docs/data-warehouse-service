:original_name: dws_06_0230.html

.. _dws_06_0230:

COPY
====

Function
--------

**COPY** copies data between tables and files.

**COPY FROM** copies data from a file to a table. **COPY TO** copies data from a table to a file.

Important Notes
---------------

-  If CNs and DNs are enabled in security mode, the **COPY FROM FILENAME** or **COPY TO FILENAME** cannot be used. Use **\\copy** to avoid this problem, for details, see "FAQs" > "Data Import and Export" > "How Do I Use \\copy to Import and Export Data?" in the *Data Warehouse Service (DWS) User Guide*.
-  **COPY** applies to only tables and does not apply to views.
-  To insert data to a table, you must have the permission to insert data.
-  If a list of columns is specified, **COPY** will only copy the data in the specified columns to or from the file. If there are any columns in the table that are not in the column list, **COPY FROM** will insert the default values for those columns.
-  If a source data file is specified, the file must be accessible from the server. If **STDIN** is specified, data is transmitted between the client and server. Separate columns by pressing **Tab**. Enter **\\.** in a new line to indicate the end of input.
-  If the number of columns in a row of the data file is smaller or larger than the expected number, **COPY FROM** displays an error message.
-  A backslash and a period (\\.) indicate the end of data. The end identifier is not required for reading data from a file and is required for copying data between client applications.
-  In **COPY FROM**, "\\N" indicates an empty character string, and "\\\\N" indicates the actual data "\\N".

-  **COPY FROM** does not support pre-processing of data during data import, for example, expression calculation and default value filling. If you need to perform pre-processing during data import, you need to import the data to a temporary table, and then run SQL statements to insert data to the table using expression or function operations. However, this method may cause I/O expansion, deteriorating data import performance.
-  Transactions will be rolled back when data format errors occur during **COPY FROM** execution. In this case, error information is insufficient so you cannot easily locate the incorrect data from a large amount of raw data.
-  **COPY FROM** and **COPY TO** apply to low concurrency and local data import and export in small amount.
-  You can use **COPY TO** to export data in TEXT or CSV format to OBS, but cannot use **COPY FROM** to import data from OBS. For CSV files that are exported to OBS and contain utf8 BOM, you are advised to use OBS to import them. Otherwise, the BOM field may fail to be identified.

Syntax
------

-  Copy the data from a file to a table:

   ::

      COPY table_name [ ( column_name [, ...] ) ]
          FROM { 'filename' | STDIN }
          [ [ USING ] DELIMITERS 'delimiters' ]
          [ WITHOUT ESCAPING ]
          [ LOG ERRORS ]
          [ LOG ERRORS data ]
          [ REJECT LIMIT 'limit' ]
          [ [ WITH ] ( option [, ...] ) ]
          | copy_option
          | FIXED FORMATTER ( { column_name( offset, length ) } [, ...] ) [ ( option [, ...] ) | copy_option [  ...] ] ];

   .. note::

      In the SQL syntax, **FIXED**, **FORMATTER ( { column_name( offset, length ) } [, ...] )**, and **[ ( option [, ...] ) \| copy_option [ ...] ]** can be in any sequence.

-  Copy the data from a table to a file:

   ::

      COPY table_name [ ( column_name [, ...] ) ]
          TO { 'filename' | STDOUT }
          [ [ USING ] DELIMITERS 'delimiters' ]
          [ WITHOUT ESCAPING ]
          [ [ WITH ] ( option [, ...] ) ]
          | copy_option
          | FIXED FORMATTER ( { column_name( offset, length ) } [, ...] ) [ ( option [, ...] ) | copy_option [  ...] ] ];

      COPY query
          TO { 'filename' | STDOUT }
          [ WITHOUT ESCAPING ]
          [ [ WITH ] ( option [, ...] ) ]
          | copy_option
          | FIXED FORMATTER ( { column_name( offset, length ) } [, ...] ) [ ( option [, ...] ) | copy_option [  ...] ] ];

   .. note::

      #. The syntax constraints of **COPY TO** are as follows:

         **(query)** is incompatible with **[USING] DELIMITER**. If the data of **COPY TO** comes from a query result, **COPY TO** cannot specify **[USING] DELIMITERS**.

      #. Use spaces to separate **copy_option** following **FIXED FORMATTTER**.

      #. **copy_option** is the native parameter, while **option** is the parameter imported by a compatible foreign table.

      #. In the SQL syntax, **FIXED**, **FORMATTER ( { column_name( offset, length ) } [, ...] )**, and **[ ( option [, ...] ) \| copy_option [ ...] ]** can be in any sequence.

   The syntax of the optional parameter **option** is as follows:

   ::

      FORMAT 'format_name'
      | OIDS [ boolean ]
      | DELIMITER 'delimiter_character'
      | NULL 'null_string'
      | HEADER [ boolean ]
      | FILEHEADER 'header_file_string'
      | FREEZE [ boolean ]
      | QUOTE 'quote_character'
      | ESCAPE 'escape_character'
      | EOL 'newline_character'
      | NOESCAPING [ boolean ]
      | FORCE_QUOTE { ( column_name [, ...] ) | * }
      | FORCE_NOT_NULL ( column_name [, ...] )
      | ENCODING 'encoding_name'
      | IGNORE_EXTRA_DATA [ boolean ]
      | FILL_MISSING_FIELDS [ boolean ]
      | COMPATIBLE_ILLEGAL_CHARS [ boolean ]
      | PRESERVE_BLANKS [ boolean ]
      | DATE_FORMAT 'date_format_string'
      | TIME_FORMAT 'time_format_string'
      | TIMESTAMP_FORMAT 'timestamp_format_string'
      | SMALLDATETIME_FORMAT 'smalldatetime_format_string'
      | SERVER 'obs_server_string'
      | BOM [ boolean ]
      | MAXROW [ integer ]
      | FILEPREFIX 'file_prefix_string'

   The syntax of optional parameter in the **copy_option** is as follows:

   ::

      OIDS
      | NULL 'null_string'
      | HEADER
      | FILEHEADER 'header_file_string'
      | FREEZE
      | FORCE_NOT_NULL column_name [, ...]
      | FORCE_QUOTE { column_name [, ...] | * }
      | BINARY
      | CSV
      | QUOTE [ AS ] 'quote_character'
      | ESCAPE [ AS ] 'escape_character'
      | EOL 'newline_character'
      | ENCODING 'encoding_name'
      | IGNORE_EXTRA_DATA
      | FILL_MISSING_FIELDS
      | COMPATIBLE_ILLEGAL_CHARS
      | PRESERVE_BLANKS
      | DATE_FORMAT 'date_format_string'
      | TIME_FORMAT 'time_format_string'
      | TIMESTAMP_FORMAT 'timestamp_format_string'
      | SMALLDATETIME_FORMAT 'smalldatetime_format_string'

Parameter Description
---------------------

-  **query**

   Indicates that the results are to be copied.

   Value range: a **SELECT** or **VALUES** command in parentheses

-  **table_name**

   Specifies the name (optionally schema-qualified) of an existing table.

   Value range: an existing table name

-  **column_name**

   Indicates an optional list of columns to be copied.

   Value range: If no column list is specified, all columns of the table will be copied.

-  **STDIN**

   Indicates that the input comes from the client application.

-  **STDOUT**

   Indicates that output goes to the client application.

-  **FIXED**

   Fixes column length. When the column length is fixed, **DELIMITER**, **NULL**, and **CSV** cannot be specified. When **FIXED** is specified, **BINARY**, **CSV**, and **TEXT** cannot be specified by **option** or **copy_option**.

   .. note::

      The definition of fixed length:

      #. The column length of each record is the same.
      #. Spaces are added to short columns. Digit type columns must be left-aligned, and character columns must be right-aligned.
      #. No delimiters are used between columns.

-  **[USING] DELIMITER 'delimiters'**

   The string that separates columns within each row (line) of the file, and it cannot be larger than 10 bytes.

   Value range: The delimiter cannot include any of the following characters: \\.abcdefghijklmnopqrstuvwxyz0123456789

   Value range: The default value is a tab character in text format and a comma in CSV format.

-  **WITHOUT ESCAPING**

   In TEXT, do not escape a backslash (\\) and the characters that follow it.

   Value range: text only.

-  **LOG ERRORS**

   If this parameter is specified, the error tolerance mechanism for data type errors in the **COPY FROM** statement is enabled. Row errors are recorded in the **public.pgxc_copy_error_log** table in the database for future reference.

   Value range: A value set while data is imported using **COPY FROM**.

   .. note::

      The restrictions of this error tolerance parameter are as follows:

      -  This error tolerance mechanism captures only the data type errors (DATA_EXCEPTION) that occur during data parsing of **COPY FROM** on a CN. Other errors, such as network errors between CNs and DNs or expression conversion errors on DNs, are not captured.
      -  Before enabling error tolerance for **COPY FROM** for the first time in a database, check whether the **public.pgxc_copy_error_log** table exists. If it does not, call the copy_error_log_create() function to create it. If it does, copy its data elsewhere and call the copy_error_log_create() function to create the table. For details about columns in the **public.pgxc_copy_error_log** table, see :ref:`Table 3 <en-us_topic_0000001811634829__table63361925092>`.
      -  While a **COPY FROM** statement with specified **LOG ERRORS** is being executed, if **public.pgxc_copy_error_log** does not exist or does not have the table definitions compliant with the predefined in copy_error_log_create(), an error will be reported. Ensure that the error table is created using the copy_error_log_create() function. Otherwise, **COPY FROM** statements with error tolerance may fail to be run.
      -  If existing error tolerance parameters (for example, **IGNORE_EXTRA_DATA**) of the **COPY** statement are enabled, the error of the corresponding type will be processed as specified by the parameters and no error will be reported. Therefore, the error table does not contain such error data.
      -  The coverage scope of this error tolerance mechanism is the same as that of a GDS foreign table. You are advised to filter query results based on table names or the timestamp of marking the start of **COPY FROM** statement execution. For details about how to process error data, see the section about handling error tables.

-  **LOG ERRORS DATA**

   The differences between **LOG ERRORS DATA** and **LOG ERRORS** are as follows:

   #. **LOG ERRORS DATA** fills the **rawrecord** field in the error tolerance table.
   #. If error content is too complex, it may fail to be written to the error tolerance table by using **LOG ERRORS DATA**, causing the task failure.

-  **REJECT LIMIT '\ limit'**

   Used with the **LOG ERROR** parameter to set the upper limit of the tolerated errors in the **COPY FROM** statement. If the number of errors exceeds the limit, later errors will be reported based on the original mechanism.

   Value range: a positive integer (1 to INTMAX) or **unlimited**

   Default value: If **LOG ERRORS** is not specified, an error will be reported. If **LOG ERRORS** is specified, the default value is **0**.

   .. note::

      Different from the GDS error tolerance mechanism, in the error tolerance mechanism described in the description of **LOG ERRORS**, the count of **REJECT LIMIT** is calculated based on the number of data parsing errors on the CN where the **COPY FROM** statement is run, not based on the number of errors on each DN.

-  **FORMATTER**

   Defining the location of each column in the data file in fixed length mode. Defining the place of each column in the data file based on column (offset, length) format.

   Value range:

   -  The value of **offset** must be larger than 0. The unit is byte.
   -  The value of **length** must be larger than 0. The unit is byte.

   The total length of all columns must be less than 1 GB.

   Replace columns that are not in the file with NULL.

-  **OPTION { option_name ' value ' }**

   Specifies all types of parameters of a compatible foreign table.

   -  FORMAT

      Specifies the format of the source data file in the foreign table.

      Value range: CSV, TEXT, FIXED, and BINARY.

      -  The CSV file can process newline characters efficiently, but cannot process certain special characters well.
      -  The TEXT file can process special characters efficiently, but cannot process newline character well.
      -  The FIXED file can process newline characters in data columns efficiently, but cannot process special characters well.
      -  All data in the BINARY file is stored/read as binary format rather than as text. It is faster than the text and CSV formats, but a binary-format file is less portable.

      Default value: **TEXT**

   -  OIDS

      Copies the OID for each row.

      .. note::

         An error is raised if OIDs are specified for a table that does not have OIDs, or in the case of copying a query.

      Value range: **true**, **on**, **false**, and **off**

      Default value: **false**

   -  DELIMITER

      Specifies the character that separates columns within each row (line) of the file.

      .. note::

         -  A delimiter cannot be \\r or \\n.
         -  A delimiter cannot be the same as null. The delimiter for CSV cannot be same as quote.
         -  The delimiter for the TEXT format data cannot contain lowercase letters, digits, or dot (.).
         -  The data length of a single row should be less than 1 GB. If the delimiters are too long and there are too many rows, the length of valid data will be affected.
         -  You are advised to use multi-characters and invisible characters for delimiters. For example, you can use multi-characters (such as $^&) and invisible characters (such as 0x07, 0x08, and 0x1b).
         -  For a multi-character delimiter, do not use the same characters, for example, **---**.

      Value range: multi-character delimiter within 10 bytes.

      Default value:

      -  A tab character in TEXT format
      -  A comma (,) in CSV format
      -  No delimiter in FIXED format

   -  NULL

      Specifies the string that represents a null value.

      Value range:

      -  The null value cannot be **\\r** or **\\n**. The maximum length is 100 characters.
      -  The null value cannot be the same as the delimiter or quote parameter.

      Default value:

      -  An empty string without quotation marks in CSV format
      -  **\\N** in TEXT format

   -  HEADER

      Specifies whether a file contains a header with the names of each column in the file. header is available only for CSV and FIXED files.

      When data is imported, if **header** is **on**, the first row of the data file will be identified as title row and ignored. If header is **off**, the first row is identified as data.

      When data is exported, if **header** is **on**, **fileheader** must be specified. If header is **off**, the exported file does not include a title row.

      Value range: true, on, false, and off

      Default value: **false**

   -  QUOTE

      Specifies the quote character used when a data value is referenced in a CSV file.

      Default value: double quotation mark ("")

      .. note::

         -  The quote parameter cannot be the same as the delimiter or null parameter.
         -  The **quote** parameter must be a single one-byte character.
         -  Invisible characters are recommended as **quote** values, such as 0x07, 0x08, and 0x1b.

   -  ESCAPE

      This option is allowed only when using CSV format. This must be a single one-byte character.

      Default value: the same as the value of QUOTE

   -  EOL 'newline_character'

      Specifies the newline character style of the imported or exported data file.

      Value range: multi-character newline characters within 10 bytes. Common newline characters include **\\r** (0x0D), **\\n** (0x0A), and **\\r\\n** (0x0D0A). Special newline characters include **$** and **#**.

      .. note::

         -  The **EOL** parameter supports only the TEXT format for data import and export and does not support the CSV or FIXED format for data import. For forward compatibility, the **EOL** parameter can be set to **0x0D** or **0x0D0A** for data export in the CSV and FIXED formats.
         -  The value of the **EOL** parameter cannot be the same as that of **DELIMITER** or **NULL**.
         -  The **EOL** parameter value cannot contain lowercase letters, digits, or dot (.).

   -  FORCE_QUOTE { ( column_name [, ...] ) \| \* }

      Forces quotation marks to be used for all non-null values in each specified column in CSV COPY TO mode. **NULL** values are not quoted.

      Value range: an existing column

   -  FORCE_NOT_NULL ( column_name [, ...] )

      Does not match the specified columns' values against the null string. This option is allowed only in **COPY FROM**, and only when using the CSV format.

      Value range: an existing column

   -  ENCODING

      Specifies that the file is encoded in the **encoding_name**. If this option is omitted, the current encoding format is used by default.

      .. note::

         Common encoding formats include UTF8, GBK, and GB18030. GB18030 has two versions: GB18030 and GB18030_2022. GB18030_2022 is the latest national standard in China prepared to support Chinese characters.

   -  IGNORE_EXTRA_DATA

      When the number of data source files exceeds the number of foreign table columns, whether ignoring excessive columns at the end of the row. This parameter is available only during data importing.

      Value range: true/on, false/off.

      -  When this parameter is **true** or **on** and the number of data source files exceeds the number of foreign table columns, excessive columns will be ignored.

      -  If the parameter is set to **false** or **off**, and the number of data source files exceeds the number of foreign table columns, the following error information will be displayed:

         ::

            extra data after last expected column

      Default value: **false**

      .. important::

         If the newline character at the end of the row is lost, setting the parameter to **true** will ignore data in the next row.

   -  COMPATIBLE_ILLEGAL_CHARS

      Enables or disables fault tolerance on invalid characters during importing. This parameter is available only for **COPY FROM**.

      Value range: true, on, false, and off

      -  When the parameter is **true** or **on**, invalid characters are tolerated and imported to the database after conversion.
      -  If the parameter is **false** or **off**, and an error occurs when there are invalid characters, the import will be interrupted.

      Default value: **false** or **off**

      .. note::

         The rule of error tolerance when you import invalid characters is as follows:

         #. **\\0** is converted to a space.
         #. Other invalid characters are converted to question marks.
         #. Setting **compatible_illegal_chars** to **true/on** enables toleration of invalid characters. If **NULL**, **DELIMITER**, **QUOTE**, and **ESCAPE** are set to spaces or question marks, errors like "illegal chars conversion may confuse COPY escape 0x20" will be displayed to prompt the user to modify parameters that may cause confusion, preventing importing errors.

   -  FILL_MISSING_FIELDS

      Specifies whether to generate an error message when the last column in a row in the source file is lost during data loading.

      Value range: **true**, **on**, **false**, and **off**

      -  If this parameter is set to **true** or **on** and the last column of a data row in a source data file is lost, the column will be replaced with **NULL** and no error message will be generated.

      -  If this parameter is set to **false** or **off** and the last column of a data row in a source data file is lost, the following error information will be displayed:

         .. code-block::

            missing data for column "tt"

      Default value: **false** or **off**

   -  DATE_FORMAT

      Imports data of the **DATE** type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: any valid DATE value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         If ORACLE is specified as the compatible database, the DATE format is TIMESTAMP. For details, see **timestamp_format** below.

   -  TIME_FORMAT

      Imports data of the TIME type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: Valid TIME. Time zones cannot be used. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  TIMESTAMP_FORMAT

      Imports data of the TIMESTAMP type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: any valid TIMESTAMP value. Time zones are not supported. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  SMALLDATETIME_FORMAT

      Imports data of the SMALLDATETIME type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: any valid SMALLDATETIME value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  SERVER

      Specifies the OBS server. **filename** is a path on OBS, indicating that data is exported to OBS.

      Value range: an existing OBS server name.

      .. note::

         This parameter is valid only for **COPY TO**.

   -  BOM

      Specifies whether to add the utf8 BOM field to the exported CSV file.

      Value range: **true**, **on**, **false**, and **off**

      Default value: **false**

      .. note::

         This parameter is valid only when **COPY TO** is executed and a valid SERVER is specified. The exported file must be encoded in UTF-8 format.

   -  MAXROW

      Maximum number of lines in an exported file. If the number of lines in an exported file exceeds this value, a new file is generated.

      The value ranges from **1** to **2147483647**.

      .. note::

         This parameter is valid only when **COPY TO** is executed and a valid SERVER is specified. When **HEADER** is set to **true**, **MAXROW** must be greater than 1. This parameter must be specified together with **FILEPREFIX**.

   -  FILEPREFIX

      Specifies the prefix of an export file name.

      Value range: a valid string that cannot start or end with a slash (/)

      .. note::

         This parameter is valid only when **COPY TO** is executed and a valid SERVER is specified. This parameter must be specified together with **MAXROW**.

   -  PRESERVE_BLANKS

      Specifies whether to retain the blank characters (including spaces, \\t, \\v, and \\f) at the end of each column during fixed-length import. This parameter is supported by version 8.2.0.100 or later clusters.

      Value range: **true**, **on**, **false**, and **off** The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on**, the trailing **\\t** is retained and pruning is not performed for column data of the BYTEAOID, CHAROID, NAMEOID, TEXTOID, BPCHAROID, VARCHAROID, NVARCHAR2OID or CSTRINGOID type.
      -  If this parameter is set to **false** or **off**, blank characters (including spaces, \\t, \\v, and \\f) at the end of all data types are pruned.

-  **COPY_OPTION { option_name ' value ' }**

   Specifies all types of native parameters of **COPY**.

   -  OIDS

      Copies the OID for each row.

      .. note::

         An error is raised if OIDs are specified for a table that does not have OIDs, or in the case of copying a query.

   -  NULL null_string

      Specifies the string that represents a null value.

      .. important::

         When using **COPY FROM**, any data item that matches this string will be stored as a **NULL** value, so you should make sure that you use the same string as you used with **COPY TO**.

      Value range:

      -  The null value cannot be **\\r** or **\\n**. The maximum length is 100 characters.
      -  The null value cannot be the same as the delimiter or quote parameter.

      Default value:

      -  **\\N** in TEXT format
      -  An empty string without quotation marks in CSV format

   -  HEADER

      Specifies whether a file contains a header with the names of each column in the file. header is available only for CSV and FIXED files.

      When data is imported, if **header** is **on**, the first row of the data file will be identified as title row and ignored. If header is **off**, the first row is identified as data.

      When data is exported, if **header** is **on**, **fileheader** must be specified. If header is **off**, the exported file does not include a title row.

   -  FILEHEADER

      Specifies a file that defines the content in the header for exported data. The file contains data description of each column.

      .. important::

         -  This parameter is available only when **header** is **on** or **true**.
         -  **fileheader** specifies an absolute path.
         -  The file can contain only one row of header information, and ends with a linefeed. Excess rows will be discarded. (Header information cannot contain linefeeds.)
         -  The length of the file including the linefeed cannot exceed 1 MB.

   -  FREEZE

      Sets the **COPY** loaded data row as **frozen**, like these data have executed **VACUUM FREEZE**.

      This is a performance option of initial data loading. The data will be frozen only when the following three requirements are met:

      -  The table being loaded has been created or truncated in the current subtransaction before copying.
      -  There are no cursors open in the current transaction.
      -  There are no original snapshots in the current transaction.

      .. note::

         When **COPY** is completed, all the other sessions will see the data immediately. This violates the normal rules of MVCC visibility and users should be aware of the potential problems this might cause.

   -  FORCE NOT NULL column_name [, ...]

      Does not match the specified columns' values against the null string. This option is allowed only in **COPY FROM**, and only when using the CSV format.

      Value range: an existing column

   -  FORCE QUOTE { column_name [, ...] \| \* }

      Forces quoting to be used for all non-NULL values in each specified column. This option is allowed only in **COPY TO**, and only when using the CSV format. **NULL** values are not quoted.

      Value range: an existing column

   -  BINARY

      The binary format option causes all data to be stored/read as binary format rather than as text. In binary mode, you cannot declare **DELIMITER**, **NULL**, or **CSV**. After specifying BINARY, CSV, FIXED and TEXT cannot be specified through **option** or **copy_option**.

   -  CSV

      Enables the CSV mode. After CSV is specified, **BINARY**, **FIXED** and **TEXT** cannot be specified through **option** or **copy_option**.

   -  QUOTE [AS] 'quote_character'

      Specifies the quote character for a CSV file.

      Default value: double quotation mark ("")

      .. note::

         -  The quote parameter cannot be the same as the delimiter or null parameter.
         -  The **quote** parameter must be a single one-byte character.
         -  Invisible characters are recommended as **quote** values, such as 0x07, 0x08, and 0x1b.

   -  ESCAPE [AS] 'escape_character'

      This option is allowed only when using CSV format. This must be a single one-byte character.

      The default value is a double quotation mark ("). If it is the same as the value of **quote**, it will be replaced with **\\0**.

   -  EOL 'newline_character'

      Specifies the newline character style of the imported or exported data file.

      Value range: multi-character newline characters within 10 bytes. Common newline characters include **\\r** (0x0D), **\\n** (0x0A), and **\\r\\n** (0x0D0A). Special newline characters include **$** and **#**.

      .. note::

         -  The **EOL** parameter supports only the TEXT format for data import and export. For forward compatibility, the **EOL** parameter can be set to **0x0D** or **0x0D0A** for data export in the CSV and FIXED formats.
         -  The value of the **EOL** parameter cannot be the same as that of **DELIMITER** or **NULL**.
         -  The **EOL** parameter value cannot contain lowercase letters, digits, or dot (.).

   -  ENCODING 'encoding_name'

      Specifies that the file is encoded in the **encoding_name**.

      Value range: a valid encoding format

      Default value: current encoding format of the database

   -  IGNORE_EXTRA_DATA

      When the number of data source files exceeds the number of foreign table columns, excess columns at the end of the row are ignored. This parameter is available only during data importing.

      If you do not use this parameter, and the number of data source files exceeds the number of foreign table columns, the following error information will be displayed:

      ::

         extra data after last expected column

   -  COMPATIBLE_ILLEGAL_CHARS

      Specifies error tolerance for invalid characters during importing. Invalid characters are converted before importing. No error message is displayed. The import is not interrupted. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      If you do not use this parameter, an error occurs when there is an invalid character, and the import stops.

      .. note::

         The rule of error tolerance when you import invalid characters is as follows:

         (1) **\\0** is converted to a space.

         (2) Other invalid characters are converted to question marks.

         (3) Setting **compatible_illegal_chars** to **true/on** enables toleration of invalid characters. If **NULL**, **DELIMITER**, **QUOTE**, and **ESCAPE** are set to spaces or question marks, errors like "illegal chars conversion may confuse COPY escape 0x20" will be displayed to prompt the user to modify parameters that may cause confusion, preventing importing errors.

   -  FILL_MISSING_FIELDS

      Specifies whether to generate an error message when the last column in a row in the source file is lost during data loading.

      Value range: **true**, **on**, **false**, and **off**

      Default value: **false** or **off**

      .. important::

         Do not specify this option. Currently, it does not enable error tolerance, but will make the parser ignore the said errors during data parsing on the CN. Such errors will not be recorded in the COPY error table (enabled using **LOG ERRORS REJECT LIMIT**) but will be reported later by DNs.

   -  DATE_FORMAT 'date_format_string'

      Imports data of the DATE type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: any valid DATE value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

      .. note::

         If ORACLE is specified as the compatible database, the DATE format is TIMESTAMP. For details, see **timestamp_format** below.

   -  TIME_FORMAT 'time_format_string'

      Imports data of the TIME type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: Valid TIME. Time zones cannot be used. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  TIMESTAMP_FORMAT 'timestamp_format_string'

      Specifies the TIMESTAMP format for data import. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: any valid TIMESTAMP value. Time zones are not supported. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  SMALLDATETIME_FORMAT 'smalldatetime_format_string'

      Imports data of the SMALLDATETIME type. The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur. The parameter is valid only for data importing using the **COPY FROM** option.

      Value range: any valid SMALLDATETIME value. For details, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

   -  PRESERVE_BLANKS

      Specifies whether to retain the blank characters (including spaces, \\t, \\v, and \\f) at the end of each column during fixed-length import. This parameter is supported by version 8.2.0.100 or later clusters.

      Value range: **true**, **on**, **false**, and **off** The default value is **false** or **off**.

      -  If this parameter is set to **true** or **on**, the trailing **\\t** is retained and pruning is not performed for column data of the BYTEAOID, CHAROID, NAMEOID, TEXTOID, BPCHAROID, VARCHAROID, NVARCHAR2OID or CSTRINGOID type.
      -  If this parameter is set to **false** or **off**, blank characters (including spaces, \\t, \\v, and \\f) at the end of all data types are pruned.

   The following special backslash sequences are recognized by **COPY FROM**:

   -  **\\b**: Backspace (ASCII 8)
   -  **\\f**: Form feed (ASCII 12)
   -  **\\n**: Newline character (ASCII 10)
   -  **\\r**: Carriage return character (ASCII 13)
   -  **\\t**: Tab (ASCII 9)
   -  **\\v**: Vertical tab (ASCII 11)
   -  **\\digits**: Backslash followed by one to three octal digits specifies the ASCII value is the character with that numeric code.
   -  **\\xdigits**: Backslash followed by an x and one or two hex digits specifies the character with that numeric code.

Examples
--------

Copy data from the **tpcds.ship_mode** file to the **/home/omm/ds_ship_mode.dat** file:

::

   COPY tpcds.ship_mode TO '/home/omm/ds_ship_mode.dat';

Write **tpcds.ship_mode** as output to **stdout**:

::

   COPY tpcds.ship_mode TO stdout;

Create the **tpcds.ship_mode_t1** table:

::

   CREATE TABLE tpcds.ship_mode_t1
   (
       SM_SHIP_MODE_SK           INTEGER               NOT NULL,
       SM_SHIP_MODE_ID           CHAR(16)              NOT NULL,
       SM_TYPE                   CHAR(30)                      ,
       SM_CODE                   CHAR(10)                      ,
       SM_CARRIER                CHAR(20)                      ,
       SM_CONTRACT               CHAR(20)
   )
   WITH (ORIENTATION = COLUMN,COMPRESSION=MIDDLE)
   DISTRIBUTE BY HASH(SM_SHIP_MODE_SK );

Copy data from **stdin** to the **tpcds.ship_mode_t1** table:

::

   COPY tpcds.ship_mode_t1 FROM stdin;

Copy data from the **/home/omm/ds_ship_mode.dat** file to the **tpcds.ship_mode_t1** table:

::

   COPY tpcds.ship_mode_t1 FROM '/home/omm/ds_ship_mode.dat';

Copy data from the **/home/omm/ds_ship_mode.dat** file to the **tpcds.ship_mode_t1** table, with the import format set to TEXT (**format 'text'**), the delimiter set to \\t' (delimiter **E'\\t'**), excessive columns ignored (**ignore_extra_data 'true'**), and characters not escaped (**noescaping 'true'**).

::

   COPY tpcds.ship_mode_t1 FROM '/home/omm/ds_ship_mode.dat' WITH(format 'text', delimiter E'\t', ignore_extra_data 'true', noescaping 'true');

Copy data from the **/home/omm/ds_ship_mode.dat** file to the **tpcds.ship_mode_t1** table, with the import format set to FIXED, fixed-length format specified (**FORMATTER(SM_SHIP_MODE_SK(0, 2), SM_SHIP_MODE_ID(2,16), SM_TYPE(18,30), SM_CODE(50,10), SM_CARRIER(61,20), SM_CONTRACT(82,20))**), excessive columns ignored (**ignore_extra_data**), and headers included (**header**).

::

   COPY tpcds.ship_mode_t1 FROM '/home/omm/ds_ship_mode.dat' FIXED FORMATTER(SM_SHIP_MODE_SK(0, 2), SM_SHIP_MODE_ID(2,16), SM_TYPE(18,30), SM_CODE(50,10), SM_CARRIER(61,20), SM_CONTRACT(82,20)) header ignore_extra_data;

Copy data from the **/home/omm/ds_ship_mode.dat** file to the **tpcds.ship_mode_t1** table, with the import format set to FIXED, fixed-length format specified (**FORMATTER(SM_SHIP_MODE_SK(0, 2), SM_SHIP_MODE_ID(2,16), SM_TYPE(18,30), SM_CODE(50,10), SM_CARRIER(61,20), SM_CONTRACT(82,20))**), excessive columns ignored (**ignore_extra_data**), headers included (**header**), and trailing **\\t** retained.

.. code-block::

   COPY tpcds.ship_mode_t1 FROM '/home/omm/ds_ship_mode.dat' (FORMAT 'fixed', FORMATTER (SM_SHIP_MODE_SK(0,2), SM_SHIP_MODE_ID(2,16), SM_TYPE(18,30), SM_CODE(50,10), SM_CARRIER(61,20), SM_CONTRACT(82,20)), PRESERVE_BLANKS'true', HEADER 'true', IGNORE_EXTRA_DATA 'true');

Export **tpcds.ship_mode_t1** as a text file **ds_ship_mode.dat** in the OBS directory **/bucket/path/**. You need to specify the server options that contain OBS access information:

::

   COPY tpcds.ship_mode_t1 TO '/bucket/path/ds_ship_mode.dat' WITH (format 'text', encoding 'utf8', server 'obs_server');

Export **tpcds.ship_mode_t1** as a CSV file in the OBS directory **/bucket/path/**. You need to specify the server options that contain OBS access information. The file contains the title line and BOM header. A single file can contain a maximum of 1000 lines. If the number of lines exceeds 1000, a new file is generated. The user-defined file name prefix is **justprefix**:

::

   COPY (select * from tpcds.ship_mode_t1 where SM_SHIP_MODE_SK=1060) TO '/bucket/path/' WITH (format 'csv', header 'on', encoding 'utf8', server 'obs_server', bom 'on', maxrow '1000', fileprefix 'justprefix');

Delete the **tpcds.ship_mode_t1** table:

::

   DROP TABLE tpcds.ship_mode_t1;
