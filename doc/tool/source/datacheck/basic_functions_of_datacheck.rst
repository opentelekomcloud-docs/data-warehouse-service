:original_name: dws_07_0204.html

.. _dws_07_0204:

Basic Functions of DataCheck
============================

Basic Functions
---------------

-  Support data check for source databases such as GaussDB(DWS), MySQL, and PostgreSQL, with the destination database being GaussDB(DWS).
-  Check common fields, such as numeric, time, and character types.
-  Support three check levels, including high, middle, and low.
-  Check schemas, table names, and column names.
-  Specify the check scope of records. By default, all records are checked.
-  Support various check methods, including **COUNT(*)**, **MAX**, **MIN**, **SUM**, **AVG**, and sampling details check.
-  Output the check result and related check details.

.. table:: **Table 1** Data check levels

   +-----------------------+-------------------------+---------------------------------------------------------------------------------------------------------------+
   | Check Level           | Description             | Syntax                                                                                                        |
   +=======================+=========================+===============================================================================================================+
   | No                    | ``-``                   | ``-``                                                                                                         |
   +-----------------------+-------------------------+---------------------------------------------------------------------------------------------------------------+
   | Low                   | Quantity check          | Number of records: **COUNT(*)**                                                                               |
   +-----------------------+-------------------------+---------------------------------------------------------------------------------------------------------------+
   | Middle                | -  Quantity check       | -  Number of records: **COUNT(*)**                                                                            |
   |                       | -  Numeric type check   | -  Value check: **MAX**, **MIN**, **SUM**, and **AVG**                                                        |
   +-----------------------+-------------------------+---------------------------------------------------------------------------------------------------------------+
   | High                  | -  Quantity check       | -  Number of records: **COUNT(*)**                                                                            |
   |                       | -  Numeric type check   | -  Value check: **MAX**, **MIN**, **SUM**, and **AVG**                                                        |
   |                       | -  Date type check      | -  Date check: **MAX**, **MIN**                                                                               |
   |                       | -  Character type check | -  Character check: **order by limit 1000**, which reads the data and checks whether the content is the same. |
   +-----------------------+-------------------------+---------------------------------------------------------------------------------------------------------------+
