:original_name: dws_01_0127.html

.. _dws_01_0127:

.. _en-us_topic_0000001764650496:

Using DSC to Migrate SQL Scripts
================================

The DSC is a CLI tool running on the Linux or Windows OS. It is dedicated to providing customers with simple, fast, and reliable application SQL script migration services. It parses the SQL scripts of source database applications using the built-in syntax migration logic, and converts them to SQL scripts applicable to GaussDB(DWS) databases. You do not need to connect the DSC to a database. It can migrate data in offline mode without service interruption. In GaussDB(DWS), you can run the migrated SQL scripts to restore the database, thereby easily migrating offline databases to the cloud.

The DSC can migrate SQL scripts of Teradata, Oracle, Netezza, MySQL, and DB2 databases.

Downloading the DSC SQL Migration Tool
--------------------------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation tree on the left, choose **Management** > **Client Connections**.

#. In the **Download Client and Driver** area, click **here** to download the DSC migration tool.

   If you have clusters of different versions, the system displays a dialog box, prompting you to select the cluster version and download the client corresponding to the cluster version. In the cluster list on the **Cluster Management** page, click the name of the specified cluster and click the **Basic Information** tab to view the cluster version.


   .. figure:: /_static/images/en-us_image_0000002110071384.png
      :alt: **Figure 1** Downloading the tool

      **Figure 1** Downloading the tool

#. After downloading the DSC tool to the local PC, use WinSCP to upload it to a Linux host.

   The user who uploads the tool must have the full control permission on the target directory of the Linux host.

Operation Guide for the DSC SQL Syntax Migration Tool
-----------------------------------------------------

For details, see "DSC - SQL Syntax Migration Tool" in the *Data Warehouse Service Tool Guide*.
