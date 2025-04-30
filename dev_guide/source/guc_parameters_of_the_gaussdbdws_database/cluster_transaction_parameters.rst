:original_name: dws_04_0934.html

.. _dws_04_0934:

Cluster Transaction Parameters
==============================

This section describes the settings and value ranges of cluster transaction parameters.

transaction_isolation
---------------------

**Parameter description**: Specifies the isolation level of the current transaction.

**Type**: USERSET

**Value range**:

-  **READ COMMITTED**: Only committed data is read. This is the default.
-  **READ UNCOMMITTED**: GaussDB(DWS) does not support **READ UNCOMMITTED**. If **READ UNCOMMITTED** is set, **READ COMMITTED** is executed instead.
-  **REPEATABLE READ**: Only the data committed before transaction start is read. Uncommitted data or data committed in other concurrent transactions cannot be read.
-  **SERIALIZABLE**: GaussDB(DWS) does not support **SERIALIZABLE**. If **SERIALIZABLE** is set, **REPEATABLE READ** is executed instead.

**Default value**: **READ COMMITTED**

transaction_read_only
---------------------

**Parameter description**: Specifies that the current transaction is a read-only transaction.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the current transaction is a read-only transaction.
-  **off** indicates that the current transaction can be a read/write transaction.

**Default value**: **off** for CNs and **on** for DNs

xc_maintenance_mode
-------------------

**Parameter description**: Specifies whether the system is in maintenance mode.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that maintenance mode is enabled.
-  **off** indicates that the maintenance mode is disabled.

**Default value**: **off**

.. important::

   Enable the maintenance mode with caution to avoid cluster data inconsistencies.

allow_concurrent_tuple_update
-----------------------------

**Parameter description**: Specifies whether to allow concurrent update.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

gtm_backup_barrier
------------------

**Parameter description**: Specifies whether to create a restoration point for the GTM starting point.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that a restoration point will be created for the GTM starting point.
-  **off** indicates that a restoration point will not be created for the GTM starting point.

**Default value**: **off**

transaction_deferrable
----------------------

**Parameter description**: Specifies whether to delay the execution of a read-only serial transaction without incurring an execution failure. Assume this parameter is set to **on**. When the server detects that the tuples read by a read-only transaction are being modified by other transactions, it delays the execution of the read-only transaction until the other transactions finish modifying the tuples. Currently, this parameter is not used in GaussDB(DWS). Similar to this parameter, the :ref:`default_transaction_deferrable <en-us_topic_0000001764491912__s05ef9312d74143928830d7d459cdc63a>` parameter is used to specify whether to allow delayed execution of a transaction.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the execution of a read-only serial transaction can be delayed.
-  **off** indicates that the execution of a read-only serial transaction cannot be delayed.

**Default value**: **off**

enforce_two_phase_commit
------------------------

**Parameter description**: This parameter is reserved for compatibility with earlier versions. This parameter is invalid in the current version.

enable_show_any_tuples
----------------------

**Parameter description**: This parameter is available only in a read-only transaction and is used for analysis. When this parameter is set to **on/true**, all versions of tuples in the table are displayed.

**Type**: USERSET

**Value range**: Boolean

-  **on/true** indicates that all versions of tuples in the table are displayed.
-  **off/false** indicates that no versions of tuples in the table are displayed.

**Default value**: **off**

idle_in_transaction_timeout
---------------------------

**Parameter description**: duration during which a transaction is allowed to be in the idle state. When a transaction is in the idle state for a period specified by this parameter, the transaction is terminated. This function takes effect only for client connections that are directly connected to CNs and does not take effect for direct DNs or internal connections. This parameter is supported only by clusters of version 8.2.1.100 or later.

**Type**: USERSET

**Value range**: 0 to 86400, in second.

**Default value**: **0**, indicating that the function is disabled.
