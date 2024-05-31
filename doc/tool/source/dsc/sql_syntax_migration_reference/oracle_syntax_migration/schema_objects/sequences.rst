:original_name: dws_mt_0112.html

.. _dws_mt_0112:

Sequences
=========

A sequence is an Oracle object used to generate a number sequence. This can be useful when you need to create an autonumber column to act as a primary key.

If :ref:`MigSupportSequence <en-us_topic_0000001772536388__en-us_topic_0000001658024910_en-us_topic_0218440495_li1568184013455>` is set to **true** (default), a sequence is created in the **PUBLIC** schema.

.. note::

   -  **CACHE** and **ORDER** cannot be migrated.
   -  In Oracle, the maximum value of **MAXVALUE** can be set to **999999999999999999999999999**. In GaussDB(DWS), the maximum value of **MAXVALUE** can be set to **9223372036854775807**.
   -  Before migrating a sequence, copy the content in the **sequence_scripts.sql** file and paste it to execute the script in all the target databases. For details, see :ref:`Executing Custom DB Scripts <en-us_topic_0000001772696064__en-us_topic_0000001657865686_en-us_topic_0000001382208082_section20896201617216>`.

Sequence
--------

**Input** - **CREATE SEQUENCE**

::

   CREATE SEQUENCE GROUP_DEF_SEQUENCE
     minvalue 1
     maxvalue 100000000000000000000
     start with 1152
     increment by 1
     cache 50
     order;

**Output**

::

   INSERT
        INTO
             PUBLIC.MIG_SEQ_TABLE (
                  SCHEMA_NAME
                  ,SEQUENCE_NAME
                  ,START_WITH
                  ,INCREMENT_BY
                  ,MIN_VALUE
                  ,MAX_VALUE
                  ,CYCLE_I
                  ,CACHE
                  ,ORDER_I
             )
        VALUES (
             UPPER( current_schema ( ) )
             ,UPPER( 'GROUP_DEF_SEQUENCE' )
             ,1152
             ,1
             ,1
             ,9223372036854775807
             ,FALSE
             ,20
             ,FALSE
        )
   ;

SEQUENCE with NOCACHE
---------------------

**Input** - **CREATE SEQUENCE** with **NOCACHE**

::

   CREATE SEQUENCE customers_seq
    START WITH 1000
    INCREMENT BY   1
    NOCACHE
    NOCYCLE;

**Output**

::

   INSERT
        INTO
             PUBLIC.MIG_SEQ_TABLE (
                  SCHEMA_NAME
                  ,SEQUENCE_NAME
                  ,START_WITH
                  ,INCREMENT_BY
                  ,MIN_VALUE
                  ,MAX_VALUE
                  ,CYCLE_I
                  ,CACHE
                  ,ORDER_I
             )
        VALUES (
             UPPER( current_schema ( ) )
             ,UPPER( 'customers_seq' )
             ,1000
             ,1
             ,1
             ,999999999999999999999999999
             ,FALSE
             ,20
             ,FALSE
        )
   ;

**Input** - **CREATE SEQUENCE** with a specified schema name
------------------------------------------------------------

**Input** - **CREATE SEQUENCE** with a specified schema name

::

   CREATE SEQUENCE scott.seq_customers
    START WITH 1000 INCREMENT BY 1
    MINVALUE 1000 MAXVALUE 999999999999999
    CACHE 20 CYCLE ORDER;

**Output**

::

   INSERT
        INTO
             PUBLIC.MIG_SEQ_TABLE (
                  SCHEMA_NAME
                  ,SEQUENCE_NAME
                  ,START_WITH
                  ,INCREMENT_BY
                  ,MIN_VALUE
                  ,MAX_VALUE
                  ,CYCLE_I
                  ,CACHE
                  ,ORDER_I
             )
        VALUES (
             UPPER( 'scott' )
             ,UPPER( 'seq_customers' )
             ,1000
             ,1
             ,1000
             ,999999999999999
             ,TRUE
             ,20
             ,FALSE
        )
   ;

CREATE SEQUENCE with a Default Value
------------------------------------

**Input** - **SEQUENCE** with a default value

::

   CREATE SEQUENCE seq_orders;

**Output**

