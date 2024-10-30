:original_name: dws_04_1045.html

.. _dws_04_1045:

PG_PUBLICATION_TABLES
=====================

**PG_PUBLICATION_TABLES** displays the mapping between a publication and its published tables. Unlike the underlying system catalog :ref:`PG_PUBLICATION_REL <dws_04_1041>`, this view expands the publications defined as **FOR ALL TABLES** and **FOR ALL TABLES IN SCHEMA**, in which each publishable table has a row. This view is supported only by clusters of version 8.2.0.100 or later.

.. table:: **Table 1** PG_PUBLICATION_TABLES columns

   ========== ==== =============================
   Name       Type Description
   ========== ==== =============================
   pubname    name Publication name
   schemaname name Name of the schema of a table
   tablename  name Table name
   ========== ==== =============================

Examples
--------

Query all published tables.

::

   SELECT * FROM PG_PUBLICATION_TABLES;
    pubname | schemaname | tablename
   ---------+------------+-----------
    mypub   | public     | t1
    mypub   | public     | t2
   (2 rows)
