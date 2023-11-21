:original_name: DWS_DS_85.html

.. _DWS_DS_85:

Reindexing a Table
==================

Index facilitate lookup of records. You need to reindex tables in the following scenarios:

-  The index is corrupted and no longer contains valid data. Although in theory this must never happen, in practice, indexes can become corrupted due to software bugs or hardware failures. Reindexing provides a recovery method.
-  The index has become "bloated". That is, it contains many empty or nearly-empty pages. This can occur with B-tree indexes in PostgreSQL under certain uncommon access patterns. Reindexing provides a way to reduce the space consumption of the index by writing a new version of the index without the dead pages.
-  You have altered a storage parameter (such as the fill factor) for an index, and wish to ensure that the change has taken full effect.

Follow the steps below to reindex a table:

#. Right-click the selected table and select **Reindex Table**.

   A pop-up message and status bar display the status of the completed operation.

   .. note::

      This operation is not supported for Partition ORC tables.
