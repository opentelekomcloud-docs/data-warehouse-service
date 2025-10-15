:original_name: dws_04_1245.html

.. _dws_04_1245:

GS_BLOCKLIST_SQL
================

**GS_BLOCKLIST_SQL** records job blocklist and exception information. This table uses **sql_hash** as the unique index to collect statistics on job exception information and record blocklist information. You can associate **GS_BLOCKLIST_SQL** with :ref:`GS_WLM_SESSION_INFO <dws_04_0566>` to obtain the **query** column and execution information of a job.

GaussDB(DWS) also provides the :ref:`GS_BLOCKLIST_SQL <dws_04_1246>` view for querying job blocklist and exception information. This view can directly display the **query** column. This view depends on **GS_WLM_SESSION_INFO**. If the **GS_WLM_SESSION_INFO** table is large, the query may take a long time.

This system catalog is supported only by clusters of version 9.1.0.200 or later.

.. important::

   -  The schema of the **GS_BLOCKLIST_SQL** system catalog is **dbms_om**.
   -  The **GS_BLOCKLIST_SQL** system catalog can be queried only in the **postgres** database. If it is queried in other databases, an error is reported.
   -  The **GS_BLOCKLIST_SQL** system catalog contains unique indexes, which are distributed on DNs in hash mode. The distributed column is **sql_hash**.
   -  Generally, constant values are ignored during **sql_hash** calculation in DML statements. However, constant values cannot be ignored in DDL, DCL, and parameter setting statements. A **sql_hash** may correspond to one or more queries.

.. table:: **Table 1** GS_BLOCKLIST_SQL columns

   +-------------+-----------+-----------+---------------------------------------------------------+
   | Name        | Type      | Reference | Description                                             |
   +=============+===========+===========+=========================================================+
   | sql_hash    | Text      | N/A       | **sql_hash** generated based on the query parsing tree. |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | block_list  | Boolean   | N/A       | Check whether a job is in the blocklist.                |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | except_num  | Integer   | N/A       | Query the number of job exceptions.                     |
   +-------------+-----------+-----------+---------------------------------------------------------+
   | except_time | Timestamp | N/A       | Query the time when the last job exception occurred.    |
   +-------------+-----------+-----------+---------------------------------------------------------+
