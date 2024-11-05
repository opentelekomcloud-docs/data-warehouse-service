:original_name: dws_04_0268.html

.. _dws_04_0268:

Examples of Exporting Data Using GDS
====================================

Exporting Data in Remote Mode
-----------------------------

The data server and the cluster reside on the same intranet, the IP address of the data server is **192.168.0.90**, and data source files are in CSV format. In this scenario, data is exported in parallel in **Remote** mode.

To export data in parallel in **Remote** mode, perform the following operations:

#. Log in to the GDS data server as user **root**, create the **/output_data** directory for storing data files, and create user **gds_user** and its user group.

   ::

      mkdir -p /output_data

#. (Optional) Create a user and the user group it belongs to. The user is used to start GDS. If the user and user group exist, skip this step.

   ::

      groupadd gdsgrp
      useradd -g gdsgrp gds_user

#. Change the owner of the **/output_data** directory on the data server to **gds_user**.

   ::

      chown -R gds_user:gdsgrp /output_data

#. Log in to the data server as user **gds_user** and start GDS.

   The GDS installation path is **/opt/bin/dws/gds**. Exported data files are stored in **/output_data/**. The IP address of the data server is **192.168.0.90**. The GDS listening port is **5000**. GDS runs in daemon mode.

   ::

      /opt/bin/dws/gds/bin/gds -d /output_data -p 192.168.0.90:5000 -H 10.10.0.1/24 -D

#. In the database, create the foreign table **foreign_tpcds_reasons** for receiving data from the data server.

   Data export mode settings are as follows:

   -  The directory for storing exported files is **/output_data/** and the GDS listening port is **5000** when GDS is started. The directory created for storing exported files is **/output_data/**. Therefore, the **location** parameter is set to **gsfs://192.168.0.90:5000/**.

   Data format parameter settings are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  **quote** is set to **E'\\x1b'**.
   -  **null** is set to an empty string without quotation marks.
   -  **escape** defaults to the value of **quote**.
   -  **header** is set to **false**, indicating that the first row is identified as a data row in an exported file.

   Based on the above settings, the foreign table is created using the following statement:

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons
      (
        r_reason_sk    integer        not null,
        r_reason_id    char(16)       not null,
        r_reason_desc  char(100)
      )
      SERVER gsmpp_server
      OPTIONS
      (
      LOCATION 'gsfs://192.168.0.90:5000/',
      FORMAT 'CSV',
      ENCODING 'utf8',
      DELIMITER E'\x08',
      QUOTE E'\x1b',
      NULL ''
      )
      WRITE ONLY;

#. In the database, export data to data files through the foreign table **foreign_tpcds_reasons**.

   ::

      INSERT INTO foreign_tpcds_reasons SELECT * FROM tpcds.reason;

#. After data export is complete, log in to the data server as user **gds_user** and stop GDS.

   The GDS process ID is **128954**.

   ::

      ps -ef|grep gds
      gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /output_data -p 192.168.0.90:5000 -D
      gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds
      kill -9 128954

.. _en-us_topic_0000001717097380__en-us_topic_0000001188482140_s855daf73006d4e05ba6d04f8db74e7f6:

Exporting Data Using Multiple Threads
-------------------------------------

The data server and the cluster reside on the same intranet, the IP address of the data server is **192.168.0.90**, and data source files are in CSV format. In this scenario, data is concurrently exported to two target tables using multiple threads in **Remote** mode.

To concurrently export data using multiple threads in **Remote** mode, perform the following operations:

#. Log in to the GDS data server as user **root**, create the **/output_data** directory for storing data files, and create the database user and its user group.

   ::

      mkdir -p /output_data
      groupadd gdsgrp
      useradd -g gdsgrp gds_user

#. Change the owner of the **/output_data** directory on the data server to **gds_user**.

   ::

      chown -R gds_user:gdsgrp /output_data

