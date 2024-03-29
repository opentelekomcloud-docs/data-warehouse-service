:original_name: dws_04_0874.html

.. _dws_04_0874:

USER_TAB_COLUMNS
================

**USER_TAB_COLUMNS** displays information about table columns accessible to the current user.

.. table:: **Table 1** USER_TAB_COLUMNS columns

   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | Name           | Type                   | Description                                                                                                  |
   +================+========================+==============================================================================================================+
   | owner          | character varying(64)  | Table owner                                                                                                  |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | table_name     | character varying(64)  | Table name                                                                                                   |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | column_name    | character varying(64)  | Column name                                                                                                  |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | data_type      | character varying(128) | Data type of the column                                                                                      |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | column_id      | integer                | Sequence number of the column when the table is created                                                      |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | data_length    | integer                | Length of the column in the unit of bytes                                                                    |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | comments       | text                   | Comments                                                                                                     |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | avg_col_len    | numeric                | Average length of a column in the unit of bytes                                                              |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | nullable       | bpchar                 | Whether the column can be empty. For the primary key constraint and non-null constraint, the value is **n**. |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | data_precision | integer                | Precision of the data type. This parameter is valid for the numeric data type and **NULL** for other types.  |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | data_scale     | integer                | Number of decimal places. This parameter is valid for the numeric data type and **0** for other types.       |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
   | char_length    | numeric                | Column length in the unit of bytes which is valid only for the varchar, nvarchar2, bpchar, and char types.   |
   +----------------+------------------------+--------------------------------------------------------------------------------------------------------------+
