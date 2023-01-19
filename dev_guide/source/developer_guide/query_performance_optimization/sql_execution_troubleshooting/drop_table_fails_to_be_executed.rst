:original_name: dws_04_0494.html

.. _dws_04_0494:

DROP TABLE Fails to Be Executed
===============================

Problem
-------

**DROP TABLE** fails to be executed in the following scenarios:

-  A user runs the **\\dt+** command using **gsql** and finds that the *table_name* table does not exist. When the user runs the **CREATE TABLE table_name** command, an error message indicating that the *table_name* table exists is displayed. When the user runs the **DROP TABLE** *table_name* command, an error message indicating that the *table_name* table does not exist is displayed. In this case, the *table_name* table cannot be recreated.
-  A user runs the **\\dt+** command using **gsql** and finds that the *table_name* table exists in the database. When the user runs the **DROP TABLE** *table_name* command, an error message indicating that the *table_name* table does not exist is displayed. In this case, the *table_name* table cannot be recreated.

Possible Causes
---------------

The table_name table exists on some nodes only.

Troubleshooting Method
----------------------

In the preceding scenarios, if **DROP TABLE** *table_name* fails to be executed, run **DROP TABLE IF EXISTS** *table_name* to successfully drop *table_name*.
