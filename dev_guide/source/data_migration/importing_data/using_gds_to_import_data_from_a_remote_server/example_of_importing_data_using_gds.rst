:original_name: dws_04_0198.html

.. _dws_04_0198:

Example of Importing Data Using GDS
===================================

.. _en-us_topic_0000001233761893__section2041618243291:

Parallel Import from Multiple Data Servers
------------------------------------------

The data servers and the cluster reside on the same intranet. The IP addresses are **192.168.0.90** and **192.168.0.91**. Source data files are in CSV format.

#. Log in to each GDS data server as user **root** and create the **/input_data** directory for storing data files on the servers. The following takes the data server whose IP address is **192.168.0.90** as an example. Operations on the other server are the same.

   .. code-block::

      mkdir -p /input_data

#. (Optional) Create a user and the user group it belongs to. The user is used to start GDS. If the user and user group exist, skip this step.

   .. code-block::

      groupadd gdsgrp
      useradd -g gdsgrp gds_user

#. Evenly distribute CSV source data files to the **/input_data** directories on the data servers.

#. Change the owners of source data files and the **/input_data** directory on each data server to **gds_user**. The data server whose IP address is **192.168.0.90** is used as an example.

   .. code-block::

      chown -R gds_user:gdsgrp /input_data

#. Log in to each data server as user **gds_user** and start GDS.

   The GDS installation path is **/opt/bin/dws/gds**. Source data files are stored in **/input_data/**. The IP addresses of the data servers are **192.168.0.90** and **192.168.0.91**. The GDS listening port is **5000**. GDS runs in daemon mode.

   Start GDS on the data server whose IP address is **192.168.0.90**.

   .. code-block::

      /opt/bin/dws/gds/bin/gds -d /input_data -p 192.168.0.90:5000 -H 10.10.0.1/24 -D

   Start GDS on the data server whose IP address is **192.168.0.91**.

   .. code-block::

      /opt/bin/dws/gds/bin/gds -d /input_data -p 192.168.0.91:5000 -H 10.10.0.1/24  -D

#. Use a tool to connect to the database.

#. Create the target table **tpcds.reasons**.

   ::

      CREATE TABLE tpcds.reasons
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      );

#. Create the foreign table **tpcds.foreign_tpcds_reasons** for receiving data from the data server.

   Data export mode settings are as follows:

   -  Set the import mode to **Normal**.
   -  When GDS is started, the source data file directory is **/input_data** and the GDS listening port is **5000**. Therefore, set **location** to **gsfs://192.168.0.90:5000/\* \| gsfs://192.168.0.91:5000/\***.

   Information about the data format is configured based on data format parameters specified during data export. The parameter configurations are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  **quote** is set to **E'\\x1b'**.
   -  **null** is set to an empty string without quotation marks.
   -  **escape** defaults to the value of **quote**.
   -  **header** is set to **false**, indicating that the first row is identified as a data row in an imported file.

   Configure import error tolerance parameters as follows:

   -  Set **PER NODE REJECT LIMIT** (number of allowed data format errors) to **unlimited**. In this case, all the data format errors detected during data import will be tolerated.
   -  Set **LOG INTO** to **err_tpcds_reasons**. The data format errors detected during data import will be recorded in the **err_tpcds_reasons** table.

   Based on the above settings, the foreign table is created using the following statement:

   ::

      CREATE FOREIGN TABLE tpcds.foreign_tpcds_reasons
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      )
      SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/* | gsfs://192.168.0.91:5000/*', format 'CSV',mode 'Normal', encoding 'utf8', delimiter E'\x08', quote E'\x1b', null '', fill_missing_fields 'false') LOG INTO err_tpcds_reasons PER NODE REJECT LIMIT 'unlimited';

#. Import data through the foreign table **tpcds.foreign_tpcds_reasons** to the target table **tpcds.reasons**.

   ::

      INSERT INTO tpcds.reasons SELECT * FROM tpcds.foreign_tpcds_reasons;

#. Query data import errors in the **err_tpcds_reasons** table and rectify the errors (if any). For details, see :ref:`Handling Import Errors <dws_04_0196>`.

   ::

      SELECT * FROM err_tpcds_reasons;

#. After data import is complete, log in to each data server as user **gds_user** and stop GDS.

   The data server whose IP address is **192.168.0.90** is used as an example. The GDS process ID is **128954**.

   .. code-block::

      ps -ef|grep gds
      gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /input_data -p 192.168.0.90:5000 -D
      gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds
      kill -9 128954

.. _en-us_topic_0000001233761893__section197704383292:

Data Import Using Multiple Threads
----------------------------------

The data servers and the cluster reside on the same intranet. The server IP address is **192.168.0.90**. Source data files are in CSV format. Data will be imported to two tables using multiple threads in **Normal** mode.

#. Log in to the GDS data server as user **root**, and then create the data file directory **/input_data** and its sub-directories **/input_data/import1/** and **/input_data/import2/**.

   .. code-block::

      mkdir -p /input_data

#. Store the source data files of the target table **tpcds.reasons1** in **/input_data/import1/** and the source data files of the target table **tpcds.reasons2** in **/input_data/import2/**.

#. (Optional) Create a user and the user group it belongs to. The user is used to start GDS. If the user and user group already exist, skip this step.

   .. code-block::

      groupadd gdsgrp
      useradd -g gdsgrp gds_user

#. Change the owners of source data files and the **/input_data** directory on the data server to **gds_user**.

   .. code-block::

      chown -R gds_user:gdsgrp /input_data

