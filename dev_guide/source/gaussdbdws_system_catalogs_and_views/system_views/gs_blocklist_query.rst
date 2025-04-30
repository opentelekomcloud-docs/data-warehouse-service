:original_name: dws_04_1202.html

.. _dws_04_1202:

GS_BLOCKLIST_QUERY
==================

**GS_BLOCKLIST_QUERY** is used to query job blocklist and exception information. This view is obtained by associating system catalogs :ref:`GS_BLOCKLIST_QUERY <dws_04_1006>` and :ref:`GS_WLM_SESSION_INFO <dws_04_0566>`, and deduplicating query results. If the **GS_WLM_SESSION_INFO** table is large, the query may take a long time.

.. important::

   -  The schema of the **GS_BLOCKLIST_QUERY** view is **pg_catalog**.
   -  The **GS_BLOCKLIST_QUERY** view can be queried only in the **postgres** database. If it is queried in other databases, an error is reported.
   -  Generally, constant values are ignored during unique SQL ID calculation in DML statements. However, constant values cannot be ignored in DDL, DCL, and parameter setting statements. A **unique_sql_id** may correspond to one or more queries.

.. table:: **Table 1** GS_BLOCKLIST_QUERY columns

   +---------------+-----------+-----------+------------------------------------------------------+
   | Column        | Type      | Reference | Description                                          |
   +===============+===========+===========+======================================================+
   | unique_sql_id | Bigint    | N/A       | Query ID generated based on the query parsing tree.  |
   +---------------+-----------+-----------+------------------------------------------------------+
   | block_list    | Boolean   | N/A       | Check whether a job is in the blocklist.             |
   +---------------+-----------+-----------+------------------------------------------------------+
   | except_num    | Integer   | N/A       | Query the number of job exceptions.                  |
   +---------------+-----------+-----------+------------------------------------------------------+
   | except_time   | Timestamp | N/A       | Query the time when the last job exception occurred. |
   +---------------+-----------+-----------+------------------------------------------------------+
   | query         | Text      | N/A       | Statement to be executed.                            |
   +---------------+-----------+-----------+------------------------------------------------------+
