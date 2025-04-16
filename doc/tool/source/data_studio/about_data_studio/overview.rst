:original_name: dws_07_0012.html

.. _dws_07_0012:

Overview
========

Data Studio provides a graphical interface which supports essential features of the database. This simplifies database development and application building tasks.

Data Studio allows the database developer to manage and create database objects (database, schema, functions, procedures, tables, sequences, columns, indexes, constraints, views, and tablespaces), execute SQL statements or SQL scripts, edit and execute PL/SQL statements, as well as import and export table data.

Data Studio also allows the database developer to debug and fix defects in the PL/SQL code using debugging operations such as **Step Into**, **Step Out**, **Step Over**, **Continue**, and **Terminate**.

The following figure provides the operational context of database and Data Studio.

|image1|

.. _en-us_topic_0000001860318645__section18536183110235:

Data Studio GUI
---------------

.. table:: **Table 1** Introduction to the GUI

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | GUI Name                          | Description                                                                                                                                                                                                                                                                                     |
   +===================================+=================================================================================================================================================================================================================================================================================================+
   | Main Menu                         | Provides basic operations of Data Studio.                                                                                                                                                                                                                                                       |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Toolbar                           | Provides entries to common operations.                                                                                                                                                                                                                                                          |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **SQL Terminal** tab              | Executes SQL statements and functions/procedures.                                                                                                                                                                                                                                               |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **PL/SQL Viewer** tab             | Displays the content of function/procedures.                                                                                                                                                                                                                                                    |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **CallStack** pane                | Displays the execution stack.                                                                                                                                                                                                                                                                   |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Breakpoints** pane              | Displays all breakpoints that have been set.                                                                                                                                                                                                                                                    |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Variables** pane                | Shows variables and their values.                                                                                                                                                                                                                                                               |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **SQL Assistant** tab             | Displays suggestion or reference for the information entered in the **SQL Terminal** and **PL/SQL Viewer**.                                                                                                                                                                                     |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Result** tab                    | Displays the result(s) of an executed function/procedure, or an SQL statement.                                                                                                                                                                                                                  |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Message** tab                   | Displays the process output, standard input, standard output, and standard errors.                                                                                                                                                                                                              |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Object Browser** pane           | Contains a hierarchical tree display of database connection(s) and related database objects to which users can access. All created schemas, except the public schemas, are grouped under **Catalogs** by default, and user schemas are grouped under **Schemas** of the corresponding database. |
   |                                   |                                                                                                                                                                                                                                                                                                 |
   |                                   | .. note::                                                                                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                                 |
   |                                   |    **Object Browser** displays only the objects that meet the permission requirements of the current user.                                                                                                                                                                                      |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **Minimized Window Panel** pane   | Used to open the **Callstack**, **Breakpoints**, and **Variables** panes. This pane is displayed only when Callstack, Breakpoints or Variables pane or all three are minimized.                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Search Toolbar                    | Used to search objects from **Object browser**.                                                                                                                                                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Performance Specifications
--------------------------

The loading and operation performance of Data Studio depends on the number of objects to be loaded in **Object Browser**, including tables, views, and columns.

Memory consumption also depends on the number of loaded objects.

To improve object loading performance and better utilize memory, you are advised to divide an object into multiple namespaces, and to avoid using namespaces that contain a large number of objects and cause data skew. By default, Data Studio loads the namespaces in the *search_path* set for the user logged in. Other namespaces and objects are loaded only when needed.

To improve performance, you are advised to load all objects. Do not load objects based on user permissions. :ref:`Table 2 <en-us_topic_0000001860318645__en-us_topic_0185264599_table18121154132>` describes the minimum access permissions required to the listed objects in **Object Browser**.

.. _en-us_topic_0000001860318645__en-us_topic_0185264599_table18121154132:

.. table:: **Table 2** Minimum permission requirements

   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Object Type | Permission Type                                           | Object Browser - Minimum Permission |
   +=============+===========================================================+=====================================+
   | Databases   | Create, Connect, Temporary/Temp, All                      | Connect                             |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Schema      | Create, Usage, All                                        | Usage                               |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Tables      | Select, Insert, Update, Delete, Truncate, References, All | Select                              |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Columns     | Select, Insert, Update, References, All                   | Select                              |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Views       | Select, Insert, Update, Delete, Truncate, References, All | Select                              |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Sequences   | Usage, Select, Update, All                                | Usage                               |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Functions   | Execute, All                                              | Execute                             |
   +-------------+-----------------------------------------------------------+-------------------------------------+

To improve the performance of the finding and replacing operations, you are advised to break a line that contains more than 10,000 characters into multiple short lines.

The following test items and results can help you learn the performance of Data Studio.

+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
| Recommended maximum memory (current version)                                                                                                   |                                                                                           | 1.4 GB   |
+================================================================================================================================================+===========================================================================================+==========+
| Performance (The database contains a 150 KB table and a 150 KB view, each containing three columns. The maximum memory configuration is used.) |                                                                                           |          |
+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
| >                                                                                                                                              | Time taken to refresh namespaces in **Object Browser**                                    | 15s      |
+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
| >                                                                                                                                              | Time taken for initial loading and expanding of all tables/views in **Object Browser**    | 90s-120s |
+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
| >                                                                                                                                              | Time taken for subsequent loading and expanding of all tables/views in **Object Browser** | <10s     |
+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
| >                                                                                                                                              | Total used memory                                                                         | 700 MB   |
+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+

.. note::

   The performance data is for reference only. The actual performance may vary according to the application scenario.

.. |image1| image:: /_static/images/en-us_image_0000001860199221.jpg
