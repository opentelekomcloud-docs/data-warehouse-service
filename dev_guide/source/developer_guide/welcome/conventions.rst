:original_name: dws_04_0005.html

.. _dws_04_0005:

Conventions
===========

Example Conventions
-------------------

+---------+--------------------------------------------------------------------------------------------------+
| Example | Description                                                                                      |
+=========+==================================================================================================+
| dbadmin | Indicates the user operating and maintaining GaussDB(DWS) appointed when the cluster is created. |
+---------+--------------------------------------------------------------------------------------------------+
| 8000    | Indicates the port number used by GaussDB(DWS) to monitor connection requests from the client.   |
+---------+--------------------------------------------------------------------------------------------------+

SQL examples in this manual are developed based on the TPC-DS model. Before you execute the examples, install the TPC-DS benchmark by following the instructions on the official website https://www.tpc.org/tpcds/.

SQL Syntax Text Conventions
---------------------------

To better understand the syntax usage, you can refer to the SQL syntax text conventions described as follows:

+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| Format                     | Description                                                                                                                             |
+============================+=========================================================================================================================================+
| Uppercase characters       | Indicates that keywords must be in uppercase.                                                                                           |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| Lowercase characters       | Indicates that parameters must be in lowercase.                                                                                         |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| [ ]                        | Indicates that the items in brackets [] are optional.                                                                                   |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ...                        | Indicates that preceding elements can appear repeatedly.                                                                                |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| [ x \| y \| ... ]          | Indicates that one item is selected from two or more options or no item is selected.                                                    |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... }          | Indicates that one item is selected from two or more options.                                                                           |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| [x \| y \| ... ] [ ... ]   | Indicates that multiple parameters or no parameter can be selected. If multiple parameters are selected, separate them with spaces.     |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| [ x \| y \| ... ] [ ,... ] | Indicates that multiple parameters or no parameter can be selected. If multiple parameters are selected, separate them with commas (,). |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... } [ ... ]  | Indicates that at least one parameter can be selected. If multiple parameters are selected, separate them with spaces.                  |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... } [ ,... ] | Indicates that at least one parameter can be selected. If multiple parameters are selected, separate them with commas (,).              |
+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
