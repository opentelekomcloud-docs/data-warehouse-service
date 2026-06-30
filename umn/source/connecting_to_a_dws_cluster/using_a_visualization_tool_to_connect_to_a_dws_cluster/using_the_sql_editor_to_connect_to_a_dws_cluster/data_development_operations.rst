:original_name: dws_01_0804.html

.. _dws_01_0804:

Data Development Operations
===========================

Metadata management includes the hierarchical display of metadata. The hierarchy begins with the data source at the root, branching into databases and user roles. Databases include system schemas, user schemas, and foreign servers. System schemas and user schemas are distinguished by OIDs and system schemas cannot be changed or deleted. User schemas include common/partitioned tables, foreign tables, views, functions, sequences, and synonyms. A table contains columns, constraints, indexes, partitions, and triggers. The LIST and INFO APIs are provided to query the metadata lists and details.

The following figure shows the metadata list. Currently, databases, schemas, common tables, fields, indexes, constraints, and partitions can be added.


.. figure:: /_static/images/en-us_image_0000002235495244.png
   :alt: **Figure 1** Metadata information hierarchy

   **Figure 1** Metadata information hierarchy

.. _en-us_topic_0000002235334760__section1111952082518:

Creating a Database
-------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source**. After a :ref:`data source is connected <dws_01_0803>`, right-click the database name and click **Create Database**.
#. On the **Create Database** page, set the parameters as required.

   .. table:: **Table 1** Parameters for creating a database

      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter        | Description                                                                                                                            |
      +==================+========================================================================================================================================+
      | Database Name    | Database name                                                                                                                          |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Owner            | Database owner                                                                                                                         |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Compatibility    | Database compatibility mode. The available options include **Oracle**, **MySQL**, and **Teradata**. The default setting is **Oracle**. |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Encoding         | Encoding mode of the database. SQL_ASCII is recommended.                                                                               |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Connection Limit | The value cannot be less than **-1**. The value **-1** indicates that there is no limit.                                               |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Description      | Description of the database                                                                                                            |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | SQL Preview      | You can click **Preview** to view the SQL syntax for creating the database.                                                            |
      +------------------+----------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

.. _en-us_topic_0000002235334760__section1137116196325:

Adding a Schema
---------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source**. The database created in :ref:`Creating a Database <en-us_topic_0000002235334760__section1111952082518>` contains user schemas, system schemas, and external servers. System schemas can only be viewed.
#. Right-click a user schema name and select **Create Schema**.
#. On the **Create Schema** page, set parameters as required.

   .. table:: **Table 2** Parameters for creating a schema

      +-------------+----------------------------------------------------------------------+
      | Parameter   | Description                                                          |
      +=============+======================================================================+
      | Schema Name | Schema name                                                          |
      +-------------+----------------------------------------------------------------------+
      | Owner       | Schema owner                                                         |
      +-------------+----------------------------------------------------------------------+
      | Description | Schema description                                                   |
      +-------------+----------------------------------------------------------------------+
      | SQL Preview | Click **Preview** to display the SQL syntax for creating the schema. |
      +-------------+----------------------------------------------------------------------+

#. Click **OK**.

.. _en-us_topic_0000002235334760__section10598644105110:

