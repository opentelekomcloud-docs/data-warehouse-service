:original_name: dws_04_0715.html

.. _dws_04_0715:

GS_USER_TRANSACTION
===================

**GS_USER_TRANSACTION** provides transaction information about users on a single CN. The database records the number of times that each user commits and rolls back transactions and the response time of transaction commitment and rollback, in microseconds.

.. table:: **Table 1** **GS_USER_TRANSACTION** columns

   ================ ====== =====================
   Column           Type   Description
   ================ ====== =====================
   usename          Name   Username
   commit_counter   Bigint Number of the commits
   rollback_counter Bigint Number of rollbacks
   resp_min         Bigint Minimum response time
   resp_max         Bigint Maximum response time
   resp_avg         Bigint Average response time
   resp_total       Bigint Total response time
   ================ ====== =====================
