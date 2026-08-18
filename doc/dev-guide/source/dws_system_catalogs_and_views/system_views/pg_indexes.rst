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

   SELECT * FROM pg_indexes WHERE tablename = 'user_info';

|image1|

Query information about indexes of all tables in a specified schema in the current database.

::

   SELECT tablename, indexname, indexdef FROM pg_indexes WHERE schemaname = 'public' ORDER BY tablename,indexname;

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002622009099.png
.. |image2| image:: /_static/images/en-us_image_0000002591529644.png
