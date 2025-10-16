:original_name: dws_06_0117.html

.. _dws_06_0117:

Transaction Management
======================

GaussDB(DWS) supports the ACID properties of database transactions. It provides the **READ COMMITTED** and **REPEATABLE READ** isolation levels of transactions.

Concepts
--------

-  A transaction refers to an operation that consists of multiple steps, either all successful or all failed.

-  A database transaction is a logical unit in the execution process of DBMS. It consists of a limited number of database operations in a certain sequence (generally all the operations between **BEGIN TRANSACTION** and **END TRANSACTION**). These operations are either fully completed or not performed at all.

Purposes
--------

The purposes of database transactions are as follows:

-  To provide a method for restoring the database operation sequence from a failure while keeping database consistency even in case of system failure.
-  To provide isolation between programs accessing a database concurrently, so that they do not interfere one another in this process.

Transaction Execution Process
-----------------------------

After a transaction is committed in DBMS, DBMS needs to ensure that all operations in the transaction are successfully completed and the results are permanently stored in the database. If an operation in the transaction fails, all the operations in the transaction must be rolled back to the status before the transaction is executed. Transactions run independently and do not interfere with each other or affect database running.

Transaction Properties
----------------------

A transaction has atomicity, consistency, isolation, and durability (ACID) properties.

-  Atomicity: All the operations in a transaction are inseparable. They are either fully completed or not executed at all.

   For example, if account A transfers an amount of money to account B, $500 are deducted from account A and $500 added to account B. If the amount fails to be added to account B, it cannot be deducted from account A. If atomicity is not guaranteed, the account balances will be consistent.

-  Consistency: A transaction can only bring the database from one valid state to another, maintaining database invariants. Any data written to the database must be valid according to all defined rules, including data accuracy, concatenation, and spontaneous execution of scheduled tasks.

   For example, if account A transfers $500 to account B, $500 is deducted from account A and added to account B. The sum of the deducted amount (-$500) and the added amount (+500) should be 0. The total account balance of accounts A and B remains unchanged, no matter whether the money is transferred.

-  Isolation: The execution of a transaction cannot be interfered by other transactions. Isolation means that the operations inside a transaction and data used are isolated from other concurrent transactions. The concurrent transactions do not affect each other.

   Transactions are often executed concurrently. Isolation ensures that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially. Transaction isolation is divided into different levels, including **READ UNCOMMITTED**, **READ COMMITTED**, **REPEATABLE READ**, and **SERIALIZABLE**.

-  Durability: Once a transaction is committed, its modifications are permanently saved to the database. Even if the system is faulty, committed modifications will not be lost.

.. table:: **Table 1** ACID usage

   +-------------+---------------------------------------------------------------------+
   | ACID        | Purpose                                                             |
   +=============+=====================================================================+
   | Atomicity   | Concurrency control and fault recovery                              |
   +-------------+---------------------------------------------------------------------+
   | Consistency | SQL integrity constraints (primary key and foreign key constraints) |
   +-------------+---------------------------------------------------------------------+
   | Isolation   | Concurrency control                                                 |
   +-------------+---------------------------------------------------------------------+
   | Durability  | Fault recovery                                                      |
   +-------------+---------------------------------------------------------------------+

Common concurrency control technologies include lock-based and timestamp-based concurrency control. GaussDB(DWS) uses the two-phase lock technology for DDL statements and uses multi-version concurrency control (MVCC) for DML statements. GaussDB(DWS) databases fault recovery is based on WAL logs. MVCC mainly uses redo logs to ensure transaction read/write consistency.

Isolation Levels
----------------

Isolation prevents data inconsistency during the execution of concurrent transactions. A transaction isolation level specifies how concurrent transactions process the same object.

In GaussDB(DWS), transaction isolation levels are controlled by the GUC parameter **transaction_isolation** or the :ref:`SET TRANSACTION <dws_06_0264>` syntax. The following isolation levels are supported. The default isolation level is **READ COMMITTED**.

-  **READ COMMITTED**: Only committed data is read.
-  **READ UNCOMMITTED**: GaussDB(DWS) does not support **READ UNCOMMITTED**. If **READ UNCOMMITTED** is set, **READ COMMITTED** is used instead.
-  **REPEATABLE READ**: Only the data committed before transaction start is read. Uncommitted data or data committed in other concurrent transactions cannot be read.
-  **SERIALIZABLE**: GaussDB(DWS) does not support **SERIALIZABLE**. If **SERIALIZABLE** is set, **REPEATABLE READ** is used instead.

Transaction Control Syntax
--------------------------

-  Starting a transaction

   GaussDB(DWS) starts a transaction using **START TRANSACTION** and **BEGIN**. For details, see :ref:`START TRANSACTION <dws_06_0265>` and :ref:`BEGIN <dws_06_0257>`.

-  Setting a transaction

   GaussDB(DWS) sets a transaction using **SET TRANSACTION** or **SET LOCAL TRANSACTION**. For details, see :ref:`SET TRANSACTION <dws_06_0264>`.

-  Committing a transaction

   GaussDB(DWS) commits all operations of a transaction using **COMMIT** or **END**. For details, see :ref:`COMMIT | END <dws_06_0259>`.

-  Rolling back a transaction

   If a fault occurs during a transaction and the transaction cannot proceed, the system performs rollback to cancel all the completed database operations related to the transaction. For details, see :ref:`ROLLBACK <dws_06_0266>`.

   .. note::

      If an execution request (not in a transaction block) received in the database contains multiple statements, the statements will be packed into a transaction. If one of the statements fails, the entire request will be rolled back.

