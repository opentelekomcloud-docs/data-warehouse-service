:original_name: DWS_DS_72.html

.. _DWS_DS_72:

Overview
========

This section describes how to create a common table.

A table is a logical structure maintained by a database administrator and consists of rows and columns. You can define a table as a part of your data definitions from the data perspective. Before defining a table, you need to define a database and a schema. This section describes how to use Data Studio to create a table. To define a table in the database, perform the following steps:

#. In the **Object Browser** pane, right-click **Regular Tables**, and choose **Create Regular Table**.

#. Define basic table information, such as the table name and table type. For details, see :ref:`Providing Basic Information <en-us_topic_0000001234200603__en-us_topic_0185264992_section1344143101920>`.

#. Define column information, such as the column name, data type schema, data type, and column constraint. For details, see :ref:`Defining a Column <en-us_topic_0000001234200603__en-us_topic_0185264992_section546323424>`.

#. For details about how to determine table data distribution settings, see :ref:`Selecting Data Distribution <en-us_topic_0000001234200603__en-us_topic_0185264992_section728115301265>`.

#. Define column constraints for different constraint types. Constraint types include **PRIMARY KEY**, **UNIQUE**, and **CHECK**. For details, see :ref:`Defining Table Constraints <en-us_topic_0000001234200603__en-us_topic_0185264992_section440110125279>`.

#. Define table index information, such as the index name and access mode. For details, see :ref:`Defining an Index <en-us_topic_0000001234200603__en-us_topic_0185264992_section082554911302>`.

   On the **SQL Preview** tab, you can check the automatically generated SQL query. For details, see :ref:`SQL Preview <en-us_topic_0000001234200603__en-us_topic_0185264992_section7772325171112>`.

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section1344143101920:

Providing Basic Information
---------------------------

If you create a table in a schema, the current schema will be used as the schema of the table. Perform the following steps to create a common table:

#. Enter a table name. It specifies the name of the table to be created.

   .. note::

      Select the **Case** check box to retain the capitalization of the value of the **Table Name** parameter. For example, if you enter the table name **Employee**, the table name will be created as **Employee**.

   The name of the table schema is displayed in **Schema**.

#. Select a table storage mode from the **Table Orientation** drop-down list.

#. Select a table type. Its value can be:

   -  **Normal**: a normal table
   -  **Unlogged**: An unlogged table. When data is written to an unlogged table, the data is not recorded in logs. The speed of writing data to an unlogged table is much higher than that of writing data to a common table. However, an unlogged table is insecure. In the case of a conflict or abnormal shutdown, an unlogged table is automatically truncated. The content of unlogged tables is not backed up to the standby node, and no log is automatically recorded when an index is created for unlogged tables.

#. Configure **Options**.

   -  **IF NOT EXISTS**: Create the table only if a table with same name does not exist.
   -  **WITH OIDS**: Create a table and assign OIDs.
   -  Configure **Filler Factor**. The value range is 10 to 100. The default value is 100 (filled to capacity).

   If **Fill Factor** is set to a smaller value, the INSERT operation fills only the specified percentage of a table page. The free space of the page will be used to update rows on the page. In this way, the UPDATE operation can place the updated row content on the original page, which is more efficient than placing the update on a different page. Set it to 100 for a table that has never been updated. Set it to a smaller value for largely updated tables. TOAST tables do not support this parameter.

#. Enter table description in the **Description of Table** text box.

#. Click **Next** to define the column information of the table.

You can configure the following parameters of a common table:

.. table:: **Table 1** Parameters

   ============= =============== ================== =========
   Parameter     Row-store Table Column-store Table ORC Table
   ============= =============== ================== =========
   Table Type    |image1|        |image2|           |image3|
   If Not Exists |image4|        |image5|           |image6|
   With OIDS     |image7|        |image8|           |image9|
   Fill Factor   |image10|       |image11|          |image12|
   ============= =============== ================== =========

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section546323424:

Defining a Column
-----------------

