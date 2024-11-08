:original_name: dws_04_0572.html

.. _dws_04_0572:

PG_ATTRDEF
==========

**PG_ATTRDEF** stores default values of columns.

.. table:: **Table 1** PG_ATTRDEF columns

   +-----------------+--------------+---------------------------------------------------------------------------+
   | Name            | Type         | Description                                                               |
   +=================+==============+===========================================================================+
   | adrelid         | oid          | Table to which the column belongs                                         |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adnum           | smallint     | Number of the column                                                      |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adbin           | pg_node_tree | Internal representation of the default value of the column                |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adsrc           | text         | Internal representation of the readable default value                     |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adbin_on_update | pg_node_tree | Internal representation of the value of **on_update_expr**                |
   +-----------------+--------------+---------------------------------------------------------------------------+
   | adsrc_on_update | text         | Internal representation of the human-readable value of **on_update_expr** |
   +-----------------+--------------+---------------------------------------------------------------------------+
