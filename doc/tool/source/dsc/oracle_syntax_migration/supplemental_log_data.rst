:original_name: dws_07_6817.html

.. _dws_07_6817:

Supplemental Log Data
=====================

Supplemental columns can be recorded in redo log files. The process of recording these additional columns is called supplemental logging. The Oracle database supports this function, but GaussDB does not.

**Input**

::

   CREATE TABLE sad.fnd_lookup_values_t
              (
      lookup_code_id NUMBER NOT NULL /* ENABLE */
      ,lookup_code VARCHAR2 (40) NOT NULL /* ENABLE */
      ,meaning       VARCHAR2 (100)
      ,other_meaning VARCHAR2 (100)
      ,order_by_no   NUMBER
      ,start_time    DATE DEFAULT SYSDATE NOT NULL /* ENABLE */
      ,end_time    DATE
      ,enable_flag CHAR( 1 ) DEFAULT 'Y' NOT NULL /* ENABLE */
      ,disable_date DATE
      ,created_by   NUMBER ( 15 ,0 ) NOT NULL /* ENABLE */
      ,creation_date DATE NOT NULL /* ENABLE */
      ,last_updated_by NUMBER ( 15 ,0 ) NOT NULL /* ENABLE */
      ,last_update_date DATE NOT NULL /* ENABLE */
      ,last_update_login NUMBER ( 15 ,0 ) DEFAULT 0 NOT NULL /* ENABLE */
      ,description    VARCHAR2 (500)
      ,lookup_type_id NUMBER NOT NULL/* ENABLE */
      ,attribute4 VARCHAR2 (250)
      ,supplemental log data (ALL) COLUMNS
      ) ;

**Output**

::

   CREATE TABLE sad.fnd_lookup_values_t
              (
      lookup_code_id NUMBER NOT NULL /* ENABLE */
      ,lookup_code VARCHAR2 (40) NOT NULL /* ENABLE */
      ,meaning       VARCHAR2 (100)
      ,other_meaning VARCHAR2 (100)
      ,order_by_no   NUMBER
      ,start_time    DATE DEFAULT SYSDATE NOT NULL /* ENABLE */
      ,end_time    DATE
      ,enable_flag CHAR( 1 ) DEFAULT 'Y' NOT NULL /* ENABLE */
      ,disable_date DATE
      ,created_by   NUMBER ( 15 ,0 ) NOT NULL /* ENABLE */
      ,creation_date DATE NOT NULL /* ENABLE */
      ,last_updated_by NUMBER ( 15 ,0 ) NOT NULL /* ENABLE */
      ,last_update_date DATE NOT NULL /* ENABLE */
      ,last_update_login NUMBER ( 15 ,0 ) DEFAULT 0 NOT NULL /* ENABLE */
      ,description    VARCHAR2 (500)
      ,lookup_type_id NUMBER NOT NULL/* ENABLE */
      ,attribute4 VARCHAR2 (250)
      /* ,supplemental log data (ALL) COLUMNS */
      ) ;

.. note::

   The **SUPPLEMENTAL LOG DATA** functions that are not supported by GaussDB need to be commented out.

**SUPPLEMENTAL LOG DATA** is not supported by **CREATE TABLE** and needs to be commented out.

**Input**

.. code-block::

    CREATE TABLE SAD.FND_DATA_CHANGE_LOGS_T
      (    LOGID NUMBER,
           TABLE_NAME VARCHAR2(40) NOT NULL ENABLE,
           TABLE_KEY_COLUMNS VARCHAR2(200),
           TABLE_KEY_VALUES VARCHAR2(200),
           COLUMN_NAME VARCHAR2(40) NOT NULL ENABLE,
           COLUMN_CHANGE_FROM_VALUE VARCHAR2(200),
           COLUMN_CHANGE_TO_VALUE VARCHAR2(200),
           DESCRIPTION VARCHAR2(500),
            SUPPLEMENTAL LOG DATA (ALL) COLUMNS
      );

**Output**

.. code-block::

   CREATE TABLE sad.fnd_data_change_logs_t
     (
        logid                    NUMBER
        ,table_name               VARCHAR2 (40) NOT NULL /* ENABLE */
        ,table_key_columns        VARCHAR2 (200)
        ,table_key_values         VARCHAR2 (200)
        ,column_name              VARCHAR2 (40) NOT NULL /* ENABLE */
        ,column_change_from_value VARCHAR2 (200)
        ,column_change_to_value   VARCHAR2 (200)
        ,description              VARCHAR2 (500)
        /*, SUPPLEMENTAL LOG DATA (ALL) COLUMNS*/
     )
