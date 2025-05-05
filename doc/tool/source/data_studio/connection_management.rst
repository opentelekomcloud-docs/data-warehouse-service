:original_name: DWS_DS_34.html

.. _DWS_DS_34:

Connection Management
=====================

When Data Studio is started, the **New Database Connection** dialog box is displayed by default. To perform database operations, Data Studio must be connected to at least one database.

Enter the connection parameters to create a database connection between Data Studio and the database server. Hover the mouse cursor over the connection name to view the database information.

.. _en-us_topic_0000001813598612__section6296113873912:

Adding a Connection
-------------------

#. Choose **File** > **New Connection** from the main menu, or click new connection icon on the toolbar. The **New Database Connection** dialog box is displayed.

#. The table on the left lists the details of the existing connection profile(s) used to connect to the database along with the server information.

   You can populate connection parameters, such as **Connection Name**, **Host**, and **Host Port**, by double-clicking the connection name.

   .. note::

      If the password or key for any of the existing connections is damaged, you need to enter the password for whichever connection you use.

#. Configure the following parameters to create a database connection.

   .. _en-us_topic_0000001813598612__en-us_topic_0185264624_table109756101700:

   .. table:: **Table 1** Database connection information

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Field                 | Description                                                                                                                                                             | Example               |
      +=======================+=========================================================================================================================================================================+=======================+
      | Database Type         | Database type                                                                                                                                                           | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Name                  | Connection name                                                                                                                                                         | My_Connection_DB      |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Host                  | Specifies the host IP address (IPv4) or database domain name.                                                                                                           | db.dws.mycloud.com    |
      |                       |                                                                                                                                                                         |                       |
      |                       | .. note::                                                                                                                                                               | 10.xx.xx.xx           |
      |                       |                                                                                                                                                                         |                       |
      |                       |    -  If domain name length is greater than 25 characters, the complete domain name will not be displayed.                                                              |                       |
      |                       |                                                                                                                                                                         |                       |
      |                       |       For example, *test1(db.dws…com:25xxx)* cannot be displayed completely.                                                                                            |                       |
      |                       |                                                                                                                                                                         |                       |
      |                       |    -  Once the connection is created, hover the cursor over the connection name to see the server IP and version.                                                       |                       |
      |                       |                                                                                                                                                                         |                       |
      |                       |    -  The value of this field will be identified as an IP address if it contains only digits with three periods (.). Otherwise, it will be identified as a domain name. |                       |
      |                       |                                                                                                                                                                         |                       |
      |                       |    -  A domain name must meet the following requirements:                                                                                                               |                       |
      |                       |                                                                                                                                                                         |                       |
      |                       |       -  Start with a letter.                                                                                                                                           |                       |
      |                       |       -  Contains only letters, digits, hyphens (-), and periods (.) and cannot contain any other special character.                                                    |                       |
      |                       |       -  Cannot contain spaces or tabs.                                                                                                                                 |                       |
      |                       |       -  Contains 1 to 253 characters and a maximum of 63 characters between periods.                                                                                   |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Host Port             | Provide the port address.                                                                                                                                               | 8000                  |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Databases             | Specifies the database name.                                                                                                                                            | gaussdb               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Username              | Provide the username to connect to the selected database.                                                                                                               | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password              | Specifies the password for connecting to the selected database. The password is masked.                                                                                 | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Save Password         | -  **Current Session Only**: The password is saved only for the current session.                                                                                        | ``-``                 |
      |                       | -  **Do Not Save**: The password is not saved.                                                                                                                          |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   .. note::

      -  Click **Clear** to clear all fields in the **New Database Connection** dialog box.
      -  Press **Ctrl+V** to paste data in the **New Database Connection** dialog box. Right-click options are not available in the dialog boxes of Data Studio.

#. Select the **Enable SSL** option and click the **SSL** tab. For details, see :ref:`Configuring SSL Connection <dws_ds_008>`.


   .. figure:: /_static/images/en-us_image_0000001813439300.png
      :alt: **Figure 1** Configuring SSL parameters

      **Figure 1** Configuring SSL parameters

