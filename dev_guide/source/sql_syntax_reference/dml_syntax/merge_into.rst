:original_name: dws_06_0235.html

.. _dws_06_0235:

MERGE INTO
==========

Function
--------

The **MERGE INTO** statement is used to conditionally match data in a target table with that in a source table. If data matches, **UPDATE** is executed on the target table; if data does not match, **INSERT** is executed. You can use this syntax to run **UPDATE** and **INSERT** at a time for convenience.

Precautions
-----------

-  To run **MERGE INTO**, you must have the UPDATE and INSERT permissions for the target table, as well as the SELECT permission for the source table.
-  **PREPARE** is not supported.
-  **MERGE INTO** cannot be executed during redistribution.
-  **MERGE INTO** cannot be executed for target tables that contain triggers.

Syntax
------

::

   MERGE INTO table_name [ [ AS ] alias ]
   USING { { table_name | view_name } | subquery } [ [ AS ] alias ]
   ON ( condition )
   [
     WHEN MATCHED THEN
     UPDATE SET { column_name = { expression | DEFAULT } |
             ( column_name [, ...] ) = ( { expression | DEFAULT } [, ...] ) } [, ...]
     [ WHERE condition ]
   ]
   [
     WHEN NOT MATCHED THEN
     INSERT { DEFAULT VALUES |
     [ ( column_name [, ...] ) ] VALUES ( { expression | DEFAULT } [, ...] ) [, ...] [ WHERE condition ] }
   ];

Parameter Description
---------------------

-  **INTO** clause

   Specifies the target table that is being updated or has data being inserted. It cannot be a replication table.

   -  **talbe_name**

      Specifies the name of the target table.

   -  **alias**

      Specifies the alias of the target table.

      Value range: a string. It must comply with the naming convention.

-  **USING** clause

   Specifies the source table, which can be a table, view, or subquery.

-  **ON** clause

   Specifies the condition used to match data between the source and target tables. Columns in the condition cannot be updated.

-  **WHEN MATCHED** clause

   Performs the UPDATE operation if data in the source table matches that in the target table based on the condition.

   Distribution keys cannot be updated. System catalogs and system columns cannot be updated.

-  **WHEN NOT MATCHED** clause

   Specifies that the INSERT operation is performed if data in the source table does not match that in the target table based on the condition.

   The **INSERT** clause is not allowed to contain multiple **VALUES**.

   The order of **WHEN MATCHED** and **WHEN NOT MATCHED** clauses can be reversed. One of them can be used by default, but they cannot be both used at one time. Two **WHEN MATCHED** or **WHEN NOT MATCHED** clauses cannot be specified at the same time.

-  **DEFAULT**

   Specifies the default value of a column.

   It will be **NULL** if no specific default value has been assigned to it.

-  **WHERE condition**

   Specifies the conditions for the **UPDATE** and **INSERT** clauses. The two clauses will be executed only when the conditions are met. The default value can be used. System columns cannot be referenced in **WHERE condition**.

Examples
--------

Create the target table **products** and source table **newproducts**, and insert data to them.

::

   CREATE TABLE products
   (
   product_id INTEGER,
   product_name VARCHAR2(60),
   category VARCHAR2(60)
   );

   INSERT INTO products VALUES (1501, 'vivitar 35mm', 'electrncs');
   INSERT INTO products VALUES (1502, 'olympus is50', 'electrncs');
   INSERT INTO products VALUES (1600, 'play gym', 'toys');
   INSERT INTO products VALUES (1601, 'lamaze', 'toys');
   INSERT INTO products VALUES (1666, 'harry potter', 'dvd');

   CREATE TABLE newproducts
   (
   product_id INTEGER,
   product_name VARCHAR2(60),
   category VARCHAR2(60)
   );

   INSERT INTO newproducts VALUES (1502, 'olympus camera', 'electrncs');
   INSERT INTO newproducts VALUES (1601, 'lamaze', 'toys');
   INSERT INTO newproducts VALUES (1666, 'harry potter', 'toys');
   INSERT INTO newproducts VALUES (1700, 'wait interface', 'books');

Run **MERGE INTO**.

::

   MERGE INTO products p
   USING newproducts np
   ON (p.product_id = np.product_id)
   WHEN MATCHED THEN
     UPDATE SET p.product_name = np.product_name, p.category = np.category WHERE p.product_name != 'play gym'
   WHEN NOT MATCHED THEN
     INSERT VALUES (np.product_id, np.product_name, np.category) WHERE np.category = 'books';
   MERGE 4

Query updates.

::

   SELECT * FROM products ORDER BY product_id;
    product_id |  product_name  | category
   ------------+----------------+-----------
          1501 | vivitar 35mm   | electrncs
          1502 | olympus camera | electrncs
          1600 | play gym       | toys
          1601 | lamaze         | toys
          1666 | harry potter   | toys
          1700 | wait interface | books
   (6 rows)

Delete a table.

::

   DROP TABLE products;
   DROP TABLE newproducts;
