:original_name: dws_04_0580.html

.. _dws_04_0580:

PG_CONSTRAINT
=============

**PG_CONSTRAINT** records check, primary key, unique, and foreign key constraints on the tables.

.. table:: **Table 1** PG_CONSTRAINT columns

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                                                                                                |
   +=======================+=======================+============================================================================================================================================+
   | conname               | name                  | Constraint name (not necessarily unique)                                                                                                   |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | connamespace          | oid                   | OID of the namespace that contains the constraint                                                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | contype               | "char"                | -  **c** indicates check constraints.                                                                                                      |
   |                       |                       | -  **f** indicates foreign key constraints.                                                                                                |
   |                       |                       | -  **p** indicates primary key constraints.                                                                                                |
   |                       |                       | -  **u** indicates unique constraints.                                                                                                     |
   |                       |                       | -  **t** indicates trigger constraints.                                                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | condeferrable         | boolean               | Whether the constraint can be deferrable                                                                                                   |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | condeferred           | boolean               | Whether the constraint can be deferrable by default                                                                                        |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | convalidated          | boolean               | Whether the constraint is valid Currently, only foreign key and check constraints can be set to false.                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conrelid              | oid                   | Table containing this constraint. The value is **0** if it is not a table constraint.                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | contypid              | oid                   | Domain containing this constraint. The value is **0** if it is not a domain constraint.                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conindid              | oid                   | ID of the index associated with the constraint                                                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | confrelid             | oid                   | Referenced table if this constraint is a foreign key; otherwise, the value is **0**.                                                       |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | confupdtype           | "char"                | Foreign key update action code                                                                                                             |
   |                       |                       |                                                                                                                                            |
   |                       |                       | -  **a** indicates no action.                                                                                                              |
   |                       |                       | -  **r** indicates restriction.                                                                                                            |
   |                       |                       | -  **c** indicates cascading.                                                                                                              |
   |                       |                       | -  **n** indicates that the parameter is set to null.                                                                                      |
   |                       |                       | -  **d** indicates that the default value is used.                                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | confdeltype           | "char"                | Foreign key deletion action code                                                                                                           |
   |                       |                       |                                                                                                                                            |
   |                       |                       | -  **a** indicates no action.                                                                                                              |
   |                       |                       | -  **r** indicates restriction.                                                                                                            |
   |                       |                       | -  **c** indicates cascading.                                                                                                              |
   |                       |                       | -  **n** indicates that the parameter is set to null.                                                                                      |
   |                       |                       | -  **d** indicates that the default value is used.                                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | confmatchtype         | "char"                | Foreign key match type                                                                                                                     |
   |                       |                       |                                                                                                                                            |
   |                       |                       | -  **f** indicates full match.                                                                                                             |
   |                       |                       | -  **p** indicates partial match.                                                                                                          |
   |                       |                       | -  **u** indicates simple match (not specified).                                                                                           |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conislocal            | boolean               | Whether the local constraint is defined for the relationship                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | coninhcount           | integer               | Number of direct inheritance parent tables this constraint has. When the number is not **0**, the constraint cannot be deleted or renamed. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | connoinherit          | boolean               | Whether the constraint can be inherited                                                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | consoft               | boolean               | Whether the column indicates an informational constraint.                                                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conopt                | boolean               | Whether you can use Informational Constraint to optimize the execution plan.                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conkey                | smallint[]            | Column list of the constrained control if this column is a table constraint                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | confkey               | smallint[]            | List of referenced columns if this column is a foreign key                                                                                 |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conpfeqop             | oid[]                 | ID list of the equality operators for PK = FK comparisons if this column is a foreign key                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conppeqop             | oid[]                 | ID list of the equality operators for PK = PK comparisons if this column is a foreign key                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conffeqop             | oid[]                 | ID list of the equality operators for FK = FK comparisons if this column is a foreign key                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conexclop             | oid[]                 | ID list of the per-column exclusion operators if this column is an exclusion constraint                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | conbin                | pg_node_tree          | Internal representation of the expression if this column is a check constraint                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | consrc                | text                  | Human-readable representation of the expression if this column is a check constraint                                                       |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------+

.. important::

   -  **consrc** is not updated when referenced objects change; for example, it will not track renaming of columns. Rather than relying on this field, it is best to use **pg_get_constraintdef()** to extract the definition of a check constraint.
   -  **pg_class.relchecks** must be consistent with the number of check-constraint entries in this table for each relationship.

Example
-------

Query whether a specified table has a primary key.

::

   CREATE TABLE t1
   (
       C_CUSTKEY    BIGINT       ,
       C_NAME       VARCHAR(25)  ,
       C_ADDRESS    VARCHAR(40)  ,
       C_NATIONKEY  INT          ,
       C_PHONE      CHAR(15)     ,
       C_ACCTBAL    DECIMAL(15,2),
       CONSTRAINT C_CUSTKEY_KEY PRIMARY KEY(C_CUSTKEY,C_NAME)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY,C_NAME);

   SELECT conname FROM pg_constraint WHERE conrelid = 't1'::regclass AND contype = 'p';
       conname
   ---------------
    c_custkey_key
   (1 row)
