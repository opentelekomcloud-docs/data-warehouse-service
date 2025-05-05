:original_name: dws_04_0041.html

.. _dws_04_0041:

Creating and Managing GaussDB(DWS) Scheduled Tasks
==================================================

GaussDB(DWS) allows users to create scheduled tasks, which are automatically executed at specified time points, reducing O&M workload.

The database follows Oracle's scheduled task feature, allowing you to use the DBMS.JOB advanced package interfaces to create, run, delete, and modify scheduled tasks (including the task ID, task enabling and disabling, task triggering time, triggering interval, and task content).

.. note::

   -  The hybrid data warehouse (standalone) does not support scheduled tasks.
   -  The execution statements of scheduled tasks are not recorded in the :ref:`Real-time Top SQL <dws_04_0397>` logs. The statements can be recorded only in versions later than 8.2.1.
   -  By default, GaussDB(DWS) uses the UTC time. The execution time of the scheduled task needs to be converted to the time zone of the user.

Periodic Task Management
------------------------

#. Creates a test table.

   ::

      CREATE TABLE test(id int, time date);

   If the following information is displayed, the table has been created.

   ::

      CREATE TABLE

#. Create the customized storage procedure.

   ::

      CREATE OR REPLACE PROCEDURE PRC_JOB_1()
      AS
      N_NUM integer :=1;
      BEGIN
      FOR I IN 1..1000 LOOP
      INSERT INTO test VALUES(I,SYSDATE);
      END LOOP;
      END;
      /

   If the following information is displayed, the procedure has been created.

   ::

      CREATE PROCEDURE

#. Create a task.

   -  Create a task with unspecified **job_id** and execute the **PRC_JOB_1** storage procedure every two minutes.

      ::

         call dbms_job.submit('call public.prc_job_1(); ', sysdate, 'interval ''1 minute''', :a);
         job
         -----
         1
         (1 row)

   -  Create task with specified job_id.

      ::

         call dbms_job.isubmit(2,'call public.prc_job_1(); ', sysdate, 'interval ''1 minute''');
         isubmit
         ---------

         (1 row)

#. View the created task information about the current user in the **USER_JOBS** view.

   Only the system administrator can access this system view. For details about the fields, see :ref:`Table 1 <en-us_topic_0000001811609909__tfc79ceaea73a45b685f452da34d39554>`.

   ::

      postgresselect job,dbname,start_date,last_date,this_date,next_date,broken,status,interval,failures,what from user_jobs;
       job |  dbname  |         start_date         |         last_date          |         this_date          |      next_date      | broken | status |      interval       | failures |           what

      -----+----------+----------------------------+----------------------------+----------------------------+---------------------+--------+--------+---------------------+----------+----------------
      -----------
         1 | db_demo | 2022-03-25 07:58:01.829436 | 2022-03-25 07:58:03.174817 | 2022-03-25 07:58:01.829436 | 2022-03-25 07:59:01 | n      | s      | interval '1 minute' |        0 | call public.prc
      _job_1();
         2 | db_demo | 2022-03-25 07:58:15.893383 | 2022-03-25 07:58:16.608959 | 2022-03-25 07:58:15.893383 | 2022-03-25 07:59:15 | n      | s      | interval '1 minute' |        0 | call public.prc
      _job_1();
      (2 rows)

#. Stop a task.

   ::

      call dbms_job.broken(1,true);
      broken
      --------

      (1 row)

#. Start a task.

   ::

      call dbms_job.broken(1,false);
      broken
      --------

      (1 row)

#. Modify attributes of a task.

   -  Modify the **Next_date** parameter information about a task. For example, change the value of **Next_date** of Job1 to 1 hour.

      ::

         call dbms_job.next_date(1, sysdate+1.0/24);
         next_date
         -----------

         (1 row)

   -  Modify the **Interval** parameter information of a task. For example, change the value of **Interval** of Job1 to 1 hour.

      ::

         call dbms_job.interval(1,'sysdate + 1.0/24');
         interval
         ----------

         (1 row)

   -  Modify the **What** parameter information of a **JOB**. For example, change **What** of **Job1** to **insert into public.test values(333, sysdate+5)**.

      ::

         call dbms_job.what(1,'insert into public.test values(333, sysdate+5);');
         what
         ------

         (1 row)

   -  Modify **Next_date**, **Interval**, and **What** parameter information of **JOB**.

      ::

         call dbms_job.change(1, 'call public.prc_job_1();', sysdate, 'interval ''1 minute''');
         change
         --------

         (1 row)

#. Delete a job.

   ::

      call dbms_job.remove(1);
      remove
      --------

      (1 row)

#. Set job permissions.

   -  During the creation of a job, the job is bound to the user and database that created the job. Accordingly, the user and database are added to **dbname** and **log_user** columns in the **pg_job** system view, respectively.
   -  If the current user is a DBA user, system administrator, or the user who created the job (**log_user** in **pg_job**), the user has the permissions to delete or modify parameter settings of the job using the remove, change, next_data, what, or interval interface. Otherwise, the system displays a message indicating that the current user has no permission to perform operations on the JOB.
   -  If the current database is the one that created a job, (that is, **dbname** in **pg_job**), you can delete or modify parameter settings of the job using the remove, change, next_data, what, or interval interface.
   -  When deleting the database that created a job, (that is, **dbname** in **pg_job**), the system associatively deletes the job records of the database.
   -  When deleting the user who created a job, (that is, **log_user** in **pg_job**), the system associatively deletes the job records of the user.
