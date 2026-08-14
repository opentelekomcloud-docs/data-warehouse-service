:original_name: dws_04_0621.html

.. _dws_04_0621:

PG_SYNONYM
==========

**PG_SYNONYM** records the mapping between synonym object names and other database object names.

.. table:: **Table 1** **PG_SYNONYM** columns

   +--------------+------+-----------------------------------------------------------------+
   | Column       | Type | Description                                                     |
   +==============+======+=================================================================+
   | synname      | Name | Synonym name.                                                   |
   +--------------+------+-----------------------------------------------------------------+
   | synnamespace | OID  | OID of the namespace where the synonym is located.              |
   +--------------+------+-----------------------------------------------------------------+
   | synowner     | OID  | Owner of a synonym, usually the OID of the user who created it. |
   +--------------+------+-----------------------------------------------------------------+
   | synobjschema | Name | Schema name specified by the associated object.                 |
   +--------------+------+-----------------------------------------------------------------+
   | synobjname   | Name | Name of the associated object.                                  |
   +--------------+------+-----------------------------------------------------------------+
