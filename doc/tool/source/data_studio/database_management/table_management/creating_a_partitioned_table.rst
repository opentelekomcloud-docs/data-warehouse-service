:original_name: DWS_DS_018.html

.. _DWS_DS_018:

Creating a Partitioned Table
============================

Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partitioned table, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table.

Follow the steps below to define a table in your database:

#. In the **Object Browser** pane, right-click **Regular Tables**, and choose **Create Partition Table**.

#. Define basic table information, such as the table name and table type. For details, see :ref:`Providing Basic Information <en-us_topic_0000001813598940__en-us_topic_0185264910_section13466165012317>`.

#. Define column information, such as the column name, data type schema, data type, and column constraint. For details, see :ref:`Defining a Column <en-us_topic_0000001813598940__en-us_topic_0185264910_section3109182003214>`.

#. Select the data distribution information for the table. For details, see :ref:`Configuring Data Distribution <en-us_topic_0000001813598940__en-us_topic_0185264910_section183471438456>`.

#. Define column constraints for different constraint types. Constraint types include **PRIMARY KEY**, **UNIQUE**, and **CHECK**. For details, see :ref:`Defining Table Constraints <en-us_topic_0000001813598940__en-us_topic_0185264910_section820017103416>`.

#. Define table index information, such as the index name and access mode. For details, see :ref:`Defining an Index <en-us_topic_0000001813598940__en-us_topic_0185264910_section17537195584514>`.

#. Define the partition information for the table such as partition name, partition column, partition value and so on. For details, see :ref:`Defining a Partition <en-us_topic_0000001813598940__en-us_topic_0185264910_section16127182563613>`.

   On the **SQL Preview** tab, you can check the automatically generated SQL query. For details, see :ref:`SQL Preview <en-us_topic_0000001813598940__en-us_topic_0185264910_section1221111211255>`.

#. To add comments to **Column** in the **Create Partition Table** dialog box, add column information in **Description of Column (Max 5000 chars)** text box and click **Add**.

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section13466165012317:

Providing Basic Information
---------------------------

If you create a table in a schema, the current schema will be used as the schema of the table. Perform the following steps to create a partitioned table:

#. Select a table storage mode from the **Table Orientation** drop-down list.

   .. note::

      If table orientation is selected as ORC, then an HDFS Partitioned table is created. Enter the ORC version number in **ORC Version**.

#. After providing the general information about the table, click **Next** to define the columns information for the table.

   The following table describes the parameters of partitioned tables.

   .. table:: **Table 1** Parameters

      ============= ============= ================ =============
      Parameter     Row Partition Column Partition ORC Partition
      ============= ============= ================ =============
      Table Type    x             x                x
      If Not Exists Y             Y                Y
      With OIDS     x             x                x
      Fill Factor   Y             x                x
      ============= ============= ================ =============

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section3109182003214:

Defining a Column
-----------------

The following table describes the parameters of partitioned tables.

.. table:: **Table 2** Parameters

   ================ ============= ================ =============
   Parameter        Row Partition Column Partition ORC Partition
   ================ ============= ================ =============
   Array Dimensions Y             x                x
   Data Type        Y             x                x
   NOT NULL         Y             Y                Y
   Default          Y             Y                Y
   UNIQUE           Y             x                x
   CHECK            Y             x                x
   ================ ============= ================ =============

You can add, delete, and edit columns, and adjust the sequence of columns.

You can change the order of partitions in the table as required. To change the order, select the required partition and click **Up** or **Down**.

|image1|

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section1221111211255:

SQL Preview
-----------

Data Studio generates a DDL statement based on the inputs provided in **Create New table** wizard.

You can only view, select, and copy the query. You cannot edit the query.

-  To select all queries, press **Ctrl+A** or right-click and select **Select All**.
-  To copy the selected query, press **Ctrl+C** or right-click and select **Copy**.

Click **Finish** to create the table. On clicking the **Finish** button, the generated query will be sent to the server. Any errors are displayed in the dialog box and status bar.

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section16127182563613:

Defining a Partition
--------------------

The following table describes the parameters of partitioned tables.

.. table:: **Table 3** Parameters

   =============== ============= ================ =============
   Parameter       Row Partition Column Partition ORC Partition
   =============== ============= ================ =============
   Partition Type  By Range      By Range         By Value
   Partition Name  Y             Y                x
   Partition Value Y             Y                x
   =============== ============= ================ =============

#. If **Row** or **Column** is selected for **Table Orientation** on the **General** tab, **By Range** will be displayed in the **Partition Type** area. If **ORC** is selected for **Table Orientation** on the **General** tab, **By Value** will be displayed in the **Partition Type** area.

#. In the **Available Column** area, select a column and click the Right Arrow button. The column will be moved to the **Partition Column** area.

   .. note::

      -  If **Table Orientation** is set to **Row** or **Column**, only one column can be selected for partitioning.
      -  If **Table Orientation** is set to **ORC**, up to four columns can be selected for partitioning.
      -  A maximum of four columns can be selected to define partitions.

#. Enter a partition name.

#. Click the **Enter Partition Value** button next to **Partition Value**. Enter the value by which you want to partition the table in **Value** column. Click **OK**.

#. After you enter all information for partition, click **Add**.

   You can add, delete, edit and move a column.

   Change the partition sequence according to the requirements in the table. To change the order, select the required partition and click **Up** or **Down**.

   |image2|

#. After defining all partitions, click **Next**.

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section17537195584514:

Defining an Index
-----------------

For details about index definitions, see :ref:`Defining an Index <en-us_topic_0000001860199097__en-us_topic_0185264992_section082554911302>`.

.. table:: **Table 4** Parameters

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

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section820017103416:

Defining Table Constraints
--------------------------

For details about how to define table constraints, see :ref:`Defining Table Constraints <en-us_topic_0000001860199097__en-us_topic_0185264992_section440110125279>`.

.. table:: **Table 5** Parameters

   =========== ============= ================ =========
   Parameter   Row Partition Column Partition ORC Table
   =========== ============= ================ =========
   Check       Y             x                x
   Unique      Y             x                x
   Primary Key Y             x                x
   =========== ============= ================ =========

.. _en-us_topic_0000001813598940__en-us_topic_0185264910_section183471438456:

Configuring Data Distribution
-----------------------------

For details about how to configure a distribution type, see :ref:`Selecting Data Distribution <en-us_topic_0000001860199097__en-us_topic_0185264992_section728115301265>`.

.. table:: **Table 6** Parameters

   ==================== ============= ================ =============
   Parameter            Row Partition Column Partition ORC Partition
   ==================== ============= ================ =============
   DEFAULT DISTRIBUTION Y             Y                x
   Hash                 Y             Y                Y
   Replication          Y             Y                x
   ==================== ============= ================ =============

Dropping a Partition
--------------------

#. Right-click the selected index and select **Drop Partition**.

   **Drop Partition Table** dialog box is displayed.

#. Click **OK**.

   The partition is deleted from the table. Data Studio displays the status of the operation in the status bar.

Renaming a partition
--------------------

#. Right-click the selected partition and select **Rename Partition**.

   **Rename Partition Table** dialog box is displayed prompting you to provide the new name for the partition.

#. Enter new name and click **OK**.

   Data Studio displays the status of the operation in the status bar.

.. |image1| image:: /_static/images/en-us_image_0000001813599320.png
.. |image2| image:: /_static/images/en-us_image_0000001860319237.png
