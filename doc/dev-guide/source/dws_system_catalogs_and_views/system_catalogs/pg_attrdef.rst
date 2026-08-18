:original_name: dws_04_0572.html

.. _dws_04_0572:

PG_ATTRDEF
==========

**PG_ATTRDEF** stores default values of columns.

.. table:: **Table 1** PG_ATTRDEF columns

   +-----------------+--------------+---------------------------------------------------------------------------+
   | Column          | Type         | Description                                                               |
   +=================+==============+===========================================================================+
   | adrelid         | OID          | Table to which the column belongs                                         |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adnum           | Smallint     | Column No.                                                                |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adbin           | pg_node_tree | Internal representation of the column's default value                     |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adsrc           | Text         | Internal representation of the human-readable default value               |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adbin_on_update | pg_node_tree | Internal representation of the value of **on_update_expr**                |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adsrc_on_update | Text         | Internal representation of the human-readable value of **on_update_expr** |
   +-----------------+--------------+---------------------------------------------------------------------------+
