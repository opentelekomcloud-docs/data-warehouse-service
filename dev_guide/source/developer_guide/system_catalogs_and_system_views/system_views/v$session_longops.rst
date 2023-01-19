:original_name: dws_04_0881.html

.. _dws_04_0881:

V$SESSION_LONGOPS
=================

**V$SESSION_LONGOPS** displays the progress of ongoing operations.

.. table:: **Table 1** V$SESSION_LONGOPS columns

   +-----------+---------+------------------------------------------------------------------------------------+
   | Name      | Type    | Description                                                                        |
   +===========+=========+====================================================================================+
   | sid       | bigint  | OID of the running background process                                              |
   +-----------+---------+------------------------------------------------------------------------------------+
   | serial#   | integer | Sequence number of the running background process, which is **0** in GaussDB(DWS). |
   +-----------+---------+------------------------------------------------------------------------------------+
   | sofar     | integer | Completed workload, which is empty in GaussDB(DWS).                                |
   +-----------+---------+------------------------------------------------------------------------------------+
   | totalwork | integer | Total workload, which is empty in GaussDB(DWS).                                    |
   +-----------+---------+------------------------------------------------------------------------------------+
