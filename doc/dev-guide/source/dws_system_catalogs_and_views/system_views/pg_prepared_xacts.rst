:original_name: dws_04_0742.html

.. _dws_04_0742:

PG_PREPARED_XACTS
=================

**PG_PREPARED_XACTS** displays information about transactions that are currently prepared for two-phase commit.

.. table:: **Table 1** PG_PREPARED_XACTS columns

   +-------------+--------------------------+------------------------------------------+--------------------------------------------------------------------+
   | Column      | Type                     | Reference                                | Description                                                        |
   +=============+==========================+==========================================+====================================================================+
   | transaction | Xid                      | N/A                                      | Numeric transaction identifier of the prepared transaction         |
   +-------------+--------------------------+------------------------------------------+--------------------------------------------------------------------+
   | gid         | Text                     | N/A                                      | Global transaction identifier that was assigned to the transaction |
   +-------------+--------------------------+------------------------------------------+--------------------------------------------------------------------+
   | prepared    | Timestamp with time zone | N/A                                      | Time at which the transaction is prepared for commit               |
   +-------------+--------------------------+------------------------------------------+--------------------------------------------------------------------+
   | owner       | Name                     | :ref:`PG_AUTHID <dws_04_0574>`.rolname   | Name of the user that executes the transaction                     |
   +-------------+--------------------------+------------------------------------------+--------------------------------------------------------------------+
   | database    | Name                     | :ref:`PG_DATABASE <dws_04_0582>`.datname | Name of the database in which the transaction is executed          |
   +-------------+--------------------------+------------------------------------------+--------------------------------------------------------------------+
