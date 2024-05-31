:original_name: dws_04_0753.html

.. _dws_04_0753:

PG_SHARED_MEMORY_DETAIL
=======================

**PG_SHARED_MEMORY_DETAIL** displays usage information about all the shared memory contexts.

.. table:: **Table 1** PG_SHARED_MEMORY_DETAIL columns

   =========== ======== =============================================
   Name        Type     Description
   =========== ======== =============================================
   contextname text     Name of the context in the memory
   level       smallint Hierarchy of the memory context
   parent      text     Context of the parent memory
   totalsize   bigint   Total size of the shared memory, in bytes
   freesize    bigint   Remaining size of the shared memory, in bytes
   usedsize    bigint   Used size of the shared memory, in bytes
   =========== ======== =============================================
