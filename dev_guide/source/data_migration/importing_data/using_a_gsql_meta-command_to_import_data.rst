:original_name: dws_04_0208.html

.. _dws_04_0208:

.. _en-us_topic_0000001717097328:

Using a gsql Meta-Command to Import Data
========================================

The **gsql** tool of GaussDB(DWS) provides the **\\copy** meta-command to import data.

\\copy Command
--------------

For details about the **\\copy** command, see :ref:`Table 1 <en-us_topic_0000001717097328__en-us_topic_0000001233563247_t3ed8c97d36d74d0da906d0f028ff707c>`.

.. _en-us_topic_0000001717097328__en-us_topic_0000001233563247_t3ed8c97d36d74d0da906d0f028ff707c:

.. table:: **Table 1** \\copy meta-command

   +-----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Syntax                                        | Description                                                                                                                                                                                                                                                                                                                                |
   +===============================================+============================================================================================================================================================================================================================================================================================================================================+
   | \\copy { table [ ( column_list ) ] \|         | You can run this command to import or export data after logging in to the database on any gsql client. Different from the **COPY** statement in SQL, this command performs read/write operations on local files rather than files on database servers. The accessibility and permissions of the local files are restricted to local users. |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | ( query ) } { from \| to } { filename \|      | .. note::                                                                                                                                                                                                                                                                                                                                  |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | stdin \| stdout \| pstdin \| pstdout }        |    **\\copy** only applies to small-batch data import with uniform formats but poor error tolerance capability. GDS or **COPY** is preferred for data import.                                                                                                                                                                              |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | [ with ] [ binary ] [ oids ] [ delimiter      |                                                                                                                                                                                                                                                                                                                                            |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | [ as ] 'character' ] [ null [ as ] 'string' ] |                                                                                                                                                                                                                                                                                                                                            |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | [ csv [ header ] [ quote [ as ]               |                                                                                                                                                                                                                                                                                                                                            |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | 'character' ] [ escape [ as ] 'character' ]   |                                                                                                                                                                                                                                                                                                                                            |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | [ force quote column_list \| \* ] [ force     |                                                                                                                                                                                                                                                                                                                                            |
   |                                               |                                                                                                                                                                                                                                                                                                                                            |
   | not null column_list ] ]                      |                                                                                                                                                                                                                                                                                                                                            |
   +-----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Parameter Description
---------------------

-  table

   Specifies the name (possibly schema-qualified) of an existing table.

   Value range: an existing table name

-  column_list

   Specifies an optional list of columns to be copied.

   Value range: any field in the table. If the column list is not specified, all columns in the table will be copied.

-  query

   Specifies that the results will be copied.

   Valid value: a **SELECT** or **VALUES** command in parentheses.

-  filename

   Specifies the absolute path of a file. To run the **\\copy** command, the user must have the write permission for this path.

-  stdin

   Specifies that input comes from the client application.

-  stdout

   Specifies that output goes to the client application.

-  pstdin

   Specifies that input comes from the gsql client.

-  pstout

-  Specifies that output goes to the gsql client.

-  binary

   Specifies that data is stored and read in binary mode instead of text mode. In binary mode, you cannot declare **DELIMITER**, **NULL**, or **CSV**. After specifying BINARY, CSV, FIXED and TEXT cannot be specified through **option** or **copy_option**.

-  oid

   Specifies the internal OID to be copied for each row.

   .. note::

      An error is raised if OIDs are specified for a table that does not have OIDs, or in the case of copying a query.

   Valid value: **true**, **on**, **false**, and **off**.

   Default value: **false/off**

-  delimiter [ as ] 'character'

   Specifies the character that separates columns within each row (line) of the file.

   .. note::

      -  A delimiter cannot be **\\r** or **\\n**.
      -  A delimiter cannot be the same as the **null** value. The delimiter of CSV data cannot be same as the **quote** value.
      -  The delimiter of TEXT data cannot contain any of the following characters: \\.abcdefghijklmnopqrstuvwxyz0123456789
      -  The data length of a single row should be less than 1 GB. A row that has many columns using long delimiters cannot contain much valid data.
      -  You are advised to use multi-characters and invisible characters for delimiters. For example, you can use the multiple-character delimiter "$^&" and invisible delimiters, such as E'\\x07', E'\\x08', and E'\\x1b'.

   Value range: a multi-character delimiter within 10 bytes.

   Default value:

   -  A tab character in TEXT format
   -  A comma (,) in CSV format
   -  No delimiter in FIXED format

