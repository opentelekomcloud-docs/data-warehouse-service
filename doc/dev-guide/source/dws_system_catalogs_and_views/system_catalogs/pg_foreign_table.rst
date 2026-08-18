:original_name: dws_04_0593.html

.. _dws_04_0593:

PG_FOREIGN_TABLE
================

**PG_FOREIGN_TABLE** records auxiliary information about foreign tables.

.. table:: **Table 1** PG_FOREIGN_TABLE columns

   =========== ======= ====================================================
   Column      Type    Description
   =========== ======= ====================================================
   ftrelid     OID     OID of the foreign table
   ftserver    OID     OID of the server where the foreign table is located
   ftwriteonly boolean Whether data can be written in the foreign table
   ftoptions   Text[]  Foreign table options
   =========== ======= ====================================================
