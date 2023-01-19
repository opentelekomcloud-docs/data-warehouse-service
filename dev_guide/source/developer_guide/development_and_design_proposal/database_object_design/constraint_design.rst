:original_name: dws_04_0081.html

.. _dws_04_0081:

Constraint Design
=================

DEFAULT and NULL Constraints
----------------------------

-  [Proposal] If all the column values can be obtained from services, you are not advised to use the **DEFAULT** constraint, because doing so will generate unexpected results during data loading.
-  [Proposal] Add **NOT NULL** constraints to columns that never have NULL values. The optimizer automatically optimizes the columns in certain scenarios.
-  [Proposal] Explicitly name all constraints excluding **NOT NULL** and **DEFAULT**.

Partial Cluster Key
-------------------

A partial cluster key (PCK) is a local clustering technology used for column-store tables. After creating a PCK, you can quickly filter and scan fact tables using min or max sparse indexes in GaussDB(DWS). Comply with the following rules to create a PCK:

-  [Notice] Only one PCK can be created in a table. A PCK can contain multiple columns, preferably no more than two columns.
-  [Proposal] Create a PCK on simple expression filter conditions in a query. Such filter conditions are usually in the form of **col op const**, where **col** specifies a column name, **op** specifies an operator (such as =, >, >=, <=, and <), and **const** specifies a constant.
-  [Proposal] If the preceding conditions are met, create a PCK on the column having the least distinct values.

Unique Constraint
-----------------

-  [Notice] Both row-store and column-store tables support unique constraints.
-  [Proposal] The constraint name should indicate that it is a unique constraint, for example, **UNI**\ *Included columns*.

Primary Key Constraint
----------------------

-  [Notice] Both row-store and column-store tables support the primary key constraint.
-  [Proposal] The constraint name should indicate that it is a primary key constraint, for example, **PK**\ *Included columns*.

Check Constraint
----------------

-  [Notice] Check constraints can be used in row-store tables but not in column-store tables.
-  [Proposal] The constraint name should indicate that it is a check constraint, for example, **CK**\ *Included columns*.
