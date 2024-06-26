:original_name: dws_06_0234.html

.. _dws_06_0234:

LOCK
====

Function
--------

**LOCK TABLE** obtains a table-level lock.

When the lock for commands referencing a table is automatically acquired, GaussDB(DWS) always uses the lock mode with minimum constraints. Use **LOCK** if users need a more strict lock mode. For example, suppose an application runs a transaction at the **Read Committed** isolation level and needs to ensure that data in a table remains stable in the duration of the transaction. To achieve this, you could obtain **SHARE** lock mode over the table before the query. This will prevent concurrent data changes and ensure subsequent reads of the table see a stable view of committed data. It is because the **SHARE** lock mode conflicts with the **ROW EXCLUSIVE** lock acquired by writers, and your **LOCK TABLE name IN SHARE MODE** statement will wait until any concurrent holders of **ROW EXCLUSIVE** mode locks commit or roll back. Therefore, once you obtain the lock, there are no uncommitted writes outstanding; furthermore none can begin until you release the lock.

Precautions
-----------

-  **LOCK TABLE** is useless outside a transaction block: the lock would remain held only to the completion of the statement. If **LOCK TABLE** is out of any transaction block, an error is reported.
-  If no lock mode is specified, then **ACCESS EXCLUSIVE**, the most restrictive mode, is used.
-  LOCK TABLE ... IN ACCESS SHARE MODE requires the **SELECT** permission on the target table. All other forms of **LOCK** require table-level **UPDATE** and/or the **DELETE** permission.
-  There is no **UNLOCK TABLE** command. Locks are always released at transaction end.
-  **LOCK TABLE** only deals with table-level locks, and so the mode names involving **ROW** are all misnomers. These mode names should generally be read as indicating the intention of the user to acquire row-level locks within the locked table. Also, **ROW EXCLUSIVE** mode is a shareable table lock. Keep in mind that all the lock modes have identical semantics so far as **LOCK TABLE** is concerned, differing only in the rules about which modes conflict with which. For details about the rules, see :ref:`Table 1 <en-us_topic_0000001233430195__tec3c848278c344f9a15c8ab751ad98d7>`.

Syntax
------

::

   LOCK [ TABLE ] {[ ONLY ] name [, ...]| {name [ * ]} [, ...]}
       [ IN {ACCESS SHARE | ROW SHARE | ROW EXCLUSIVE | SHARE UPDATE EXCLUSIVE | SHARE | SHARE ROW EXCLUSIVE | EXCLUSIVE | ACCESS EXCLUSIVE} MODE ]
       [ NOWAIT ];

Parameter Description
---------------------

.. _en-us_topic_0000001233430195__tec3c848278c344f9a15c8ab751ad98d7:

.. table:: **Table 1** Lock mode conflicts

   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | Requested Lock Mode/Current Lock Mode | ACCESS SHARE | ROW SHARE | ROW EXCLUSIVE | SHARE UPDATE EXCLUSIVE | SHARE | SHARE ROW EXCLUSIVE | EXCLUSIVE | ACCESS EXCLUSIVE |
   +=======================================+==============+===========+===============+========================+=======+=====================+===========+==================+
   | ACCESS SHARE                          | ``-``        | ``-``     | ``-``         | ``-``                  | ``-`` | ``-``               | ``-``     | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | ROW SHARE                             | ``-``        | ``-``     | ``-``         | ``-``                  | ``-`` | ``-``               | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | ROW EXCLUSIVE                         | ``-``        | ``-``     | ``-``         | ``-``                  | X     | X                   | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | SHARE UPDATE EXCLUSIVE                | ``-``        | ``-``     | ``-``         | X                      | X     | X                   | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | SHARE                                 | ``-``        | ``-``     | X             | X                      | ``-`` | X                   | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | SHARE ROW EXCLUSIVE                   | ``-``        | ``-``     | X             | X                      | X     | X                   | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | EXCLUSIVE                             | ``-``        | X         | X             | X                      | X     | X                   | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+
   | ACCESS EXCLUSIVE                      | X            | X         | X             | X                      | X     | X                   | X         | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+

