:original_name: dws_04_1007.html

.. _dws_04_1007:

Querying a Single Table
=======================

Example table:

::

   CREATE TABLE newproducts
   (
   product_id INTEGER NOT NULL,
   product_name VARCHAR2(60),
   category VARCHAR2(60),
   quantity  INTEGER
   )
   WITH (ORIENTATION = COLUMN) DISTRIBUTE BY HASH(product_id);


   INSERT INTO newproducts VALUES (1502, 'earphones', 'electronics',150);
   INSERT INTO newproducts VALUES (1601, 'telescope', 'toys',80);
   INSERT INTO newproducts VALUES (1666, 'Frisbee', 'toys',244);
   INSERT INTO newproducts VALUES (1700, 'interface', 'books',100);
   INSERT INTO newproducts VALUES (2344, 'milklotion', 'skin care',320);
   INSERT INTO newproducts VALUES (3577, 'dumbbell', 'sports',550);
   INSERT INTO newproducts VALUES (1210, 'necklace', 'jewels', 200);

Simple Queries
--------------

Run the **SELECT... FROM...** statement to obtain the result from the database.

::

   SELECT category FROM newproducts;
     category
   ------------
    electr
    sports
    jewels
    toys
    books
    skin care
    toys
   (7 rows)

Filtering Test Results
----------------------

Run the **WHERE** statement to filter the query result and find the queried part.

::

   SELECT * FROM newproducts WHERE category='toys';
    product_id | product_name | category | quantity
   ------------+--------------+----------+----------
          1601 | telescope    | toys     |       80
          1666 | Frisbee      | toys     |      244
   (2 rows)

Sorting Results
---------------

Use the **ORDER BY** statement to sort query results.

::

   SELECT product_id,product_name,category,quantity FROM newproducts ORDER BY quantity DESC;
    product_id | product_name |  category   | quantity
   ------------+--------------+-------------+----------
          3577 | dumbbell     | sports      |      550
          2344 | milklotion   | skin care   |      320
          1666 | Frisbee      | toys        |      244
          1210 | necklace     | jewels      |      200
          1502 | earphones    | electronics |      150
          1700 | interface    | books       |      100
          1601 | telescope    | toys        |       80
   (7 rows)

Limiting the Number of Query Results
------------------------------------

If you want the query to return only part of the result, you can use the **LIMIT** statement to limit the number of records returned in the query result.

::

   SELECT product_id,product_name,category,quantity FROM newproducts ORDER BY quantity DESC limit 5;
    product_id | product_name |  category   | quantity
   ------------+--------------+-------------+----------
          3577 | dumbbell     | sports      |      550
          2344 | milklotion   | skin care   |      320
          1666 | Frisbee      | toys        |      244
          1210 | necklace     | jewels      |      200
          1502 | earphones    | electronics |      150
   (5 rows)

Aggregated Query
----------------

If you want query data comprehensively, you can use the **GROUP BY** statement and aggregate functions to construct an aggregated query.

::

   SELECT category, string_agg(quantity,',') FROM newproducts group by category;
     category   | string_agg
   -------------+------------
    toys        | 80,244
    books       | 100
    sports      | 550
    jewels      | 200
    skin care   | 320
    electronics | 150