#. Log in to the data server as user **gds_user** and start GDS.

   The GDS installation path is **/opt/bin/dws/gds**. Source data files are stored in **/input_data/**. The IP address of the data server is **192.168.0.90**. The GDS listening port is **5000**. GDS runs in daemon mode. The degree of parallelism is 2. A recursive directory is specified.

   .. code-block::

      /opt/bin/dws/gds/bin/gds -d /input_data -p 192.168.0.90:5000 -H 10.10.0.1/24  -D -t 2 -r

#. Use a tool to connect to the database.

#. In the database, create the target tables **tpcds.reasons1** and **tpcds.reasons2**.

   ::

      CREATE TABLE tpcds.reasons1
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) ;

   ::

      CREATE TABLE tpcds.reasons2
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) ;

#. In the database, create the foreign tables **tpcds.foreign_tpcds_reasons1** and **tpcds.foreign_tpcds_reasons2** for the source data.

   The foreign table **tpcds.foreign_tpcds_reasons1** is used as an example to describe how to configure parameters in a foreign table.

   Data export mode settings are as follows:

   -  Set the import mode to **Normal**.
   -  When GDS is started, the configured source data file directory is **/input_data** and the GDS listening port is **5000**. However, source data files are actually stored in **/input_data/import1/**. Therefore, set **location** to **gsfs://192.168.0.90:5000/import1/\***.

   Information about the data format is configured based on data format parameters specified during data export. The parameter configurations are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  **quote** is set to **E'\\x1b'**.
   -  **null** is set to an empty string without quotation marks.
   -  **escape** defaults to the value of **quote**.
   -  **header** is set to **false**, indicating that the first row is identified as a data row in an imported file.

   Configure import error tolerance parameters as follows:

   -  Set **PER NODE REJECT LIMIT** (number of allowed data format errors) to **unlimited**. In this case, all the data format errors detected during data import will be tolerated.
   -  Set **LOG INTO** to **err_tpcds_reasons1**. The data format errors detected during data import will be recorded in the **err_tpcds_reasons1** table.
   -  If the last column of a source data file is missing, the **fill_missing_fields** parameter is automatically set to **NULL**.

   Based on the preceding settings, the foreign table **tpcds.foreign_tpcds_reasons1** is created using the following statement:

   ::

      CREATE FOREIGN TABLE tpcds.foreign_tpcds_reasons1
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/import1/*', format 'CSV',mode 'Normal', encoding 'utf8', delimiter E'\x08', quote E'\x1b', null '',fill_missing_fields 'on')LOG INTO err_tpcds_reasons1 PER NODE REJECT LIMIT 'unlimited';

   Based on the preceding settings, the foreign table **tpcds.foreign_tpcds_reasons2** is created using the following statement:

   ::

      CREATE FOREIGN TABLE tpcds.foreign_tpcds_reasons2
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/import2/*', format 'CSV',mode 'Normal', encoding 'utf8', delimiter E'\x08', quote E'\x1b', null '',fill_missing_fields 'on')LOG INTO err_tpcds_reasons2 PER NODE REJECT LIMIT 'unlimited';

#. Import data through the foreign table **tpcds.foreign_tpcds_reasons1** to **tpcds.reasons1** and through **tpcds.foreign_tpcds_reasons2** to **tpcds.reasons2**.

   ::

      INSERT INTO tpcds.reasons1 SELECT * FROM tpcds.foreign_tpcds_reasons1;

   ::

      INSERT INTO tpcds.reasons2 SELECT * FROM tpcds.foreign_tpcds_reasons2;

#. Query data import errors in the **err_tpcds_reasons1** and **err_tpcds_reasons2** tables and rectify the errors (if any). For details, see :ref:`Handling Import Errors <dws_04_0196>`.

   ::

      SELECT * FROM err_tpcds_reasons1;
      SELECT * FROM err_tpcds_reasons2;

#. After data import is complete, log in to the data server as user **gds_user** and stop GDS.

   The GDS process ID is **128954**.

   .. code-block::

      ps -ef|grep gds
      gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /input_data -p 192.168.0.90:5000 -D -t 2 -r
      gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds
      kill -9 128954

Importing Data Through a Pipe File
----------------------------------

#. Start the GDS.

   .. code-block::

      /opt/bin/dws/gds/bin/gds -d /***/gds_data/ -D -p 192.168.0.1:7789 -l /***/gds_log/aa.log -H 0/0 -t 10 -D

   If you need to set the timeout interval of a pipe, use the **--pipe-timeout** parameter.