A column defines a unit of information within a table's row. Each row is an entry in the table. Each column is a category of information that applies to all rows. When you add a table to a database, you can define the columns that compose it. Columns determine the type of data that the table can hold. After providing the general information about the table, click the **Columns** tab to define the list of table columns. Each column contains name, data type, and other optional properties.

You can perform the following operations only in a common table:

-  :ref:`Deleting a Column <en-us_topic_0000001234200603__en-us_topic_0185264992_section165532164212>`
-  :ref:`Editing a Column <en-us_topic_0000001234200603__en-us_topic_0185264992_section171818329428>`
-  :ref:`Moving a Column <en-us_topic_0000001234200603__en-us_topic_0185264992_section16456451142110>`

To define a column, perform the following steps:

#. Enter the column name in **Column Name** field. It specifies the name of a column to be created in the new table. This must be a unique name in the table.

   .. note::

      Select the **Case** check box to retain the capitalization of the value of the **Column Name** parameter. For example, if the column name entered is "Name", then the column name is created as "Name".

#. Configure **Array Dimensions**. It specifies the array dimensions for the column.

   **Example:** If array dimension for a column is defined as integer [], then it will add the column data as single dimension array.

   |image13|

   The marks column in the above table was created as single dimension and subject column as two dimensions.

#. Select the data type of the column from the **Data Type** drop-down list. For example, **bigint** for integer values.

   For complex data types,

   -  Select the required schema from the **Data type Schema** drop-down list.
   -  Select the corresponding data type from the **Data Type** drop-down list. This list displays the tables and views for the selected schema.

      .. note::

         User-defined data types are not available for selection.

#. Enter the precision/size value of the data type entered in the **Precision/Size** field. This parameter is valid only when the data type can be defined by precision or size.

#. Select the scale of the data type entered in the **Scale** field.

#. Choose the following **Column Constraints** if required:

   -  **NOT NULL**: The column cannot contain null values.
   -  **UNIQUE**: The column may contain only unique values.
   -  **DEFAULT**: The default value used when no value is defined for the column.
   -  **Check**: An expression producing a Boolean result, which new or updated rows must satisfy for an INSERT or UPDATE operation to succeed.

#. To add comments to **Column** in the **Create Regular Table** dialog box, add column information in **Description of Column (Max 5000 chars)** text box and click **Add**. You can also add comments in the column addition dialog box. You can check comments in general table properties.

#. After you enter all information for new column, click **Add**. You can also delete a column from a list or change the order of columns. After defining all columns, click **Next**.

You can configure the following parameters of a column in a common table:

.. table:: **Table 2** Parameters

   ================ =============== ================== =========
   Parameter        Row-store Table Column-store Table ORC Table
   ================ =============== ================== =========
   Array Dimensions Y               x                  x
   Data Type Schema Y               x                  x
   NOT NULL         Y               Y                  Y
   Default          Y               Y                  Y
   UNIQUE           Y               x                  x
   CHECK            Y               x                  x
   ================ =============== ================== =========

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section165532164212:

Deleting a Column
-----------------

To delete a column, perform the following steps:

#. Select a column.
#. Click **Delete**.

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section171818329428:

Editing a Column
----------------

Follow the steps to edit a column:

#. Select a column.
#. Click **Edit**.
#. Edit the column details as required and click **Update** to save changes.

   .. note::

      You must complete the edit operation and save the changes to continue with other operations.

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section16456451142110:

Moving a Column
---------------

You can move a column in a table. To move a column, select the column and click **Up** or **Down**.

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section728115301265:

Selecting Data Distribution
---------------------------

Data distribution specifies how the table is distributed or replicated among data nodes.

Select one of the following options for the distribution type:

