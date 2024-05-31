:original_name: dws_16_0179.html

.. _dws_16_0179:

.. _en-us_topic_0000001772536548:

Quotation Marks
===============

Single Quotation Mark (')
-------------------------

Aliases in MySQL contain single quotation marks, which are not supported by GaussDB(DWS). DSC changes them to double quotation marks during migration.

**Input**

.. code-block::

   select name as 'mingzi' from t1;

**Output**

.. code-block::

   SELECT
     name AS "mingzi"
   FROM
     t1;

Backquote (`)
-------------

Aliases and column names in MySQL contain backquotes, which are not supported by GaussDB(DWS). DSC changes them to double quotation marks during migration.

**Input**

.. code-block::

   select `name` as `mingzi` from t1;

**Output**

.. code-block::

   SELECT
     "name" AS "mingzi"
   FROM
     t1;

Double Quotation Mark (")
-------------------------

In MySQL, constant strings are enclosed in double quotation marks, which are not supported by GaussDB(DWS). DSC changes them to single quotation marks during migration.

**Input**

.. code-block::

   select name from t1 where name = "test2";

**Output**

.. code-block::

   SELECT
     name
   FROM
     t1
   WHERE
     name = 'test2';
