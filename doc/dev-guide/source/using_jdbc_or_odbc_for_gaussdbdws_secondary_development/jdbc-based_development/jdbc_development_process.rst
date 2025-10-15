:original_name: dws_04_0089.html

.. _dws_04_0089:

JDBC Development Process
========================

Java Database Connectivity (JDBC) is a Java API for executing SQL statements. It provides a unified access interface for multiple relational databases, enabling applications to work with data based on it. GaussDB(DWS) supports JDBC 4.0 and requires JDK 1.6 or later for code compiling. It does not support JDBC-ODBC Bridge. The following figure shows the JDBC application development process.


.. figure:: /_static/images/en-us_image_0000002044100078.png
   :alt: **Figure 1** JDBC-based application development process

   **Figure 1** JDBC-based application development process

.. table:: **Table 1** JDBC development process

   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Procedure               | Description                                                                                                                                 |
   +=========================+=============================================================================================================================================+
   | Load the driver.        | Download the JDBC driver and edit and load it in the program.                                                                               |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Connect to a database.  | Connect to the database through the JDBC driver.                                                                                            |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Execute SQL statements. | Applications operate database data by executing SQL statements.                                                                             |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Process the result set. | Different types of result sets have different application scenarios. Applications need to select the appropriate result set type as needed. |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Close the connection.   | Make sure to close the database connection after completing the required data operations.                                                   |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
