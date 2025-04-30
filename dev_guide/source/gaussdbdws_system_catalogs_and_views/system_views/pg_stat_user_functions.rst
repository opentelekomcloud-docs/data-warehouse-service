:original_name: dws_04_0764.html

.. _dws_04_0764:

PG_STAT_USER_FUNCTIONS
======================

**PG_STAT_USER_FUNCTIONS** displays user-defined function status information in the namespace. (The language of the function is non-internal language.)

.. table:: **Table 1** PG_STAT_USER_FUNCTIONS columns

   +------------+------------------+----------------------------------------------------------------------------------+
   | Column     | Type             | Description                                                                      |
   +============+==================+==================================================================================+
   | funcid     | OID              | Function OID                                                                     |
   +------------+------------------+----------------------------------------------------------------------------------+
   | schemaname | Name             | Schema name                                                                      |
   +------------+------------------+----------------------------------------------------------------------------------+
   | funcname   | Name             | Name of the function                                                             |
   +------------+------------------+----------------------------------------------------------------------------------+
   | calls      | Bigint           | Number of times this function has been called                                    |
   +------------+------------------+----------------------------------------------------------------------------------+
   | total_time | Double precision | Total time spent in this function and all other functions called by it           |
   +------------+------------------+----------------------------------------------------------------------------------+
   | self_time  | Double precision | Total time spent in this function itself, excluding other functions called by it |
   +------------+------------------+----------------------------------------------------------------------------------+
