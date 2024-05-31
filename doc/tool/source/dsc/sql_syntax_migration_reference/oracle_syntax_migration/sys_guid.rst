:original_name: dws_07_6819.html

.. _dws_07_6819:

SYS_GUID
========

SYS_GUID is a built-in function which returns the Global Unique Identifier (GUID) for a row in a table. It accepts no arguments and returns a RAW value of 16 bytes.

**Input**

::

   CREATE TABLE sad.fnd_data_change_logs_t
     (
       logid                    NUMBER,
       table_name               VARCHAR2 (40) NOT NULL /* ENABLE */
       ,table_key_columns        VARCHAR2 (200),
    table_key_values         VARCHAR2 (200),
    column_name              VARCHAR2 (40) NOT NULL /* ENABLE */
    ,column_change_from_value VARCHAR2 (200),
    column_change_to_value   VARCHAR2 (200),
    organization_id          NUMBER,
    created_by               NUMBER (15, 0) NOT NULL /* ENABLE */
    ,creation_date            DATE NOT NULL /* ENABLE */
    ,last_updated_by          NUMBER (15, 0) NOT NULL /* ENABLE */
    ,last_update_date         DATE NOT NULL /* ENABLE */
    ,last_update_login        NUMBER (15, 0) DEFAULT 0 NOT NULL /* ENABLE */
    ,description              VARCHAR2 (500),
    sys_id                   VARCHAR2 (32) DEFAULT Sys_guid( )
    /*, SUPPLEMENTAL LOG DATA (ALL) COLUMNS*/
     );

**Output**

::

   CREATE TABLE sad.fnd_data_change_logs_t
     (
       logid                    NUMBER,
       table_name               VARCHAR2 (40) NOT NULL /* ENABLE */
       ,table_key_columns        VARCHAR2 (200),
    table_key_values         VARCHAR2 (200),
    column_name              VARCHAR2 (40) NOT NULL /* ENABLE */
    ,column_change_from_value VARCHAR2 (200),
    column_change_to_value   VARCHAR2 (200),
    organization_id          NUMBER,
    created_by               NUMBER (15, 0) NOT NULL /* ENABLE */
    ,creation_date            DATE NOT NULL /* ENABLE */
    ,last_updated_by          NUMBER (15, 0) NOT NULL /* ENABLE */
    ,last_update_date         DATE NOT NULL /* ENABLE */
    ,last_update_login        NUMBER (15, 0) DEFAULT 0 NOT NULL /* ENABLE */
    ,description              VARCHAR2 (500),
    sys_id                   VARCHAR2 (32) DEFAULT MIG_ORA_EXT.Sys_guid( )
    /*, SUPPLEMENTAL LOG DATA (ALL) COLUMNS*/
     );
