:original_name: dws_04_0492.html

.. _dws_04_0492:

Low Query Efficiency
====================

A query task that used to take a few milliseconds to complete is now requiring several seconds, and that used to take several seconds is now requiring even half an hour. This section describes how to analyze and rectify such low efficiency issues.

Procedure
---------

Perform the following procedure to locate the cause of this fault.

#. Run the **analyze** command to analyze the database.

   The **analyze** command updates data statistics information, such as data sizes and attributes in all tables. This is a lightweight command and can be executed frequently. If the query efficiency is improved or restored after the command execution, the autovacuum process does not function well and requires further analysis.

#. Check whether the query statement returns unnecessary information.

   For example, if we only need the first 10 records in a table but the query statement searches all records in the table, the query efficiency is fine for a table containing only 50 records but very low for a table containing 50,000 records.

   If an application requires only a part of data information but the query statement returns all information, add a LIMIT clause to the query statement to restrict the number of returned records. In this way, the database optimizer can optimize space and improve query efficiency.

#. Check whether the query statement still has a low response even when it is solely executed.

   Run the query statement when there are no or only a few other query requests in the database, and observe the query efficiency. If the efficiency is high, the previous issue is possibly caused by a heavily loaded host in the database system or an inefficient execution plan.

#. Check the same query statement repeatedly to check the query efficiency.

   One major cause that will reduce query efficiency is that the required information is not cached in the memory or is replaced by other query requests because of insufficient memory resources.

   Run the same query statement repeatedly. If the query efficiency increases gradually, the previous issue might be caused by this reason.
