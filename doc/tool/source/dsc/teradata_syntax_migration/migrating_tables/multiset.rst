:original_name: dws_16_0067.html

.. _dws_16_0067:

.. _en-us_topic_0000001860318713:

MULTISET
========

**MULTISET** is a normal table, which is supported by all the DBs. Migration tool supports MULTISET and SET tables.

MULTISET table can be used with VOLATILE.

**Input: CREATE MULTISET TABLE**

::

    CREATE VOLATILE MULTISET TABLE T1 (c1 int ,c2 int);

**Output:**

::

   CREATE
       LOCAL TEMPORARY TABLE
       T1 (
            c1 INTEGER
           ,c2 INTEGER
          )
   ;
