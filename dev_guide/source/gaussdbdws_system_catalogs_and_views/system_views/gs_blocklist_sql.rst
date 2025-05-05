:original_name: dws_04_1246.html

.. _dws_04_1246:

GS_BLOCKLIST_SQL
================

**GS_BLOCKLIST_SQL** is used to query job blocklist and exception information. This view is obtained by associating system catalogs :ref:`GS_BLOCKLIST_SQL <dws_04_1245>` and :ref:`GS_WLM_SESSION_INFO <dws_04_0566>`, and deduplicating query results. If the **GS_WLM_SESSION_INFO** table is large, the query may take a long time.

This view is supported only by 9.1.0.200 and later cluster versions.

.. important::

   -  The schema stored in the **GS_BLOCKLIST_SQL** view is **pg_catalog**.
   -  The **GS_BLOCKLIST_SQL** view can be queried only in the **postgres** database. If it is queried in other databases, an error is reported.
   -  Generally, constant values are ignored during **sql_hash** calculation in DML statements. However, constant values cannot be ignored in DDL, DCL, and parameter setting statements. A **sql_hash** may correspond to one or more queries.

.. table:: **Table 1** GS_BLOCKLIST_QUERY columns

   +-------------+-----------+-----------+---------------------------------------------------------+
   | Column      | Type      | Reference | Description                                             |
   +=============+===========+===========+=========================================================+
   | sql_hash    | Text      | N/A       | **sql_hash** generated based on the query parsing tree. |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | block_list  | Boolean   | N/A       | Whether a job is in the blocklist.                      |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | except_num  | Integer   | N/A       | Number of job exceptions.                               |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | except_time | Timestamp | N/A       | Time when the last job exception occurred.              |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | query       | Text      | N/A       | Statement to be executed.                               |
   +-------------+-----------+-----------+---------------------------------------------------------+
