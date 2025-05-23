:original_name: dws_04_0747.html

.. _dws_04_0747:

PG_RUNNING_XACTS
================

**PG_RUNNING_XACTS** displays information about running transactions on the current node.

.. table:: **Table 1** PG_RUNNING_XACTS columns

   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | Column      | Type    | Description                                                                                       |
   +=============+=========+===================================================================================================+
   | handle      | Integer | Handle corresponding to the transaction in GTM                                                    |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | gxid        | Xid     | Transaction ID                                                                                    |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | state       | tinyint | Transaction status (**3**: prepared or **0**: starting)                                           |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | node        | Text    | Node name                                                                                         |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | xmin        | Xid     | Minimum transaction ID **xmin** on the node                                                       |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | vacuum      | boolean | Whether the current transaction is lazy vacuum                                                    |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | timeline    | Bigint  | Number of database restarts                                                                       |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | prepare_xid | Xid     | Transaction ID in the **prepared** status. If the status is not **prepared**, the value is **0**. |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | pid         | Bigint  | Thread ID corresponding to the transaction                                                        |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
   | next_xid    | Xid     | Transaction ID sent from a CN to a DN                                                             |
   +-------------+---------+---------------------------------------------------------------------------------------------------+
