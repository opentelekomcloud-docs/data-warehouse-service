:original_name: dws_06_0374.html

.. _dws_06_0374:

EXTERNAL SCHEMA System Functions
================================

System functions related to EXTERNAL SCHEMA are supported only by clusters of version 8.3.0 or later.

pg_get_external_schema_table_list(schema_name text)
---------------------------------------------------

Description: Obtains the full table name information of an external schema. Only LakeFormation is supported.

Input parameter: **schema_name** indicates the name of the external schema.

**Return type**: SETOF record

Example:

::

   SELECT * FROM pg_get_external_schema_table_list('ex_lf');
    database_name |           table_name
   ---------------+---------------------------------
    dws-ci-hlt-db | lf_hms_lineitem_parq
    dws-ci-hlt-db | lf_hms_lineitem_parq
    dws-ci-hlt-db | lf_hms_part_parq
    dws-ci-hlt-db | lf_hms_part_parq
    dws-ci-hlt-db | lf_hms_supplier_parq
    dws-ci-hlt-db | lf_hms_partsupp_parq
    dws-ci-hlt-db | lf_hms_partsupp_parq
    dws-ci-hlt-db | lf_hms_nation_parq
    dws-ci-hlt-db | lf_hms_region_parq
    dws-ci-hlt-db | lf_hms_customer_parq
   (10 rows)

pg_get_external_schema_table_options(schema_name text, table_name text)
-----------------------------------------------------------------------

Description: Obtains the attribute information of an external schema table.

Input parameters: **schema_name** indicates the name of the external schema, and **table_name** indicates the table name.

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

pg_get_external_schema_table_col(schema_name text, table_name text)
-------------------------------------------------------------------

**Description**: Obtains the column information of an external schema table.

Input parameters: **schema_name** indicates the name of the external schema, and **table_name** indicates the table name.

Return type: SETOF record

Example:

::

   SELECT * FROM pg_get_external_schema_table_col('ex_lf', 'test_lf');
           col_name        |   col_type    | part_col
   ------------------------+---------------+----------
    field_smallint         | smallint      | f
    field_int              | int           | f
    field_integer          | int           | f
    field_bigint           | bigint        | f
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
