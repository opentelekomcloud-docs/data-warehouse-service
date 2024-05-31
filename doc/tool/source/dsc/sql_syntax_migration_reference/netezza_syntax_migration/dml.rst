:original_name: dws_07_6826.html

.. _dws_07_6826:

DML
===

Gauss keyword: SOURCE specified as column-alias without AS keyword
------------------------------------------------------------------

+----------------------------------------------+-----------------------------------------------+
| Netezza Syntax                               | Syntax After Migration                        |
+==============================================+===============================================+
| ::                                           | ::                                            |
|                                              |                                               |
|    SELECT SUBSTR( OP_SOURCE ,1 ,4 ) SOURCE   |    SELECT SUBSTR( OP_SOURCE ,1 ,4 ) AS SOURCE |
|         , ONLINE_FLAG, 'TRD' AS SRC_SYS      |         , ONLINE_FLAG, 'TRD' AS SRC_SYS       |
|         , CURRENT_TIMESTAMP AS ETL_LOAD_TIME |         , CURRENT_TIMESTAMP AS ETL_LOAD_TIME  |
|      FROM tb_keyword_source;                 |      FROM tb_keyword_source;                  |
+----------------------------------------------+-----------------------------------------------+

FREEZE
------

+-------------------------------------------------------+---------------------------------------------------------+
| Netezza Syntax                                        | Syntax After Migration                                  |
+=======================================================+=========================================================+
| ::                                                    | ::                                                      |
|                                                       |                                                         |
|    INSERT INTO tmp_tb_keyword_freeze                  |    INSERT INTO tmp_tb_keyword_freeze                    |
|         ( BRANCH_CODE, FREEZE, UNFREEZE, BRAN_JYSDM ) |         ( BRANCH_CODE, "FREEZE", UNFREEZE, BRAN_JYSDM ) |
|    SELECT BRANCH_CODE, FREEZE, UNFREEZE, BRAN_JYSDM   |    SELECT BRANCH_CODE, "FREEZE", UNFREEZE, BRAN_JYSDM   |
|      FROM tb_keyword_freeze;                          |      FROM tb_keyword_freeze;                            |
+-------------------------------------------------------+---------------------------------------------------------+

.. note::

   A new configuration parameter "keywords_addressed_using_doublequote" should be introduced with the below value:

   keywords_addressed_using_doublequote=freeze

   keywords_addressed_using_as=owner,attribute,source,freeze

   create table t12 (c1 int, FREEZE varchar(10)); ==> create table t12 (c1 int, "freeze" varchar(10));

   select c1, Freeze from t12; ==> select c1, "freeze" from t12;

   select c1 freeze from t12; ==> select c1 as freeze from t12;

OWNER (AS should be specified)
------------------------------

+-----------------------------------+-----------------------------------+
| Netezza Syntax                    | Syntax After Migration            |
+===================================+===================================+
| ::                                | ::                                |
|                                   |                                   |
|    SELECT username owner          |    SELECT username AS owner       |
|      FROM tb_ntz_keyword_owner;   |      FROM tb_ntz_keyword_owner;   |
+-----------------------------------+-----------------------------------+

ATTRIBUTE (AS should be specified)
----------------------------------

+-----------------------------------------------------------+--------------------------------------------------------------+
| Netezza Syntax                                            | Syntax After Migration                                       |
+===========================================================+==============================================================+
| ::                                                        | ::                                                           |
|                                                           |                                                              |
|    SELECT t1.etl_date, substr(t1.attribute,1,1) attribute |    SELECT t1.etl_date, substr(t1.attribute,1,1) AS attribute |
|         , t1.cust_no, t1.branch_code                      |         , t1.cust_no, t1.branch_code                         |
|      FROM ( SELECT etl_date,attribute,cust_no,branch_code |      FROM ( SELECT etl_date,attribute,cust_no,branch_code    |
|               FROM tb_ntz_keyword_attribute               |               FROM tb_ntz_keyword_attribute                  |
|              WHERE etl_date = CURRENT_DATE ) t1;          |              WHERE etl_date = CURRENT_DATE ) t1;             |
+-----------------------------------------------------------+--------------------------------------------------------------+
