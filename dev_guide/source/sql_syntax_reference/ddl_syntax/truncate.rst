:original_name: dws_06_0225.html

.. _dws_06_0225:

TRUNCATE
========

Function
--------

**TRUNCATE** quickly removes all rows from a database table.

It has the same effect as an unqualified **DELETE** on each table, but it is faster since it does not actually scan the tables. This is most useful on large tables.

Precautions
-----------

-  **TRUNCATE TABLE** has the same function as a **DELETE** statement with no **WHERE** clause, emptying a table.
-  **TRUNCATE TABLE** uses less system and transaction log resources as compared with **DELETE**.

   -  **DELETE** deletes a row each time, and records the deletion of each row in the transaction log.
   -  **TRUNCATE TABLE** deletes all rows in a table by releasing the data page storing the table data, and records the releasing of the data page only in the transaction log.

-  The differences between **TRUNCATE**, **DELETE**, and **DROP** are as follows:

   -  **TRUNCATE TABLE** deletes content, releases space, but does not delete definitions.
   -  **DELETE TABLE** deletes content, but does not delete definitions nor release space.
   -  **DROP TABLE** deletes content and definitions, and releases space.

Syntax
------

-  **TRUNCATE** empties a table or set of tables.

::

   TRUNCATE [ TABLE ] [ ONLY ] {[[database_name.]schema_name.]table_name [ * ]} [, ... ]
       [ CONTINUE IDENTITY ] [ CASCADE | RESTRICT ];

-  Truncate the data in a partition.

::

   ALTER TABLE [ IF EXISTS  ] { [ ONLY  ] [[database_name.]schema_name.]table_name
                              | table_name *
                              | ONLY ( table_name )  }
       TRUNCATE PARTITION { partition_name
                          | FOR (  partition_value  [, ...] )  } ;

Parameter Description
---------------------

-  **ONLY**

   If **ONLY** is specified, only the specified table is cleared. Otherwise, the table and all its subtables (if any) are cleared.

-  **database_name**

   Database name of the target table

-  **schema_name**

   Schema name of the target table

-  **table_name**

   Specifies the name (optionally schema-qualified) of a target table.

   Value range: an existing table name

-  **CONTINUE IDENTITY**

   Does not change the values of sequences. This is the default.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically truncates all tables that have foreign-key references to any of the named tables, or to any tables added to the group due to **CASCADE**.
   -  **RESTRICT** (default): refuses to truncate if any of the tables have foreign-key references from tables that are not listed in the command.

-  **partition_name**

   Indicates the partition in the target partition table.

   Value range: An existing partition name.

-  **partition_value**

   Specifies the value of the specified partition key.

   The value specified by **PARTITION FOR** can uniquely identify a partition.

   Value range: The partition key of the partition to be deleted.

   .. important::

      When the **PARTITION FOR** clause is used, the entire partition where **partition_value** is located is cleared.

Examples
--------

Create a partitioned table **customer_address**:

::

   DROP TABLE IF EXISTS customer_address;
   CREATE TABLE customer_address
   (
       ca_address_sk       INTEGER                  NOT NULL   ,
       ca_address_id       CHARACTER(16)            NOT NULL   ,
       ca_street_number    CHARACTER(10)                       ,
       ca_street_name      CHARACTER varying(60)               ,
       ca_street_type      CHARACTER(15)                       ,
       ca_suite_number     CHARACTER(10)
   )
   DISTRIBUTE BY HASH (ca_address_sk)
   PARTITION BY RANGE(ca_address_sk)
   (
           PARTITION P1 VALUES LESS THAN(2450815),
           PARTITION P2 VALUES LESS THAN(2451179),
           PARTITION P3 VALUES LESS THAN(2451544),
           PARTITION P4 VALUES LESS THAN(MAXVALUE)
   );

Clear the **p1** partition of the **customer_address** table:

::

   ALTER TABLE customer_address TRUNCATE PARTITION p1;

Clear a partitioned table:

::

   TRUNCATE TABLE customer_address;
