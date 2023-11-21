:original_name: dws_07_6827.html

.. _dws_07_6827:

Index Migration
===============

Unique Index
------------

+-------------------------------------------------------------------------+---------------------------------------------------------------------------+
| Netezza Syntax                                                          | Syntax After Migration                                                    |
+=========================================================================+===========================================================================+
| .. code-block::                                                         | .. code-block::                                                           |
|                                                                         |                                                                           |
|    CREATE TABLE  prod                                                   |    CREATE TABLE  prod                                                     |
|    (                                                                    |    (                                                                      |
|         prod_no              number(6)      not null    unique,         |         prod_no              number(6)      not null   /* unique */,      |
|         prod_name         national character varying(32)      not null, |         prod_name         national character varying(32)      not null,   |
|         prod_desc          clob                                         |         prod_desc          clob                                           |
|    )                                                                    |    ) WITH(ORIENTATION=COLUMN)                                             |
|    DISTRIBUTE ON (prod_no)                                              |    DISTRIBUTE BY HASH (prod_no)                                           |
|    ORGANIZE   ON (prod_no, prod_name)                                   |    /* ORGANIZE   ON (prod_no, prod_name) */                               |
|    ;                                                                    |    ;                                                                      |
|    ----------                                                           |    ----------                                                             |
|    CREATE TABLE  prod                                                   |    CREATE TABLE  prod                                                     |
|    (                                                                    |    (                                                                      |
|         prod_no              number(6)      not null                    |         prod_no              number(6)      not null                      |
|         CONSTRAINT UQ_prod unique,                                      |      /* CONSTRAINT UQ_prod unique */,                                     |
|         prod_name         national character varying(32)      not null, |         prod_name         national character varying(32)      not null,   |
|         prod_desc          clob                                         |         prod_desc          clob                                           |
|    )                                                                    |    ) WITH(ORIENTATION=COLUMN)                                             |
|    DISTRIBUTE ON (prod_no)                                              |    DISTRIBUTE BY HASH (prod_no)                                           |
|    ORGANIZE   ON (prod_no, prod_name)                                   |    /* ORGANIZE   ON (prod_no, prod_name) */                               |
|    ;                                                                    |    ;                                                                      |
|    ----------                                                           |    ----------                                                             |
|    CREATE TABLE  prod                                                   |    CREATE TABLE  prod                                                     |
|    (                                                                    |    (                                                                      |
|         prod_no              number(6)      not null    PRIMARY KEY,    |         prod_no              number(6)      not null   /* PRIMARY KEY */, |
|         prod_name         national character varying(32)      not null, |         prod_name         national character varying(32)      not null,   |
|         prod_desc          clob                                         |         prod_desc          clob                                           |
|    )                                                                    |    ) WITH(ORIENTATION=COLUMN)                                             |
|    DISTRIBUTE ON (prod_no)                                              |    DISTRIBUTE BY HASH (prod_no)                                           |
|    ORGANIZE   ON (prod_no, prod_name)                                   |    /* ORGANIZE   ON (prod_no, prod_name) */                               |
|    ;                                                                    |    ;                                                                      |
|    ----------                                                           |    ----------                                                             |
|    CREATE TABLE  prod                                                   |    CREATE TABLE  prod                                                     |
|    (                                                                    |    (                                                                      |
|         prod_no              number(6)      not null,                   |         prod_no              number(6)      not null,                     |
|         prod_name         national character varying(32)      not null, |         prod_name         national character varying(32)      not null,   |
|         prod_desc          clob,                                        |         prod_desc          clob  /*,                                      |
|         constraint          uq_prod    UNIQUE (prod_no)                 |        constraint          uq_prod    UNIQUE (prod_no)  */                |
|    )                                                                    |    ) WITH(ORIENTATION=COLUMN)                                             |
|    DISTRIBUTE ON (prod_no)                                              |    DISTRIBUTE BY HASH (prod_no)                                           |
|    ORGANIZE   ON (prod_no, prod_name)                                   |    /* ORGANIZE   ON (prod_no, prod_name)*/                                |
|    ;                                                                    |    ;                                                                      |
|    ----------                                                           |    ----------                                                             |
|    CREATE TABLE  prod                                                   |    CREATE TABLE  prod                                                     |
|    (                                                                    |    (                                                                      |
|         prod_no              number(6)      not null,                   |         prod_no              number(6)      not null,                     |
|         prod_name         national character varying(32)      not null, |         prod_name         national character varying(32)      not null,   |
|         prod_desc          clob                                         |         prod_desc          clob                                           |
|    )                                                                    |    )                                                                      |
|    DISTRIBUTE ON (prod_no)                                              |    DISTRIBUTE BY HASH (prod_no)                                           |
|    ORGANIZE   ON (prod_no, prod_name)                                   |    /*ORGANIZE   ON (prod_no, prod_name)*/                                 |
|    ;                                                                    |    ;                                                                      |
|    ALTER TABLE prod                                                     |    /*                                                                     |
|        ADD constraint          uq_prod    UNIQUE (prod_no);             |    ALTER TABLE prod                                                       |
|                                                                         |        ADD constraint          uq_prod    UNIQUE (prod_no);               |
|                                                                         |    */                                                                     |
+-------------------------------------------------------------------------+---------------------------------------------------------------------------+

.. note::

   This feature is applicable only for COLUMN store. For ROW store, Unique Index should not be commented.
