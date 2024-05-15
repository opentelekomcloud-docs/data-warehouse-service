:original_name: dws_06_0169.html

.. _dws_06_0169:

CREATE ROW LEVEL SECURITY POLICY
================================

Function
--------

**CREATE ROW LEVEL SECURITY POLICY** creates a row-level access control policy for a table.

The policy takes effect only after row-level access control is enabled (by running **ALTER TABLE**... **ENABLE ROW LEVEL SECURITY**).

Currently, row-level access control affects the read (**SELECT**, **UPDATE**, **DELETE**) of data tables and does not affect the write (**INSERT** and **MERGE INTO**) of data tables. The table owner or system administrators can create an expression in the **USING** clause. When the client reads the data table, the database server combines the expressions that meet the condition and applies it to the execution plan in the statement rewriting phase of a query. For each tuple in a data table, if the expression returns **TRUE**, the tuple is visible to the current user; if the expression returns **FALSE** or **NULL**, the tuple is invisible to the current user.

A row-level access control policy name is specific to a table. A data table cannot have row-level access control policies with the same name. Different data tables can have the same row-level access control policy.

Row-level access control policies can be applied to specified operations (**SELECT**, **UPDATE**, **DELETE**, and **ALL**). **ALL** indicates that **SELECT**, **UPDATE**, and **DELETE** will be affected. For a new row-level access control policy, the default value **ALL** will be used if you do not specify the operations that will be affected.

Row-level access control policies can be applied to a specified user (role) or to all users (**PUBLIC**). For a new row-level access control policy, the default value **PUBLIC** will be used if you do not specify the user that will be affected.

Precautions
-----------

-  Row-level access control policies can be defined for row-store tables, row-store partitioned tables, column-store tables, column-store partitioned tables, replication tables, unlogged tables, and hash tables.

-  Row-level access control policies cannot be defined for HDFS tables, foreign tables, and temporary tables.

-  Row-level access control policies cannot be defined for views.

-  A maximum of 100 row-level access control policies cannot be defined for a table.

-  Users with administrator permissions and initial O&M users (Ruby) are not subject to row-level access control and can view full data of the table.

-  Tables queried by using SQL statements, views, functions, and stored procedures are affected by row-level access control policies.

-  The type of a column on which a row-level access control policy depends cannot be changed. For example, the following modifications are not supported:

   ::

      ALTER TABLE public.all_data ALTER COLUMN role TYPE text;

Syntax
------

::

   CREATE [ ROW LEVEL SECURITY ] POLICY policy_name ON table_name
       [ AS { PERMISSIVE | RESTRICTIVE } ]
       [ FOR { ALL | SELECT | UPDATE | DELETE } ]
       [ TO { role_name | PUBLIC } [, ...] ]
       USING ( using_expression )

Parameter Description
---------------------

-  *policy_name*

   Specifies the name of a row-level access control policy to be created. The names of row-level access control policies for a table must be unique.

-  *table_name*

   Specifies the name of a table to which a row-level access control policy is applied.

-  **PERMISSIVE**

   Specifies that the row-level access control policy is to be created as a permissive policy. For a given query, all applicable permissive policies are combined using the OR operator. Row-level access control policies are permissive by default.

-  **RESTRICTIVE**

   Specifies that the row-level access control policy is to be created as a restrictive policy. For a given query, all applicable restrictive policies are combined using the AND operator.

   .. important::

      At least one permissive policy is required to grant access to data records. If only restrictive policies are used, no records will be accessible. When both permissive and restrictive policies are used, a record is accessible only when it passes at least one permissive policy and all restrictive policies.

