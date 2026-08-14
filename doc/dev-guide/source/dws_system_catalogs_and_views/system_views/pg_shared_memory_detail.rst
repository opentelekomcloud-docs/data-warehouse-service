:original_name: dws_04_0753.html

.. _dws_04_0753:

PG_SHARED_MEMORY_DETAIL
=======================

**PG_SHARED_MEMORY_DETAIL** displays usage information about all the shared memory contexts.

.. table:: **Table 1** PG_SHARED_MEMORY_DETAIL columns

   =========== ======== ==============================================
   Column      Type     Description
   =========== ======== ==============================================
   contextname Text     Name of the memory context.
   level       Smallint Hierarchy of the memory context.
   parent      Text     Parent memory context.
   totalsize   Bigint   Total size of the shared memory, in bytes.
   freesize    Bigint   Remaining size of the shared memory, in bytes.
   usedsize    Bigint   Used size of the shared memory, in bytes.
   =========== ======== ==============================================
