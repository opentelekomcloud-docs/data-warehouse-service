:original_name: dws_06_0340.html

.. _dws_06_0340:

Schema Visibility Inquiry Functions
===================================


Schema Visibility Inquiry Functions
-----------------------------------

Schema visibility inquiry functions perform visibility checks on database objects. For functions and operators, an object in the search path is visible if there is no object of the same name and argument data type(s) earlier in the path. For operator classes, both name and associated index access method are considered.

All these functions require OIDs to identify the objects to be checked. If you want to test an object by name, it is convenient to use the OID alias types (**regclass**, **regtype**, **regprocedure**, **regoperator**, **regconfig**, or **regdictionary**).

For example, a table is said to be visible if its containing schema is in the search path and no table of the same name appears earlier in the search path. This is equivalent to the statement that the table can be referenced by name without explicit schema qualification. For example, to list the names of all visible tables:

::

   SELECT relname FROM pg_class WHERE pg_table_is_visible(oid);

pg_collation_is_visible(collation_oid)
--------------------------------------

Description: Queries whether the collation is visible in search path.

Return type: boolean

pg_conversion_is_visible(conversion_oid)
----------------------------------------

Description: Queries whether the conversion is visible in search path.

Return type: boolean

pg_function_is_visible(function_oid)
------------------------------------

Description: Queries whether the function is visible in search path.

Return type: boolean

pg_opclass_is_visible(opclass_oid)
----------------------------------

Description: Queries whether the operator class is visible in search path.

Return type: boolean

pg_operator_is_visible(operator_oid)
------------------------------------

Description: Queries whether the operator is visible in search path.

Return type: boolean

pg_opfamily_is_visible(opclass_oid)
-----------------------------------

Description: Queries whether the operator family is visible in search path.

Return type: boolean

pg_table_is_visible(table_oid)
------------------------------

Description: Queries whether the table is visible in search path.

Return type: boolean

pg_ts_config_is_visible(config_oid)
-----------------------------------

Description: Queries whether the text search configuration is visible in search path.

Return type: boolean

pg_ts_dict_is_visible(dict_oid)
-------------------------------

Description: Queries whether the text search dictionary is visible in search path.

Return type: boolean

pg_ts_parser_is_visible(parser_oid)
-----------------------------------

Description: Queries whether the text search parser is visible in search path.

Return type: boolean

pg_ts_template_is_visible(template_oid)
---------------------------------------

Description: Queries whether the text search template is visible in search path.

Return type: boolean

pg_type_is_visible(type_oid)
----------------------------

Description: Queries whether the type (or domain) is visible in search path.

Return type: boolean