**LOCK** parameters are as follows:

-  **name**

   The name (optionally schema-qualified) of an existing table to lock.

   The tables are locked one-by-one in the order specified in the **LOCK TABLE** command.

   Value range: an existing table name

-  **ONLY**

   **Only** locks only this table. If **Only** is not specified, this table and all its sub-tables are locked.

-  **ACCESS SHARE**

   **ACCESS SHARE** allows only read operations on a table. In general, any SQL statements that only read a table and do not modify it will acquire this lock mode. The **SELECT** command acquires a lock of this mode on referenced tables.

-  **ROW SHARE**

   **ROW SHARE** allows concurrent read of a table but does not allow any other operations on the table.

   **SELECT FOR UPDATE** and **SELECT FOR SHARE** automatically acquire the **ROW SHARE** lock on the target table and add the **ACCESS SHARE** lock to other referenced tables except **FOR SHARE** and **FOR UPDATE**.

-  **ROW EXCLUSIVE**

   Like **ROW SHARE**, **ROW EXCLUSIVE** allows concurrent read of a table and modification of data in the table. **UPDATE**, **DELETE**, and **INSERT** automatically acquire the **ROW SHARE** lock on the target table and add the **ACCESS SHARE** lock to other referenced tables. Generally, all commands that modify table data acquire the **ROW EXCLUSIVE** lock for tables.

-  **SHARE UPDATE EXCLUSIVE**

   This mode protects a table against concurrent schema changes and VACUUM runs.

   Acquired by VACUUM (without FULL), ANALYZE, CREATE INDEX CONCURRENTLY, and some forms of ALTER TABLE.

-  **SHARE**

   **SHARE** allows concurrent queries of a table but does not allow modification of the table.

   Acquired by CREATE INDEX (without CONCURRENTLY).

-  **SHARE ROW EXCLUSIVE**

   **SHARE ROW EXCLUSIVE** protects a table against concurrent data changes, and is self-exclusive so that only one session can hold it at a time.

   No SQL statements automatically acquire this lock mode.

-  **EXCLUSIVE**

   **EXCLUSIVE** allows concurrent queries of the target table but does not allow any other operations.

   This mode allows only concurrent **ACCESS SHARE** locks; that is, only reads from the table can proceed in parallel with a transaction holding this lock mode.

   No SQL statements automatically acquire this lock mode on user tables. However, it will be acquired on some system tables in case of some operations.

-  **ACCESS EXCLUSIVE**

   This mode guarantees that the holder is the only transaction accessing the table in any way.

   Acquired by the **ALTER TABLE**, **DROP TABLE**, **TRUNCATE**, **REINDEX**, **CLUSTER**, and **VACUUM FULL** commands.

   This is also the default lock mode for **LOCK TABLE** statements that do not specify a mode explicitly.

-  **NOWAIT**

   Specifies that **LOCK TABLE** should not wait for any conflicting locks to be released: if the specified lock(s) cannot be acquired immediately without waiting, the transaction is aborted.

   If **NOWAIT** is not specified, **LOCK TABLE** obtains a table-level lock, waiting if necessary for any conflicting locks to be released.

Examples
--------

Obtain a **SHARE** lock on a primary key table when going to perform inserts into a foreign key table:

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
   START TRANSACTION;

   LOCK TABLE customer_address IN SHARE MODE;

   SELECT ca_address_sk FROM customer_address WHERE ca_address_sk=5;

   COMMIT;

Obtain a **SHARE ROW EXCLUSIVE** lock on a primary key table when going to perform a delete operation:

::

   DROP TABLE IF EXISTS customer;
   CREATE TABLE customer AS TABLE customer_address;

   START TRANSACTION;

   LOCK TABLE customer IN SHARE ROW EXCLUSIVE MODE;

   DELETE FROM customer WHERE ca_address_sk IN(SELECT ca_address_sk FROM customer WHERE ca_address_sk < 6 );

   DELETE FROM customer WHERE ca_address_sk = 7;

   COMMIT;
