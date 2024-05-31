:original_name: dws_07_6821.html

.. _dws_07_6821:

.. _en-us_topic_0000001819336317:

Tables
======

.. _en-us_topic_0000001819336317__en-us_topic_0000001706224181_en-us_topic_0238518431_en-us_topic_0237362467_en-us_topic_0202685975_section1332761015481:

Distribution Key
----------------

DISTRIBUTE ON (column) should be migrated to DISTRIBUTE BY HASH (column).

+-----------------------------------------------------------------------------+-----------------------------------------------------------------------------+
| Netezza Syntax                                                              | Syntax After Migration                                                      |
+=============================================================================+=============================================================================+
| ::                                                                          | ::                                                                          |
|                                                                             |                                                                             |
|    CREATE TABLE  N_AG_AMT_H                                                 |    CREATE TABLE  N_AG_AMT_H                                                 |
|    (                                                                        |    (                                                                        |
|         AG_NO                national character varying(50)    not null,    |         AG_NO                national character varying(50)    not null,    |
|         AG_CATEG_CD      national character varying(12)    not null,        |         AG_CATEG_CD      national character varying(12)    not null,        |
|         AMT_TYPE_CD      national character varying(12)    not null,        |         AMT_TYPE_CD      national character varying(12)    not null,        |
|         DATA_START_DT   date                                      not null, |         DATA_START_DT   date                                      not null, |
|         CCY_CD               national character varying(3)     not null,    |         CCY_CD               national character varying(3)     not null,    |
|         DATA_END_DT      date                                               |         DATA_END_DT      date                                               |
|    )                                                                        |    ) WITH(ORIENTATION=COLUMN)                                               |
|    DISTRIBUTE ON (AG_NO, AG_CATEG_CD, AMT_TYPE_CD)                          |    DISTRIBUTE BY HASH (AG_NO, AG_CATEG_CD, AMT_TYPE_CD)                     |
|    ORGANIZE   ON (AG_CATEG_CD, AMT_TYPE_CD, DATA_END_DT)                    |    /* ORGANIZE   ON (AG_CATEG_CD, AMT_TYPE_CD, DATA_END_DT) */              |
|    ;                                                                        |    ;                                                                        |
+-----------------------------------------------------------------------------+-----------------------------------------------------------------------------+

.. _en-us_topic_0000001819336317__en-us_topic_0000001706224181_en-us_topic_0238518431_en-us_topic_0237362467_en-us_topic_0202685975_section15902134185010:

ORGANIZE ON
-----------

ORGANIZE ON will be commented out.

+-----------------------------------------------------------------------------+----------------------------------------------------------------------------+
| Netezza Syntax                                                              | Syntax After Migration                                                     |
+=============================================================================+============================================================================+
| ::                                                                          | ::                                                                         |
|                                                                             |                                                                            |
|    CREATE TABLE  N_AG_AMT_H                                                 |    CREATE TABLE  N_AG_AMT_H                                                |
|    (                                                                        |    (                                                                       |
|         AG_NO               national character varying(50) not null,        |         AG_NO                national character varying(50) not null,      |
|         AG_CATEG_CD     national character varying(12) not null,            |         AG_CATEG_CD      national character varying(12) not null,          |
|         AMT_TYPE_CD     national character varying(12) not null,            |         AMT_TYPE_CD      national character varying(12) not null,          |
|         DATA_START_DT  date                                      not null,  |         DATA_START_DT date                                      not null,  |
|         CCY_CD              national character varying(3)     not null,     |         CCY_CD               national character varying(3)     not null,   |
|         DATA_END_DT     date                                                |       DATA_END_DT      date                                                |
|    )                                                                        |    ) WITH(ORIENTATION=COLUMN)                                              |
|    DISTRIBUTE ON (AG_NO, AG_CATEG_CD, AMT_TYPE_CD)                          |    DISTRIBUTE BY HASH (AG_NO, AG_CATEG_CD, AMT_TYPE_CD)                    |
|    ORGANIZE ON (AG_CATEG_CD, AMT_TYPE_CD, DATA_END_DT)                      |    /* ORGANIZE ON (AG_CATEG_CD, AMT_TYPE_CD, DATA_END_DT)*/                |
|    ;                                                                        |    ;                                                                       |
+-----------------------------------------------------------------------------+----------------------------------------------------------------------------+

Large Field Type
----------------

The row-store supports BLOB and CLOB. Column storage does not support BLOB, but it supports CLOB.

+---------------------------------------------------------------------------+---------------------------------------------------------------------------+
| Netezza Syntax                                                            | Syntax After Migration                                                    |
+===========================================================================+===========================================================================+
| ::                                                                        | ::                                                                        |
|                                                                           |                                                                           |
|    CREATE TABLE  prod                                                     |    CREATE TABLE  prod                                                     |
|     (                                                                     |     (                                                                     |
|          prod_no              number(6)      not null,                    |          prod_no              number(6)      not null,                    |
|          prod_name         national character varying(32)      not null,  |          prod_name         national character varying(32)      not null,  |
|          prod_desc          clob,                                         |          prod_desc          clob,                                         |
|          prod_image        blob                                           |          prod_image       bytea                                           |
|     )                                                                     |     ) WITH(ORIENTATION=COLUMN)                                            |
|     DISTRIBUTE ON (prod_no, prod_name)                                    |     DISTRIBUTE BY HASH (prod_no, prod_name)                               |
|     ORGANIZE   ON (prod_no, prod_name)                                    |     /* ORGANIZE   ON (prod_no, prod_name) */                              |
|     ;                                                                     |     ;                                                                     |
+---------------------------------------------------------------------------+---------------------------------------------------------------------------+