#. Log in to the data server as user **gds_user** and start GDS.

   The GDS installation path is **/opt/bin/dws/gds**. Exported data files are stored in **/output_data/**. The IP address of the data server is **192.168.0.90**. The GDS listening port is **5000**. GDS runs in daemon mode. The degree of parallelism is 2.

   ::

      /opt/bin/dws/gds/bin/gds -d /output_data -p 192.168.0.90:5000 -H 10.10.0.1/24 -D -t 2

#. In GaussDB(DWS), create the foreign tables **foreign_tpcds_reasons1** and **foreign_tpcds_reasons2** for receiving data from the data server.

   -  Data export mode settings are as follows:

      -  The directory for storing exported files is **/output_data/** and the GDS listening port is **5000** when GDS is started. The directory created for storing exported files is **/output_data/**. Therefore, the **location** parameter is set to **gsfs://192.168.0.90:5000/**.

   -  Data format parameter settings are as follows:

      -  **format** is set to **CSV**.
      -  **encoding** is set to **UTF-8**.
      -  **delimiter** is set to **E'\\x08'**.
      -  **quote** is set to **E'\\x1b'**.
      -  **null** is set to an empty string without quotation marks.
      -  **escape** defaults to the value of **quote**.
      -  **header** is set to **false**, indicating that the first row is identified as a data row in an exported file.

   Based on the preceding settings, the foreign table **foreign_tpcds_reasons1** is created using the following statement:

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons1
      (
        r_reason_sk    integer     not null,
        r_reason_id    char(16)    not null,
        r_reason_desc  char(100)
      )
      SERVER gsmpp_server
      OPTIONS
      (
      LOCATION 'gsfs://192.168.0.90:5000/',
      FORMAT 'CSV',
      ENCODING 'utf8',
      DELIMITER E'\x08',
      QUOTE E'\x1b',
      NULL ''
      )
      WRITE ONLY;

   Based on the preceding settings, the foreign table **foreign_tpcds_reasons2** is created using the following statement:

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons2
      (
        r_reason_sk    integer     not null,
        r_reason_id    char(16)    not null,
        r_reason_desc  char(100)
      )
      SERVER gsmpp_server
      OPTIONS
      (
      LOCATION 'gsfs://192.168.0.90:5000/',
      FORMAT 'CSV',
      DELIMITER E'\x08',
      QUOTE E'\x1b',
      NULL ''
      )
      WRITE ONLY;

#. In the database, export data from table **reasons1** through the foreign table **foreign_tpcds_reasons1** and from table **reasons2** through the foreign table **foreign_tpcds_reasons2** to **/output_data**.

   ::

      INSERT INTO foreign_tpcds_reasons1 SELECT * FROM tpcds.reason;

   ::

      INSERT INTO foreign_tpcds_reasons2 SELECT * FROM tpcds.reason;

#. After data export is complete, log in to the data server as user **gds_user** and stop GDS.

   The GDS process ID is **128954**.

   ::

      ps -ef|grep gds
      gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /output_data -p 192.168.0.90:5000 -D -t 2
      gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds
      kill -9 128954

Exporting Data Through a Pipe
-----------------------------

#. Start GDS.

   ::

      gds -d /***/gds_data/ -D -p 192.168.0.1:7789 -l /***/gds_log/aa.log -H 0/0 -t 10 -D

   If you need to set the timeout interval of a pipe, use the **--pipe-timeout** parameter.

#. Export data.

   a. Log in to the database, create an internal table, and write data to the table.

      ::

         CREATE TABLE test_pipe( id integer not null, gender text not null, name text ) ;


         INSERT INTO test_pipe values(1,2,'11111111111111');
         INSERT INTO test_pipe values(2,2,'11111111111111');
         INSERT INTO test_pipe values(3,2,'11111111111111');
         INSERT INTO test_pipe values(4,2,'11111111111111');
         INSERT 0 1

   b. Create a write-only foreign table.

      .. code-block::

         CREATE FOREIGN TABLE foreign_test_pipe_tw( id integer not null, age text not null, name  text ) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://192.168.0.1:7789/', FORMAT 'text', DELIMITER ',',  NULL '', EOL '0x0a' ,file_type 'pipe', auto_create_pipe 'false') WRITE ONLY;

   c. Execute the export statement. In this case, the statements are blocked.

      ::

         INSERT INTO foreign_test_pipe_tw select * from test_pipe;

#. Export data through the GDS pipes.

   a. Log in to GDS and go to the GDS data directory.

      ::

         cd /***/gds_data/

   b. Create a pipe. If **auto_create_pipe** is set to **true**, skip this step.

      ::

         mkfifo postgres_public_foreign_test_pipe_tw.pipe

      .. note::

         A pipe will be automatically cleared after an operation is complete. To perform another operation, create a pipe file again.

   c. Read data from the pipe and write it to a new file.

      ::

         cat postgres_public_foreign_test_pipe_tw.pipe > postgres_public_foreign_test_pipe_tw.txt

   d. To compress the exported files, run the following command:

      ::

         gzip -9 -c < postgres_public_foreign_test_pipe_tw.pipe  > out.gz

   e. To export the content from the pipe to the HDFS server, run the following command:

      ::

         cat postgres_public_foreign_test_pipe_tw.pipe  | hdfs dfs -put -  /user/hive/***/test_pipe.txt