Adding a Common Table
---------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Switch to the **Data Source** panel and add a schema (for details, see :ref:`Adding a Schema <en-us_topic_0000002235334760__section1137116196325>`). A schema contains structures such as common tables, foreign tables, views, functions, sequences, and synonyms.
#. Right-click the name of the common table and click **Create Common Table** to add a table. The dialog box for adding a common table contains options such as **Attribute**, **Column**, **Data Distribution**, **Partition**, **Index**, and **Constraint**. The **Attribute** and **Column** fields are mandatory. You can click **SQL Preview** to query the SQL statement for creating a table.

   .. table:: **Table 3** Parameters for adding a data table

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tab                               | Description                                                                                                                                                                                                     |
      +===================================+=================================================================================================================================================================================================================+
      | Attribute                         | -  **Data Table Name**: Set the data table name.                                                                                                                                                                |
      |                                   | -  **Table Orientation**: You can select **ROW** or **COLUMN**.                                                                                                                                                 |
      |                                   | -  **Available or not Partition**: Select whether to create a partitioned table.                                                                                                                                |
      |                                   | -  **Description**: Description of the new data table.                                                                                                                                                          |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Column                            | Click **Add Column** and set the following parameters:                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                 |
      |                                   | -  **Column Name**: Set the column name.                                                                                                                                                                        |
      |                                   | -  **Data Type**: Select the data type of the new column from the drop-down list box.                                                                                                                           |
      |                                   | -  **Length**: Total number of digits. If this parameter is dimmed, the length is fixed.                                                                                                                        |
      |                                   | -  **Precision**: Number of decimal places. If this parameter is dimmed, the precision cannot be set.                                                                                                           |
      |                                   | -  **Non-null**: Select whether the new column is not null.                                                                                                                                                     |
      |                                   | -  **Unique**: Select whether the new column is unique.                                                                                                                                                         |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Data Distribution                 | The options are as follows:                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                 |
      |                                   | -  **ROUNDROBIN**: Each row of data in the table is sent to each DN in sequence.                                                                                                                                |
      |                                   | -  **REPLICATION**: Each row of data in the table exists on all DNs. Each DN has complete table data.                                                                                                           |
      |                                   | -  **HASH**: Specified columns are hashed and data is distributed to specified DNs through mapping.                                                                                                             |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Partition                         | On the **Partition** panel, you can select a partition type (range partition or list partition) and optional columns (corresponding to table fields). Click **Add Partition** and set the following parameters: |
      |                                   |                                                                                                                                                                                                                 |
      |                                   | -  **Partition Name**: Set the partition name.                                                                                                                                                                  |
      |                                   | -  **Partition Value**: Select a value from the range based on the optional columns.                                                                                                                            |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Indexes                           | Click **Add Index** and set the following parameters:                                                                                                                                                           |
      |                                   |                                                                                                                                                                                                                 |
      |                                   | -  **Index Name**: Set the index name. You can select **Unique Index**.                                                                                                                                         |
      |                                   | -  **Access Mode**: Select an index access mode from the drop-down list box. B-tree indexes are recommended.                                                                                                    |
      |                                   | -  **Index Type**: The options are **Column** and **Expression**.                                                                                                                                               |
      |                                   | -  **Condition Index**: The **WHERE** condition constraint can be added.                                                                                                                                        |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Table Constraints                 | Click **Add Constraint** and set the following parameters:                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                 |
      |                                   | -  **Constraint Type**: The value can be **check**, **unique**, or **primary**.                                                                                                                                 |
      |                                   | -  **Expression** (**check**): Enter field constraints.                                                                                                                                                         |
      |                                   | -  **Constraint Name**: Set the constraint name.                                                                                                                                                                |
      |                                   | -  **Optional Column** (unique\\primary): Select an optional column from the drop-down list box.                                                                                                                |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SQL Preview                       | Click **Preview** to display the SQL syntax for creating a common table.                                                                                                                                        |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Editing a Common Table
----------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source**. You can edit the created common table. For details about how to create a common table, see :ref:`Adding a Common Table <en-us_topic_0000002235334760__section10598644105110>`.
#. Right-click the name of the common table name to modify it. The following table describes the modification operations.

   .. table:: **Table 4** Table modification operations

      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operation                | Description                                                                                                                                                                                                                                                                            |
      +==========================+========================================================================================================================================================================================================================================================================================+
      | Modifying a common table | You can click **Modify** to modify the name and schema of a table, and specify whether it is a partitioned table.                                                                                                                                                                      |
      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Deleting a common table  | Click **Delete** to delete a common table.                                                                                                                                                                                                                                             |
      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operating columns        | Click **Operate Column** to add, edit, and delete columns. You can edit the column name, data type, length, and specify whether it is a non-NULL column. Batch adding of columns is also available.                                                                                    |
      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operating indexes        | Click **Operate Index** to add indexes, edit indexes (index names), and delete indexes in batches. Right-click a specified index name and click **Edit Index** to modify the attributes.                                                                                               |
      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operating constraints    | Click **Operate Constraint** to add constraints, edit constraints (constraint names and optional columns), and delete constraints in batches. Right-click a specified constraint name and click **Edit Constraint** to modify the attributes.                                          |
      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operating partitions     | Click **Operate Partition** (unavailable for non-partitioned tables) to add, edit, and delete partitions. You can edit the partition name. Batch adding of partitions is also available. Right-click a specified partition name and click **Edit Partition** to modify the attributes. |
      +--------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Confirm the information and click **OK**.

Viewing Data in a Common Table
------------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source** and right-click the data table name.
#. Click **View Details** to add, filter, edit, and delete data in a common table. Right-click a partition name and select **View Details** to add, filter, edit, or delete partition data.

Checking Views
--------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source**, right-click a view name, and select **View Data** from the shortcut menu to check the views of the database.

