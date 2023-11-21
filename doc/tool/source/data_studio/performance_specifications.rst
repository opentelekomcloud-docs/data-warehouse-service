:original_name: DWS_DS_144.html

.. _DWS_DS_144:

Performance Specifications
==========================

The loading and operation performance of Data Studio depends on the number of objects to be loaded in **Object Browser**, including tables, views, and columns.

Memory consumption also depends on the number of loaded objects.

To improve object loading performance and better utilize memory, you are advised to divide an object into multiple namespaces, and to avoid using namespaces that contain a large number of objects and cause data skew. By default, Data Studio loads the namespaces in the *search_path* set for the user logged in. Other namespaces and objects are loaded only when needed.

To improve performance, you are advised to load all objects. Do not load objects based on user permissions. :ref:`Table 1 <en-us_topic_0000001234200679__en-us_topic_0185264599_table18121154132>` describes the minimum access permissions required to list objects in the **Object Browser**.

.. _en-us_topic_0000001234200679__en-us_topic_0185264599_table18121154132:

.. table:: **Table 1** Minimum permission requirements

   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Object Type | Type                                                      | Object Browser - Minimum Permission |
   +=============+===========================================================+=====================================+
   | Database    | Create, Connect, Temporary/Temp, All                      | Connect                             |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Schema      | Create, Usage, All                                        | Usage                               |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Table       | Select, Insert, Update, Delete, Truncate, References, All | Select                              |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Column      | Select, Insert, Update, References, All                   | Select                              |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | View        | Select, Insert, Update, Delete, Truncate, References, All | Select                              |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Sequences   | Usage, Select, Update, All                                | Usage                               |
   +-------------+-----------------------------------------------------------+-------------------------------------+
   | Function    | Execute, All                                              | Execute                             |
   +-------------+-----------------------------------------------------------+-------------------------------------+

To improve the performance of find and replace operations, you are advised to break a line that contains more than 10,000 characters into multiple short lines.

The following test items and results can help you learn the performance of Data Studio.

+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
| Recommended maximum memory (current version)                                                                                                   |                                                                                           | 1.4 GB   |
+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+----------+
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