-  Other operations on transactions

   -  **SAVEPOINT** establishes a new savepoint within the current transaction. The transaction can be rolled back to the savepoint. You can roll back the commands executed after a savepoint but retain the commands executed before the savepoint. For details, see :ref:`SAVEPOINT <dws_06_0263>`.
   -  **ROLLBACK TO SAVEPOINT** rolls back a transaction to a savepoint. It implicitly deletes all the savepoints established after that savepoint. For details, see :ref:`ROLLBACK TO SAVEPOINT <dws_06_0269>`.
   -  **RELEASE SAVEPOINT** deletes a savepoint in a transaction. For details, see :ref:`RELEASE SAVEPOINT <dws_06_0267>`.

Transaction Example
-------------------

A customer buys a $100 item in a store using an e-payment account. At least two operations are involved: 1. $100 is deducted from the customer's account. 2. $100 is added to the store's account. In DBMS, the two operations must be both completed or not executed at all.

#. Create sample data.

   Create an account balance table and insert data. (Assume the store's and the customer's accounts each have $500.)

   ::

      CREATE TABLE customer_info (
          NAME VARCHAR(32) PRIMARY KEY,
          MONEY INTEGER
      );
      INSERT INTO customer_info (name, money) VALUES ('buyer', 500), ('shop', 500);

   The table data shows that the store and customer each have $500.

   ::

      SELECT * FROM customer_info;

   |image1|

#. Simulate a successful transaction.

   Deduct $100 from the customer's account and add $100 to the store's account.

   ::

      UPDATE customer_info SET money = money-100 WHERE name IN (SELECT name FROM customer_info WHERE name = 'buyer');
      UPDATE customer_info SET money = money+100 WHERE name IN (SELECT name FROM customer_info WHERE name = 'shop');

      SELECT * FROM customer_info;

   |image2|

#. Restore initial values.

   ::

      UPDATE customer_info SET money=500;
      select * from customer_info;

   |image3|

#. Simulate a transaction failure.

   $100 is deducted from the customer's account but fails to be added to the store's account.

   a. Deduct $100 from the customer's account.

      ::

         UPDATE customer_info SET money = money-100 WHERE name IN (SELECT name FROM customer_info WHERE name = 'buyer');

   b. The store finds a payment problem and terminates subsequent operations. An error is reported when the amount of money is added to the store's account. The execution of the following statement is terminated. (Only the store thinks that there is a problem with payment.)

      ::

         UPDATE customer_info SET money = money+100 WHERE name IN (SELECT name FROM customer_info WHERE name = 'shop');

   c. Query the account balances. The consumer has paid $100 but store does not receive it.

      ::

         SELECT * FROM customer_info;

      |image4|

Without ACID properties, the account balances will be incorrect once an error occurs during SQL statement execution..

**Simulate the rollback of an abnormal database transaction.**

#. Restore initial values.

   ::

      UPDATE customer_info SET money=500;

#. Deduct $100 from the customer's account.

   ::

      BEGIN TRANSACTION;
      UPDATE customer_info SET money = money-100 WHERE name IN (SELECT name FROM customer_info WHERE name = 'buyer');

#. An error is reported when the amount of money is added to the store's account. The execution of the following statement is terminated.

   ::

      UPDATE customer_info SET money = money+100 WHERE name IN (SELECT name FROM customer_info WHERE name = 'shop');

#. Roll back the transaction. All the completed database operations related to the transaction are canceled.

   |image5|

   ::

      END TRANSACTION;
      ROLLBACK

#. Query the account balances. The query result shows that the account balances remain unchanged. If an error occurs during transaction execution, the database is rolled back to the state before the transaction starts. The integrity of the database is not damaged.

   ::

      SELECT * FROM customer_info;

   |image6|

Two-Phase Transaction
---------------------

GaussDB(DWS) uses the distributed shared nothing architecture. Table data is distributed on different nodes. One or more statements on the client may modify data on multiple nodes at the same time. In this case, a distributed transaction is generated. GaussDB(DWS) uses two-phase commit transactions to ensure data consistency and atomicity in distributed transactions. Two-phase commit divides transaction commit into two phases, usually for transactions that contain write operations. When data is written to different nodes, the atomicity requirement of the transaction must be met, that is, either all data is committed or all data is rolled back.

Two-phase commit is not supported in the following scenarios:

-  Explicit two-phase commit of **PREPARE TRANSACTION** is not supported.

   ::

      BEGIN;
      PREPARE TRANSACTION 'p1';

   |image7|

-  The file mappings of system catalogs cannot be modified in a two-phase transaction.

   ::

      REINDEX TABLE pg_class;

   |image8|

-  Transaction snapshots cannot be committed and exported in cross-node transactions.

   ::

      BEGIN;
      CREATE TABLE t1(a int);
      SELECT pg_export_snapshot();
      END;

   |image9|

.. |image1| image:: /_static/images/en-us_image_0000001903326154.png
.. |image2| image:: /_static/images/en-us_image_0000001903326250.png
.. |image3| image:: /_static/images/en-us_image_0000001903486262.png
.. |image4| image:: /_static/images/en-us_image_0000001940645809.png
.. |image5| image:: /_static/images/en-us_image_0000001903486502.png
.. |image6| image:: /_static/images/en-us_image_0000001903326710.png
.. |image7| image:: /_static/images/en-us_image_0000001903486686.png
.. |image8| image:: /_static/images/en-us_image_0000001903486794.png
.. |image9| image:: /_static/images/en-us_image_0000001903486834.png
