:original_name: dws_01_0142.html

.. _dws_01_0142:

Dumping the Database Audit Logs
===============================

GaussDB(DWS) records information (audit logs) about connections and user activities in your database. With the information, you can monitor the database to ensure security and facilitate fault troubleshooting and historical operation record locating. These audit logs are stored in the database by default. You can also dump them to OBS so that users who are responsible for monitoring the database can view the logs more conveniently.

You can perform the following operations on the GaussDB(DWS) console:

-  :ref:`Enabling Audit Log Dumps <en-us_topic_0000001135053162__en-us_topic_0000001145696613_section8182105814130>`
-  :ref:`Modifying Audit Log Dump Configurations <en-us_topic_0000001135053162__en-us_topic_0000001145696613_section097518211410>`
-  :ref:`Viewing Audit Log Dumps <en-us_topic_0000001135053162__en-us_topic_0000001145696613_section1227433741613>`
-  :ref:`Disabling Audit Log Dumps <en-us_topic_0000001135053162__en-us_topic_0000001145696613_section156597111415>`

.. _en-us_topic_0000001135053162__en-us_topic_0000001145696613_section8182105814130:

Enabling Audit Log Dumps
------------------------

After a data warehouse cluster is created, you can enable audit log dump for it to dump audit logs to OBS.

Before enabling audit log dump, ensure that the following conditions are met:

-  You have created an OBS bucket for storing the audit logs. For details, see "Console Operation Guide > Managing Buckets > Creating a Bucket" in the *Object Storage Service User Guide*.

The procedure is as follows:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Clusters**.

#. In the cluster list, click the name of the cluster for which you want to enable audit log dump. On the page that is displayed, click **Security Settings**.

#. In the **Audit Settings** area, enable **Audit Log Dump**.

   |image1| indicates that the function is enabled. |image2| indicates that the function is disabled.

   When you enable audit log dump for a project in a region for the first time, the system prompts you to create an agency named **DWSAccessOBS**. After the agency is created, GaussDB(DWS) can dump audit logs to OBS. By default, only cloud accounts or users with **Security Administrator** permissions can create agencies. IAM users under an account do not have the permission for creating agencies by default. Contact a user with the permission and complete the authorization on the current page.

   -  **OBS Bucket**: Name of the OBS bucket used to store the audit data. If no OBS bucket is available, click **View OBS Bucket** to access the OBS console and create one. For details, see **Console Operation Guide > Managing Buckets > Creating a Bucket** in the *Object Storage Service User Guide*.
   -  **OBS Path**: User-defined directory on OBS for storing audit files. Different directory levels are separated by forward slashes (/). The value is a string containing 1 to 50 characters, which cannot start with a forward slash (/). If the entered OBS path does not exist, the system creates one and dumps data to it.
   -  **Dump Interval (Minute)**: Interval based on which GaussDB(DWS) periodically dumps data to OBS. The value ranges from 5 to 43200. The unit is minute.

#. Click **Apply**.

   If **Configuration Status** is **Applying**, the system is saving the settings.

   Wait for a moment and then refresh **Configuration Status**. When **Configuration Status** is **Synchronized**, the configuration is saved and takes effect.

.. _en-us_topic_0000001135053162__en-us_topic_0000001145696613_section097518211410:

Modifying Audit Log Dump Configurations
---------------------------------------

After audit log dump is enabled, you can modify the dump configurations, for example, modifying the OBS bucket, path, and dump interval.

The procedure is as follows:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Clusters**.

#. In the cluster list, click the name of the cluster for which you want to modify the audit log dump configurations. On the page that is displayed, click **Security Settings**.

#. In the **Audit Settings** area, modify the **Audit Log Dump** configurations.

#. Click **Apply**.

   If **Configuration Status** is **Applying**, the system is saving the settings.

   Wait for a moment and then refresh **Configuration Status**. When **Configuration Status** is **Synchronized**, the configuration is saved and takes effect.

.. _en-us_topic_0000001135053162__en-us_topic_0000001145696613_section1227433741613:

Viewing Audit Log Dumps
-----------------------

After audit log dump is enabled, you can view the dumped audit logs on OBS.

The procedure is as follows:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Clusters**.

#. In the cluster list, click the name of the target cluster. On the page that is displayed, click **Security Settings**.

#. In the **Audit Settings** area, click **View Dump Record**.

#. In the **Audit Log Dump Records** dialog box, click **View OBS Bucket**. The OBS console page is displayed.

#. Select the OBS bucket and folder where the logs are stored to view the log files.

   You can download and decompress the files to view. The fields of audit log files are described as follows:

   .. table:: **Table 1** Log file fields

      +-----------------+-------------------------------------------------------------+
      | Name            | Description                                                 |
      +=================+=============================================================+
      | time            | Indicates the operation time.                               |
      +-----------------+-------------------------------------------------------------+
      | type            | Indicates the operation type.                               |
      +-----------------+-------------------------------------------------------------+
      | result          | Indicates the operation result.                             |
      +-----------------+-------------------------------------------------------------+
      | username        | Indicates the name of the user who initiates the operation. |
      +-----------------+-------------------------------------------------------------+
      | database        | Indicates the database name.                                |
      +-----------------+-------------------------------------------------------------+
      | client_conninfo | Indicates the client connection information.                |
      +-----------------+-------------------------------------------------------------+
      | object_name     | Indicates the operation object name.                        |
      +-----------------+-------------------------------------------------------------+
      | detail_info     | Indicates the detailed information about the operation.     |
      +-----------------+-------------------------------------------------------------+
      | node_name       | Indicates the node name.                                    |
      +-----------------+-------------------------------------------------------------+
      | thread_id       | Indicates the thread ID.                                    |
      +-----------------+-------------------------------------------------------------+
      | local_port      | Indicates the local port.                                   |
      +-----------------+-------------------------------------------------------------+
      | remote_port     | Indicates the remote port.                                  |
      +-----------------+-------------------------------------------------------------+

.. _en-us_topic_0000001135053162__en-us_topic_0000001145696613_section156597111415:

Disabling Audit Log Dumps
-------------------------

You can disable audit log dumps if you do not want to dump audit logs to OBS.

The procedure is as follows:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Clusters**.

#. In the cluster list, click the name of the cluster for which you want to disable audit log dump. On the page that is displayed, click **Security Settings**.

#. In the **Audit Settings** area, disable audit log dump.

   |image3| indicates that the function is disabled.

#. Click **Apply**.

   If **Configuration Status** is **Applying**, the system is saving the settings.

   Wait for a moment and then refresh **Configuration Status**. When **Configuration Status** is **Synchronized**, the configuration is saved and takes effect.

.. |image1| image:: /_static/images/en-us_image_0000001181172585.png
.. |image2| image:: /_static/images/en-us_image_0000001181012665.jpg
.. |image3| image:: /_static/images/en-us_image_0000001181172589.jpg
