:original_name: dws_04_0732.html

.. _dws_04_0732:

PG_INDEXES
==========

**PG_INDEXES** displays access to useful information about each index in the database.

.. table:: **Table 1** PG_INDEXES columns

   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | Column     | Type | Reference                                  | Description                                                 |
   +============+======+============================================+=============================================================+
   | schemaname | Name | :ref:`PG_NAMESPACE <dws_04_0600>`.nspname  | Name of the schema that contains tables and indexes         |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | tablename  | Name | :ref:`PG_CLASS <dws_04_0578>`.relname      | Name of the table for which the index serves                |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | indexname  | Name | :ref:`PG_CLASS <dws_04_0578>`.relname      | Index name                                                  |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | tablespace | Name | :ref:`PG_TABLESPACE <dws_04_0622>`.spcname | Name of the tablespace that contains the index              |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | indexdef   | Text | N/A                                        | Index definition (a reconstructed **CREATE INDEX** command) |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+

Example
-------

Query the index information about a specified table.

::

   SELECT * FROM pg_indexes WHERE tablename = 'mytable';
    schemaname | tablename |   indexname    | tablespace |                                   indexdef
   ------------+-----------+----------------+------------+-------------------------------------------------------------------------------
    public     | mytable   | idx_mytable_id |            | CREATE INDEX idx_mytable_id ON mytable USING btree (id) TABLESPACE pg_default
   (1 row)

Query information about indexes of all tables in a specified schema in the current database.

::

   SELECT tablename, indexname, indexdef FROM pg_indexes WHERE schemaname = 'public' ORDER BY tablename,indexname;
    tablename |     indexname      |                                              indexdef
   -----------+--------------------+-----------------------------------------------------------------------------------------------------
    books     | books_pkey         | CREATE UNIQUE INDEX books_pkey ON books USING btree (id) TABLESPACE pg_default
    books     | idx_books_tags_gin | CREATE INDEX idx_books_tags_gin ON books USING gin (tags) TABLESPACE pg_default
    customer  | c_custkey_key      | CREATE UNIQUE INDEX c_custkey_key ON customer USING btree (c_custkey, c_name) TABLESPACE pg_default
    mytable   | idx_mytable_id     | CREATE INDEX idx_mytable_id ON mytable USING btree (id) TABLESPACE pg_default
    test1     | idx_test_id        | CREATE INDEX idx_test_id ON test1 USING btree (id) TABLESPACE pg_default
    v0        | v0_pkey            | CREATE UNIQUE INDEX v0_pkey ON v0 USING btree (c) TABLESPACE pg_default
   (6 rows)
