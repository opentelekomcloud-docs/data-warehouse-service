:original_name: dws_01_0075.html

.. _dws_01_0075:

Configuring the Database Audit Logs
===================================

Prerequisites
-------------

Database audit logs are configured on the **Security Settings** page. You can change security settings only when the cluster status is **Available** and **Unbalanced**, and **Task Information** cannot be **Creating snapshot**, **Scaling out**, **Changing all specifications**, **Configuring**, or **Restarting**.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters** > **Dedicated Cluster**.

#. In the cluster list, click the name of a cluster. Choose **Security**.

   By default, **Configuration Status** is **Synchronized**, which indicates that the latest database results are displayed.

#. In the **Audit Settings** area, set the audit items:

   .. note::

      The default audit log retention policy is space-first, which means audit logs will be automatically deleted when the size of audit logs on a single node exceeds 1 GB. This function prevents node faults or low performance caused by high disk space occupied by audit logs.

   :ref:`Table 1 <en-us_topic_0000001659054638__en-us_topic_0000001372999374_en-us_topic_0000001098656870_table48954270153356>` describes the detailed information about the audit items.

   .. _en-us_topic_0000001659054638__en-us_topic_0000001372999374_en-us_topic_0000001098656870_table48954270153356:

   .. table:: **Table 1** Audit items

      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Audit Item                  | Description                                                                                                                                                                   |
      +=============================+===============================================================================================================================================================================+
      | Unauthorized access         | Specifies whether to record unauthorized operations. This parameter is disabled by default.                                                                                   |
      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DML operations              | Specifies whether to record **INSERT**, **UPDATE**, and **DELETE** operations on tables. This parameter is disabled by default.                                               |
      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SELECT operations           | Specifies whether to record the **SELECT** operation. This parameter is disabled by default.                                                                                  |
      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Stored procedure executions | Specifies whether to record operations when executing the stored procedure or user-defined functions. This parameter is disabled by default.                                  |
      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | COPY operations             | Specifies whether to record the **COPY** operation. This parameter is disabled by default.                                                                                    |
      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DDL operations              | Specifies whether to record the **CREATE**, **DROP**, and **ALTER** operations of specified database objects. **DATABASE**, **SCHEMA**, and **USER** are selected by default. |
      +-----------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Except the audit items listed in :ref:`Table 1 <en-us_topic_0000001659054638__en-us_topic_0000001372999374_en-us_topic_0000001098656870_table48954270153356>`, key audit items in :ref:`Table 2 <en-us_topic_0000001659054638__en-us_topic_0000001372999374_en-us_topic_0000001098656870_table24262392153654>` are enabled by default on GaussDB(DWS).

   .. _en-us_topic_0000001659054638__en-us_topic_0000001372999374_en-us_topic_0000001098656870_table24262392153654:

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

   For more information, see :ref:`Enabling Audit Log Dumps <en-us_topic_0000001658895326__en-us_topic_0000001372520098_en-us_topic_0000001145696613_section8182105814130>`.

#. Click **Apply**.

   Click |image1|. The configuration status **Applying** indicates that the configurations are being saved.

   When the status changes to **Synchronized**, the configurations are saved and take effect.

.. |image1| image:: /_static/images/en-us_image_0000001759579473.png
