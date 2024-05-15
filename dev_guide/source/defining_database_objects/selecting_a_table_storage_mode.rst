:original_name: dws_04_0026.html

.. _dws_04_0026:

Selecting a Table Storage Mode
==============================

GaussDB(DWS) supports hybrid row and column storage. When creating a table, you can set the table storage mode to row storage or column storage.

Row storage stores tables to disk partitions by row, and column storage stores tables to disk partitions by column. By default, a table is created in row storage mode. For details about differences between row storage and column storage, see :ref:`Figure 1 <en-us_topic_0000001233883387__fig1417354233018>`.

.. _en-us_topic_0000001233883387__fig1417354233018:

.. figure:: /_static/images/en-us_image_0000001188323816.png
   :alt: **Figure 1** Differences between row storage and column storage

   **Figure 1** Differences between row storage and column storage

In the preceding figure, the upper left part is a row-store table, and the upper right part shows how the row-store table is stored on a disk; the lower left part is a column-store table, and the lower right part shows how the column-store table is stored on a disk.

The row/column storage of a table is specified by the **orientation** attribute in the table definition. The value **row** indicates a row-store table and **column** indicates a column-store table. The default value is **row**. Each storage mode applies to specific scenarios. Select an appropriate mode when creating a table.

.. table:: **Table 1** Table storage modes and scenarios

   +-----------------+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Storage Mode    | Benefit                                                                                     | Drawback                                                                      | Application Scenarios                                                                                       |
   +=================+=============================================================================================+===============================================================================+=============================================================================================================+
   | Row storage     | Data is stored by row. When you query a row of data, you can quickly locate the target row. | All data in the queried row is read while only a few columns are needed.      | #. The number of columns in the table is small, and most fields in the table are queried.                   |
   |                 |                                                                                             |                                                                               | #. Point queries (simple index-based query that returns only a few records) are performed.                  |
   |                 |                                                                                             |                                                                               | #. Add, Delete, Modify, and Query operations on entire rows are frequently performed.                       |
   +-----------------+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Column storage  | #. Only necessary columns in a query are read.                                              | It is not suitable for INSERT or UPDATE operations on a small amount of data. | #. Query a few columns in a table that contains a large number of columns.                                  |
   |                 | #. The homogeneity of data within a column facilitates efficient compression.               |                                                                               | #. Statistical analysis queries (requiring a large number of association and grouping operations)           |
   |                 |                                                                                             |                                                                               | #. Ad hoc queries (using uncertain query conditions and unable to utilize indexes to scan row-store tables) |
   +-----------------+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+

Creating a Row-store Table
--------------------------

For example, to create a row-store table named **customer_t1**, run the following command:

::

   CREATE TABLE customer_t1
   (
     state_ID   CHAR(2),
     state_NAME VARCHAR2(40),
     area_ID    NUMBER
   );

Creating a column-store table.
------------------------------

For example, to create a column-store table named **customer_t2**, run the following command:

::

   CREATE TABLE customer_t2
   (
     state_ID   CHAR(2),
     state_NAME VARCHAR2(40),
     area_ID    NUMBER
   )
   WITH (ORIENTATION = COLUMN);

Table Compression
-----------------

Table compression can be enabled when a table is created. Table compression enables data in the table to be stored in compressed format to reduce memory usage.

In scenarios where I/O is large (much data is read and written) and CPU is sufficient (little data is computed), select a high compression ratio. In scenarios where I/O is small and CPU is insufficient, select a low compression ratio. Based on this principle, you are advised to select different compression ratios and test and compare the results to select the optimal compression ratio as required. Specify a compressions ratio using the **COMPRESSION** parameter. The supported values are as follows:

-  The valid value of column-store tables is **YES**, **NO**, **LOW**, **MIDDLE**, or **HIGH**, and the default value is **LOW**.
-  The valid values of row-store tables are **YES** and **NO**, and the default is **NO**. (The row-store table compression function is not put into commercial use. To use this function, contact technical support.)

The service scenarios applicable to each compression level are described in the following table.

+-------------------+------------------------------------------------------------------------------+
| Compression Level | Application Scenario                                                         |
+===================+==============================================================================+
| LOW               | The system CPU usage is high and the disk storage space is sufficient.       |
+-------------------+------------------------------------------------------------------------------+
| MIDDLE            | The system CPU usage is moderate and the disk storage space is insufficient. |
+-------------------+------------------------------------------------------------------------------+
| HIGH              | The system CPU usage is low and the disk storage space is insufficient.      |
+-------------------+------------------------------------------------------------------------------+

For example, to create a compressed column-store table named **customer_t3**, run the following command:

::

   CREATE TABLE customer_t3
   (
     state_ID   CHAR(2),
     state_NAME VARCHAR2(40),
     area_ID    NUMBER
   )
   WITH (ORIENTATION = COLUMN,COMPRESSION=middle);