+----------------------+-----------------------------------------------------------------------------------------+
| Distribution Type    | Description                                                                             |
+======================+=========================================================================================+
| DEFAULT DISTRIBUTION | The default distribution type will be assigned for this table.                          |
+----------------------+-----------------------------------------------------------------------------------------+
| REPLICATION          | Each row of the table will be replicated in all the data nodes of the database cluster. |
+----------------------+-----------------------------------------------------------------------------------------+
| HASH                 | Each row of the table will be placed based on the hash value of the specified column.   |
+----------------------+-----------------------------------------------------------------------------------------+
| RANGE                | Each row of the table will be placed based on the range value.                          |
+----------------------+-----------------------------------------------------------------------------------------+
| LIST                 | Each row of the table will be placed based on the list value.                           |
+----------------------+-----------------------------------------------------------------------------------------+

After selecting data distribution, click **Next**.

The following table lists the data distribution parameters that can be configured for common tables.

.. table:: **Table 3** Distribution types

   ==================== =============== ================== =========
   Distribution Type    Row-store Table Column-store Table ORC Table
   ==================== =============== ================== =========
   DEFAULT DISTRIBUTION Y               Y                  x
   HASH                 Y               Y                  Y
   REPLICATION          Y               Y                  x
   ==================== =============== ================== =========

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section440110125279:

Defining Table Constraints
--------------------------

Creating constraints is optional. A table can have one (and only one) primary key. Creating the primary key is a good practice.

You can select the following types of constraints from the **Constraint Type** drop-down list:

-  :ref:`Primary Key <en-us_topic_0000001234200603__en-us_topic_0185264992_section29185940>`
-  :ref:`UNIQUE <en-us_topic_0000001234200603__en-us_topic_0185264992_section61346876>`
-  :ref:`CHECK <en-us_topic_0000001234200603__en-us_topic_0185264992_section15250979>`

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section29185940:

Primary Key
-----------

The primary key is the unique identity of a row and consists of one or more columns.

Only one primary key can be specified for a table, either as a column constraint or as a table constraint. The primary key constraint must name a set of columns that is different from other sets of columns named by any unique constraint defined for the same table.

Set the constraint type to **PRIMARY KEY** and enter the constraint name. Select a column from the **Available Columns** list and click **Add**. If you need a multi-column primary key, repeat this step for another column.

**Fill Factor** for a table is in the range 10 and 100 (unit: %). The default value is 100 (filled to capacity). If **Fill Factor** is set to a smaller value, the INSERT operation fills only the specified percentage of a table page. The free space of the page will be used to update rows on the page. In this way, the UPDATE operation can place the updated row content on the original page, which is more efficient than placing the update on a different page. Set it to 100 for a table that has never been updated. Set it to a smaller value for largely updated tables. TOAST tables do not support this parameter.

**DEFERRABLE**: Defer an option.

**INITIALLY DEFERRED**: Check the constraint at the specified time point.

In the **Constraints** area, click **Add**.

You can click **Delete** to delete a primary key from the list.

Mandatory parameters are marked with asterisks (``*``).

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section61346876:

UNIQUE
------

Set the constraint type to **UNIQUE** and enter the constraint name.

Select a column from the **Available Columns** list and click **Add**. To configure unique for multiple columns, repeat this step for another column. After adding the first column, the UNIQUE constraint name will be automatically filled. The name can be modified.

**Fill Factor**: For details, see :ref:`Primary Key <en-us_topic_0000001234200603__en-us_topic_0185264992_section29185940>`.

**DEFERRABLE**: For details, see :ref:`Primary Key <en-us_topic_0000001234200603__en-us_topic_0185264992_section29185940>`.

**INITIALLY DEFERRED**: For details, see :ref:`Primary Key <en-us_topic_0000001234200603__en-us_topic_0185264992_section29185940>`.

You can click **Delete** to delete UNIQUE from the list.

Mandatory parameters are marked with asterisks (``*``).

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section15250979:

CHECK
-----

Set the constraint type to **CHECK** and enter the constraint name.

When the INSERT or UPDATE operation is performed, and if the check expression fails, then table data is not altered.

If you double-click column in **Available Columns** list, it is inserted to **Check Expression** edit line to current cursor position.

