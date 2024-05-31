:original_name: dws_16_0088.html

.. _dws_16_0088:

.. _en-us_topic_0000001772536460:

ANALYZE
=======

The Teradata **SELECT** command (:ref:`short key <en-us_topic_0000001772696108>` SEL) is used to specify the table columns from which data is to be retrieved.

**ANALYZE** is used in GaussDB(DWS) for collecting optimizer statistics, which is used for improving query performance.

**Input: ANALYZE with INSERT**

::

   INSERT INTO employee(empno,ename)  Values (1,'John');
   COLLECT STAT on employee;

**Output**

::

   INSERT INTO employee( empno, ename)
   SELECT 1 ,'John';
   ANALYZE employee;

**Input: ANALYZE with UPDATE**

::

   UPD employee SET ename = 'Jane'
           WHERE ename = 'John';
   COLLECT STAT on employee;

**Output**

::

   UPDATE employee SET ename = 'Jane'
    WHERE ename = 'John';
   ANALYZE employee;

**Input: ANALYZE with DELETE**

::

   DEL FROM employee WHERE ID > 10;
   COLLECT STAT on employee;

**Output**

.. code-block:: text

   DELETE FROM employee WHERE ID > 10;
   ANALYZE employee;
