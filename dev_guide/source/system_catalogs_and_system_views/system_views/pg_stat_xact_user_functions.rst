:original_name: dws_04_0772.html

.. _dws_04_0772:

PG_STAT_XACT_USER_FUNCTIONS
===========================

**PG_STAT_XACT_USER_FUNCTIONS** displays statistics about function execution.

.. table:: **Table 1** PG_STAT_XACT_USER_FUNCTIONS columns

   +------------+------------------+----------------------------------------------------------------------------------+
   | Name       | Type             | Description                                                                      |
   +============+==================+==================================================================================+
   | funcid     | oid              | Function OID                                                                     |
   +------------+------------------+----------------------------------------------------------------------------------+
   | schemaname | name             | Schema name                                                                      |
   +------------+------------------+----------------------------------------------------------------------------------+
   | funcname   | name             | Name of the function                                                             |
   +------------+------------------+----------------------------------------------------------------------------------+
   | calls      | bigint           | Number of times this function has been called                                    |
   +------------+------------------+----------------------------------------------------------------------------------+
   | total_time | double precision | Total time spent in this function and all other functions called by it           |
   +------------+------------------+----------------------------------------------------------------------------------+
   | self_time  | double precision | Total time spent in this function itself, excluding other functions called by it |
   +------------+------------------+----------------------------------------------------------------------------------+