-  *command*

   Specifies the SQL operations affected by a row-level access control policy, including **ALL**, **SELECT**, **UPDATE**, and **DELETE**. If this parameter is not specified, the default value **ALL** will be used, covering **SELECT**, **UPDATE**, and **DELETE**.

   If *command* is set to **SELECT**, only tuple data that meets the condition (the return value of *using_expression* is **TRUE**) can be queried. The operations that are affected include **SELECT**, **UPDATE.... RETURNING**, and **DELETE... RETURNING**.

   If *command* is set to **UPDATE**, only tuple data that meets the condition (the return value of *using_expression* is **TRUE**) can be updated. The operations that are affected include **UPDATE**, **UPDATE ... RETURNING**, and **SELECT ... FOR UPDATE/SHARE**.

   If *command* is set to **DELETE**, only tuple data that meets the condition (the return value of *using_expression* is **TRUE**) can be deleted. The operations that are affected include **DELETE** and **DELETE ... RETURNING**.

   The following table describes the relationship between row-level access control policies and SQL statements.

   .. table:: **Table 1** Relationship between row-level security policies and SQL statements

      +-----------------------------+-------------------+-------------------+-------------------+
      | Command                     | SELECT/ALL Policy | UPDATE/ALL Policy | DELETE/ALL Policy |
      +=============================+===================+===================+===================+
      | **SELECT**                  | Existing row      | No                | No                |
      +-----------------------------+-------------------+-------------------+-------------------+
      | **SELECT FOR UPDATE/SHARE** | Existing row      | Existing row      | No                |
      +-----------------------------+-------------------+-------------------+-------------------+
      | **UPDATE**                  | No                | Existing row      | No                |
      +-----------------------------+-------------------+-------------------+-------------------+
      | **UPDATE RETURNING**        | Existing row      | Existing row      | No                |
      +-----------------------------+-------------------+-------------------+-------------------+
      | **DELETE**                  | No                | No                | Existing row      |
      +-----------------------------+-------------------+-------------------+-------------------+
      | **DELETE RETURNING**        | Existing row      | No                | Existing row      |
      +-----------------------------+-------------------+-------------------+-------------------+

-  *role_name*

   Specifies database users affected by a row-level access control policy.

   If this parameter is not specified, the default value **PUBLIC** will be used, indicating that all database users will be affected. You can specify multiple affected database users.

   .. important::

      System administrators are not affected by row access control.

-  *using_expression*

   Specifies an expression defined for a row-level access control policy (return type: boolean).

   The expression cannot contain aggregate functions and window functions. In the statement rewriting phase of a query, if row-level access control for a data table is enabled, the expressions that meet the specified conditions will be added to the plan tree. The expression is calculated for each tuple in the data table. For **SELECT**, **UPDATE**, and **DELETE**, row data is visible to the current user only when the return value of the expression is **TRUE**. If the expression returns **FALSE**, the tuple is invisible to the current user. In this case, the user cannot view the tuple through the **SELECT** statement, update the tuple through the **UPDATE** statement, or delete the tuple through the **DELETE** statement.

Examples
--------

Create user **alice**.

::

   CREATE ROLE alice PASSWORD '{Password}';

Create user **bob**.

::

   CREATE ROLE bob PASSWORD '{Password}';

Create the data table **public.all_data**:

::

   CREATE TABLE public.all_data(id int, role varchar(100), data varchar(100));

Insert data into the data table:

::

   INSERT INTO all_data VALUES(1, 'alice', 'alice data');
   INSERT INTO all_data VALUES(2, 'bob', 'bob data');
   INSERT INTO all_data VALUES(3, 'peter', 'peter data');

Grant the read permission for the **all_data** table to users **alice** and **bob**:

::

   GRANT SELECT ON all_data TO alice, bob;

Enable row-level access control.

::

   ALTER TABLE all_data ENABLE ROW LEVEL SECURITY;

Create a row-level access control policy to specify that the current user can view only their own data:

::

   CREATE ROW LEVEL SECURITY POLICY all_data_rls ON all_data USING(role = CURRENT_USER);

View information about the **all_data** table:

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
   Distribute By: HASH(id)
   Location Nodes: ALL DATANODES
   Options: orientation=row, compression=no, enable_rowsecurity=true

Run **SELECT**.

::

   SELECT * FROM all_data;
    id | role  |    data
   ----+-------+------------
     1 | alice | alice data
     2 | bob   | bob data
     3 | peter | peter data
   (3 rows)
   EXPLAIN(COSTS OFF) SELECT * FROM all_data;
            QUERY PLAN
   ----------------------------
    Streaming (type: GATHER)
      Node/s: All datanodes
      ->  Seq Scan on all_data
   (3 rows)

Switch to the **alice** user.

::

   set role alice password '{Password}';

Perform the SELECT operation.

::

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

Helpful Links
-------------

:ref:`DROP ROW LEVEL SECURITY POLICY <dws_06_0200>`
