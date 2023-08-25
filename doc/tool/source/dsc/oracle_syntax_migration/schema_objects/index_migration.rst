:original_name: dws_mt_0110.html

.. _dws_mt_0110:

Index Migration
===============

When an index is created in GaussDB(DWS), a schema name cannot be specified along with the index name. The index will be automatically created in the schema where the index table is created.


.. figure:: /_static/images/en-us_image_0000001233800835.png
   :alt: **Figure 1** Input: INDEX

   **Figure 1** Input: INDEX


.. figure:: /_static/images/en-us_image_0000001233922325.png
   :alt: **Figure 2** Output: INDEX

   **Figure 2** Output: INDEX

**Input - Function-based indexes by using CASE**

A function-based index is an index that is created on the results of a function or expression on top of a column.

**Output**

.. code-block::

   CREATE
        UNIQUE index GCC_RSRC_ASSIGN_U1
             ON GCC_PLAN.GCC_RSRC_ASSIGN_T (
             (CASE
                  WHEN( ENABLE_FLAG = 'Y' AND ASSIGN_TYPE = '13' AND WORK_ORDER_ID IS NOT NULL )
                  THEN WORK_ORDER_ID
                  ELSE NULL
             END)
        ) ;

.. note::

   The expression or function needs to be put inside brackets.

**Input - Function-based indexes by using DECODE**

.. code-block::

   CREATE UNIQUE index GCC_PLAN_N2
             ON GCC_PLAN.GCC_PLAN_T (
             DECODE (
                  ENABLE_FLAG
                  ,'Y'
                  ,BUSINESS_ID
                  ,NULL
             )
        ) ;

**Output**

.. code-block::

   CREATE UNIQUE index GCC_PLAN_N2
             ON GCC_PLAN.GCC_PLAN_T (
             (DECODE (
                  ENABLE_FLAG
                  ,'Y'
                  ,BUSINESS_ID
                  ,NULL
             ))
        ) ;

.. note::

   The expression or function needs to be put inside brackets.

**ORA_HASH**

ORA_HASH is a function that computes a hash value for a given expression or column. If this function is specified on the column(s) in the CREATE INDEX statement, this function will be removed.

**Input**

.. code-block::

   create index SD_WO.WO_WORK_ORDER_T_N3 on SD_WO.WO_WORK_ORDER_T (PROJECT_NUMBER, ORA_HASH(WORK_ORDER_NAME));

**Output**

.. code-block::

   CREATE
   index WO_WORK_ORDER_T_N3
   ON SD_WO.WO_WORK_ORDER_T (
   PROJECT_NUMBER
   ,ORA_HASH( WORK_ORDER_NAME )
   ) ;

**DECODE**

If DECODE function in the CREATE INDEX statement is used as a part of a column, the following error will be reported: "syntax error at or near 'DECODE' (Script - gcc_plan_t.sql)".

**Input**

.. code-block::

   create unique index GCC_PLAN.GCC_PLAN_N2 on GCC_PLAN.GCC_PLAN_T (DECODE(ENABLE_FLAG,'Y',BUSINESS_ID,NULL));

**Output**

.. code-block::

   CREATE
   UNIQUE index GCC_PLAN_N2
   ON GCC_PLAN.GCC_PLAN_T (
   DECODE (
   ENABLE_FLAG
   ,'Y'
   ,BUSINESS_ID
   ,NULL
   )
   ) ;

**CASE statement**

The CASE statement is not supported in the CREATE INDEX statement.

**Input**

.. code-block::

   CREATE
        UNIQUE index GCC_RSRC_ASSIGN_U1
             ON GCC_PLAN.GCC_RSRC_ASSIGN_T (
             (CASE
                  WHEN( ENABLE_FLAG = 'Y' AND ASSIGN_TYPE = '13' AND WORK_ORDER_ID IS NOT NULL )
                  THEN WORK_ORDER_ID
                  ELSE NULL
             END)
        ) ;

**Output**

.. code-block::

   CREATE UNIQUE INDEX gcc_rsrc_assign_u1
     ON gcc_plan.gcc_rsrc_assign_t ( (( CASE
                                                           WHEN( enable_flag = 'Y'
                                                                     AND assign_type = '13'
                                                                     AND work_order_id IS NOT NULL )
                                                     THEN work_order_id
                                                     ELSE NULL END )) );
