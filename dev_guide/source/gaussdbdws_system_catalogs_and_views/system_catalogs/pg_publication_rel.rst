:original_name: dws_04_1041.html

.. _dws_04_1041:

PG_PUBLICATION_REL
==================

**PG_PUBLICATION_REL** records the mapping between publications and tables in the current database, which is a many-to-many mapping. This system catalog is supported only by clusters of version 8.2.0.100 or later.

.. note::

   To check detailed information, you are advised to use the :ref:`PG_PUBLICATION_TABLES <dws_04_1045>` view.

.. table:: **Table 1** PG_PUBLICATION_REL columns

   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+
   | Name    | Type | Reference                               | Description                                                                |
   +=========+======+=========================================+============================================================================+
   | oid     | oid  | ``-``                                   | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+
   | prpubid | oid  | :ref:`PG_PUBLICATION <dws_04_1040>`.oid | Publication OID in the mapping                                             |
   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+
   | prrelid | oid  | :ref:`PG_CLASS <dws_04_0578>`.oid       | OID of the mapped table                                                    |
   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+

Examples
--------

View all mappings between publications and tables.

::

   postgres=# SELECT * FROM pg_publication_rel;
    prpubid | prrelid
   ---------+---------
      16797 |   16757
      16797 |   16776
   (2 rows)
