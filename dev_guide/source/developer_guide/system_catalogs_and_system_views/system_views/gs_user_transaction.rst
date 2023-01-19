:original_name: dws_04_0715.html

.. _dws_04_0715:

GS_USER_TRANSACTION
===================

**GS_USER_TRANSACTION** provides transaction information about users on a single CN. The database records the number of times that each user commits and rolls back transactions and the response time of transaction commitment and rollback, in microseconds.

.. table:: **Table 1** **GS_USER_TRANSACTION** columns

   ================ ====== ==========================
   Name             Type   Description
   ================ ====== ==========================
   usename          name   Username
   commit_counter   bigint Number of the commit times
   rollback_counter bigint Number of rollbacks
   resp_min         bigint Minimum response time
   resp_max         bigint Maximum response time
   resp_avg         bigint Average response time
   resp_total       bigint Total response time
   ================ ====== ==========================
