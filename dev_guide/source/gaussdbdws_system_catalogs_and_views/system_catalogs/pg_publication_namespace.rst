:original_name: dws_04_1043.html

.. _dws_04_1043:

PG_PUBLICATION_NAMESPACE
========================

**PG_PUBLICATION_NAMESPACE** records the mapping between publications and schemas in the current database, which is a many-to-many mapping. This system catalog is supported only by clusters of version 8.2.0.100 or later.

.. table:: **Table 1** PG_PUBLICATION_NAMESPACE columns

   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+
   | Name    | Type | Reference                               | Description                                                                |
   +=========+======+=========================================+============================================================================+
   | OID     | OID  | ``-``                                   | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+
   | prpubid | OID  | :ref:`PG_PUBLICATION <dws_04_1040>`.oid | Publication OID in the mapping                                             |
   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+
   | pnnspid | OID  | :ref:`PG_NAMESPACE <dws_04_0600>`.oid   | Schema OID in the mapping                                                  |
   +---------+------+-----------------------------------------+----------------------------------------------------------------------------+

Examples
--------

View all mappings between publications and schemas.

::

   SELECT * FROM pg_publication_namespace;
    pnpubid | pnnspid
   ---------+---------
      16797 |   16796
   (1 row)
