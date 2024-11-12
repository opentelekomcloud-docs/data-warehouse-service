:original_name: dws_01_0075.html

.. _dws_01_0075:

Configuring the Database Audit Logs
===================================

Prerequisites
-------------

Database audit logs are configured on the **Security Settings** page. You can change security settings only when the cluster status is **Available** and **Unbalanced**, and **Task Information** cannot be **Creating snapshot**, **Scaling out**, **Changing all specifications**, **Configuring**, or **Restarting**.

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of a cluster. Choose **Security**.

   By default, **Configuration Status** is **Synchronized**, which indicates that the latest database results are displayed.

#. In the **Audit Settings** area, set the audit items:

   .. note::

      The default audit log retention policy is space-first, which means audit logs will be automatically deleted when the size of audit logs on a single node exceeds 1 GB. This function prevents node faults or low performance caused by high disk space occupied by audit logs.

   :ref:`Table 1 <en-us_topic_0000001924569232__en-us_topic_0000001098656870_table48954270153356>` describes the detailed information about the audit items.

   .. _en-us_topic_0000001924569232__en-us_topic_0000001098656870_table48954270153356:

   .. table:: **Table 1** Audit items

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Audit Item                        | Description                                                                                                                                                                                             |
      +===================================+=========================================================================================================================================================================================================+
      | Unauthorized access               | Specifies whether to record unauthorized operations. This parameter is disabled by default.                                                                                                             |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DQL operations                    | **SELECT** operations can be selected.                                                                                                                                                                  |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    8.1.1.100 and later versions support the **DQL operations** audit item.                                                                                                                              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DML operations                    | Specifies whether to record **INSERT**, **UPDATE**, and **DELETE** operations on tables. This parameter is disabled by default.                                                                         |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    8.1.1.100 and later versions support fine-grained splitting of audit items, and the **COPY** and **MERGE** options are added.                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DDL operations                    | Specifies whether to record the **CREATE**, **DROP**, and **ALTER** operations of specified database objects. **DATABASE**, **SCHEMA**, and **USER** are selected by default.                           |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    8.1.1.100 and later versions support **TABLE**, **DATA SOURCE**, and **NODE GROUP** operations. These operations are enabled by default.                                                             |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Other operations                  | Specifies whether to record other operations. Only the **TRANSACTION** and **CURSOR** operations are selected by default.                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    -  8.1.1.100 and later versions support the **Other operations** audit item.                                                                                                                         |
      |                                   |    -  You are advised to select **TRANSACTION**. Otherwise, statements in a transaction will not be audited.                                                                                            |
      |                                   |    -  You are advised to select **CURSOR**. Otherwise, **SELECT** statements in a cursor will not be audited. The Data Studio client automatically encapsulates **SELECT** statements using **CURSOR**. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Except the audit items listed in :ref:`Table 1 <en-us_topic_0000001924569232__en-us_topic_0000001098656870_table48954270153356>`, key audit items in :ref:`Table 2 <en-us_topic_0000001924569232__en-us_topic_0000001098656870_table24262392153654>` are enabled by default on GaussDB(DWS).

   .. _en-us_topic_0000001924569232__en-us_topic_0000001098656870_table24262392153654:

   .. table:: **Table 2** Key audit items

      +-----------------+-----------------------------------------------------------+
      | Parameter       | Description                                               |
      +=================+===========================================================+
      | Key audit items | Records successful and failed logins and logout.          |
      +-----------------+-----------------------------------------------------------+
      |                 | Records database startup, stop, recovery, and switchover. |
      +-----------------+-----------------------------------------------------------+
      |                 | Records user locking and unlocking.                       |
      +-----------------+-----------------------------------------------------------+
      |                 | Records the grants and reclaims of user permissions.      |
      +-----------------+-----------------------------------------------------------+
      |                 | Records the audit function of the **SET** operation.      |
      +-----------------+-----------------------------------------------------------+

#. Enable or disable audit log dumps.

   For more information, see :ref:`Enabling Audit Log Dumps <en-us_topic_0000001952008045__en-us_topic_0000001145696613_section8182105814130>`.

#. Click **Apply**.

   Click |image1|. The configuration status **Applying** indicates that the configurations are being saved.

   When the status changes to **Synchronized**, the configurations are saved and take effect.

.. |image1| image:: /_static/images/en-us_image_0000001951848657.png
