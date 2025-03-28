:original_name: dws_04_0061.html

.. _dws_04_0061:

GaussDB(DWS) Row-Level Access Control
=====================================

The row-level access control feature enables database access control to be accurate to each row of data tables. In this way, the same SQL query may return different results for different users.

You can create a row-level access control policy for a data table. The policy defines an expression that takes effect only for specific database users and SQL operations. When a database user accesses the data table, if a SQL statement meets the specified row-level access control policies of the data table, the expressions that meet the specified condition will be combined by using **AND** or **OR** based on the attribute type (**PERMISSIVE** \| **RESTRICTIVE**) and applied to the execution plan in the query optimization phase.

Row-level access control is used to control the visibility of row-level data in tables. By predefining filters for data tables, the expressions that meet the specified condition can be applied to execution plans in the query optimization phase, which will affect the final execution result. Currently, the SQL statements that can be affected include **SELECT**, **UPDATE**, and **DELETE**.

Scenario 1: A table summarizes the data of different users. Users can view only their own data.

::

   -- Create users alice, bob, and peter.
   CREATE ROLE alice PASSWORD 'password';
   CREATE ROLE bob PASSWORD 'password';
   CREATE ROLE peter PASSWORD 'password';

   -- Create the public.all_data table that contains user information.
   CREATE TABLE public.all_data(id int, role varchar(100), data varchar(100));

   -- Insert data into the data table.
   INSERT INTO all_data VALUES(1, 'alice', 'alice data');
   INSERT INTO all_data VALUES(2, 'bob', 'bob data');
   INSERT INTO all_data VALUES(3, 'peter', 'peter data');

   -- Grant the read permission for the all_data table to users alice, bob, and peter.
   GRANT SELECT ON all_data TO alice, bob, peter;

   -- Enable row-level access control.
   ALTER TABLE all_data ENABLE ROW LEVEL SECURITY;

   -- Create a row-level access control policy to specify that the current user can view only their own data.
   CREATE ROW LEVEL SECURITY POLICY all_data_rls ON all_data USING(role = CURRENT_USER);

   -- View table details.
    \d+ all_data
                                  Table "public.all_data"
    Column |          Type          | Modifiers | Storage  | Stats target | Description
   --------+------------------------+-----------+----------+--------------+-------------
    id     | integer                |           | plain    |              |
    role   | character varying(100) |           | extended |              |
    data   | character varying(100) |           | extended |              |
   Row Level Security Policies:
       POLICY "all_data_rls"
         USING (((role)::name = "current_user"()))
   Has OIDs: no
   Distribute By: HASH(id)
   Location Nodes: ALL DATANODES
   Options: orientation=row, compression=no, enable_rowsecurity=true

   -- Switch to user alice and run SELECT * FROM all_data.
   SET ROLE alice PASSWORD 'password';
   SELECT * FROM all_data;
    id | role  |    data
   ----+-------+------------
     1 | alice | alice data
   (1 row)

   EXPLAIN(COSTS OFF) SELECT * FROM all_data;
                              QUERY PLAN
   ----------------------------------------------------------------
    Streaming (type: GATHER)
      Node/s: All datanodes
      ->  Seq Scan on all_data
            Filter: ((role)::name = 'alice'::name)
    Notice: This query is influenced by row level security feature
   (5 rows)

   -- Switch to user peter and run SELECT * FROM .all_data.
   SET ROLE peter PASSWORD 'password';
   SELECT * FROM all_data;
    id | role  |    data
   ----+-------+------------
     3 | peter | peter data
   (1 row)

    EXPLAIN(COSTS OFF) SELECT * FROM all_data;
                              QUERY PLAN
   ----------------------------------------------------------------
    Streaming (type: GATHER)
      Node/s: All datanodes
      ->  Seq Scan on all_data
            Filter: ((role)::name = 'peter'::name)
    Notice: This query is influenced by row level security feature
   (5 rows)
