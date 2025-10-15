:original_name: dws_04_0793.html

.. _dws_04_0793:

PG_VIEWS
========

**PG_VIEWS** displays basic information about each view in the database.

.. table:: **Table 1** PG_VIEWS columns

   +------------+------+-------------------------------------------+-------------------------------------------+
   | Column     | Type | Reference                                 | Description                               |
   +============+======+===========================================+===========================================+
   | schemaname | Name | :ref:`PG_NAMESPACE <dws_04_0600>`.nspname | Name of the schema that contains the view |
   +------------+------+-------------------------------------------+-------------------------------------------+
   | viewname   | Name | :ref:`PG_CLASS <dws_04_0578>`.relname     | View name                                 |
   +------------+------+-------------------------------------------+-------------------------------------------+
   | viewowner  | Name | :ref:`PG_AUTHID <dws_04_0574>`.rolname    | Owner of the view                         |
   +------------+------+-------------------------------------------+-------------------------------------------+
   | definition | Text | ``-``                                     | Definition of the view                    |
   +------------+------+-------------------------------------------+-------------------------------------------+

Example
-------

Query all the views in a specified schema.

::

   SELECT * FROM pg_views WHERE schemaname = 'myschema';
    schemaname | viewname | viewowner |                                    definition
   ------------+----------+-----------+----------------------------------------------------------------------------------
    myschema   | myview   | dbadmin   | SELECT  * FROM pg_tablespace WHERE (pg_tablespace.spcname = 'pg_default'::name);
    myschema   | v1       | dbadmin   | SELECT  * FROM t1 WHERE (t1.c1 > 200);
   (2 rows)
