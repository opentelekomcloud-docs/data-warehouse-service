:original_name: DWS_DS_39.html

.. _DWS_DS_39:

Refreshing a Database Connection
================================

Perform the following steps to refresh a database connection.

#. In the **Object Browser** pane, right-click the selected connection name and select **Refresh** or press **F5**.

   The status of the completed operation is displayed in the status bar.

The time taken to refresh a database depends on the number of objects in the database. Therefore, perform this operation as required on large databases.

-  If you right-click the connection name and select **Refresh**, the connection will be refreshed and updated with the latest content on the server.

-  If you right-click **Function/Procedure** and select **Refresh**, all functions/procedures and tables under the schema will be refreshed and updated with the latest content on the server.

   If a stored procedure has been deleted from the database before the refresh operation, this stored procedure will be deleted from **Object Browser** only when the refresh operation is performed.

-  If you right-click a specific function/procedure and select **Refresh**, this function/procedure will be refreshed and updated with the latest content on the server.
-  If you refresh the entire database or connection, all child objects of schemas in **search_path**, as well as the schemas already expanded by the user, will be loaded again.
-  If you reconnect to the database, only schema objects saved under **search_path** will be loaded. Objects that have been expanded will not be loaded.
-  A database and multiple objects under it cannot be refreshed simultaneously.

Exporting/Importing Connection Details
--------------------------------------

Data Studio allows you to export or import connection details from the connection dialog for future reference.

The following parameters can be exported:

-  SSL Mode
-  Connection name
-  Server IP
-  Server Port
-  Database Name
-  Username
-  clSSLCertificatePath
-  clSSLKeyPath
-  profileId
-  rootCertFilePathText
-  connctionDriverName
-  schemaExclusionList
-  schemaInclusionList
-  loadLimit
-  privilegeBasedObAcess
-  databaseVersion
-  savePrdOption
-  dbType
-  version

Perform the following steps to import or export a connection configuration file:

#. Click **File** in the menu bar.

   The following window is displayed:

   |image1|

#. Select **Export Connections** to export a connection configuration file.

   The **Export Connection Profiles** dialog box is displayed. You can select the connections to be exported in this dialog box.

   |image2|

   Select the connections you want to export and enter the name of the file where the exported connections will be saved. Click **OK**.

   Select the location where you want to save the file and click **OK**.

   |image3|

   The following dialog box is displayed after the connections are exported.

   |image4|

#. Select **Import Connections** to import a connection configuration file.

#. Select the file you want to import and click **Open**.

   |image5|

   If the connections to be imported match the existing ones, a dialog box is displayed as follows.

   |image6|

   -  **Replace**: The imported connection configuration file will be replaced with the existing one.
   -  **Copy, but keep both files**: The imported connection configuration file will be renamed.
   -  **Don't Copy**: The existing connection configuration file will remain unchanged.
   -  **Do this for all conflicts**: The same operation will be repeated for all the matches.

   Click any of the preceding options as required and click **OK**.

.. note::

   **Password** and **SSL password** parameters will not be exported.

.. |image1| image:: /_static/images/en-us_image_0000001098833224.png
.. |image2| image:: /_static/images/en-us_image_0000001098673396.jpg
.. |image3| image:: /_static/images/en-us_image_0000001098833226.png
.. |image4| image:: /_static/images/en-us_image_0000001145833081.jpg
.. |image5| image:: /_static/images/en-us_image_0000001145513225.jpg
.. |image6| image:: /_static/images/en-us_image_0000001098833222.jpg
