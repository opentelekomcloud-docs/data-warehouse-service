:original_name: dws_04_0617.html

.. _dws_04_0617:

PG_SHDESCRIPTION
================

**PG_SHDESCRIPTION** records optional comments for shared database objects. Descriptions can be manipulated with the **COMMENT** command and viewed with psql's **\\d** commands.

See also **PG_DESCRIPTION**, which performs a similar function for descriptions involving objects within a single database.

Unlike most system catalogs, **PG_SHDESCRIPTION** is shared across all databases of a cluster. There is only one copy of **PG_SHDESCRIPTION** per cluster, not one per database.

.. table:: **Table 1** PG_SHDESCRIPTION columns

   +-------------+------+-----------------------------------+--------------------------------------------------------------+
   | Name        | Type | Reference                         | Description                                                  |
   +=============+======+===================================+==============================================================+
   | objoid      | oid  | Any OID column                    | OID of the object this description pertains to               |
   +-------------+------+-----------------------------------+--------------------------------------------------------------+
   | classoid    | oid  | :ref:`PG_CLASS <dws_04_0578>`.oid | OID of the system catalog where the object resides           |
   +-------------+------+-----------------------------------+--------------------------------------------------------------+
   | description | text | ``-``                             | Arbitrary text that serves as the description of this object |
   +-------------+------+-----------------------------------+--------------------------------------------------------------+
