:original_name: dws_07_0692.html

.. _dws_07_0692:

Example of Importing Data Using GDS
===================================

Example: Parallel Import from Multiple Data Servers
---------------------------------------------------

The data servers reside on the same intranet as the cluster. Their IP addresses are 192.168.0.90 and 192.168.0.91. Source data files are in CSV format.

#. Create the target table **tpcds.reasons**.

   ::

      CREATE TABLE tpcds.reasons
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      );

#. Log in to each GDS data server as user **root** and create the **/input_data** directory for storing data files on the servers. The following takes the data server whose IP address is 192.168.0.90 as an example. Operations on the other server are the same.

   .. code-block::

      mkdir -p /input_data

#. (Optional) Create a user and the user group it belongs to. The user is used to start GDS. If the user and user group already exist, skip this step.

   .. code-block::

      groupadd gdsgrp
      useradd -g gdsgrp gds_user

#. Evenly distribute source data files to the **/input_data** directories on the data servers.

#. Change the owners of source data files and the **/input_data** directory on each data server to **gds_user**. The data server with the IP address 192.168.0.90 is used as an example.

   .. code-block::

      chown -R gds_user:gdsgrp /input_data

#. Log in to each data server as user **gds_user** and start GDS.

   The GDS installation path is **/opt/bin/dws/gds**. Source data files are stored in **/input_data/**. The IP addresses of the data servers are 192.168.0.90 and 192.168.0.91. The GDS listening port is 5000. GDS runs in daemon mode.

   Start GDS on the data server whose IP address is 192.168.0.90.

   .. code-block::

      /opt/bin/dws/gds/gds -d /input_data -p 192.168.0.90:5000 -H 10.10.0.1/24 -D

   Start GDS on the data server whose IP address is 192.168.0.91.

   .. code-block::

      /opt/bin/dws/gds/gds -d /input_data -p 192.168.0.91:5000 -H 10.10.0.1/24  -D

#. Create the foreign table **tpcds.foreign_tpcds_reasons** for the source data.

   Set import mode parameters as follows:

   -  Set the import mode to **Normal**.
   -  When GDS is started, the source data file directory is **/input_data** and the GDS listening port is 5000. Therefore, set **location** to **gsfs://192.168.0.90:5000/\* \| gsfs://192.168.0.91:5000/\***.

   Information about the data format is set based on data format parameters specified during data export. The parameter settings are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  **quote** is set to **0x1b**.
   -  **null** is set to an empty string without quotation marks.
   -  **escape** is set to the same value as that of **quote** by default.
   -  **header** is set to **false**, indicating that the first row is regarded as a data row when a file is imported.

   Set import error tolerance parameters as follows:

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

#. Query data import errors in the **err_tpcds_reasons** table and rectify the errors (if any). For details, see :ref:`Handling Import Errors <dws_07_0056>`.

   ::

      SELECT * FROM err_tpcds_reasons;

#. After data import is complete, log in to each data server as user **gds_user** and stop GDS.

   The data server with the IP address 192.168.0.90 is used as an example. The GDS process ID is 128954.

   .. code-block::

      ps -ef|grep gds
      gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /input_data -p 192.168.0.90:5000 -D
      gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds
      kill -9 128954

Example: Data Import Using Multiple Threads
-------------------------------------------

The data server resides on the same intranet as the cluster. The server IP address is 192.168.0.90. Source data files are in CSV format. Data will be imported to two tables using multiple threads in **Normal** mode.

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

   The GDS installation path is **/gds**. Source data files are stored in **/input_data/**. The IP address of the data server is 192.168.0.90. The GDS listening port is 5000. GDS runs in daemon mode. The degree of parallelism is 2. A recursive directory is specified.

   .. code-block::

      /gds/gds -d /input_data -p 192.168.0.90:5000 -H 10.10.0.1/24  -D -t 2 -r

#. In the database, create the foreign tables **tpcds.foreign_tpcds_reasons1** and **tpcds.foreign_tpcds_reasons2** for the source data.

   The foreign table **tpcds.foreign_tpcds_reasons1** is used as an example to describe how to configure parameters in a foreign table.

   Set import mode parameters as follows:

   -  Set the import mode to **Normal**.
   -  When GDS is started, the configured source data file directory is **/input_data** and the GDS listening port is 5000. However, source data files are actually stored in **/input_data/import1/**. Therefore, set **location** to **gsfs://192.168.0.90:5000/import1/\***.

   Information about the data format is set based on data format parameters specified during data export. The parameter settings are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  **quote** is set to **0x1b**.
   -  **null** is set to an empty string without quotation marks.
   -  **escape** is set to the same value as that of **quote** by default.
   -  **header** is set to **false**, indicating that the first row is regarded as a data row when a file is imported.

   Set import error tolerance parameters as follows:

   -  Set **PER NODE REJECT LIMIT** (number of allowed data format errors) to **unlimited**. In this case, all the data format errors detected during data import will be tolerated.
   -  Set **LOG INTO** to **err_tpcds_reasons1**. The data format errors detected during data import will be recorded in the **err_tpcds_reasons1** table.
   -  If the last column (**fill_missing_fields**) in a source data file is missing, the **NULL** column will be automatically added to the target file.

   Based on the preceding settings, the foreign table **tpcds.foreign_tpcds_reasons1** is created as follows:

   ::

      CREATE FOREIGN TABLE tpcds.foreign_tpcds_reasons1
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/import1/*', format 'CSV',mode 'Normal', encoding 'utf8', delimiter E'\x08', quote E'\x1b', null '',fill_missing_fields 'on')LOG INTO err_tpcds_reasons1 PER NODE REJECT LIMIT 'unlimited';

   Based on the preceding settings, the foreign table **tpcds.foreign_tpcds_reasons2** is created as follows:

   ::

      CREATE FOREIGN TABLE tpcds.foreign_tpcds_reasons2
      (
        r_reason_sk integer not null,
        r_reason_id char(16) not null,
        r_reason_desc char(100)
      ) SERVER gsmpp_server OPTIONS (location 'gsfs://192.168.0.90:5000/import2/*', format 'CSV',mode 'Normal', encoding 'utf8', delimiter E'\x08', quote E'\x1b', null '',fill_missing_fields 'on')LOG INTO err_tpcds_reasons2 PER NODE REJECT LIMIT 'unlimited';

#. Import data to **tpcds.reasons1** using the foreign table **tpcds.foreign_tpcds_reasons1**, and to **tpcds.reasons2** using the foreign table **tpcds.foreign_tpcds_reasons2**.

   ::

      INSERT INTO tpcds.reasons1 SELECT * FROM tpcds.foreign_tpcds_reasons1;

   ::

      INSERT INTO tpcds.reasons2 SELECT * FROM tpcds.foreign_tpcds_reasons2;

#. Query data import errors in the **err_tpcds_reasons1** and **err_tpcds_reasons2** tables and rectify the errors (if any). For details, see :ref:`Handling Import Errors <dws_07_0056>`.

   ::

      SELECT * FROM err_tpcds_reasons1;
      SELECT * FROM err_tpcds_reasons2;

#. After data import is complete, log in to the data server as user **gds_user** and stop GDS.

   The GDS process ID is 128954.

   .. code-block::

      ps -ef|grep gds
      gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /input_data -p 192.168.0.90:5000 -D -t 2 -r
      gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds
      kill -9 128954
