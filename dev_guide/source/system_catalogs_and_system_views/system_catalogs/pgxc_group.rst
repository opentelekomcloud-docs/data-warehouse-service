:original_name: dws_04_0634.html

.. _dws_04_0634:

PGXC_GROUP
==========

**PGXC_GROUP** records information about node groups.

.. table:: **Table 1** PGXC_GROUP columns

   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                       |
   +=======================+=======================+===================================================================+
   | group_name            | name                  | Node Group name.                                                  |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | in_redistribution     | "char"                | Whether redistribution is required                                |
   |                       |                       |                                                                   |
   |                       |                       | -  **n** indicates that the Node Group is not redistributed.      |
   |                       |                       | -  **y** indicates the source Node Group in redistribution.       |
   |                       |                       | -  **t** indicates the destination Node Group in redistribution.  |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | group_members         | oidvector_extend      | Node OID list of the Node Group                                   |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | group_buckets         | text                  | Distributed data bucket group                                     |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | is_installation       | boolean               | Whether to install a sub-cluster                                  |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | group_acl             | aclitem[]             | Access permissions                                                |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
   | group_kind            | "char"                | Node Group type                                                   |
   |                       |                       |                                                                   |
   |                       |                       | -  **i** indicates an installation Node Group.                    |
   |                       |                       | -  **n** indicates a Node Group in a common, non-logical cluster. |
   |                       |                       | -  **v** indicates a Node Group in a logical cluster.             |
   |                       |                       | -  **e** indicates an elastic cluster.                            |
   +-----------------------+-----------------------+-------------------------------------------------------------------+
