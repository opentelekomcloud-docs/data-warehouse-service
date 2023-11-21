:original_name: DWS_DS_34.html

.. _DWS_DS_34:

Adding a Connection
===================

Performing the following steps to establish a new database connection.

#. Choose **File > New Connection** from the main menu, or

   click |image1| on the toolbar, or press **Ctrl+N** to connect to the database. The **New Database Connection** dialog box is displayed.

   .. note::

      While establishing a connection, if the preference file is corrupted or the preferences values are invalid, then an error message is displayed informing you that preference values are invalid and default values are set for preferences. To complete establishing a new database connection operation, click **OK**.

#. The table on the left lists the details of the existing connection profile(s) used to connect to the database along with the server information.

   .. note::

      The server information will be displayed only after one successful connection.

   -  Double clicking a connection name populates the connection parameters such as **Connection Name**, **Host**, and **Host Port**.

   .. note::

      If password is corrupted for any of the existing connection profile or the key is corrupted, then the password field needs to be filled in for all created connections.

   -  Clicking |image2| displays different pop-up messages based on the connection status of database.

      -  If the database connection is active, then **Remove Connection Confirmation** pop-up is displayed. Click **Yes** to disconnect all databases.

      -  If the database connection is not active, then **Remove Connection Confirmation** pop-up is displayed.

   -  Clicking |image3| without a connection name displays a pop-up stating to select at least one connection profile.

#. Provide the following credentials to enter a new set of parameters to connect to the database:

   .. note::

      -  You can click **Clear** to clear all fields in the **New Database Connection** dialog box.
      -  Use shortcut key (**Ctrl+V**) to paste data in Connection window. Data Studio does not support right-click options for all dialog boxes.

   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Field Name            | Description                                                                                                                                                                                    | Example               |
   +=======================+================================================================================================================================================================================================+=======================+
   | Database Type         | Select the database type.                                                                                                                                                                      | ``-``                 |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Connection Name       | Provide a connection name.                                                                                                                                                                     | My_Connection_DB      |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Host                  | Provide the IP address (IPv4) or server domain name.                                                                                                                                           | db.dws.mycloud.com    |
   |                       |                                                                                                                                                                                                |                       |
   |                       | .. note::                                                                                                                                                                                      | 10.xx.xx.xx           |
   |                       |                                                                                                                                                                                                |                       |
   |                       |    -  If domain name length is greater than 25 characters, then the complete domain name will not be displayed.                                                                                |                       |
   |                       |                                                                                                                                                                                                |                       |
   |                       |       **Example:** *test1(db.dws…com:25xxx)*                                                                                                                                                   |                       |
   |                       |                                                                                                                                                                                                |                       |
   |                       |    -  Hovering over the connection name once the connection is established will show the server IP and version.                                                                                |                       |
   |                       |                                                                                                                                                                                                |                       |
   |                       |    -  Entry made in this field will be validated for IP address if it has format of digits with three separators (.). Any entry not meeting this validation will be considered as domain name. |                       |
   |                       |                                                                                                                                                                                                |                       |
   |                       |    -  A typical domain name:                                                                                                                                                                   |                       |
   |                       |                                                                                                                                                                                                |                       |
   |                       |       -  Starts with a letter.                                                                                                                                                                 |                       |
   |                       |       -  Allows letters, digits, hyphens (-), and period (.). All other special characters are not allowed.                                                                                    |                       |
   |                       |       -  Does not allow space/tabs.                                                                                                                                                            |                       |
   |                       |       -  Length cannot exceed 253 characters and a maximum of 63 characters is allowed in between periods.                                                                                     |                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Host Port             | Provide the port address.                                                                                                                                                                      | 8000                  |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Database Name         | Provide the database name.                                                                                                                                                                     | gaussdb               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | User Name             | Provide the user name to connect to the selected database.                                                                                                                                     | ``-``                 |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Password              | Provide the password to connect to the database. The password text is masked.                                                                                                                  | ``-``                 |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   -  Select an option from the **Save Password** drop-down list. The options available are:

      -  **Permanently**: Saves the password even after exiting Data Studio. While establishing the connection for the first time this option will not be available. To hide or view this drop-down option, see the :ref:`Password <en-us_topic_0000001234042179__en-us_topic_0185264975_section1327033652211>` section.
      -  **Current Session Only**: Saves the password only for the current session.
      -  **Do Not Save**: Does not save the password. If this option is selected, Data Studio will prompt for the password for certain operations like:

         -  :ref:`Creating a Database <dws_ds_41>`
         -  :ref:`Renaming a Database <dws_ds_45>`
         -  :ref:`Debugging a PL/SQL Function <dws_ds_62>`
         -  :ref:`Working with SQL Terminals <dws_ds_128>`

   -  **Enable SSL** check box is selected by default.

