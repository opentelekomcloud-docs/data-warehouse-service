:original_name: DWS_DS_78.html

.. _DWS_DS_78:

Overview
========

Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partitioned table, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table.

Follow the steps below to define a table in your database:

#. In the **Object Browser** pane, right-click **Regular Tables**, and choose **Create Partition Table**.

#. Define basic table information, such as the table name and table type. For details, see :ref:`Providing Basic Information <en-us_topic_0000001233922163__en-us_topic_0185264910_section13466165012317>`.

#. Define column information, such as the column name, data type schema, data type, and column constraint. For details, see :ref:`Defining a Column <en-us_topic_0000001233922163__en-us_topic_0185264910_section3109182003214>`.

#. Select the data distribution information for the table. For details, see :ref:`Change Order of Partition <en-us_topic_0000001233922163__en-us_topic_0185264910_section175791628113519>`.

#. Define column constraints for different constraint types. Constraint types include **PRIMARY KEY**, **UNIQUE**, and **CHECK**. For details, see :ref:`Defining Table Constraints <en-us_topic_0000001233922163__en-us_topic_0185264910_section820017103416>`.

#. Define table index information, such as the index name and access mode. For details, see :ref:`Defining an Index <en-us_topic_0000001233922163__en-us_topic_0185264910_section17537195584514>`.

#. Define the partition information for the table such as partition name, partition column, partition value and so on. For details, see :ref:`Defining a Partition <en-us_topic_0000001233922163__en-us_topic_0185264910_section16127182563613>`.

   On the **SQL Preview** tab, you can check the automatically generated SQL query. For details, see :ref:`Checking the SQL Preview <en-us_topic_0000001233922163__en-us_topic_0185264910_section1221111211255>`.

#. To add comments to **Column** in the **Create Partition Table** dialog box, add column information in **Description of Column (Max 5000 chars)** text box and click **Add**.

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section13466165012317:

Providing Basic Information
---------------------------

Provide the following information to create a table:

For details, see :ref:`Providing Basic Information <en-us_topic_0000001234200603__en-us_topic_0185264992_section1344143101920>`.

-  Table Name
-  Schema
-  Options
-  Description of Table

Perform the following steps to configure other parameters:

#. Select a table storage mode from the **Table Orientation** drop-down list.

   .. note::

      If table orientation is selected as ORC, then an HDFS Partitioned table is created.

#. Enter the ORC version number in the **ORC Version** field. This is applicable only for HDFS Partitioned table.

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

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section3109182003214:

Defining a Column
-----------------

For details, see :ref:`Defining a Column <en-us_topic_0000001234200603__en-us_topic_0185264992_section546323424>`.

The following table describes the parameters of partitioned tables.

.. table:: **Table 2** Parameters

   ================ ============= ================ =============
   Field            Row Partition Column Partition ORC Partition
   ================ ============= ================ =============
   Array Dimensions Y             x                x
   Data Type        Y             x                x
   NOT NULL         Y             Y                Y
   Default          Y             Y                Y
   UNIQUE           Y             x                x
   CHECK            Y             x                x
   ================ ============= ================ =============

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section175791628113519:

Change Order of Partition
-------------------------

You can change the order of partition as required in the table. To change the order, select the required partition and click **Up** or **Down**.

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section1221111211255:

Checking the SQL Preview
------------------------

For details, see :ref:`SQL Preview <en-us_topic_0000001234200603__en-us_topic_0185264992_section7772325171112>`.

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section3311330142512:

Editing a Partition
-------------------

Perform the following steps to edit a partition:

#. Select a partition.
#. Click **Edit**.
#. Edit partition configurations as needed and click **Update** to save the changes.

   .. note::

      You must complete the edit operation and save the changes to continue with other operations.

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section1150418339359:

Deleting a Partition
--------------------

Perform the following steps to delete a partition:

#. Select a partition.
#. Click **Delete**.

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section16127182563613:

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

Perform the following steps to define a table partition:

#. If **Row** or **Column** is selected for **Table Orientation** on the **General** tab, **By Range** will be displayed in the **Partition Type** area. If **ORC** is selected for **Table Orientation** on the **General** tab, **By Value** will be displayed in the **Partition Type** area.

#. In the **Available Column** area, select a column and click |image1|.

   The column will be moved to the **Partition Column** area.

   .. note::

      -  If **Table Orientation** is set to **Row** or **Column**, only one column can be selected for partitioning.
      -  If **Table Orientation** is set to **ORC**, up to four columns can be selected for partitioning.
      -  A maximum of four columns can be selected to define partitions.

#. Enter a partition name.

#. Click |image2| next to **Partition Value**.

   a. Enter the value by which you want to partition the table in **Value** column.
   b. Click **OK**.

#. After you enter all information for partition, click **Add**.

#. After defining all partitions, click **Next**.

You can perform the following operations on the partitions of a row-or column-partitioned table, but not on ORC partitioned tables:

:ref:`Deleting a Partition <en-us_topic_0000001233922163__en-us_topic_0185264910_section1150418339359>`

:ref:`Editing a Partition <en-us_topic_0000001233922163__en-us_topic_0185264910_section3311330142512>`

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section17537195584514:

Defining an Index
-----------------

For details about index definitions, see :ref:`Defining an Index <en-us_topic_0000001234200603__en-us_topic_0185264992_section082554911302>`.

.. table:: **Table 4** Parameters

   ======================= ============= ================ =============
   Parameter               Row Partition Column Partition ORC Partition
   ======================= ============= ================ =============
   Unique Indexes          Y             x                x
   btree                   Y             Y                x
   gin                     Y             Y                x
   gist                    Y             Y                x
   hash                    Y             Y                x
   psort                   Y             Y                x
   spgist                  Y             Y                x
   Fill Factor             Y             x                x
   User Defined Expression Y             x                x
   Partial Index           Y             x                x
   ======================= ============= ================ =============

.. _en-us_topic_0000001233922163__en-us_topic_0185264910_section820017103416:

Defining Table Constraints
--------------------------

For details about how to define table constraints, see :ref:`Defining Table Constraints <en-us_topic_0000001234200603__en-us_topic_0185264992_section440110125279>`.

.. table:: **Table 5** Parameters

   =========== ========= ================ =============
   Parameter   Partition Column Partition ORC Partition
   =========== ========= ================ =============
   Check       Y         x                x
   Unique      Y         x                x
   Primary Key Y         x                x
   =========== ========= ================ =============

Configuring Data Distribution
-----------------------------

For details about how to select a distribution type, see :ref:`Selecting Data Distribution <en-us_topic_0000001234200603__en-us_topic_0185264992_section728115301265>`.

.. table:: **Table 6** Parameters

   ==================== ============= ================ =============
   Parameter            Row Partition Column Partition ORC Partition
   ==================== ============= ================ =============
   DEFAULT DISTRIBUTION Y             Y                x
   Hash                 Y             Y                Y
   Replication          Y             Y                x
   ==================== ============= ================ =============

.. |image1| image:: /_static/images/en-us_image_0000001188681134.jpg
.. |image2| image:: /_static/images/en-us_image_0000001234042251.jpg
