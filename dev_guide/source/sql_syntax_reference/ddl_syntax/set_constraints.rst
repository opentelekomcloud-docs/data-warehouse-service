:original_name: dws_06_0221.html

.. _dws_06_0221:

SET CONSTRAINTS
===============

Function
--------

**SET CONSTRAINTS** sets the behavior of constraint checking within the current transaction.

**IMMEDIATE** constraints are checked at the end of each statement. **DEFERRED** constraints are not checked until transaction commit. Each constraint has its own **IMMEDIATE** or **DEFERRED** mode.

Upon creation, a constraint is given one of three characteristics **DEFERRABLE INITIALLY DEFERRED**, **DEFERRABLE INITIALLY IMMEDIATE**, or **NOT DEFERRABLE**. The third class is always **IMMEDIATE** and is not affected by the **SET CONSTRAINTS** command. The first two classes start every transaction in specified modes, but its behaviors can be changed within a transaction by **SET CONSTRAINTS**.

**SET CONSTRAINTS** with a list of constraint names changes the mode of just those constraints (which must all be deferrable). If multiple constraints match a name, the name is affected by all of these constraints. **SET CONSTRAINTS ALL** changes the modes of all deferrable constraints.

When **SET CONSTRAINTS** changes the mode of a constraint from **DEFERRED** to **IMMEDIATE**, the new mode takes effect retroactively: any outstanding data modifications that would have been checked at the end of the transaction are instead checked during the execution of the **SET CONSTRAINTS** command. If any such constraint is violated, the **SET CONSTRAINTS** fails (and does not change the constraint mode). Therefore, **SET CONSTRAINTS** can be used to force checking of constraints to occur at a specific point in a transaction.

Only foreign key constraints are affected by this setting. Check and unique constraints are always checked immediately when a row is inserted or modified.

Precautions
-----------

**SET CONSTRAINTS** sets the behavior of constraint checking only within the current transaction. Therefore, if you execute this command outside of a transaction block (**START TRANSACTION/COMMIT** pair), it will not appear to have any effect.

Syntax
------

::

   SET CONSTRAINTS  { ALL  |  { name  }  [, ...]  }  { DEFERRED  | IMMEDIATE  } ;

Parameter Description
---------------------

-  **name**

   Specifies the constraint name.

   Value range: an existing constraint name, which can be found in the system catalog **pg_constraint**.

-  **ALL**

   Indicates all constraints.

-  **DEFERRED**

   Indicates that constraints are not checked until transaction commit.

-  **IMMEDIATE**

   Indicates that constraints are checked at the end of each statement.

Examples
--------

Set that constraints are checked when a transaction is committed.

::

   SET CONSTRAINTS ALL DEFERRED;
