:original_name: dws_16_0073.html

.. _dws_16_0073:

.. _en-us_topic_0000001860318745:

ANALYZE
=======

The **ANALYZE** statement of Teradata is used to migrate tables.

**Input: CREATE TABLE with INDEX**

::

   CREATE TABLE EMP27 AS emp21 WITH DATA
   PRIMARY INDEX (EMPNO) ON COMMIT PRESERVE ROWS;

**Output:**

.. code-block::

   Begin
   CREATE TABLE EMP27
   ( LIKE emp21 INCLUDING ALL EXCLUDING PARTITION EXCLUDING RELOPTIONS EXCLUDING
   DISTRIBUTION )
   DISTRIBUTE BY HASH ( EMPNO ) ;
   INSERT INTO EMP27
   select * from emp21 ;
   end ;
   /
   ANALYZE Emp27 (EmpNo);
