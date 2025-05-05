:original_name: dws_16_0068.html

.. _dws_16_0068:

.. _en-us_topic_0000001860198845:

TITLE
=====

The keyword **TITLE** is supported for Teradata Permanent, Global Temporary and Volatile tables. In the migration process, the TITLE text is migrated as a comment.

.. note::

   If the TITLE text is split across multiple lines, then in the migrated script, the line breaks (ENTER) are replaced with a space.

**Input: CREATE TABLE with TITLE**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) TITLE 'column_a'
   );

**Output:**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) /* TITLE 'column_a' */
   );

**Input: TABLE with multiline TITLE**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) TITLE 'This is a
   very long title'
   );

**Output:**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) /* TITLE 'This is a  very long title' */
   );

**Input: TABLE with COLUMN TITLE**

DSC migrates COLUMN TITLE as a new outer query.

::

   SELECT customer_id (TITLE 'cust_id')
   FROM Customer_T
   WHERE cust_id > 10;

**Output:**

::

   SELECT
             customer_id  AS "cust_id"
        FROM
             (
                  SELECT
                            customer_id
                       FROM
                            Customer_T
                       WHERE
                            cust_id > 10
             )
   ;

**Input: TABLE with COLUMN TITLE and QUALIFY**

::

   SELECT ord_id
   (TITLE 'Order_Id'), order_date, customer_id
     FROM order_t
   WHERE Order_Id > 100
   QUALIFY ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY order_date DESC) <= 5;

**Output:**

.. code-block::

   SELECT
             "mig_tmp_alias1" AS "Order_Id"
        FROM
             (
                  SELECT
                            ord_id AS "mig_tmp_alias1"
                            ,ROW_NUMBER( ) OVER( PARTITION BY customer_id ORDER BY order_date DESC ) AS ROW_NUM1
                       FROM
                            order_t
                       WHERE
                            Order_Id > 100
             ) Q1
        WHERE
             Q1.ROW_NUM1 <= 5
   ;

TITLE with ALIAS
----------------

If the TITLE is accompanied with an ALIAS, the tool will migrate it as follows:

-  **TITLE with AS**: Tool will migrate it with the AS alias.
-  **TITLE with NAMED:** Tool will migrate it with NAMED alias.
-  **TITLE with NAMED and AS**: Tool will migrate it with AS alias.

**Input: TABLE TITLE with NAMED and AS**

::

   SELECT  Acct_ID (TITLE 'Acc Code') (NAMED XYZ)  AS "Account Code"
           ,Acct_Name (TITLE 'Acc Name')
   FROM    GT_JCB_01030_Acct_PBU
   where "Account Code" > 500  group by "Account Code" ,Acct_Name ;

**Output:**

.. code-block::

   SELECT
             Acct_ID AS "Account Code"
             ,Acct_Name AS "Acc Name"
        FROM
             GT_JCB_01030_Acct_PBU
        WHERE
             Acct_ID > 500
        GROUP BY
             Acct_ID ,Acct_Name
   ;

.. note::

   Currently the Migration tool supports the migration of the TITLE command included in the initial CREATE/ALTER statement. The subsequent references of the TITLE specified column are not supported. For example, in the CREATE TABLE statement below, the column **eid** with the TITLE Employee ID will be migrated to a comment but the reference of **eid** in the SELECT statement will be retained as it is.

   Input

   ::

      CREATE TABLE tab1 ( eid INT TITLE 'Employee ID');
      SELECT eid FROM tab1;

   Output

   ::

      CREATE TABLE tab1 (eid INT /*TITLE 'Employee ID'*/);
      SELECT eid from tab1;

TITLE with CREATE VIEW
----------------------

**Input:**

.. code-block::

   REPLACE VIEW ${STG_VIEW}.B971_AUMSUMMARY${TABLE_SUFFIX_INC}
   AS
   LOCK TABLE ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC} FOR ACCESS
   SELECT   AUM_DATE (TITLE '    ')
         ,CLNTCODE (TITLE '    ')
         ,ACCTYPE (TITLE '    ')
         ,CCY (TITLE '  ')
         ,BAL_AMT (TITLE '  ')
         ,MON_BAL_AMT (TITLE '    ')
         ,HK_CLNTCODE (TITLE '   ')
         ,MNT_DATE (TITLE '    ')
   FROM ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC};
   it should be migrated as below:
   CREATE OR REPLACE VIEW ${STG_VIEW}.B971_AUMSUMMARY${TABLE_SUFFIX_INC}
   AS
   /*LOCK TABLE ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC} FOR ACCESS */
   SELECT   AUM_DATE  /* (TITLE '    ') */
         ,CLNTCODE  /* (TITLE '    ') */
         ,ACCTYPE  /* (TITLE '    ') */
         ,CCY  /* (TITLE '  ') */
         ,BAL_AMT  /* (TITLE '  ') */
         ,MON_BAL_AMT  /* (TITLE '    ') */
         ,HK_CLNTCODE  /* (TITLE '   ') */
         ,MNT_DATE  /* (TITLE '    ') */
   FROM ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC};

**Output:**

.. code-block::

   CREATE OR REPLACE VIEW ${STG_VIEW}.B971_AUMSUMMARY${TABLE_SUFFIX_INC}
   AS
   /*LOCK TABLE ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC} FOR ACCESS */
   SELECT   AUM_DATE  /* (TITLE '    ') */
         ,CLNTCODE  /* (TITLE '    ') */
         ,ACCTYPE  /* (TITLE '    ') */
         ,CCY  /* (TITLE '  ') */
         ,BAL_AMT  /* (TITLE '  ') */
         ,MON_BAL_AMT  /* (TITLE '    ') */
         ,HK_CLNTCODE  /* (TITLE '   ') */
         ,MNT_DATE  /* (TITLE '    ') */
   FROM ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC};
