:original_name: dws_04_0106.html

.. _dws_04_0106:

GaussDB(DWS) Stored Procedure Development Specifications
========================================================

.. _en-us_topic_0000002100746058__en-us_topic_0000002098487000_section15429113495012:

Suggestion 5.1: Simplifying Stored Procedures and Avoiding Nesting
------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  The maintenance cost for complex and nested stored procedures is high, making fault locating and recovery time-consuming.

   **Solution:**

   -  Avoid using stored procedures altogether or limit their usage to a single layer. Nested stored procedures should be avoided.
   -  Create a corresponding log table for the stored procedure design and record information before and after key steps in the log table. Follow the steps below to implement this.

Saving and viewing logs

#. Create a log table.

   ::

      CREATE TABLE func_exec_log
      (
      id varchar2(32) default lower(sys_guid()),
      pro_name varchar2(60),
      exec_times int,
      log_date date,
      deal_date date,
      log_mesage text
      );

#. Create a table and import data.

   ::

      CREATE TABLE demo_table(data_id int, data_number int);
      INSERT INTO demo_table values(generate_series(1,1000),generate_series(1,1000));

#. Create a service stored procedure.

   ::

      CREATE OR REPLACE FUNCTION demo_table_process(out exe_info text)
      LANGUAGE plpgsql
      AS $$
      declare v_count int;
      pro_result text;
      fun_name   text;
      exec_times   int;
      begin
      fun_name := 'demo_table_process';
      select nvl(max(exec_times), '0') + 1 into exec_times from func_exec_log where pro_name = fun_name;
      -- Insert data into the service table.
      insert into demo_table values (dbms_random.value(1, 1000)::int,generate_series(1, dbms_random.value(10000, 20000)::int));
      get diagnostics v_count = ROW_COUNT;
      exe_info = sysdate || '# step1:insert count:' || v_count || ' rows;';
      -- Delete specified data from a service table.
      delete from demo_table where data_id = dbms_random.value(1, 1000)::int;
      get diagnostics v_count = ROW_COUNT;
      exe_info = exe_info || sysdate || '# step2:delete count:' || v_count || ' rows;';
      -- Update service table data.
      update demo_table set data_number = dbms_random.value(1, 100)::int where data_id = dbms_random.value(1, 1000)::int;
      exe_info = exe_info || sysdate || '# step3:update count:' || sql%rowcount || ' rows';
      -- Record logs either before the entire program ends or after each step completes. You can also create a function specifically for logging purposes.
      insert into func_exec_log(pro_name, exec_times, log_date, deal_date, log_mesage) values (fun_name,exec_times,sysdate,split_part(regexp_split_to_table(exe_info, ';'), '#', 1),split_part(regexp_split_to_table(exe_info, ';'), '#', 2));
      -- EXCEPTION is used to ensure that logs can be properly recorded when the insertion, update, or deletion exits abnormally.
      EXCEPTION
      WHEN OTHERS THEN
      pro_result := exe_info || sysdate || '# exception error message is: ' || sqlerrm;
      insert into func_exec_log(pro_name, exec_times, log_date, deal_date, log_mesage) values(fun_name,exec_times,sysdate,split_part(regexp_split_to_table(pro_result, ';'), '#', 1),split_part(regexp_split_to_table(pro_result, ';'), '#', 2));
      END; $$;

#. Invoke the stored procedure (normal execution).

   ::

      SELECT demo_table_process();

#. View the created log table to check the service running status.

   .. code-block::

      SELECT * FROM func_exec_log ORDER BY log_date desc,deal_date,log_mesage;

   |image1|

#. Invoke the stored procedure again to construct an execution exception.

   .. code-block::

      SELECT demo_table_process();  -- Delete the data_number column of demo_table to construct an exception, and then call the stored procedure again.

#. View the log to check the service running status.

.. _en-us_topic_0000002100746058__en-us_topic_0000002098487000_section188550284514:

Rule 5.2: Avoiding Non-CREATE DDL Operations in Stored Procedures
-----------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  A stored procedure is a large transaction. If a non-CREATE DDL operation, especially one with a high lock level, is executed, it can block external access to related tables during the stored procedure's execution window.

   **Solution:**

   -  Avoid using non-CREATE DDL operations within stored procedures whenever possible. If there is a necessity to use such operations, carefully assess the duration of the stored procedures and the potential impact of the DDL operations. It is advised to schedule non-CREATE DDL operations during off-peak hours when external access services are less active.

.. |image1| image:: /_static/images/en-us_image_0000002100390820.png
