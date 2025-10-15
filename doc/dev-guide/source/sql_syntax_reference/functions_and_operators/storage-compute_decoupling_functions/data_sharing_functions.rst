:original_name: dws_06_0383.html

.. _dws_06_0383:

Data Sharing Functions
======================

pgxc_group_add_subscription(src_vw_name, target_vw_name)
--------------------------------------------------------

Description: In a storage-compute decoupling architecture, a KV subscription relationship is established between logical clusters (Virtual Warehouses, VWs). Once established, the KV cache of the consumer VW periodically synchronizes the SST file incrementally from the OBS cudesc snapshot directory of the producer to the local host. This allows tables in the producer VW to be queried across VWs in the consumer VM. The created subscription relationship can be found in the **pgxc_group_subscription** table. This function is supported only by clusters of version 9.0.3 or later.

Return type: void

The following information is displayed:

.. table:: **Table 1** Columns returned by pgxc_group_add_subscription(src_vw_name, target_vw_name)

   +----------------+------+-----------------------------------------------------------------------------+
   | Column         | Type | Description                                                                 |
   +================+======+=============================================================================+
   | src_vw_name    | text | Producer VW name, which is usually used as the VW to which data is written. |
   +----------------+------+-----------------------------------------------------------------------------+
   | target_vw_name | text | Consumer VW name, which is usually used as the VW for reading data.         |
   +----------------+------+-----------------------------------------------------------------------------+

The following is an example:

::

   SELECT pgxc_group_add_subscription('write_group', 'single_read_group');
   pgxc_group_add_subscription
   ------------------

   (1 row)

pgxc_group_drop_subscription(src_vw_name, target_vw_name)
---------------------------------------------------------

Description: To delete an established KV subscription relationship in a storage and compute separation architecture, remove the records from the **pgxc_group_subscription** table. After deletion, the consumer VW can no longer query tables in the producer VW across VWs. This function is supported only by clusters of version 9.0.3 or later.

Return type: void

The following information is displayed:

.. table:: **Table 2** Columns returned by pgxc_group_drop_subscription(src_vw_name, target_vw_name)

   +----------------+------+-----------------------------------------------------------------------------+
   | Column         | Type | Description                                                                 |
   +================+======+=============================================================================+
   | src_vw_name    | text | Producer VW name, which is usually used as the VW to which data is written. |
   +----------------+------+-----------------------------------------------------------------------------+
   | target_vw_name | text | Consumer VW name, which is usually used as the VW for reading data.         |
   +----------------+------+-----------------------------------------------------------------------------+

The following is an example:

.. code-block::

   SELECT  pgxc_group_drop_subscription('write_group', 'single_read_group');
   pgxc_group_drop_subscription
   ------------------------------
   (1 row)
