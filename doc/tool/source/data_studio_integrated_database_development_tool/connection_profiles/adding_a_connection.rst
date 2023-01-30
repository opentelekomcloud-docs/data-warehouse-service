:original_name: DWS_DS_34.html

.. _DWS_DS_34:

Adding a Connection
===================

Perform the following steps to create a database connection.

#. Choose **File** > **New Connection** from the main menu.

   Alternatively, click |image1| on the toolbar, or press **Ctrl+N** to connect to the database. The **New Database Connection** dialog box is displayed.

   .. note::

      If the preference file is damaged or the preference settings are invalid during connection creation, an error message will be displayed indicating that the preferred value is invalid and prompting you to restore the default preference settings. Click **OK** to complete the operation of creating a database connection.

#. The table in the left of the dialog box lists the details of existing connections and server information.

   .. note::

      The server information will be displayed only after the connection succeeds.

   -  You can populate connection parameters, such as **Connection Name**, **Host**, and **Host Port**, by double-clicking the connection name.

   .. note::

      If the password or key for any of the existing connections is damaged, you need to enter the password for whichever connection you use.

   -  If you click |image2|, different pop-up messages based on the connection statuses of database will be displayed.

      -  If the database connection is active, the **Remove Connection Confirmation** dialog box is displayed. Click **Yes** to disconnect all databases.

      -  If the database connection is not active, the **Remove Connection** dialog box is displayed.

   -  If you click |image3| without selecting a connection name, a dialog box is displayed prompting you to select at least one connection.

#. Configure the following parameters to create a database connection.

   .. note::

      -  Click **Clear** to clear all fields in the **New Database Connection** dialog box.
      -  Press **Ctrl+V** to paste data in the **New Database Connection** dialog box. Right-click options are not available in the dialog boxes of Data Studio.

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Field Name            | Description                                                                                                                                                             | Example                 |
   +=======================+=========================================================================================================================================================================+=========================+
   | Database Type         | Specifies the database type.                                                                                                                                            | ``-``                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Connection Name       | Specifies the connection name.                                                                                                                                          | My_Connection_DB        |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Host                  | Specifies the host IP address (IPv4) or database domain name.                                                                                                           | db.dws.mycloud.com      |
   |                       |                                                                                                                                                                         |                         |
   |                       | .. note::                                                                                                                                                               | 10.\ *xx*.\ *xx*.\ *xx* |
   |                       |                                                                                                                                                                         |                         |
   |                       |    -  If domain name length is greater than 25 characters, the complete domain name will not be displayed.                                                              |                         |
   |                       |                                                                                                                                                                         |                         |
   |                       |       For example, *test1(db.dws…com:25xxx)* cannot be displayed completely.                                                                                            |                         |
   |                       |                                                                                                                                                                         |                         |
   |                       |    -  Once the connection is created, hover the cursor over the connection name to see the server IP and version.                                                       |                         |
   |                       |                                                                                                                                                                         |                         |
   |                       |    -  The value of this field will be identified as an IP address if it contains only digits with three periods (.). Otherwise, it will be identified as a domain name. |                         |
   |                       |                                                                                                                                                                         |                         |
   |                       |    -  A domain name must meet the following requirements:                                                                                                               |                         |
   |                       |                                                                                                                                                                         |                         |
   |                       |       -  Starts with a letter.                                                                                                                                          |                         |
   |                       |       -  Contains only letters, digits, hyphens (-), and periods (.) and cannot contain any other special character.                                                    |                         |
   |                       |       -  Cannot contain spaces or tabs.                                                                                                                                 |                         |
   |                       |       -  Contains 1 to 253 characters and a maximum of 63 characters between periods.                                                                                   |                         |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Host Port             | Specifies the port address.                                                                                                                                             | 8000                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Database Name         | Specifies the database name.                                                                                                                                            | gaussdb                 |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | User Name             | Specifies the username for connecting to the selected database.                                                                                                         | ``-``                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Password              | Specifies the password for connecting to the selected database. The password is masked.                                                                                 | ``-``                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+

   -  In the **Save Password** drop-down list, select one of the following options:

      -  **Permanently**: The password is saved even after you exit the database. This option is unavailable upon the first connection creation. To hide or view this option, see :ref:`Save Password Permanently <en-us_topic_0000001098673336__en-us_topic_0185264975_section1327033652211>`.
      -  **Current Session Only**: The password is saved only for the current session.
      -  **Do Not Save**: The password is not saved. If you select this option, Data Studio will prompt you to enter the password for the following operations:

         -  :ref:`Creating a Database <dws_ds_41>`
         -  :ref:`Renaming a Database <dws_ds_45>`
         -  :ref:`Debugging a PL/SQL Function <dws_ds_62>`
         -  :ref:`Using SQL Terminals <dws_ds_128>`

   -  The **Enable SSL** option is selected by default.