#. Verify the exported data.

   a. Check whether the exported file is correct.

      ::

         cat postgres_public_foreign_test_pipe_tw.txt
         3,2,11111111111111
         1,2,11111111111111
         2,2,11111111111111
         4,2,11111111111111

   b. View the compressed file.

      ::

         vim out.gz
         3,2,11111111111111
         1,2,11111111111111
         2,2,11111111111111
         4,2,11111111111111

   c. View the data exported to the HDFS server.

      ::

         hdfs dfs -cat /user/hive/***/test_pipe.txt
         3,2,11111111111111
         1,2,11111111111111
         2,2,11111111111111
         4,2,11111111111111

Exporting Data Through Multi-Process Pipes
------------------------------------------

GDS also supports importing and exporting data through multi-process pipes. That is, one foreign table corresponds to multiple GDSs.

The following takes exporting a local file as an example.

#. Start multiple GDSs.

   ::

      gds -d /***/gds_data/ -D -p 192.168.0.1:7789 -l /***/gds_log/aa.log -H 0/0 -t 10 -D
      gds -d /***/gds_data_1/ -D -p 192.168.0.1:7790 -l /***/gds_log/aa.log -H 0/0 -t 10 -D

   If you need to set the timeout interval of a pipe, use the **--pipe-timeout** parameter.

#. Export data.

   a. Log in to the database and create an internal table.

      ::

         CREATE TABLE test_pipe (id integer not null, gender text not null, name  text);

   b. Write data.

      ::

         INSERT INTO test_pipe values(1,2,'11111111111111');
         INSERT INTO test_pipe values(2,2,'11111111111111');
         INSERT INTO test_pipe values(3,2,'11111111111111');
         INSERT INTO test_pipe values(4,2,'11111111111111');

   c. Create a write-only foreign table.

      ::

         CREATE FOREIGN TABLE foreign_test_pipe_tw( id integer not null, age text not null, name  text ) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://192.168.0.1:7789/|gsfs://192.168.0.1:7790/', FORMAT 'text', DELIMITER ',',  NULL '', EOL '0x0a' ,file_type 'pipe', auto_create_pipe 'false') WRITE ONLY;

   d. Execute the export statement. In this case, the statements are blocked.

      ::

         INSERT INTO foreign_test_pipe_tw select * from test_pipe;

#. Export data through the GDS pipes.

   a. Log in to GDS and go to each GDS data directory.

      ::

         cd /***/gds_data/
         cd /***/gds_data_1/

   b. Create a pipe. If **auto_create_pipe** is set to **true**, skip this step.

      ::

         mkfifo postgres_public_foreign_test_pipe_tw.pipe

   c. Read each pipe and write the new file to the pipes.

      .. code-block::

         cat postgres_public_foreign_test_pipe_tw.pipe > postgres_public_foreign_test_pipe_tw.txt

#. Verify the exported data.

   ::

      cat /***/gds_data/postgres_public_foreign_test_pipe_tw.txt
      3,2,11111111111111

   ::

      cat /***/gds_data_1/postgres_public_foreign_test_pipe_tw.txt
      1,2,11111111111111
      2,2,11111111111111
      4,2,11111111111111

Multi-table Joint Export
------------------------

GDS supports joint import and export of multiple tables, that is, a foreign table is joined with multiple internal tables.

The following takes exporting a local file as an example.

#. Start GDS.

   ::

      gds -d /***/gds_data/ -D -p 192.168.0.1:7789 -l /***/gds_log/aa.log -H 0/0 -t 10 -D

#. Export data of joined tables.

   a. Log in to the database and create multiple internal tables.

      ::

         CREATE TABLE pipe1(id integer not null, name character varying(30) not null);
         CREATE TABLE pipe2(id integer not null, province character varying(20));

   b. Write data.

      ::

         INSERT INTO pipe1 values(1,'Apple');
         INSERT INTO pipe1 values(2,'Banana');
         INSERT INTO pipe2 values(1,'Beijing');
         INSERT INTO pipe2 values(2,'Shanghai');

   c. Create a write-only foreign table.

      ::

         CREATE FOREIGN TABLE test_write ( id integer not null, name character varying(30) not null, province character varying(20) )   SERVER gsmpp_server OPTIONS(LOCATION 'gsfs://192.168.0.1:7789/',FORMAT 'csv',ENCODING 'utf8',DELIMITER ',',NULL '') WRITE ONLY;

   d. Execute the export statement. In this case, the statements are blocked.

      ::

         INSERT INTO test_write select pipe1.id,pipe1.name,pipe2.province FROM pipe1 join pipe2 on pipe1.id = pipe2.id;

#. Verify the exported data.

   ::

      cat /***/gds_data/test_write.dat.0
      2,Banana,Shanghai
      1,Apple,Beijing
