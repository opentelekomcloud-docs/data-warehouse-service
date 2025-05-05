:original_name: dws_16_0093.html

.. _dws_16_0093:

.. _en-us_topic_0000001860199049:

UPDATE
======

DSC supports migration of the **UPDATE** (:ref:`short key <en-us_topic_0000001860198793>` UPD) statements.

**Input: UPDATE with TABLE ALIAS**

::

   UPDATE T1
     FROM tab1 T1, tab2 T2
      SET c1 = T2.c1
        , c2 = T2.c2
    WHERE T1.c3 = T2.c3;

**Output**

::

   UPDATE tab1 T1
      SET c1 = T2.c1
        , c2 = T2.c2
     FROM tab2 T2
    WHERE T1.c3 = T2.c3;

**Input: UPDATE with TABLE ALIAS** **using a sub query**

::

   UPDATE t1
     FROM tab1 t1, ( SELECT c1, c2 FROM tab2
                      WHERE c2 > 100 ) t2
      SET c1 = t2.c1
    WHERE t1.c2 = t2.c2;

**Output**

::

    UPDATE tab1 t1
      SET c1 = t2.c1
     FROM ( SELECT c1, c2 FROM tab2
             WHERE c2 > 100 ) t2
    WHERE t1.c2 = t2.c2;

**Input: UPDATE with ANALYZE**

::

   UPD employee SET ename = 'Jane'
           WHERE ename = 'John';
   COLLECT STAT on employee;

**Output**

::

   UPDATE employee SET ename = 'Jane'
    WHERE ename = 'John';
   ANALYZE employee;
