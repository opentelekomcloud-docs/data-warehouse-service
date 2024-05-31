:original_name: dws_04_0596.html

.. _dws_04_0596:

PG_JOBS
=======

**PG_JOBS** records detailed information about jobs created by users. Dedicated threads poll the **pg_jobs** table and trigger jobs based on scheduled job execution time. This table belongs to the Shared Relation category. All job records are visible to all databases.

.. table:: **Table 1** PG_JOBS columns

   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | Name            | Type                        | Description                                                              |
   +=================+=============================+==========================================================================+
   | job_id          | integer                     | Job ID, primary key, unique (with a unique index)                        |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | what            | text                        | Job content                                                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | log_user        | oid                         | Username of the job creator                                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | priv_user       | oid                         | User ID of the job executor                                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | job_db          | oid                         | OID of the database where the job is executed                            |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | job_nsp         | oid                         | OID of the namespace where a job is running                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | job_node        | oid                         | CN node on which the job will be created and executed                    |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | is_broken       | boolean                     | Indicates whether the current job is invalid.                            |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | start_date      | timestamp without time zone | Start time of the first job execution, accurate to millisecond           |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | next_run_date   | timestamp without time zone | Scheduled time of the next job execution, accurate to millisecond        |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | failure_count   | smallint                    | Number of consecutive failures.                                          |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | interval        | text                        | Job execution interval                                                   |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | last_start_date | timestamp without time zone | Start time of the last job execution, accurate to millisecond            |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | last_end_date   | timestamp without time zone | End time of the last job execution, accurate to millisecond              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | last_suc_date   | timestamp without time zone | Start time of the last successful job execution, accurate to millisecond |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | this_run_date   | timestamp without time zone | Start time of the ongoing job execution, accurate to millisecond         |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
