:original_name: dws_04_0732.html

.. _dws_04_0732:

PG_INDEXES
==========

**PG_INDEXES** displays access to useful information about each index in the database.

.. table:: **Table 1** PG_INDEXES columns

   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | Name       | Type | Reference                                  | Description                                                 |
   +============+======+============================================+=============================================================+
   | schemaname | name | :ref:`PG_NAMESPACE <dws_04_0600>`.nspname  | Name of the schema that contains tables and indexes         |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | tablename  | name | :ref:`PG_CLASS <dws_04_0578>`.relname      | Name of the table for which the index serves                |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | indexname  | name | :ref:`PG_CLASS <dws_04_0578>`.relname      | Index name                                                  |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | tablespace | name | :ref:`PG_TABLESPACE <dws_04_0622>`.spcname | Name of the tablespace that contains the index              |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
   | indexdef   | text | ``-``                                      | Index definition (a reconstructed **CREATE INDEX** command) |
   +------------+------+--------------------------------------------+-------------------------------------------------------------+
