:original_name: dws_04_0495.html

.. _dws_04_0495:

Different Data Is Displayed for the Same Table Queried By Multiple Users
========================================================================

Problem
-------

Two users log in to the same database human_resource and run the **select count(*) from areas** statement separately to query the areas table, but obtain different results.

Possible Causes
---------------

Check whether the two users really query the same table. In a relational database, a table is identified by three elements: **database**, **schema**, and **table**. In this issue, **database** is **human_resource** and **table** is **areas**. Then, check **schema**. Log in as users **dbadmin** and **user01** separately. It is found that **search_path** is **public** for **dbadmin** and *$user* for **user01**. By default, a schema having the same name as user **dbadmin**, the cluster administrator, is not created. That is, all tables will be created in **public** if no schema is specified. However, when a common user, such as **user01**, is created, the same-name schema (**user01**) is created by default. That is, all tables are created in **user01** if the schema is not specified. In conclusion, both the two users are operating the table, causing that the same-name table is not really the same table.

Troubleshooting Method
----------------------

Use *schema*\ **.table** to determine a table for query.
