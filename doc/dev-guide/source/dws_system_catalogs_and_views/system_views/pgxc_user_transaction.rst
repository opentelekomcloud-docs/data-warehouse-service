:original_name: dws_04_0830.html

.. _dws_04_0830:

PGXC_USER_TRANSACTION
=====================

**PGXC_USER_TRANSACTION** provides transaction information about users on all CNs. It is accessible only to users with system administrator rights. This view is valid only if the real-time resource monitoring function is enabled, that is, if :ref:`enable_resource_track <en-us_topic_0000001811490709__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is **on**.

.. table:: **Table 1** **PGXC_USER_TRANSACTION** columns

   ================ ====== ======================
   Column           Type   Description
   ================ ====== ======================
   node_name        Name   Node name.
   usename          Name   Username.
   commit_counter   Bigint Number of commits.
   rollback_counter Bigint Number of rollbacks.
   resp_min         Bigint Minimum response time.
   resp_max         Bigint Maximum response time.
   resp_avg         Bigint Average response time.
   resp_total       Bigint Total response time.
   ================ ====== ======================
