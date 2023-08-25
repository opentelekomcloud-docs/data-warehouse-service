:original_name: dws_07_6841.html

.. _dws_07_6841:

Tables
======

GaussDB Keyword
---------------

If a keyword is used as a column name, quotes (") must be added, for example, "order".

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_bd2           |    CREATE TABLE tbl_bd2           |
|     (                             |     (                             |
|     ORDER NUMBER(10),             |     "ORDER" NUMBER(10),           |
|     USER varchar2(30),            |     "USER" varchar2(30),          |
|     DATE VARCHAR(10               |     "DATE" VARCHAR(10             |
|     );                            |     );                            |
+-----------------------------------+-----------------------------------+

Data Type (I)
-------------

LONG VARCHAR should be changed to CLOB.

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2           |
|     (                             |     (                             |
|     ID VARCHAR(36),               |     ID VARCHAR(36),               |
|     NAME LONG VARCHAR             |     NAME CLOB                     |
|     );                            |     );                            |
+-----------------------------------+-----------------------------------+

LONG VARGRAPHIC.

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2           |
|     (                             |     (                             |
|     ID VARCHAR(36),               |     ID VARCHAR(36),               |
|     NAME LONG VARGRAPHIC          |     NAME CLOB                     |
|     );                            |     );                            |
+-----------------------------------+-----------------------------------+

Foreign Key
-----------

Below attributes of Foreign key constraint should be commented:

-  ON UPDATE RESTRICT
-  ENFORCED
-  ENABLE QUERY OPTIMIZATION

   +----------------------------------------------------+----------------------------------------------------+
   | DB2 Syntax                                         | Syntax After Migration                             |
   +====================================================+====================================================+
   | .. code-block::                                    | .. code-block::                                    |
   |                                                    |                                                    |
   |    ALTER TABLE "SCH"."TBL_DB2"                     |    ALTER TABLE "SCH"."TBL_DB2"                     |
   |     ADD CONSTRAINT "Const_Name" FOREIGN KEY("ID")  |     ADD CONSTRAINT "Const_Name" FOREIGN KEY("ID")  |
   |     REFRENCES "SCH"."TBL_DB2_1"("ID")              |     REFRENCES "SCH"."TBL_DB2_1"("ID")              |
   |     ON DELETE CASCADE                              |     ON DELETE CASCADE                              |
   |     ON UPDATE RESTRICT                             |     /*ON UPDATE RESTRICT                           |
   |     ENFORCED                                       |     ENFORCED                                       |
   |     ENABLE QUERY OPTIMIZATION;                     |     ENABLE QUERY OPTIMIZATION*/;                   |
   +----------------------------------------------------+----------------------------------------------------+

Sequence
--------

built-in auto-increment function.

+--------------------------------------------------------------+----------------------------------------------------------+
| DB2 Syntax                                                   | Syntax After Migration                                   |
+==============================================================+==========================================================+
| .. code-block::                                              | .. code-block::                                          |
|                                                              |                                                          |
|    CREATE TABLE tbl_db2                                      |    CREATE SEQUENCE mseq_tbl_db2_id                       |
|     (                                                        |     START WITH +1                                        |
|     ID BIGINT NOT NULL GENERATED BY DEFAULT AS IDENTITY (    |     INCREMENT BY +1                                      |
|           START WITH +1                                      |     MINVALUE +1                                          |
|           INCREMENT BY +1                                    |     MAXVALUE +9223372036854775807                        |
|           MINVALUE +1                                        |     NOCYCLE                                              |
|           MAXVALUE +9223372036854775807                      |     CACHE 20                                             |
|           NO CYCLE                                           |     NOORDER;                                             |
|           CACHE 20                                           |     CREATE TABLE tbl_db2                                 |
|           NO ORDER ) ,                                       |     (                                                    |
|     NAME VARCHAR2(50),                                       |     ID BIGINT NOT NULL DEFAULT mseq_tbl_db2_id.NEXTVAL,  |
|     ORDER VARCHAR2(100)                                      |     NAME VARCHAR2(50),                                   |
|     );                                                       |     "ORDER" VARCHAR2(100)                                |
|                                                              |     );                                                   |
|                                                              |                                                          |
+--------------------------------------------------------------+----------------------------------------------------------+

Tablespace
----------

TABLESPACE for a table to be placed is specified with IN clause.

+-------------------------------------------------------+-------------------------------------------------------+
| DB2 Syntax                                            | Syntax After Migration                                |
+=======================================================+=======================================================+
| .. code-block::                                       | .. code-block::                                       |
|                                                       |                                                       |
|    CREATE TABLE tbl_db2                               |    CREATE TABLE tbl_db2                               |
|     (                                                 |     (                                                 |
|     ID number(20) NOT NULL DEFAULT IDENTITY.NEXTVAL,  |     ID number(20) NOT NULL DEFAULT IDENTITY.NEXTVAL,  |
|     NAME VARCHAR2(50)                                 |     NAME VARCHAR2(50)                                 |
|     )                                                 |     )                                                 |
|     IN tbs1 ;                                         |     TABLESPACE tbs1 ;                                 |
+-------------------------------------------------------+-------------------------------------------------------+

Default
-------

WITH DEFAULT is specified to specified DEFAULT value.

+--------------------------------------+-----------------------------------+
| DB2 Syntax                           | Syntax After Migration            |
+======================================+===================================+
| .. code-block::                      | .. code-block::                   |
|                                      |                                   |
|    CREATE TABLE tbl_db2              |    CREATE TABLE tbl_db2           |
|     (                                |     (                             |
|     ID number(20) ,                  |     ID number(20) ,               |
|     NAME VARCHAR2(50),               |     NAME VARCHAR2(50),            |
|     STATUS CHAR(1) WITH DEFAULT '0'  |     STATUS CHAR(1) DEFAULT '0'    |
|     );                               |     );                            |
+--------------------------------------+-----------------------------------+

DEFAULT specified without value.

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2           |
|     (                             |     (                             |
|     ID number(20) ,               |     ID number(20) ,               |
|     NAME VARCHAR2(50),            |     NAME VARCHAR2(50),            |
|     STATUS CHAR(1) WITH DEFAULT   |     STATUS CHAR(1)                |
|     );                            |     );                            |
+-----------------------------------+-----------------------------------+

Data Type (II)
--------------

CLOB(1048576)

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2           |
|     (                             |     (                             |
|     ID number(20) ,               |     ID number(20) ,               |
|     NAME VARCHAR2(50),            |     NAME VARCHAR2(50),            |
|     REMARKS CLOB(1048576));       |     REMARKS CLOB                  |
|                                   |     );                            |
+-----------------------------------+-----------------------------------+

BLOB(2048000)

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2           |
|     (                             |     (                             |
|     ID number(20) ,               |     ID number(20) ,               |
|     NAME VARCHAR2(50),            |     NAME VARCHAR2(50),            |
|     REMARKS BLOB(2048000)         |     REMARKS BLOB                  |
|     );                            |     );                            |
+-----------------------------------+-----------------------------------+

LOB Options
-----------

LOGGED/UNLOGGED

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2           |
|     (                             |     (                             |
|     "ID" number(20) ,             |     "ID" number(20) ,             |
|     "NAME" VARCHAR2(50),          |     "NAME" VARCHAR2(50),          |
|     "REMARKS" BLOB LOGGED         |     "REMARKS" BLOB /*LOGGED */    |
|     );                            |     );                            |
+-----------------------------------+-----------------------------------+

COMPACT/NOT COMPACT

+----------------------------------------+--------------------------------------------------+
| DB2 Syntax                             | Syntax After Migration                           |
+========================================+==================================================+
| .. code-block::                        | .. code-block::                                  |
|                                        |                                                  |
|    CREATE TABLE tbl_db2                |    CREATE TABLE tbl_db2                          |
|     (                                  |     (                                            |
|     "ID" number(20) ,                  |     "ID" number(20) ,                            |
|     "NAME" VARCHAR2(50),               |     "NAME" VARCHAR2(50),                         |
|     "REMARKS" BLOB LOGGED NOT COMPACT  |     "REMARKS" BLOB /*LOGGED */ /* NOT COMPACT*/  |
|     );                                 |     );                                           |
+----------------------------------------+--------------------------------------------------+

Organize By
-----------

Organize By

+-----------------------------------+------------------------------------+
| DB2 Syntax                        | Syntax After Migration             |
+===================================+====================================+
| .. code-block::                   | .. code-block::                    |
|                                   |                                    |
|    CREATE TABLE tbl_db2           |    CREATE TABLE tbl_db2            |
|     (                             |     (                              |
|     "ID" number(20) ,             |     "ID" number(20) ,              |
|     "NAME" VARCHAR2(50),          |     "NAME" VARCHAR2(50),           |
|     "REMARKS" BLOB                |     "REMARKS" BLOB                 |
|     )                             |     )                              |
|     IN tbs1                       |     TABLESPACE tbs1                |
|     ORGANIZE BY ("ID","NAME");    |     /*ORGANIZE BY ("ID","NAME")*/; |
+-----------------------------------+------------------------------------+