#. Perform the following steps to enable SSL:

   a. Select the **Enable SSL** option.

   b. Click the **SSL** tab.

      |image4|

   c. Provide the files listed in :ref:`SSL Certificates <dws_ds_154>` to use secure connections.

      -  Click |image5| of **Client SSL Certificate** and select a client SSL certificate.
      -  Click |image6| of **Client SSL Key** and select a client SSL key.
      -  Click |image7| of **Root Certificate** and select a root certificate.
      -  Select an SSL mode from the **SSL Mode** drop-down list. Refer to table below for description of different SSL modes.

         .. note::

            -  If **SSL Mode** is set to **verify-ca** or **verify-full**, **Root Certificate** must be selected.
            -  Data Studio prompts you to enter the client key upon the first access to the **gs_dump** feature.

         +-------------+--------------------------------------------------------------------------------------------+
         | SSL Mode    | Description                                                                                |
         +=============+============================================================================================+
         | require     | The certificate will not be verified as the used SSL factory does not need to be verified. |
         +-------------+--------------------------------------------------------------------------------------------+
         | verify-ca   | The certificate authority (CA) will be verified using the corresponding SSL factory.       |
         +-------------+--------------------------------------------------------------------------------------------+
         | verify-full | The CA and database will be verified using the corresponding SSL factory.                  |
         +-------------+--------------------------------------------------------------------------------------------+

         .. note::

            -  You can a valid **Client SSL Certificate** and **Client SSL Key** to export DDL and data from Data Studio using secure connections.
            -  If the selected **Client SSL Certificate** and **Client SSL Key** are invalid, the export will fail. For details, see :ref:`Troubleshooting <dws_ds_145>`.
            -  If you deselect **Enable SSL** and proceed, the **Connection Security Alert** dialog box is displayed. Refer to :ref:`Security Disclaimer <en-us_topic_0000001098673336__en-us_topic_0185264975_section8272203611226>` to determine whether to display this dialog box.

               -  **Continue**: continues to use insecure connections
               -  **Cancel**: enables SSL
               -  **Do not show again**: The **Connection Security Alert** dialog box is not displayed for the subsequent connections of logged Data Studio instances.

            -  For details, see the server manual.

#. Perform the following steps to set **Fast Load Options**:

   a. Click the **Advanced** tab.

      |image8|

   b. When creating a connection, enter the schema names separated by commas in the **Include** field to load these schemas preferentially.

   c. When creating a connection, enter the schema names separated by commas in the **Exclude** field to avoid loading these schemas preferentially.

   d. Select either of the following options for **Load Objects**:

      -  **All objects**: loads all objects
      -  **Objects allowed as per user privilege**: loads only objects that the user has permissions for accessing. For details about the minimum permissions for accessing objects listed in **Object Browser**, see :ref:`Table 1 <en-us_topic_0000001098833174__en-us_topic_0185264599_table18121154132>`.

      .. note::

         The default value is **Objects allowed as per user privilege**.

   e. Enter the number of database objects that can be loaded in **Load Limit**. The maximum number is 30,000.

      .. note::

         -  If the number of object types (such as tables and views) of the schema entered in **Include** is greater than the value of **Load Limit**, only the parent objects of the schema will be loaded. This indicates that child objects containing more than three parameters will not be loaded, such as columns, constraints, indexes, and functions.
         -  Schema names provided in **Include** and **Exclude** are validated.

         -  If you cannot access the schema specified in **Include**, an error message of the schema will be displayed during connection.
         -  If you cannot access the schema specified in **Exclude**, the schema will not be loaded in **Object Browser** after the connection is created.

