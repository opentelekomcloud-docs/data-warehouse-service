:original_name: dws_04_0204.html

.. _dws_04_0204:

Data Import Using COPY FROM STDIN
=================================

This method is applicable to low-concurrency scenarios where a small volume of data is to be imported.

Use either of the following methods to write data to GaussDB(DWS) using the **COPY FROM STDIN** statement:

-  Write data into GaussDB(DWS) by typing.
-  Import data from a file or database to GaussDB(DWS) through the CopyManager interface driven by JDBC. You can use any parameters in the **COPY** syntax.