#. Follow the steps below to enable SSL:

   a. Select the **Enable SSL** option.

   b. Click the **SSL** tab.

      |image4|

   c. Provide the following information. The following files are required for secured connection. Refer to :ref:`SSL Certificates <dws_ds_154>` section.

      -  To select the **Client SSL Certificate**, click |image5| and select the Client SSL Certificate.
      -  To select the **Client SSL Key**, click |image6| and select the Client SSL key.
      -  To select the **Root Certificate**, click |image7| and select the Root Certificate.
      -  Select the SSL Mode from **SSL Mode** drop-down. Refer to table below for description of different SSL modes.

         .. note::

            -  If **SSL Mode** is set to **verify-ca**, **Root Certificate** must be selected.
            -  DS prompt for the Client key while accessing the gs_dump feature for the first time.

         +-----------+---------------------------------------------------------------------------------------------------------------+
         | SSL Mode  | Description                                                                                                   |
         +===========+===============================================================================================================+
         | require   | Selecting require will not check the validity of the certificates since a non-validating SSL factory is used. |
         +-----------+---------------------------------------------------------------------------------------------------------------+
         | verify-ca | Selecting verify-ca checks if the CA is correct using a validating SSL factory.                               |
         +-----------+---------------------------------------------------------------------------------------------------------------+

         .. note::

            -  Selecting **Client SSL Certificate** and **Client SSL Key** ensures secured connection for export of DDL and data using Data Studio.
            -  Selecting invalid file for Client SSL Certificate and/or Client SSL Key will result in export failure. For details, see :ref:`Troubleshooting <dws_ds_145>`.
            -  If you deselect **Enable SSL** check box and proceed, then **Connection Security Alert** dialog box is displayed. Refer to :ref:`Security Disclaimer <en-us_topic_0000001234042179__en-us_topic_0185264975_section8272203611226>` for information to display this security alert or not.

               -  **Continue**: Continues to use insecure connections.
               -  **Cancel**: Enables SSL.
               -  **Do not show again**: If you select this option, the **Connection Security Alert** dialog box is not displayed for the subsequent connections of logged Data Studio instances.

            -  Refer to server manuals for more details.

#. Follow the steps below to set the **Fast Load Options**:

   a. Click the **Advanced** tab.

      |image8|

   b. Enter the schema names using comma separator to load on priority while establishing a connection in the **Include** field.

   c. Enter the schema names using comma separator to avoid loading on priority while establishing a connection in the **Exclude** field.

   d. Select an option from the **Load Objects** options. The options available are:

      -  **All Objects**: Loads all objects.
      -  **Objects allowed as per user privilege**: Loads only objects for which the user has permissions. For details about the minimum access permissions for objects listed in the object browser, see :ref:`Table 1 <en-us_topic_0000001234200679__en-us_topic_0185264599_table18121154132>`.

      .. note::

         The default value is **Objects allowed as per user privilege**.

   e. Enter the load limit in **Load Limit** field. The maximum value allowed is 30000. This is the database object count.

      .. note::

         -  If the number of object types (tables, view..) of the schema mentioned in the **Include** field is greater than the value entered in the **Load Limit** field, then the only the parent objects for a schema will be loaded. This implies that child objects like columns, constraints, indexes, functions with more than three parameters, and so on will not be loaded.
         -  Schema names provided in the Include and Exclude lists are validated.

         -  If you do not have access to the schema name entered in the **Include** field, then an appropriate error message is displayed for that schema during connection.
         -  If you do not have access to the schema name entered in the **Exclude** field, then the schema will not be loaded in **Object Browser** after connection is established.

