:original_name: dws_03_2104.html

.. _dws_03_2104:

How Do I Query the Information About GaussDB(DWS) Column-Store Tables?
======================================================================

The following SQL statements are used to query common information about column-store tables:

Create a column-store table named my_table, and insert data into the table.

::

   CREATE TABLE my_table
   (
       product_id INT,
       product_name VARCHAR2(40),
       product_quantity INT
    )
   WITH (ORIENTATION = COLUMN)
   PARTITION BY range(product_quantity)
   (
   partition my_table_p1 values less than(600),
   partition my_table_p2 values less than(800),
   partition my_table_p3 values less than(950),
   partition my_table_p4 values less than(1000));

   INSERT INTO my_table VALUES(1011, 'tents', 720);
   INSERT INTO my_table VALUES(1012, 'hammock', 890);
   INSERT INTO my_table VALUES(1013, 'compass', 210);
   INSERT INTO my_table VALUES(1014, 'telescope', 490);
   INSERT INTO my_table VALUES(1015, 'flashlight', 990);
   INSERT INTO my_table VALUES(1016, 'ropes', 890);

Run the following command to view the created column-store partitioned table:

::

   SELECT * FROM my_table;
    product_id | product_name | product_quantity
   ------------+--------------+------------------
          1013 | compass      |              210
          1014 | telescope    |              490
          1011 | tents        |              720
          1015 | flashlight   |              990
          1012 | hammock      |              890
          1016 | ropes        |              890
   (6 rows)

Querying the Boundary of a Partition
------------------------------------

::

   SELECT relname, partstrategy, boundaries FROM pg_partition where parentid=(select parentid from pg_partition where relname='my_table');
      relname   | partstrategy | boundaries
   -------------+--------------+------------
    my_table    | r            |
    my_table_p1 | r            | {600}
    my_table_p2 | r            | {800}
    my_table_p3 | r            | {950}
    my_table_p4 | r            | {1000}
   (5 rows)

Querying the Number of Columns in a Column-Store Table
------------------------------------------------------

::

   SELECT count(*) FROM ALL_TAB_COLUMNS where table_name='my_table';
    count
   -------
        3
   (1 row)

Querying Data Distribution on DNs
---------------------------------

::

   SELECT table_skewness('my_table');
              table_skewness
   ------------------------------------
    ("dn_6007_6008        ",3,50.000%)
    ("dn_6009_6010        ",2,33.333%)
    ("dn_6003_6004        ",1,16.667%)
    ("dn_6001_6002        ",0,0.000%)
    ("dn_6005_6006        ",0,0.000%)
    ("dn_6011_6012        ",0,0.000%)
   (6 rows)

Querying the Names of the Cudesc and Delta Tables in Partition P1 on a DN
-------------------------------------------------------------------------

::

   EXECUTE DIRECT ON (dn_6003_6004) 'select a.relname from pg_class a, pg_partition b where (a.oid=b.reldeltarelid or a.oid=b.relcudescrelid) and b.relname=''my_table_p1''';
          relname
   ----------------------
    pg_delta_part_60317
    pg_cudesc_part_60317
   (2 rows)
