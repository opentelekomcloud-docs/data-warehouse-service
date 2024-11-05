:original_name: dws_04_0621.html

.. _dws_04_0621:

PG_SYNONYM
==========

**PG_SYNONYM** records the mapping between synonym object names and other database object names.

.. table:: **Table 1** **PG_SYNONYM** columns

   +--------------+------+-----------------------------------------------------------------+
   | Name         | Type | Description                                                     |
   +==============+======+=================================================================+
   | synname      | name | Synonym name.                                                   |
   +--------------+------+-----------------------------------------------------------------+
   | synnamespace | oid  | OID of the namespace where the synonym is located.              |
   +--------------+------+-----------------------------------------------------------------+
   | synowner     | oid  | Owner of a synonym, usually the OID of the user who created it. |
   +--------------+------+-----------------------------------------------------------------+
   | synobjschema | name | Schema name specified by the associated object.                 |
   +--------------+------+-----------------------------------------------------------------+
   | synobjname   | name | Name of the associated object.                                  |
   +--------------+------+-----------------------------------------------------------------+
