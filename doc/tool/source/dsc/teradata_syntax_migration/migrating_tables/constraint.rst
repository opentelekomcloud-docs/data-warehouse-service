:original_name: dws_16_0070.html

.. _dws_16_0070:

.. _en-us_topic_0000001860318621:

CONSTRAINT
==========

A table CONSTRAINT is applied to multiple columns. Migration tool supports the following constraints:

-  REFERENCES constraint / FOREIGN KEY: migration currently NOT supported by tool.
-  PRIMARY KEY constraint: migration supported by tool.
-  UNIQUE constraint: migration supported by tool.

**Input: CREATE TABLE with CONSTRAINT**

::

   CREATE SET TABLE DP_SEDW.T_170UT_HOLDER_ACCT, NO FALLBACK,
      NO BEFORE JOURNAL, NO AFTER JOURNAL
    ( BUSINESSDATE   VARCHAR(10)
    , SOURCESYSTEM   VARCHAR(5)
    , UPLOADCODE     VARCHAR(1)
    , HOLDER_NO      VARCHAR(7)  NOT NULL
    , POSTAL_ADD_4   VARCHAR(40)
    , EPF_IND         CHAR(1)
    , CONSTRAINT uq_t_170ut_hldr UNIQUE ( SOURCESYSTEM, UPLOADCODE, HOLDER_NO )
         ) PRIMARY INDEX ( HOLDER_NO, SOURCESYSTEM ) ;

**Output:**

::

   CREATE TABLE DP_SEDW.T_170UT_HOLDER_ACCT
      ( BUSINESSDATE      VARCHAR( 10 )
      , SOURCESYSTEM      VARCHAR( 5 )
      , UPLOADCODE        VARCHAR( 1 )
      , HOLDER_NO         VARCHAR( 7 )   NOT NULL
      , POSTAL_ADD_4      VARCHAR( 40 )
      , EPF_IND           CHAR( 1 )
      , CONSTRAINT uq_t_170ut_hldr UNIQUE ( SOURCESYSTEM, UPLOADCODE, HOLDER_NO )
              )
   DISTRIBUTE BY HASH ( HOLDER_NO, SOURCESYSTEM );

**Input:**

After table creation, CONSTRAINT can be added to a table column to put some restriction at column level by using ALTER statement.

::

   CREATE TABLE GCC_PLAN.T1033 ( ROLLOUT_PLAN_LINE_ID NUMBER NOT NULL,
                                                               UDF_FIELD_VALUE_ID NUMBER NOT NULL) ;
   ALTER TABLE GCC_PLAN.T1033
   ADD CONSTRAINT UDF_FIELD_VALUE_ID_PK UNIQUE (UDF_FIELD_VALUE_ID) ;

**Output:**

::

   CREATE TABLE GCC_PLAN.T1033 ( ROLLOUT_PLAN_LINE_ID NUMBER NOT NULL,
                                                               UDF_FIELD_VALUE_ID NUMBER NOT NULL,
                                                               CONSTRAINT UDF_FIELD_VALUE_ID_PK
                                                               UNIQUE (UDF_FIELD_VALUE_ID) ;

.. note::

   Need to put CONSTRAINT creation syntax inside table creation script after all column declaration.