#. Import data.

   a. Log in to the database and create an internal table.

      ::

         CREATE TABLE test_pipe_1( id integer not null, sex text not null, name  text );

   b. Create a read-only foreign table.

      ::

         CREATE FOREIGN TABLE foreign_test_pipe_tr( like test_pipe ) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://192.168.0.1:7789/foreign_test_pipe.pipe', FORMAT 'text', DELIMITER ',',  NULL '', EOL '0x0a' ,file_type 'pipe',auto_create_pipe 'false');

   c. Execute the import statement. The statement is blocked.

      ::

         INSERT INTO test_pipe_1 SELECT * FROM foreign_test_pipe_tr;

#. Import data through the GDS pipes.

   a. Log in to GDS and go to the GDS data directory.

      .. code-block::

         cd /***/gds_data/

   b. Create a pipe. If **auto_create_pipe** is set to **true**, skip this step.

      .. code-block::

         mkfifo foreign_test_pipe.pipe;

      .. note::

         A pipe will be automatically cleared after an operation is complete. To perform another operation, create a pipe file again.

   c. Write data to the pipe.

      .. code-block::

         cat postgres_public_foreign_test_pipe_tw.txt > foreign_test_pipe.pipe

   d. To read the compressed file to the pipe, run the following command.

      .. code-block::

         gzip -d < out.gz > foreign_test_pipe.pipe

   e. To read the HDFS file to the pipe, run the following command.

      .. code-block::

         hdfs dfs -cat - /user/hive/***/test_pipe.txt > foreign_test_pipe.pipe

#. View the result returned by the import statement.

   ::

      INSERT INTO test_pipe_1 select * from foreign_test_pipe_tr;
      INSERT 0 4
      SELECT * FROM test_pipe_1;
      id | sex |      name
      ----+-----+----------------
      3 | 2   | 11111111111111
      1 | 2   | 11111111111111
      2 | 2   | 11111111111111
      4 | 2   | 11111111111111
      (4 rows)

Importing Data Through Multi-Process Pipes
------------------------------------------

GDS also supports importing data through multi-process pipes. That is, one foreign table corresponds to multiple GDSs.

The following takes importing a local file as an example.

#. Start multiple GDSs. If the GDSs have been started, skip this step.

   .. code-block::

      /opt/bin/dws/gds/bin/gds -d /***/gds_data/ -D -p 192.168.0.1:7789 -l /***/gds_log/aa.log -H 0/0 -t 10 -D
      /opt/bin/dws/gds/bin/gds -d /***/gds_data_1/ -D -p 192.168.0.1:7790 -l /***/gds_log_1/aa.log -H 0/0 -t 10 -D

   If you need to set the timeout interval of a pipe, use the **--pipe-timeout** parameter.

#. Import data.

   a. Log in to the database and create an internal table.

      ::

         CREATE TABLE test_pipe( id integer not null, sex text not null, name  text );

   b. Create a read-only foreign table.

      ::

         CREATE FOREIGN TABLE foreign_test_pipe_tr( like test_pipe ) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://192.168.0.1:7789/foreign_test_pipe.pipe|gsfs://192.168.0.1:7790/foreign_test_pipe.pipe', FORMAT 'text', DELIMITER ',', NULL '', EOL '0x0a' , file_type 'pipe', auto_create_pipe 'false');

   c. Execute the import statement. The statement is blocked.

      ::

         INSERT INTO test_pipe_1 select * from foreign_test_pipe_tr;

