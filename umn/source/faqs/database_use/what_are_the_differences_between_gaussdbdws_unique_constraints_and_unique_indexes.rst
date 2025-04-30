:original_name: dws_03_1002.html

.. _dws_03_1002:

What Are the Differences Between GaussDB(DWS) Unique Constraints and Unique Indexes?
====================================================================================

-  The concepts of a unique constraint and a unique index are different.

   A unique constraint specifies that the values in a column or a group of columns are all unique. If **DISTRIBUTE BY REPLICATION** is not specified, the column table that contains only unique values must contain distribution columns.

   A unique index is used to ensure the uniqueness of a field value or the value combination of multiple fields. **CREATE UNIQUE INDEX** creates a unique index.

-  The functions of a unique constraint and a unique index are different.

   Constraints are used to ensure data integrity, and indexes are used to facilitate query.

-  The usages of a unique constraint and a unique index are different.

   #. Both unique constraints and unique indexes can be used to ensure the uniqueness of column values which can be NULL.
   #. When a unique constraint is created, a unique index with the same name is automatically created. The index cannot be deleted separately. When the constraint is deleted, the index is automatically deleted. A unique constraint uses a unique index to ensure data uniqueness. GaussDB(DWS) row-store tables support unique constraints, but column-store tables do not.
   #. A created unique index is independent and can be deleted separately. Currently in GaussDB(DWS), unique indexes can only be created using B-Tree.
   #. If you want to have both a unique constraint and a unique index on a column, and they can be deleted separately, you can create a unique index and then a unique constraint with the same name.
   #. If a field in a table is to be used as a foreign key of another table, the field must have a unique constraint (or it is a primary key). If the field has only a unique index, an error is reported.

Example: Create a composite index for two columns, which is not required to be a unique index.

::

   CREATE TABLE t (n1 number,n2 number,n3 number,PRIMARY KEY (n3));
   CREATE INDEX t_idx ON t(n1,n2);

GaussDB (DWS) supports multiple unique indexes for a table.

::

   CREATE UNIQUE INDEX u_index ON t(n3);
   CREATE UNIQUE INDEX u_index1 ON t(n3);

You can use the index **t_idx** created in the example above to create a unique constraint **t_uk**, which is unique only on column **n1**. A unique constraint is stricter than a unique index.

::

   ALTER TABLE t ADD CONSTRAINT t_uk UNIQUE USING INDEX u_index;
