:original_name: dws_07_6814.html

.. _dws_07_6814:

COMPRESS Phrase
===============

**Input - COMPRESS Phrase**

::

   CREATE TABLE test_tab (
     id            NUMBER(10)    NOT NULL,
     description   VARCHAR2(100) NOT NULL,
     created_date  DATE          NOT NULL,
     created_by    VARCHAR2(50)  NOT NULL,
     updated_date  DATE,
     updated_by    VARCHAR2(50)
   )
   NOCOMPRESS
   PARTITION BY RANGE (created_date) (
     PARTITION test_tab_q1 VALUES LESS THAN (TO_DATE('01/04/2003', 'DD/MM/YYYY')) COMPRESS,
     PARTITION test_tab_q2 VALUES LESS THAN (MAXVALUE)
   );

**Output**

::

   CREATE
        TABLE
             test_tab (
                  id NUMBER (10) NOT NULL
                  ,description VARCHAR2 (100) NOT NULL
                  ,created_date DATE NOT NULL
                  ,created_by VARCHAR2 (50) NOT NULL
                  ,updated_date DATE
                  ,updated_by VARCHAR2 (50)
             ) /*NOCOMPRESS*/
             PARTITION BY RANGE (created_date) (
                  PARTITION test_tab_q1
                  VALUES LESS THAN (
                       TO_DATE( '01/04/2003' ,'DD/MM/YYYY' )
                  ) /*COMPRESS*/
                  ,PARTITION test_tab_q2
                  VALUES LESS THAN (MAXVALUE)
   ) ;
