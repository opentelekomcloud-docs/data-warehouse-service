:original_name: dws_mt_0137.html

.. _dws_mt_0137:

String Functions
================

This section describes the following string functions:

-  :ref:`LISTAGG <en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section19572182825420>`
-  :ref:`STRAGG <en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section1645916521546>`
-  :ref:`WM_CONCAT <en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section266693805512>`
-  :ref:`NVL2 and REPLACE <en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section11995195695518>`
-  :ref:`QUOTE <en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section866063713564>`

.. _en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section19572182825420:

LISTAGG
-------

**LISTAGG** is used to order data in columns within each group specified in the **ORDER BY** clause and concatenates the order results.


.. figure:: /_static/images/en-us_image_0000001188681114.png
   :alt: **Figure 1** Input - Listagg

   **Figure 1** Input - Listagg


.. figure:: /_static/images/en-us_image_0000001188362642.png
   :alt: **Figure 2** Output - Listagg

   **Figure 2** Output - Listagg

LISTAGG can be migrated after **MigSupportForListAgg** is set to **false**.

**Input- LISTAGG**

.. code-block::

   SELECT LISTAGG(BRANCH_ID, ',') WITHIN GROUP(ORDER BY AREA_ORDER) PRODUCTRANGE
                          FROM (SELECT DISTINCT VB.BRANCH_ID,
                                                VB.VER_ID,
                                                VB.AREA_ORDER
                                  FROM SPMS_VERSION_BRANCH VB, SPMS_NODE_SET NS
                                 WHERE VB.BRANCH_TYPE IN ('1', '3')
                                   AND VB.AGENCY_BRANCH = NS.BRANCH_ID);

**Output**

.. code-block::

   SELECT LISTAGG (BRANCH_ID,',') WITHIN GROUP (
    ORDER BY AREA_ORDER ) PRODUCTRANGE
    FROM ( SELECT
            DISTINCT VB.BRANCH_ID
            ,VB.VER_ID
            ,VB.AREA_ORDER
             FROM
             SPMS_VERSION_BRANCH VB
            ,SPMS_NODE_SET NS
             WHERE VB.BRANCH_TYPE IN (
              '1','3')
            AND VB.AGENCY_BRANCH = NS.BRANCH_ID)
   ;

.. _en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section1645916521546:

STRAGG
------

**STRAGG** is a string aggregate function used to collect values from multiple rows into a comma-separated string.

**Input-\ STRAGG**

.. code-block::

   SELECT DEPTNO,ENAME,STRAGG(ename) over (partition by deptno order by
              ename RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
                   AS ENAME_STR FROM EMP;

**Output**

.. code-block::

   SELECT DEPTNO,ENAME,STRING_AGG (
       ename,',') over( partition BY deptno ORDER BY
    ename RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED
    FOLLOWING ) AS ENAME_STR
       FROM EMP
   ;

.. _en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section266693805512:

WM_CONCAT
---------

**WM_CONCAT** is used to aggregate data from a number of rows into a single row, giving a list of data associated with a specific value.


.. figure:: /_static/images/en-us_image_0000001234200719.png
   :alt: **Figure 3** Input - WM_Concat

   **Figure 3** Input - WM_Concat


.. figure:: /_static/images/en-us_image_0000001234200717.png
   :alt: **Figure 4** Output - WM_Concat

   **Figure 4** Output - WM_Concat

.. _en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section11995195695518:

NVL2 and REPLACE
----------------

**NVL2**\ ( expression, value1, value2) is a function used to determine the value returned by a query based on whether a specified expression is null or not. If the expression is not null, then NVL2 returns value1. If the expression is null, then NVL2 returns value2.

**Input - NVL2**

.. code-block::

   NVL2(Expr1, Expr2, Expr3)

**Output**

.. code-block::

   DECODE(Expr1, NULL, Expr3, Expr2)

The **REPLACE** function is used to return char with every occurrence of search_string replaced with replacement_string. If replacement_string is omitted or null, then all occurrences of search_string are removed.

The **REPLACE** function in Oracle contains two mandatory parameters and one optional parameter. The **REPLACE** function in GaussDB(DWS) contains three mandatory parameters.

**Input - Nested REPLACE**

.. code-block::

   CREATE
        OR REPLACE FUNCTION F_REPLACE_COMMA ( IS_STR IN VARCHAR2 ) RETURN VARCHAR2 IS BEGIN
                  IF
                            IS_STR IS NULL
                            THEN RETURN NULL ;
                       ELSE
                       RETURN REPLACE( REPLACE( IS_STR ,'a' ) ,CHR ( 10 ) ) ;
             END IF ;
   END F_REPLACE_COMMA ;
   /

**Output**

.. code-block::

   CREATE
        OR REPLACE FUNCTION F_REPLACE_COMMA ( IS_STR IN VARCHAR2 ) RETURN VARCHAR2 IS BEGIN
                  IF
                            IS_STR IS NULL
                            THEN RETURN NULL ;
                       ELSE
                       RETURN REPLACE( REPLACE( IS_STR ,'a' ,'' ) ,CHR ( 10 ) ,'' ) ;
             END IF ;
   end ;
   /

**Input - More than one REPLACE**

.. code-block::

   SELECT
             REPLACE( 'JACK and JUE' ,'J', '' ) "Changes"
             ,REPLACE( 'JACK1 and JUE' ,'J' ) "Changes1"
             ,REPLACE( 'JACK2 and JUE' ,'J' ) "Changes2"
        FROM
             DUAL
   ;

**Output**

.. code-block::

   SELECT
             REPLACE( 'JACK and JUE' ,'J' ,'' ) "Changes"
             ,REPLACE( 'JACK1 and JUE' ,'J' ,'' ) "Changes1"
             ,REPLACE( 'JACK2 and JUE' ,'J' ,'' ) "Changes2"
        FROM
             DUAL
   ;

**Input - REPLACE** **with Three parameters**

.. code-block::

   SELECT
             REPLACE( '123tech123' ,'123', '1')
        FROM
             dual
   ;

**Output**

.. code-block::

   SELECT
             REPLACE( '123tech123' ,'123' , '1' )
        FROM
             dual
   ;

.. _en-us_topic_0000001188521064__en-us_topic_0238518398_en-us_topic_0237362203_en-us_topic_0202727281_section866063713564:

QUOTE
-----

**QUOTE** allows the user to embed single-quotes in literal strings without having to resort to double quotes. That is, you can use single quotes to specify a literal string.

For example:

.. code-block::

   SELECT q'[I'm using quote operator in SQL statement]' "Quote (q) Operator" FROM dual;


.. figure:: /_static/images/en-us_image_0000001188362646.png
   :alt: **Figure 5** Input - Quote

   **Figure 5** Input - Quote


.. figure:: /_static/images/en-us_image_0000001188681116.png
   :alt: **Figure 6** Output - Quote

   **Figure 6** Output - Quote