In the **Constraints** area, click **Add**. You can click **Delete** to delete CHECK from the list. Mandatory parameters are marked with asterisks (``*``). After defining all constraints, click **Next**.

The following table lists the table constraint parameters that can be configured for common tables.

.. table:: **Table 4** Constraint types

   =============== =============== ================== =========
   Constraint Type Row-store Table Column-store Table ORC Table
   =============== =============== ================== =========
   CHECK           Y               x                  x
   UNIQUE          Y               x                  x
   PRIMARY KEY     Y               x                  x
   =============== =============== ================== =========

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section082554911302:

Defining an Index
-----------------

Indexes are optional. They are used to enhance database performance. This operation constructs an index on the specified column(s) of the specified table. Select the **Unique Index** check box to enable this option.

Choose the name of the index method from the **Access Method** list. The default method is B-tree.

The **Fill factor** for an index is a percentage that determines how full the index method will try to pack index pages. For B-trees, leaf pages are filled to this percentage during initial index build, and also when extending the index at the right (adding new largest key values). If pages subsequently become completely full, they will be split, leading to gradual degradation in the index's efficiency. B-trees use a default fill factor of 90, but any integer value from 10 to 100 can be selected. If the table is static, then a fill factor of 100 can minimize the index's physical size. For heavily updated tables, an explain plan smaller fill factor is better to minimize the need for page splits. Other indexing methods use different fill factors but work in similar ways. The default fill factor varies between methods.

You can either enter a user-defined expression for the index or you can create the index using the **Available Columns** list. Select the column in the **Available Columns** list and click **Add**. If you need a multi-column index, repeat this step for other columns.

After entering the required information for the new index, click **Add**.

You can also delete an index from the list using the **Delete** button. After defining all indexes, click **Next**.

You can configure the following parameters of an index in a common table.

.. table:: **Table 5** Parameters

   ======================= =============== ================== =========
   Parameter               Row-store Table Column-store Table ORC Table
   ======================= =============== ================== =========
   Unique Indexes          Y               x                  x
   btree                   Y               Y                  x
   gin                     Y               Y                  x
   gist                    Y               Y                  x
   hash                    Y               Y                  x
   psort                   Y               Y                  x
   spgist                  Y               Y                  x
   Fill Factor             Y               x                  x
   User Defined Expression Y               x                  x
   Partial Index           Y               x                  x
   ======================= =============== ================== =========

.. _en-us_topic_0000001234200603__en-us_topic_0185264992_section7772325171112:

SQL Preview
-----------

Data Studio generates a DDL statement based on the inputs provided in **Create New table** wizard.

You can only view, select, and copy the query. You cannot edit the query.

-  To select all queries, press **Ctrl+A** or right-click and select **Select All**.
-  To copy the selected query, press **Ctrl+C** or right-click and select **Copy**.

Click **Finish** to create the table. On clicking the **Finish** button, the generated query will be sent to the server. Any errors are displayed in the dialog box and status bar.

.. |image1| image:: /_static/images/en-us_image_0000001233800817.png
.. |image2| image:: /_static/images/en-us_image_0000001188202704.png
.. |image3| image:: /_static/images/en-us_image_0000001234042261.png
.. |image4| image:: /_static/images/en-us_image_0000001188521224.png
.. |image5| image:: /_static/images/en-us_image_0000001233922307.png
.. |image6| image:: /_static/images/en-us_image_0000001234200743.png
.. |image7| image:: /_static/images/en-us_image_0000001233800819.png
.. |image8| image:: /_static/images/en-us_image_0000001233800813.png
.. |image9| image:: /_static/images/en-us_image_0000001188681140.png
.. |image10| image:: /_static/images/en-us_image_0000001234200747.png
.. |image11| image:: /_static/images/en-us_image_0000001188521220.png
.. |image12| image:: /_static/images/en-us_image_0000001188362668.png
.. |image13| image:: /_static/images/en-us_image_0000001188681144.jpg
