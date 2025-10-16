:original_name: dws_04_0108.html

.. _dws_04_0108:

DATABASE Object Design
======================

.. _en-us_topic_0000002100746046__en-us_topic_0000002135726589_section9462145153713:

Rule 2.1: Avoiding Direct Usage of Built-in Databases Such As postgres and gaussdb
----------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  If the code or the compatibility setting of the built-in databases does not meet service requirements, you may need to migrate your data again.
   -  The time for changes to be applied may be prolonged if all services use built-in databases.

   **Solution:**

   -  To meet the specific requirements of each service, it is recommended to create a dedicated database and allocate it accordingly.

.. _en-us_topic_0000002100746046__en-us_topic_0000002135726589_section812010258370:

Rule 2.2: Selecting the Suitable Database Code During Database Creation
-----------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Selecting the wrong database code may result in displaying garbled characters, and it is not possible to directly change the database code. In such cases, you will need to create a database and import the data again.

   **Solution:**

   -  It is advisable to set the **ENCODING** to the UTF-8 format during database creation, unless there are specific requirements for a different encoding.

.. _en-us_topic_0000002100746046__en-us_topic_0000002135726589_section12587184219378:

Rule 2.3: Choosing the Right Database Type for Compatibility with the Database to Be Created
--------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Selecting the wrong type can lead to behavior inconsistencies after migrating the database from a different vendor to GaussDB(DWS). Unfortunately, it is not possible to directly change the compatibility setting. The only solution is to create a database and import the data again.

   **Solution:**

   -  GaussDB(DWS) supports compatibility with databases like Teradata, Oracle, and MySQL. You can specify **DBCOMPATIBILITY** to set the compatible database type when creating a database.

.. _en-us_topic_0000002100746046__en-us_topic_0000002135726589_section7642125613377:

Suggestion 2.4: Creating the Objects with Associated Calculations in the Same Database
--------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Cross-database access tends to have poorer performance compared to performing operations within the same database.

   **Solution:**

   -  If multiple databases are created, it is advisable to create the objects requiring associated calculations in the same database.