#. Perform the following steps to set **Fast Load Options**:

   Click the **Advanced** tab.


   .. figure:: /_static/images/en-us_image_0000001860319005.png
      :alt: **Figure 2** Configuring fast load options

      **Figure 2** Configuring fast load options

   -  Enter the schema names separated by commas in the **Include** field to load these schemas preferentially.
   -  Enter the schema names separated by commas in the **Exclude** field to avoid loading these schemas preferentially.

   -  Select either of the following options for **Load Objects**:

      -  **All Objects**: Loads all objects.
      -  **Objects allowed as per user privilege**: loads only objects that the user has permissions for accessing. For details about the minimum permissions for accessing objects listed in **Object Browser**, see :ref:`Table 2 <en-us_topic_0000001860318645__en-us_topic_0185264599_table18121154132>`.

      .. note::

         The default value is **Objects allowed as per user privilege**.

   -  Enter the number of database objects that can be loaded in **Load Limit**. The maximum number is 30,000.

      .. note::

         -  If the number of object types (such as tables and views) of the schema entered in **Include** is greater than the value of **Load Limit**, only the parent objects of the schema will be loaded. This indicates that child objects containing more than three parameters will not be loaded, such as columns, constraints, indexes, and functions.
         -  Schema names provided in **Include** and **Exclude** are validated.

         -  If you cannot access the schema specified in **Include**, an error message of the schema will be displayed during connection.
         -  If you cannot access the schema specified in **Exclude**, the schema will not be loaded in **Object Browser** after the connection is created.

#. Click **OK**.

   The status bar displays the status of the completed operation.

   When Data Studio is connecting to the database, the connection status is displayed as follows:

   |image1|

   Once the connection is created, all schemas will be displayed in the **Object Browser** pane.

   .. note::

      -  You can still log in to Data Studio even if the password has expired, but a message indicating that some operations may not be performed normally will be displayed. For details about how to change the configurations, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.
      -  PostgreSQL schema names are not displayed in the **Object Browser** pane.

Renaming a Connection
---------------------

#. In the **Object Browser** pane, right-click the selected connection name and select **Rename Connection**.

   A **Rename Connection** dialog box is displayed, prompting you to enter the new connection name.

#. Enter the new connection name. Click **OK** to rename the connection.

   The status bar displays the status of the completed operation.

   .. note::

      The new connection name must be unique. Otherwise, the rename operation will fail.

Editing a Connection
--------------------

#. In the **Object Browser** pane, right-click the selected connection name and select **Edit Connection**. The **Edit Connection** dialog box is displayed.

   To edit an active connection, you need to disable the connection and then open the connection with the new properties. A warning message about connection resetting is displayed.

#. Edit the connection parameters. For details, see :ref:`Table 1 <en-us_topic_0000001813598612__en-us_topic_0185264624_table109756101700>`.

   .. note::

      The database type and name cannot be modified.

#. Click **OK**.

   .. note::

      -  You can click **Clear** to clear all fields in the **Edit Database Connection** dialog box.
      -  If you click **OK** without modifying any connection parameters, a dialog box is displayed, indicating that the modification is not saved. After the connection parameters are modified, a dialog box is displayed.

#. If SSL is not enabled, a **Connection Security Alert** dialog box is displayed. Click **Continue** to proceed with insecure connections or click **Cancel** to return to the **Edit Connection** dialog box to enable SSL.

   **Do not show again** option is used to hide the **Connection Security Alert** dialog box for subsequent connections.

   A dialog box is displayed asking users to confirm whether the database whose connection has been edited is deleted.

#. Click **Yes** to proceed to updating the connection information and reconnecting the connection with the updated parameters.

   The status bar displays the status of the completed operation.

Removing a Connection
---------------------

#. Right-click the selected connection name and select **Remove Connection**.

   A confirmation dialog box is displayed.

#. Click **Yes** to remove the server connection.

   The status bar displays the status of the completed operation.

   This operation will remove the connection from the **Object Browser**. Any unsaved data will be lost.

Refreshing Connection Data
--------------------------

#. In the **Object Browser** pane, right-click the selected connection name and select **Refresh** or press **F5** to update the connection with the latest content on the server.

   The status bar displays the status of the completed operation.

-  The time taken to refresh a database depends on the number of objects in the database. Therefore, you are advised to perform this operation during off-peak hours in a large-scale database.
-  If you refresh the entire database or connection, all child objects of schemas in **search_path**, as well as the schemas already expanded by the user, will be loaded again.
-  If you reconnect to the database, only schema objects saved under **search_path** will be loaded. Objects that have been expanded will not be loaded.
-  A database and multiple objects under it cannot be refreshed simultaneously.

Viewing Connection Properties
-----------------------------

#. Right-click the selected connection and select **Properties**.

   The status bar displays the status of the completed operation.

   Properties of the selected connection are displayed.

   .. note::

      If the property of a created connection is modified, then open the properties of the connection again to view the updated information.

.. |image1| image:: /_static/images/en-us_image_0000001813599088.png
