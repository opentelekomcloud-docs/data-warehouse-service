:original_name: dws_04_0596.html

.. _dws_04_0596:

PG_JOBS
=======

**PG_JOBS** records detailed information about jobs created by users. Dedicated threads poll the **pg_jobs** table and trigger jobs based on scheduled job execution time. This table belongs to the Shared Relation category. All job records are visible to all databases.

.. table:: **Table 1** PG_JOBS columns

   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | Name            | Type                        | Description                                                              |
   +=================+=============================+==========================================================================+
   | job_id          | Integer                     | Job ID, primary key, unique (with a unique index)                        |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | what            | Text                        | Job content                                                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | log_user        | OID                         | Username of the job creator                                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | priv_user       | OID                         | User ID of the job executor                                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | job_db          | OID                         | OID of the database where the job is executed                            |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | job_nsp         | OID                         | OID of the namespace where a job is running                              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | job_node        | OID                         | CN node on which the job will be created and executed                    |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | is_broken       | Boolean                     | Whether the current job is invalid                                       |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | start_date      | Timestamp without time zone | Start time of the first job execution, accurate to millisecond           |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | next_run_date   | Timestamp without time zone | Scheduled time of the next job execution, accurate to millisecond        |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | failure_count   | Smallint                    | Number of consecutive failures                                           |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | interval        | Text                        | Job execution interval                                                   |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | last_start_date | Timestamp without time zone | Start time of the last job execution, accurate to millisecond            |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | last_end_date   | Timestamp without time zone | End time of the last job execution, accurate to millisecond              |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | last_suc_date   | Timestamp without time zone | Start time of the last successful job execution, accurate to millisecond |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
   | this_run_date   | Timestamp without time zone | Start time of the ongoing job execution, accurate to millisecond         |
   +-----------------+-----------------------------+--------------------------------------------------------------------------+
