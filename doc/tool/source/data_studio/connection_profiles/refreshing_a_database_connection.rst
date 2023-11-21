:original_name: DWS_DS_39.html

.. _DWS_DS_39:

Refreshing a Database Connection
================================

Follow the steps below to refresh the database connection:

#. In the **Object Browser** pane, right-click the selected connection name and select **Refresh** or press **F5**.

   The status bar displays the status of the completed operation.

The time taken to refresh the database depends on the number of objects present in the database. For a large database, it is recommended to perform this operation only if required.

-  If you right-click the connection name and select **Refresh**, the connection profile is refreshed. During refresh, the connection will be updated with the latest copy from the server.

-  If you right-click the Schema and select **Refresh**, all functions/procedures and tables under the schema are refreshed. During refresh, all functions/procedures and tables are updated with the latest copy from the server.

   If any stored function/procedure is deleted from the database before the refresh operation, then it will be removed from the **Object Browser** only after you perform the refresh operation.

-  If you right-click a specific function/procedure and select **Refresh**, the specific function/procedure is refreshed. During refresh, the specific function/procedure is updated with the latest copy from the server.
-  If you refresh at database level or connection profile level, then all the child objects of schema in **search_path** along with the schema already expanded by the user will be loaded.
-  If you re-connect to the Database, then only schema objects saved under **search_path** will be loaded. Previously expanded objects will not be loaded.
-  Database and multiple objects under a database cannot be refreshed simultaneously.

Exporting/Importing Connection Details
--------------------------------------

Data Studio provides the option to export/import connection details from the connection dialog for future reference.

The following fields are exported:

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

Follow the steps to access the feature:

#. Click **File** on Menu Bar.

   The following window is displayed:

   |image1|

#. Select **Export Connections** to export the configuration files.

   The **Export Connection Profiles** dialog box is displayed. You can select the links to be exported in this window.

   |image2|

   Select the connections you want to export and enter a file name where the exported connections will be saved. Click **OK**.

   Select the location where you want to save the file and click **OK**.

   |image3|

   A dialog box is displayed once the connections are exported successfully.

   |image4|

#. Select **Import Connections** to import the configuration files.

#. Select the file you want to import and click **Open**.

   |image5|

   If the connection to be imported matches the existing connection, a dialog box is displayed as follows:

   |image6|

   -  **Replace**: The imported connection configuration file will be replaced with the existing one.
   -  **Copy, but keep both files**: The imported connection configuration file will be renamed.
   -  **Don't Copy**: The existing connection configuration file will remain unchanged.
   -  **Do this for all conflicts**: The same operation will be repeated for all the matches.

   Click any of the given options as required and click **OK**.

.. note::

   Password and SSL password fields will not be exported.

.. |image1| image:: /_static/images/en-us_image_0000001234200723.png
.. |image2| image:: /_static/images/en-us_image_0000001188362650.jpg
.. |image3| image:: /_static/images/en-us_image_0000001188202684.png
.. |image4| image:: /_static/images/en-us_image_0000001188362648.jpg
.. |image5| image:: /_static/images/en-us_image_0000001188521202.jpg
.. |image6| image:: /_static/images/en-us_image_0000001233800795.jpg
