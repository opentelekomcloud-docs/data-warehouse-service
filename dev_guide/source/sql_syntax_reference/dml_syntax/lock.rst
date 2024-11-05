:original_name: dws_06_0234.html

.. _dws_06_0234:

LOCK
====

Function
--------

**LOCK TABLE** obtains a table-level lock.

When the lock for commands referencing a table is automatically acquired, GaussDB(DWS) always uses the lock mode with minimum constraints. Use **LOCK** if a more strict lock mode is needed. For example, suppose an application runs a transaction at the **Read Committed** isolation level and needs to ensure that data in a table remains stable in the duration of the transaction. To achieve this, you could obtain **SHARE** lock mode over the table before the query. This will prevent concurrent data changes and ensure subsequent reads of the table see a stable view of committed data. It is because the **SHARE** lock mode conflicts with the **ROW EXCLUSIVE** lock acquired by writers, and your **LOCK TABLE name IN SHARE MODE** statement will wait until any concurrent holders of **ROW EXCLUSIVE** mode locks commit or roll back. Therefore, once you obtain the lock, there are no uncommitted writes outstanding; furthermore none can begin until you release the lock.

Precautions
-----------

-  **LOCK TABLE** is useless outside a transaction block: the lock would remain held only to the completion of the statement. If **LOCK TABLE** is out of any transaction block, an error is reported.
-  If no lock mode is specified, then **ACCESS EXCLUSIVE**, the most restrictive mode, is used.
-  LOCK TABLE ... IN ACCESS SHARE MODE requires the **SELECT** permission on the target table. All other forms of **LOCK** require table-level **UPDATE** and/or the **DELETE** permission.
-  There is no **UNLOCK TABLE** command. Locks are always released at transaction end.
-  **LOCK TABLE** only deals with table-level locks, and so the mode names involving **ROW** are all misnomers. These mode names should generally be read as indicating the intention of the user to acquire row-level locks within the locked table. Also, **ROW EXCLUSIVE** mode is a shareable table lock. Keep in mind that all the lock modes have identical semantics so far as **LOCK TABLE** is concerned, differing only in the rules about which modes conflict with which. For details about the rules, see :ref:`Table 1 <en-us_topic_0000001510400945__tec3c848278c344f9a15c8ab751ad98d7>`.

Syntax
------

::

   LOCK [ TABLE ] {[ ONLY ] name [, ...]| {name [ * ]} [, ...]}
       [ IN {ACCESS SHARE | ROW SHARE | ROW EXCLUSIVE | SHARE UPDATE EXCLUSIVE | SHARE | SHARE ROW EXCLUSIVE | EXCLUSIVE | ACCESS EXCLUSIVE | UPDATE EXCLUSIVE} MODE ]
       [ NOWAIT ] [LOCAL COORDINATOR ONLY];

Parameter Description
---------------------

.. _en-us_topic_0000001510400945__tec3c848278c344f9a15c8ab751ad98d7:

.. table:: **Table 1** Lock mode conflicts

   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | Requested Lock Mode/Current Lock Mode | ACCESS SHARE | ROW SHARE | ROW EXCLUSIVE | SHARE UPDATE EXCLUSIVE | SHARE | SHARE ROW EXCLUSIVE | EXCLUSIVE | ACCESS EXCLUSIVE | UPDATE EXCLUSIVE |
   +=======================================+==============+===========+===============+========================+=======+=====================+===========+==================+==================+
   | ACCESS SHARE                          | ``-``        | ``-``     | ``-``         | ``-``                  | ``-`` | ``-``               | ``-``     | X                | ``-``            |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | ROW SHARE                             | ``-``        | ``-``     | ``-``         | ``-``                  | ``-`` | ``-``               | X         | X                | ``-``            |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | ROW EXCLUSIVE                         | ``-``        | ``-``     | ``-``         | ``-``                  | X     | X                   | X         | X                | ``-``            |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | SHARE UPDATE EXCLUSIVE                | ``-``        | ``-``     | ``-``         | X                      | X     | X                   | X         | X                | ``-``            |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | SHARE                                 | ``-``        | ``-``     | X             | X                      | ``-`` | X                   | X         | X                | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | SHARE ROW EXCLUSIVE                   | ``-``        | ``-``     | X             | X                      | X     | X                   | X         | X                | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | EXCLUSIVE                             | ``-``        | X         | X             | X                      | X     | X                   | X         | X                | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | ACCESS EXCLUSIVE                      | X            | X         | X             | X                      | X     | X                   | X         | X                | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+
   | UPDATE EXCLUSIVE                      | ``-``        | ``-``     | ``-``         | ``-``                  | X     | X                   | X         | X                | X                |
   +---------------------------------------+--------------+-----------+---------------+------------------------+-------+---------------------+-----------+------------------+------------------+

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

-  **UPDATE EXCLUSIVE**

   The **UPDATE EXCLUSIVE** lock allows concurrent **(AUTO) VACUUM** and **(AUTO) ANALYZE**, but does not allow concurrent **(AUTO) VACUUM**.

   .. note::

      -  This parameter is supported only by clusters of version 8.2.1.300 or later.
      -  The **UPDATE EXCLUSIVE** lock is used only in the **VACUUM** syntax.

-  **NOWAIT**

   Specifies that **LOCK TABLE** should not wait for any conflicting locks to be released: if the specified lock(s) cannot be acquired immediately without waiting, the transaction is aborted.

   If **NOWAIT** is not specified, **LOCK TABLE** obtains a table-level lock, waiting if necessary for any conflicting locks to be released.

-  **LOCAL COORDINATOR ONLY**

   Specifies that LOCK TABLE is executed only on the CN that receives the current session request and is not delivered to other CNs or all DNs. This is used only for metadata operations to improve efficiency.

   .. important::

      -  This parameter is supported by clusters of version 8.2.0.100 or later.
      -  Currently, only the ACCESS SHARE lock mode is supported. If other lock modes are used, an error will be reported.

Examples
--------

Obtain a **SHARE** lock on a primary key table when going to perform inserts into a foreign key table.

::

   START TRANSACTION;

   LOCK TABLE tpcds.reason IN SHARE MODE;

   SELECT r_reason_desc FROM tpcds.reason WHERE r_reason_sk=5;
   r_reason_desc
   -----------
    Parts missing
   (1 row)

   COMMIT;

Obtain a **SHARE ROW EXCLUSIVE** lock on a primary key table when performing a delete operation.

::

   CREATE TABLE tpcds.reason_t1 AS TABLE tpcds.reason;

   START TRANSACTION;

   LOCK TABLE tpcds.reason_t1 IN SHARE ROW EXCLUSIVE MODE;

   DELETE FROM tpcds.reason_t1 WHERE r_reason_desc IN(SELECT r_reason_desc FROM tpcds.reason_t1 WHERE r_reason_sk < 6 );

   DELETE FROM tpcds.reason_t1 WHERE r_reason_sk = 7;

   COMMIT;

Delete the **tpcds.reason_t1** table.

::

   DROP TABLE tpcds.reason_t1;

Add the ACCESS SHARE lock to the table and set the lock scope to the current CN.

.. code-block::

   BEGIN;
   LOCK TABLE lock_test IN ACCESS SHARE MODE LOCAL COORDINATOR ONLY;
   SELECT pg_get_tabledef('lock_test');
   END;