-  null [ as ] 'string'

   Specifies that a string represents a null value in a data file.

   Value range:

   -  A null value cannot be **\\r** or **\\n**. The maximum length is 100 characters.
   -  A null value cannot be the same as the **delimiter** or **quote** value.

   Default value:

   -  An empty string without quotation marks in CSV format
   -  **\\N** in TEXT format

-  header

   Specifies whether a data file contains a table header. **header** is available only for CSV and FIXED files.

   In data import scenarios, if **header** is **on**, the first row of the data file will be identified as the header and ignored. If **header** is **off**, the first row will be identified as a data row.

   If header is **on**, **fileheader** must be specified. **fileheader** specifies the content in the header. If header is **off**, the exported file does not contain a header.

   Valid value: **true**, **on**, **false**, and **off**.

   Default value: **false/off**

-  quote [ as ] 'character'

   Specifies the quote character used when a data value is referenced in a CSV file.

   Default value: double quotation mark ("").

   .. note::

      -  The **quote** value cannot be the same as the **delimiter** or **null** value.
      -  The **quote** value must be a single-byte character.
      -  You are advised to use invisible characters as quotes, for example, E'\\x07', E'\\x08', and E'\\x1b'.

-  escape [ as ] 'character'

   This option is allowed only when using CSV format. This must be a single one-byte character.

   Default value: double quotation mark (""). If the value is the same as the **quote** value, it will be replaced with **\\0**.

-  force quote column_list \| \*

   Quotes all not-null values in each declared column when **CSV COPY TO** is used. NULL values will not be quoted.

   Value range: an existing column.

-  force not null column_list

   In CSV COPY FROM mode, processes each specified column as though it were quoted and hence not a null value.

   Value range: an existing column.

Example
-------

Create the target table **copy_example**.

::

   create table copy_example
   (
       col_1 integer,
       col_2 text,
       col_3 varchar(12),
       col_4 date,
       col_5 time
   );

-  Example 1: Copy data from **stdin** to the target table **copy_example**.

   ::

      \copy copy_example from stdin csv;

   When you see the **>>** characters, you can start entering data. To finish your input, type a backslash and a period (\\.).

   ::

      Enter data to be copied followed by a newline.
      End with a backslash and a period on a line by itself.
      >> 1,"iamtext","iamvarchar",2006-07-07,12:00:00
      >> \.

-  Example 2: The **example.csv** file is in the local directory **/local/data/** and the file contains the header line. (|) is used as the delimiter, and the double quotation marks are used for **quote**. The content is as follows:

   iamheader

   ``1|"iamtext"|"iamvarchar"|2006-07-07|12:00:00``

   ``2|"iamtext"|"iamvarchar"|2022-07-07|19:00:02``

   Import data from the local file **example.csv** to the target table **copy_example**. If the header option is **on**, the first row is automatically ignored. By default, quotation marks are used for **quote**.

   ::

      \copy copy_example from  '/local/data/example.csv' with(header 'on', format 'csv', delimiter '|', date_format 'yyyy-mm-dd',  time_format 'hh24:mi:ss');

-  Example 3: The **example.csv** file is in the local directory **/local/data/**. The comma (,) is used as the delimiter, and the quotation mark (") is used for **quote**. The last field is missing in the first line, and one more field is added in the second line. The content is as follows:

   1,"iamtext","iamvarchar",2006-07-07

   2,"iamtext","iamvarchar",2022-07-07,19:00:02,12:00:00

   To import data from the local file **example.csv** to the target table **copy_example**, you don't need to specify the delimiter since the default delimiter is (,). Additionally, the fault tolerance parameters **IGNORE_EXTRA_DATA** and **FILL_MISSING_FIELDS** are set, which means that missing fields will be replaced with **NULL** and extra fields will be ignored.

   ::

      \copy copy_example from  '/local/data/example.csv' with( format 'csv', date_format 'yyyy-mm-dd',  time_format 'hh24:mi:ss', IGNORE_EXTRA_DATA 'true', FILL_MISSING_FIELDS 'true');

-  Example 4: Export the content of the **copy_example** table to **stdout** in CSV format, use double quotation marks as for **quote**, and use quotes to enclose the fourth and fifth columns.

   ::

      \copy copy_example to stdout CSV quote as '"' force quote col_4,col_5;