#. Click **OK**.

   The status of the completed operation is displayed in the status bar.

   When Data Studio is connecting to the database, the connection status is displayed as follows:

   |image9|

   Once the connection is created, all schemas will be displayed in the **Object Browser** pane.

   .. note::

      -  Data Studio allows you to login even if the password has expired with a message informing that some operations may not work as expected. For details, see :ref:`Password Expiry <en-us_topic_0000001098673336__en-us_topic_0185264975_section56881736153111>`.
      -  To cancel the connection, see :ref:`Canceling the Connection <en-us_topic_0000001145713081__en-us_topic_0185264624_section12413942172715>`.
      -  PostgreSQL schema names are not displayed in the **Object Browser** pane.

.. _en-us_topic_0000001145713081__en-us_topic_0185264624_section12413942172715:

Canceling the Connection
------------------------

Perform the following steps to cancel the connection:

#. Click **Cancel**.

   The **Cancel Connection** dialog box is displayed.

#. Click **Yes**.

   A confirmation dialog box is displayed.

#. Click **OK**.

Lazy Loading
------------

The lazy loading feature allows objects to be loaded only when you need.

When you connect to a database only child objects of the schema saved under **search_path** will be loaded, as shown in the following figure.

|image10|

Unloaded schemas are displayed as *Schema name* **(...)**.

|image11|

To load child objects, expand the schema. You will see that the objects under the schema are loading.

|image12|

.. note::

   If you try to load an unloaded object while another object is being loaded, a pop-up message is displayed indicating that another object is being loaded. The |image13| next to the unloaded object will disappear, and will be displayed again when you refresh the object or database level to load the object.

   Expand a schema to load and view the child objects. You can load child objects of only one schema at a time in **Object Browser**.

If you modify **search_path** after creating a connection, the modification will take effect only after the database is reconnected. The Auto Suggest feature is applicable to keywords, data types, schema names, table names, views, and table aliases of all schema objects that you have permissions for accessing.

A maximum of 50,000 objects will be loaded in the **Object Browser** pane within one minute.

The database connection timeout interval defaults to 3 minutes (180 seconds). If the connection fails within this interval, a timeout error is displayed.

You can set the **loginTimeout** value in the **Data Studio.ini** file located in the **Data Studio\\** directory.

.. note::

   When you log in to Data Studio, **pg_catalog** is loaded automatically.

.. |image1| image:: /_static/images/en-us_image_0000001145913223.png
.. |image2| image:: /_static/images/en-us_image_0000001145913221.jpg
.. |image3| image:: /_static/images/en-us_image_0000001098833254.jpg
.. |image4| image:: /_static/images/en-us_image_0000001098673430.png
.. |image5| image:: /_static/images/en-us_image_0000001145513339.jpg
.. |image6| image:: /_static/images/en-us_image_0000001098673424.jpg
.. |image7| image:: /_static/images/en-us_image_0000001145913219.jpg
.. |image8| image:: /_static/images/en-us_image_0000001145513259.png
.. |image9| image:: /_static/images/en-us_image_0000001145833117.png
.. |image10| image:: /_static/images/en-us_image_0000001099153234.png
.. |image11| image:: /_static/images/en-us_image_0000001098993256.jpg
.. |image12| image:: /_static/images/en-us_image_0000001098673426.png
.. |image13| image:: /_static/images/en-us_image_0000001098993262.png
