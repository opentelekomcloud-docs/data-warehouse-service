:original_name: dws_04_0579.html

.. _dws_04_0579:

PG_COLLATION
============

**PG_COLLATION** records the available collations, which are essentially mappings from an SQL name to operating system locale categories.

.. table:: **Table 1** PG_COLLATION columns

   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | Name            | Type            | Reference                             | Description                                                                                               |
   +=================+=================+=======================================+===========================================================================================================+
   | oid             | oid             | ``-``                                 | Row identifier (hidden attribute; must be explicitly selected)                                            |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | collname        | name            | ``-``                                 | Collation name (unique per namespace and encoding)                                                        |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | collnamespace   | oid             | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | OID of the namespace that contains this collation                                                         |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | collowner       | oid             | :ref:`PG_AUTHID <dws_04_0574>`.oid    | Owner of the collation                                                                                    |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | collencoding    | integer         | ``-``                                 | Encoding in which the collation is applicable, or **-1** if it works for any encoding                     |
   |                 |                 |                                       |                                                                                                           |
   |                 |                 |                                       | .. note::                                                                                                 |
   |                 |                 |                                       |                                                                                                           |
   |                 |                 |                                       |    You can use the **pg_encoding_to_char()** function to convert a number to the corresponding code name. |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | collcollate     | name            | ``-``                                 | **LC_COLLATE** for this collation object                                                                  |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
   | collctype       | name            | ``-``                                 | **LC_CTYPE** for this collation object                                                                    |
   +-----------------+-----------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------+