::

   INSERT
        INTO
             PUBLIC.MIG_SEQ_TABLE (
                  SCHEMA_NAME
                  ,SEQUENCE_NAME
                  ,START_WITH
                  ,INCREMENT_BY
                  ,MIN_VALUE
                  ,MAX_VALUE
                  ,CYCLE_I
                  ,CACHE
                  ,ORDER_I
             )
        VALUES (
             UPPER( current_schema ( ) )
             ,UPPER( 'seq_orders' )
             ,1
             ,1
             ,1
             ,999999999999999999999999999
             ,FALSE
             ,20
             ,FALSE
        )
   ;

NEXTVAL
-------

To migrate the NEXTVAL function, a custom function is provided for generating the next value based on **increment_by**, **max_value**, **min_value**, and **cycle**. During the DSC installation, this function should be created in all the databases where the migration is to be performed.

**NEXTVAL** supports all GaussDB(DWS) versions.

**NEXTVAL** is a system function of Oracle and is not implicitly supported by GaussDB(DWS). To support this function, DSC creates a **NEXTVAL** function in the **PUBLIC** schema. The **PUBLIC.NEXTVAL** function is used in the migrated statements.

.. note::

   If :ref:`MigSupportSequence <en-us_topic_0000001772536388__en-us_topic_0000001658024910_en-us_topic_0218440495_li1568184013455>` is set to **true**, NEXTVAL is migrated to PUBLIC.NEXTVAL('[schema].sequence').

   If :ref:`MigSupportSequence <en-us_topic_0000001772536388__en-us_topic_0000001658024910_en-us_topic_0218440495_li1568184013455>` is set to **false**, NEXTVAL is migrated to NEXTVAL('[schema].sequence').

   Before migrating the NEXTVAL function, copy the content in the **sequence_scripts.sql** file and paste it to execute the script in all the target databases. For details, see :ref:`Executing Custom DB Scripts <en-us_topic_0000001772696064__en-us_topic_0000001657865686_en-us_topic_0000001382208082_section20896201617216>`.

**Input** - **NEXTVAL**

::

   [schema.]sequence.NEXTVAL

**Output**

::

   PUBLIC.nextval('[schema.]sequence')

**Input** - **NEXTVAL**

::

   SELECT
             EMP_ID_SEQ.NEXTVAL INTO
                  SEQ_NUM
             FROM
                  dual
   ;

**Output**

::

   SELECT
             PUBLIC.NEXTVAL ('EMP_ID_SEQ') INTO
                  SEQ_NUM
             FROM
                  dual
   ;

CURRVAL
-------

To migrate the CURRVAL function, you can customize one to return the current value of a sequence. During the DSC installation, this function should be created in all the databases where the migration is to be performed.

**CURRVAL** is a system function of Oracle and is not implicitly supported by GaussDB(DWS). To support this function, DSC creates a **CURRVAL** function in the **PUBLIC** schema. The **PUBLIC.CURRVAL** function is used in the migrated statements.

.. note::

   If :ref:`MigSupportSequence <en-us_topic_0000001772536388__en-us_topic_0000001658024910_en-us_topic_0218440495_li1568184013455>` is set to **true**, CURRVAL is migrated to PUBLIC.CURRVAL('[schema].sequence').

   If :ref:`MigSupportSequence <en-us_topic_0000001772536388__en-us_topic_0000001658024910_en-us_topic_0218440495_li1568184013455>` is set to **false**, CURRVAL is migrated to CURRVAL('[schema].sequence').

   Before migrating the NEXTVAL function, copy the content in the **sequence_scripts.sql** file and paste it to execute the script in all the target databases. For details, see :ref:`Executing Custom DB Scripts <en-us_topic_0000001772696064__en-us_topic_0000001657865686_en-us_topic_0000001382208082_section20896201617216>`.

**Input** - **CURRVAL**

::

   [schema.]sequence.CURRVAL

**Output**

::

   currval('[schema.]sequence')

**Input** - **CURRVAL**

::

   INSERT
        INTO
             Line_items_tab (
                  Orderno
                  ,Partno
                  ,Quantity
             )
        VALUES (
             Order_seq.CURRVAL
             ,20321
             ,3
        )
   ;

**Output**

::

   INSERT
        INTO
             Line_items_tab (
                  Orderno
                  ,Partno
                  ,Quantity
             ) SELECT
                       PUBLIC.CURRVAL ('Order_seq')
                       ,20321
                       ,3
   ;
