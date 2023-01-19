:original_name: dws_04_0630.html

.. _dws_04_0630:

PG_USER_MAPPING
===============

**PG_USER_MAPPING** records the mappings from local users to remote.

It is accessible only to users with system administrator rights. You can use view :ref:`PG_USER_MAPPINGS <dws_04_0792>` to query common users.

.. table:: **Table 1** PG_USER_MAPPING columns

   +-----------+--------+--------------------------------------------+---------------------------------------------------------------------+
   | Name      | Type   | Reference                                  | Description                                                         |
   +===========+========+============================================+=====================================================================+
   | oid       | oid    | ``-``                                      | Row identifier (hidden attribute; must be explicitly selected)      |
   +-----------+--------+--------------------------------------------+---------------------------------------------------------------------+
   | umuser    | oid    | :ref:`PG_AUTHID <dws_04_0574>`.oid         | OID of the local role being mapped, 0 if the user mapping is public |
   +-----------+--------+--------------------------------------------+---------------------------------------------------------------------+
   | umserver  | oid    | :ref:`PG_FOREIGN_SERVER <dws_04_0592>`.oid | OID of the foreign server that contains this mapping                |
   +-----------+--------+--------------------------------------------+---------------------------------------------------------------------+
   | umoptions | text[] | ``-``                                      | Option used for user mapping. It is a keyword=value string.         |
   +-----------+--------+--------------------------------------------+---------------------------------------------------------------------+
