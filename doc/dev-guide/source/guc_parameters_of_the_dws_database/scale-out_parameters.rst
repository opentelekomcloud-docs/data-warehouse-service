:original_name: dws_04_1522.html

.. _dws_04_1522:

Scale-out Parameters
====================

enable_cluster_resize
---------------------

**Parameter description**: Specifies whether the current session is for scaling or redistributing data.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the current session is for scaling or redistributing data, and allows the execution of specific SQL statements for redistribution.
-  **off** indicates that the current session is not for scaling or redistributing data, and does not allow the execution of specific SQL statements for redistribution.

**Default value**: **off**

.. caution::

   This parameter is an internal O&M parameter and applies only to sessions for scaling or redistributing data. Do not set this parameter in service sessions.

disable_insert_into_newnodes
----------------------------

**Parameter description**: Specifies whether new tables can be created in the target node group during scale-out.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that new tables cannot be created in the target node group during scale-out. By default, new tables are created in the original node group.
-  **off** indicates that new tables are created in the target node group by default.

**Default value**: **on**

.. caution::

   This parameter is automatically controlled by the scale-out process. You are advised not to modify it.