Importing Data
--------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source** and right-click the common table name. Click **Import Data** to import data from the local Excel file or OBS bucket file to the common table.

   -  **Local import**: When uploading an Excel file, ensure it is under 30 MB. For CSV files, select separators to divide data in each row and indicate if there is a table header. If there is no header, input data in each row according to the chosen table fields.
   -  **obs import**: Choose a file from the OBS bucket or directory. Supported file types include CSV and TEXT. Set the parameters of the foreign table to be created for importing data to the OBS bucket. Use the OBS foreign table to write the OBS bucket file to the selected common table.

      .. table:: **Table 5** OBS import parameters

         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Parameter                             | Description                                                                                                                                                                                                                                                                                                                                            | Example Value |
         +=======================================+========================================================================================================================================================================================================================================================================================================================================================+===============+
         | storage location                      | Choose a file from the OBS bucket.                                                                                                                                                                                                                                                                                                                     | ``-``         |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | file format                           | Select a file format from the drop-down list. Supported formats include CSV and TEXT.                                                                                                                                                                                                                                                                  | CSV           |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | file encoding                         | Select a file encoding mode from the drop-down list. UTF8 is recommended.                                                                                                                                                                                                                                                                              | UTF8          |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Delimiter                             | Commas (,) are the default separators for CSV files, while tab characters are the default separators for TXT files.                                                                                                                                                                                                                                    | ,             |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | quote (CSV format)                    | Quotation marks are used for CSV files. The value should be a single-byte character and cannot be the same as the delimiter or null parameter.                                                                                                                                                                                                         | #             |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | newline character (TEXT format)       | When importing text data, you can specify the newline character style. The maximum length of the newline character is 10 bytes, and you can use multi-character newline characters. The supported newline characters include common ones like **\\r**, **\\n**, and **\\r\\n**, as well as other characters or character strings like **$** and **#**. | ``\r``        |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Whether not to escape (TEXT format)   | You can specify whether to escape the backslash (\\) and its following characters in the TEXT format.                                                                                                                                                                                                                                                  | Yes           |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Null value                            | You can specify how null values are represented in a data file.                                                                                                                                                                                                                                                                                        | $             |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Number of data format errors          | Maximum number of data format errors allowed during data import. The value **-1** indicates that the number of errors is not limited.                                                                                                                                                                                                                  | -1            |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Does it contain a header (CSV format) | You can specify if the exported CSV file should contain a header row that describes each column in the table. This parameter only applies to CSV files.                                                                                                                                                                                                | Yes           |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | Whether to ignore missing fields      | Enabling this function sets the last column of a row in a data source file to **NULL** if it is missing, without reporting an error message.                                                                                                                                                                                                           | Yes           |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | ignore extra data                     | You can specify whether to ignore excessive columns when the number of columns in a source data file exceeds that defined in the foreign table.                                                                                                                                                                                                        | Yes           |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
         | compatible illegal chars              | Enabling this function allows for invalid characters during data import.                                                                                                                                                                                                                                                                               | Yes           |
         +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

#. Click **OK**.
#. In the upper right corner of the page, choose **Common Functions** > **Import data list** and check whether the import is successful.

Submitting a SQL Task
---------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Sources**, log in to the data source for which you want to submit the SQL task, and select the corresponding database and schema.
#. Enter a non-query SQL statement in the text box and click **Submit SQL task** to submit the selected SQL statement to the background task for execution. A maximum of 100 non-query SQL statements can be submitted at a time. A maximum of five SQL tasks can be executed by a user.
#. Choose **Common Functions** > **SQL task list** in the upper right corner of the page to view the executed SQL tasks.
#. Click **Details** in the **Operation** column to view the execution status of each SQL statement.

Diagnosing a Plan
-----------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Sources**, log in to the data source for which you need to enter SQL statements, and select the corresponding database and schema.
#. Enter a query SQL statement in the text box and click **Planned diagnostics**. You can select **EXPLAIN** or **PERFORMANCE**. Only one query SQL statement can be diagnosed at a time. If you enter multiple query SQL statements, the first SQL statement is diagnosed by default. If you select **PERFORMANCE**, the entered SQL statement is executed and the result is returned.
#. Click **OK**. The **Planned diagnostics** page is displayed. You can view the SQL statement diagnosis result and plan diagnosis visualization result.

   -  Click **SQL diagnostics** to view the SQL statement formatting and diagnosis items.
   -  Click **Planned diagnostics** to display the plan tree node and plan diagnosis result of the SQL statement.

Viewing Statistics About Databases, Schemas, and Tables
-------------------------------------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. Click **Data Source** and double-click a database name. On the database list page that is displayed, you can search and check the detailed information about a database.
#. Click a database name in the database list to go to the schema list and view the total number of tables, total table size, and index size.
#. Click a schema name in the schema list to go to the common table list. You can view the number of rows in the table, table size, and index size.

.. _en-us_topic_0000002235334760__section383484418124:

Creating a Directory
--------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor** and click the **Scripts** tab.
#. Click **Create Directory**.

   -  **Save to Directory**: Select a parent directory from the drop-down list box. If this parameter is left blank, a level-1 directory is created by default.
   -  **Directory Name**: Set the directory name. The value can contain only letters, digits, and underscores (_).

#. Confirm the information and click **OK**.

Adding a Script
---------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor** and click the **Scripts** tab.
#. Click **Create Script**.

   -  **Save to Directory**: Select the new directory from the drop-down list box. This option is optional.
   -  **Script Name**: Set the script name. Only letters, digits, and underscores (_) are supported.
   -  **OBS Bucket**: Name of the OBS bucket for storing script files. If no OBS bucket is available, click **View OBS Bucket** to access the OBS console and create one. For details, see "Console Operation Guide" > "Managing Buckets" > "Creating a Bucket" in *Object Storage Service User Guide*.
   -  **Path**: User-defined directory for storing script files on OBS. Multi-level directories can be separated by slashes (/). The value is a string containing 1 to 50 characters, which cannot start with a forward slash (/). If you do not set this parameter, the system automatically adds a path by default.
