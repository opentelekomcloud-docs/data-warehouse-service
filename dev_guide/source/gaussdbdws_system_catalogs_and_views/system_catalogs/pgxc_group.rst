:original_name: dws_04_0634.html

.. _dws_04_0634:

PGXC_GROUP
==========

PGXC_GROUP records node group information. In storage-compute decoupling 3.0 version, each node group in a logical cluster is called a Virtual Warehouse (VW). At the storage KV layer, each VW corresponds to a vgroup.

.. table:: **Table 1** PGXC_GROUP columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                                                                                     |
   +=======================+==========================+=================================================================================================================================================================+
   | group_name            | Name                     | Node group name                                                                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | in_redistribution     | Char                     | Whether redistribution is required                                                                                                                              |
   |                       |                          |                                                                                                                                                                 |
   |                       |                          | -  **n** indicates that the NodeGroup is not redistributed.                                                                                                     |
   |                       |                          | -  **y** indicates the source NodeGroup in redistribution.                                                                                                      |
   |                       |                          | -  **t** indicates the destination NodeGroup in redistribution.                                                                                                 |
   |                       |                          | -  **s** indicates that the NodeGroup will skip redistribution.                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_members         | oidvector_extend         | Node OID list of the node group                                                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_buckets         | Text                     | Distributed data bucket group                                                                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | is_installation       | boolean                  | Whether to install a sub-cluster                                                                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_acl             | aclitem[]                | Access permissions                                                                                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_kind            | Char                     | Node group type.                                                                                                                                                |
   |                       |                          |                                                                                                                                                                 |
   |                       |                          | -  **i** indicates the installation node group, which contains all DNs.                                                                                         |
   |                       |                          | -  **n** indicates a common non-logical cluster node group.                                                                                                     |
   |                       |                          | -  **v** indicates a logical cluster node group.                                                                                                                |
   |                       |                          | -  **e** indicates the elastic cluster node group.                                                                                                              |
   |                       |                          | -  **r** indicates a replication table node group, which can only be used to create replication tables and can contain one or more logical cluster node groups. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_ckpt_csn        | Xid                      | CSN of the last incremental extraction performed on a node group                                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | vgroup_id             | Xid                      | ID of the vgroup corresponding to the node group                                                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | vgroup_bucket_count   | OID                      | Number of buckets in the vgroup corresponding to the node group                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group_ckpt_time       | Timestamp with time zone | Physical time when the last incremental extraction is performed on a node group                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | apply_kv_duration     | Integer                  | Duration of incremental scanning in the last incremental extraction of a node group, in seconds                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ckpt_duration         | Integer                  | Checkpoint duration in the last incremental extraction of a node group, in seconds                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
