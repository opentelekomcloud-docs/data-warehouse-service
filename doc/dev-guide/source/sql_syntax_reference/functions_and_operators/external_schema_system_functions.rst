:original_name: dws_06_0374.html

.. _dws_06_0374:

EXTERNAL SCHEMA System Functions
================================

System functions related to EXTERNAL SCHEMA are supported only by clusters of version 8.3.0 or later.

pg_get_external_schema_table_options(text, text)
------------------------------------------------

**Description**: Obtains the options of an external schema table.

**Input parameter**: The first input parameter is the external schema name, and the second input parameter is the table name.

**Return type**: SETOF record

Example:

::

   SELECT * FROM pg_get_external_schema_table_options('ex_lf', 'test_lf');
    option_name |            option_value
   -------------+------------------------------------
    encoding    | utf8
    format      | parquet
    foldername  | /***/***/***
   (3 rows)

pg_get_external_schema_table_col(text, text)
--------------------------------------------

**Description**: Obtains the column information of an external schema table.

**Input parameter**: The first input parameter is the external schema name, and the second input parameter is the table name.

Return type: SETOF record

Example:

::

   SELECT * FROM pg_get_external_schema_table_col('ex_lf', 'test_lf');
           col_name        |   col_type    | part_col
   ------------------------+---------------+----------
    field_smallint         | smallint      | f
    field_int              | int           | f
    field_integer          | int           | f
    fileld_bigint          | bigint        | f
    field_float            | float         | f
    field_double           | double        | f
    field_double_precision | double        | f
    field_decimal          | decimal(10,0) | f
    field_numeric          | decimal(10,0) | f
    field_timestamp        | timestamp     | f
    field_date             | date          | f
    field_varchar          | varchar(5)    | f
    field_char             | char(5)       | f
    field_boolean          | boolean       | f
    field_string           | string        | f
   (15 rows)
