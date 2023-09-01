:original_name: dws_01_0075.html

.. _dws_01_0075:

Configuring the Database Audit Logs
===================================

Prerequisites
-------------

Database audit logs are configured on the **Security Settings** page. You can change security settings only when the cluster status is **Available** and **Unbalanced**, and **Task Information** cannot be **Creating snapshot**, **Changing all specifications**, **Configuring**, or **Restarting**.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**.

#. In the cluster list, click the name of a cluster. Choose **Security**.

   By default, **Configuration Status** is **Synchronized**, which indicates that the latest database results are displayed.

#. In the **Audit Settings** area, configure the audit log retention policy.

   |image1|

   **Space priority**: Audit logs will be automatically deleted if the size of audit logs on a single node exceeds 1 GB.

   .. caution::

      -  Clusters 1.0.0 and 1.1.0 do not support audit log retention.
      -  If the planned storage space of the database is limited, select **Space priority** to prevent faulty nodes or low performance caused by insufficient disk space.

#. Enable the audit function for the following operations if necessary.

   :ref:`Table 1 <en-us_topic_0000001517913745__en-us_topic_0000001098656870_table48954270153356>` describes the detailed information about the audit items.

   .. _en-us_topic_0000001517913745__en-us_topic_0000001098656870_table48954270153356:

   .. table:: **Table 1** Audit items

      +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Audit Item          | Description                                                                                                                                                                   |
      +=====================+===============================================================================================================================================================================+
      | Unauthorized Access | Specifies whether to record unauthorized operations. This parameter is disabled by default.                                                                                   |
      +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DML Operation       | Specifies whether to record **INSERT**, **UPDATE**, and **DELETE** operations on tables. This parameter is disabled by default.                                               |
      +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DDL Operation       | Specifies whether to record the **CREATE**, **DROP**, and **ALTER** operations of specified database objects. **DATABASE**, **SCHEMA**, and **USER** are selected by default. |
      +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Except the audit items listed in :ref:`Table 1 <en-us_topic_0000001517913745__en-us_topic_0000001098656870_table48954270153356>`, key audit items in :ref:`Table 2 <en-us_topic_0000001517913745__en-us_topic_0000001098656870_table24262392153654>` are enabled by default on GaussDB(DWS).

   .. _en-us_topic_0000001517913745__en-us_topic_0000001098656870_table24262392153654:

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

   For more information, see :ref:`Enabling Audit Log Dumps <en-us_topic_0000001517355133__en-us_topic_0000001145696613_section8182105814130>`.

#. Click **Apply**.

   Click |image2|. The configuration status **Applying** indicates that the configurations are being saved.

   When the status changes to **Synchronized**, the configurations are saved and take effect.

.. |image1| image:: /_static/images/en-us_image_0000001517913893.png
.. |image2| image:: /_static/images/en-us_image_0000001517754313.png
