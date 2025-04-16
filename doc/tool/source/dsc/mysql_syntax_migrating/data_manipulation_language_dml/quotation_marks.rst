:original_name: dws_16_0179.html

.. _dws_16_0179:

Quotation Marks
===============

Single Quotation Mark (')
-------------------------

Aliases in MySQL contain single quotation marks, which are not supported by GaussDB(DWS). DSC changes them to double quotation marks during migration.

**Input**

::

   select name as 'mingzi' from t1;

**Output**

::

   SELECT
     name AS "mingzi"
   FROM
     t1;

Backquote (`)
-------------

Aliases and column names in MySQL contain backquotes, which are not supported by GaussDB(DWS). DSC changes them to double quotation marks during migration.

**Input**

::

   select `name` as `mingzi` from t1;

**Output**

::

   SELECT
     "name" AS "mingzi"
   FROM
     t1;

Double Quotation Mark (")
-------------------------

In MySQL, constant strings are enclosed in double quotation marks, which are not supported by GaussDB(DWS). DSC changes them to single quotation marks during migration.

**Input**

::

   select name from t1 where name = "test2";

**Output**

::

   SELECT
     name
   FROM
     t1
   WHERE
     name = 'test2';
