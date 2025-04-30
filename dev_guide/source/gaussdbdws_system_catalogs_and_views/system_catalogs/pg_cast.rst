:original_name: dws_04_0577.html

.. _dws_04_0577:

PG_CAST
=======

**PG_CAST** records conversion relationships between data types.

.. table:: **Table 1** PG_CAST columns

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                                       |
   +=======================+=======================+===================================================================================================================================================+
   | castsource            | OID                   | OID of the source data type                                                                                                                       |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | casttarget            | OID                   | OID of the target data type                                                                                                                       |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | castfunc              | OID                   | OID of the conversion function. If the value is **0**, no conversion function is required.                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | castcontext           | Char                  | Conversion mode between the source and target data types                                                                                          |
   |                       |                       |                                                                                                                                                   |
   |                       |                       | -  **e** indicates that only explicit conversion can be performed (using the CAST or :: syntax).                                                  |
   |                       |                       | -  **i** indicates that only implicit conversion can be performed.                                                                                |
   |                       |                       | -  **a** indicates that both explicit and implicit conversion can be performed between data types.                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | castmethod            | Char                  | Conversion method                                                                                                                                 |
   |                       |                       |                                                                                                                                                   |
   |                       |                       | -  **f** indicates that conversion is performed using the specified function in the **castfunc** column.                                          |
   |                       |                       | -  **b** indicates that binary forcible conversion rather than the specified function in the **castfunc** column is performed between data types. |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
