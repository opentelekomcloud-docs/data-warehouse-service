:original_name: dws_04_0830.html

.. _dws_04_0830:

PGXC_USER_TRANSACTION
=====================

**PGXC_USER_TRANSACTION** provides transaction information about users on all CNs. It is accessible only to users with system administrator rights. This view is valid only if the real-time resource monitoring function is enabled, that is, if :ref:`enable_resource_track <en-us_topic_0000001510522653__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is **on**.

.. table:: **Table 1** **PGXC_USER_TRANSACTION** columns

   ================ ====== ==========================
   Name             Type   Description
   ================ ====== ==========================
   node_name        name   Node name
   usename          name   Username
   commit_counter   bigint Number of the commit times
   rollback_counter bigint Number of rollbacks
   resp_min         bigint Minimum response time
   resp_max         bigint Maximum response time
   resp_avg         bigint Average response time
   resp_total       bigint Total response time
   ================ ====== ==========================