#. Click **OK** to establish the connection successfully.

   The status bar displays the status of the completed operation.

   While Data Studio is connecting to the database, the following status bar shows the status:

   |image9|

   Once the connection is established, all schema objects will be displayed in the **Object Browser** pane.

   .. note::

      -  Data Studio allows you to log in even if the password has expired with a message informing that some operations may not work as expected. For details, see :ref:`Password Expiry <en-us_topic_0000001234042179__en-us_topic_0185264975_section56881736153111>`.
      -  To cancel the connection, see :ref:`Canceling the Connection <en-us_topic_0000001188521154__en-us_topic_0185264624_section12413942172715>`.
      -  Postgres specific schemas are not displayed in the Object Browser.

.. _en-us_topic_0000001188521154__en-us_topic_0185264624_section12413942172715:

Canceling the Connection
------------------------

Follow the steps below to cancel the connection:

#. Click **Cancel**.

   The **Cancel Connection** dialog box is displayed.

#. Click **Yes**.

   A message confirmation dialog box is displayed.

#. Click **OK**.

Lazy Loading
------------

Lazy loading feature delays the loading of objects until required.

When you connect to a database only child objects of schema saved under **search_path** will be loaded as shown below:

|image10|

Unloaded schemas are displayed as *schema name* **(...)**.

|image11|

To load child objects, expand the schema. During expansion of schema, the objects under the schema will show as loading:

|image12|

.. note::

   If you try to load an unloaded object while loading is in progress for another object, a pop-up message is displayed informing you that another loading is in progress. The |image13| icon next to the unloaded object disappears. Refresh at the object or database level to display this icon again for loading.

   Expand schema to load and view the child objects. The Object Browser can load child objects of only one schema at a time.

If **search_path** is modified after establishing connection, then the changes will be reflected only after reconnecting the database. Auto-suggest works on keywords, data types, schema names, table names, views, and table name aliases for all schema objects that you have access.

A maximum of 50,000 objects will be loaded in the **Object** **Browser** pane within 1 minute.

The database connection timeout interval defaults to 3 minutes (180 seconds). If the connection fails within this interval, a timeout error is displayed.

You can set the **loginTimeout** value in the **Data Studio.ini** file located in the **Data Studio\\** directory.

.. note::

   When you log in to the Data Studio, **pg_catalog** is loaded automatically.

.. |image1| image:: /_static/images/en-us_image_0000001188202890.png
.. |image2| image:: /_static/images/en-us_image_0000001188681178.jpg
.. |image3| image:: /_static/images/en-us_image_0000001234200783.jpg
.. |image4| image:: /_static/images/en-us_image_0000001188202742.png
.. |image5| image:: /_static/images/en-us_image_0000001188521408.jpg
.. |image6| image:: /_static/images/en-us_image_0000001188681324.jpg
.. |image7| image:: /_static/images/en-us_image_0000001234200929.jpg
.. |image8| image:: /_static/images/en-us_image_0000001188521258.png
.. |image9| image:: /_static/images/en-us_image_0000001234042295.png
.. |image10| image:: /_static/images/en-us_image_0000001188521260.png
.. |image11| image:: /_static/images/en-us_image_0000001188681176.jpg
.. |image12| image:: /_static/images/en-us_image_0000001188202744.png
.. |image13| image:: /_static/images/en-us_image_0000001188362710.png
