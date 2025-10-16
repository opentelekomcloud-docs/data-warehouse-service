:original_name: dws_04_1006.html

.. _dws_04_1006:

GS_BLOCKLIST_QUERY
==================

**GS_BLOCKLIST_QUERY** records job blocklist and exception information. This table uses **unique_sql_id** as the unique index to collect statistics on job exception information and record blocklist information. You can associate **GS_BLOCKLIST_QUERY** with :ref:`GS_WLM_SESSION_INFO <dws_04_0566>` to obtain the **query** column and execution information of a job.

.. important::

   -  The schema of the **GS_BLOCKLIST_QUERY** system catalog is **dbms_om**.
   -  The **GS_BLOCKLIST_QUERY** system catalog can be queried only in the **postgres** database. If it is queried in other databases, an error is reported.
   -  The **GS_BLOCKLIST_QUERY** system catalog contains unique indexes, which are distributed on DNs in hash mode. The distributed column is **unique_sql_id**.
   -  Generally, constant values are ignored during Unique SQL ID calculation in DML statements. However, constant values cannot be ignored in DDL, DCL, and parameter setting statements. A **unique_sql_id** may correspond to one or more queries.

GaussDB(DWS) also provides the :ref:`GS_BLOCKLIST_QUERY <dws_04_1202>` view for querying job blocklist and exception information. This view can directly display the **query** column. This view depends on **GS_WLM_SESSION_INFO**. If the **GS_WLM_SESSION_INFO** table is large, the query may take a long time.

.. table:: **Table 1** GS_BLOCKLIST_QUERY columns

   +---------------+-----------+-----------+------------------------------------------------------+
   | Name          | Type      | Reference | Description                                          |
   +===============+===========+===========+======================================================+
   | unique_sql_id | Bigint    | N/A       | Query ID generated based on the query parsing tree.  |
   +---------------+-----------+-----------+------------------------------------------------------+
   | block_list    | Boolean   | N/A       | Check whether a job is in the blocklist.             |
   +---------------+-----------+-----------+------------------------------------------------------+
   | except_num    | Integer   | N/A       | Query the number of job exceptions.                  |
   +---------------+-----------+-----------+------------------------------------------------------+
   | except_time   | Timestamp | N/A       | Query the time when the last job exception occurred. |
   +---------------+-----------+-----------+------------------------------------------------------+
