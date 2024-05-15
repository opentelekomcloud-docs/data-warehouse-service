:original_name: dws_04_0061.html

.. _dws_04_0061:

Row-Level Access Control
========================

The row-level access control feature restricts users to access only specific data rows in the data table, ensuring data read and write security.

Configuring Row-Level Access Control
------------------------------------

Row-level access control is used to control the visibility of row-level data in tables. By predefining filters for data tables, the expressions that meet the specified condition can be applied to execution plans in the query optimization phase, which will affect the final execution result. Currently, the SQL statements that can be affected include **SELECT**, **UPDATE**, and **DELETE**.

-  You can use the CREATE ROW LEVEL SECURITY POLICY statement to create a row-level security policy on a table.

   This policy works only for expressions that take effect for specific database users and SQL operations. When a database user accesses the data table, if a SQL statement meets the specified row-level access control policies of the data table, the expressions that meet the specified condition will be combined by using **AND** or **OR** based on the attribute type (**PERMISSIVE** \| **RESTRICTIVE**) and applied to the execution plan in the query optimization phase.

-  After a row-level access control policy is created for a table, it takes effect only when the row-level access control switch (**ALTER TABLE**...\ **ENABLE ROW LEVEL SECURITY**) of the table is turned on.

Example of Row-Level Access Control
-----------------------------------

The data of all users is aggregated in table **all_data**. Implement row-level access control on this table so that different users can view only their own data.

#. Create users **alice**, **bob**, and **peter**.

   ::

      CREATE ROLE alice PASSWORD '********';
      CREATE ROLE bob PASSWORD '********';
      CREATE ROLE peter PASSWORD '********';

   Create table **all_data** and insert data of different users into it.

   ::

      CREATE TABLE public.all_data(id int, role varchar(100), data varchar(100));

      INSERT INTO all_data VALUES(1, 'alice', 'alice data');
      INSERT INTO all_data VALUES(2, 'bob', 'bob data');
      INSERT INTO all_data VALUES(3, 'peter', 'peter data');

#. Grant the read permission on table **all_data** to users **alice**, **bob**, and **peter**.

   ::

      GRANT SELECT ON all_data TO alice, bob, peter;

#. Run the **ALTER TABLE** *tablename* **ENABLE ROW LEVEL SECURITY** statement to enable the row-level access control.

   ::

      ALTER TABLE all_data ENABLE ROW LEVEL SECURITY;

#. Run the **CREATE ROW LEVEL SECURITY POLICY** statement to create a row-level access control policy so that the current user can view only its own data.

   ::

      CREATE ROW LEVEL SECURITY POLICY all_data_rls ON all_data USING(role = CURRENT_USER);

#. View information about the **all_data** table.

   ::

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
      Distribute By: ROUND ROBIN
      Location Nodes: ALL DATANODES
      Options: orientation=row, compression=no, enable_rowsecurity=true

#. Switch to user **alice** and query the data in table **all_data**. The query result shows that the row-level access control policy takes effect. User **alice** can only view its own data.

   ::

      SET ROLE alice PASSWORD '********';

      SELECT * FROM all_data;
       id | role  |    data
      ----+-------+------------
        1 | alice | alice data

   The execution plan of the query is displayed, indicating that access to table **all_data** is under the row-level access control.

   ::

      EXPLAIN(COSTS OFF) SELECT * FROM all_data;
                                 QUERY PLAN
      ----------------------------------------------------------------
        id |          operation
       ----+------------------------------
         1 | ->  Streaming (type: GATHER)
         2 |    ->  Seq Scan on all_data

               Predicate Information (identified by plan id)
       --------------------------------------------------------------
         2 --Seq Scan on all_data
               Filter: ((role)::name = 'alice'::name)
       Notice: This query is influenced by row level security feature

         ====== Query Summary =====
       -------------------------------
       System available mem: 4833280KB
       Query Max mem: 4833280KB
       Query estimated mem: 1024KB
      (16 rows)

#. Switch to user **peter** and query the data in table **all_data**. The query result shows that the row-level access control policy takes effect. User **peter** can only view its own data.

   ::

      SET ROLE peter PASSWORD '********';

      SELECT * FROM all_data;
       id | role  |    data
      ----+-------+------------
        3 | peter | peter data
      (1 row)

   The execution plan of the table query is displayed, indicating that the query of table **all_data** is under the row-level access control.

   ::

      EXPLAIN(COSTS OFF) SELECT * FROM all_data;
                                 QUERY PLAN
      ----------------------------------------------------------------
        id |          operation
       ----+------------------------------
         1 | ->  Streaming (type: GATHER)
         2 |    ->  Seq Scan on all_data

               Predicate Information (identified by plan id)
       --------------------------------------------------------------
         2 --Seq Scan on all_data
               Filter: ((role)::name = 'peter'::name)
       Notice: This query is influenced by row level security feature

         ====== Query Summary =====
       -------------------------------
       System available mem: 4833280KB
       Query Max mem: 4833280KB
       Query estimated mem: 1024KB
      (16 rows)