#. Import data through the GDS pipes.

   a. Log in to GDS and go to each GDS data directory.

      .. code-block::

         cd /***/gds_data/
         cd /***/gds_data_1/

   b. Create a pipe. If **auto_create_pipe** is set to **true**, skip this step.

      .. code-block::

         mkfifo foreign_test_pipe.pipe;

   c. Read each pipe and write the new file to the pipes.

      .. code-block::

         cat postgres_public_foreign_test_pipe_tw.txt > foreign_test_pipe.pipe

#. View the result returned by the import statement.

   ::

      INSERT INTO test_pipe_1 select * from foreign_test_pipe_tr;
      INSERT 0 4
      SELECT * FROM test_pipe_1;
      id | sex |      name
      ----+-----+----------------
      3 | 2   | 11111111111111
      1 | 2   | 11111111111111
      2 | 2   | 11111111111111
      4 | 2   | 11111111111111
      (4 rows)

Direct Data Import Between Clusters
-----------------------------------

#. Start the GDS. (If the process has been started, skip this step.)

   .. code-block::

      gds -d /***/gds_data/ -D -p GDS_IP:GDS_PORT -l /***/gds_log/aa.log -H 0/0 -t 10 -D

   If you need to set the timeout interval of a pipe, use the **--pipe-timeout** parameter.

#. Export data from the source database.

   a. Log in to the target database, create an internal table, and write data to the table.

      .. code-block::

         CREATE TABLE test_pipe( id integer not null, sex text not null, name  text );
         INSERT INTO test_pipe values(1,2,'11111111111111');
         INSERT INTO test_pipe values(2,2,'11111111111111');
         INSERT INTO test_pipe values(3,2,'11111111111111');
         INSERT INTO test_pipe values(4,2,'11111111111111');
         INSERT INTO test_pipe values(5,2,'11111111111111');

   b. Create a write-only foreign table.

      .. code-block::

         CREATE FOREIGN TABLE foreign_test_pipe( id integer not null, age text not null, name  text ) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://GDS_IP:GDS_PORT/', FORMAT 'text', DELIMITER ',', NULL '', EOL '0x0a' ,file_type 'pipe') WRITE ONLY;

   c. Execute the import statement. The statement is blocked.

      .. code-block::

         INSERT INTO foreign_test_pipe SELECT * FROM test_pipe;

#. Import data to the target cluster.

   a. Create an internal table.

      .. code-block::

         CREATE TABLE test_pipe (id integer not null, sex text not null, name text);

   b. Create a read-only foreign table.

      .. code-block::

         CREATE FOREIGN TABLE foreign_test_pipe(like test_pipe) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://GDS_IP:GDS_PORT/', FORMAT 'text', DELIMITER ',', NULL '', EOL '0x0a' , file_type 'pipe', auto_create_pipe 'false');

   c. Run the following command to import data to the table.

      .. code-block::

         INSERT INTO test_pipe SELECT * FROM foreign_test_pipe;

#. View the result returned by the import statement from the target cluster.

   .. code-block::

      SELECT * FROM test_pipe;
       id | sex |      name
      ----+-----+----------------
        3 | 2   | 11111111111111
        6 | 2   | 11111111111111
        7 | 2   | 11111111111111
        1 | 2   | 11111111111111
        2 | 2   | 11111111111111
        4 | 2   | 11111111111111
        5 | 2   | 11111111111111
        8 | 2   | 11111111111111
        9 | 2   | 11111111111111
      (9 rows)

.. note::

   By default, the pipeline file exported from or imported to GDS is named in the format of *Database name*\ \_\ *Schema name*\ \_\ *Foreign table name* **.pipe**. Therefore, the database name and schema name of the target cluster must be the same as those of the source cluster. If the database or schema is inconsistent, you can specify the same pipe file in the URL of the **location**.

   Example:

   -  Pipe name specified by a write-only foreign table.

      .. code-block::

         CREATE FOREIGN TABLE foreign_test_pipe(id integer not null, age text not null, name  text) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://GDS_IP:GDS_PORT/foreign_test_pipe.pipe', FORMAT 'text', DELIMITER ',',  NULL '', EOL '0x0a' ,file_type 'pipe') WRITE ONLY;

   -  Pipe name specified by a read-only foreign table.

      .. code-block::

         CREATE FOREIGN TABLE foreign_test_pipe(like test_pipe) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://GDS_IP:GDS_PORT/foreign_test_pipe.pipe', FORMAT 'text', DELIMITER ',',  NULL '', EOL '0x0a' ,file_type 'pipe',auto_create_pipe 'false');
