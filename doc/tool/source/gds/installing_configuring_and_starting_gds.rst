:original_name: dws_07_0759.html

.. _dws_07_0759:

Installing, Configuring, and Starting GDS
=========================================

Scenario
--------

GaussDB(DWS) uses GDS to allocate the source data for parallel data import. Deploy GDS on the data server.

If a large volume of data is stored on multiple data servers, install, configure, and start GDS on each server. Then, data on all the servers can be imported in parallel. The procedure for installing, configuring, and starting GDS is the same on each data server. This section describes how to perform this procedure on one data server.

Context
-------

#. The GDS version must match the cluster version. For example, GDS V100R008C00 matches DWS 1.3.\ *X*. Otherwise, the import or export may fail, or the import or export process may fail to respond.

   Therefore, use the latest version of GDS. After the database is upgraded, download the latest version of GaussDB(DWS) GDS as instructed in :ref:`Procedure <en-us_topic_0000001503363660__en-us_topic_0000001188163708_se2a774b311864348a516131937610513>`. When the import or export starts, GaussDB(DWS) checks the GDS versions. If the versions do not match, an error message is displayed and the import or export is terminated.

   To obtain the version number of GDS, run the following command in the GDS decompression directory:

   .. code-block::

      gds -V

   To view the database version, run the following SQL statement after connecting to the database:

   ::

      SELECT version();

.. _en-us_topic_0000001503363660__en-us_topic_0000001188163708_se2a774b311864348a516131937610513:

Procedure
---------

#. Before using GDS to import or export data, see **Preparing an ECS as the GDS Server** and **Downloading the GDS Package** in "Tutorial: Using GDS to Import Data from a Remote Server" in *Data Warehouse Service (DWS) User Guide*.

#. .. _en-us_topic_0000001503363660__en-us_topic_0000001188163708_l242cf61617ff44ffbcfa868e55b91d88:

   Log in as user **root** to the data server where GDS is to be installed and run the following command to create the directory for storing the GDS package:

   .. code-block::

      mkdir -p /opt/bin/dws

#. Upload the GDS package to the created directory.

   Use the SUSE Linux package as an example. Upload the GDS package **dws_client_8.**\ *x.x*\ **\_suse_x64.zip** to the directory created in the previous step.

#. .. _en-us_topic_0000001503363660__en-us_topic_0000001188163708_li16883354813:

   (Optional) If SSL is used, upload the SSL certificates to the directory created in :ref:`2 <en-us_topic_0000001503363660__en-us_topic_0000001188163708_l242cf61617ff44ffbcfa868e55b91d88>`.

#. Go to the directory and decompress the package.

   .. code-block::

      cd /opt/bin/dws
      unzip dws_client_8.x.x_suse_x64.zip

#. Create a GDS user and the user group to which the user belongs. This user is used to start GDS and read source data.

   .. code-block::

      groupadd gdsgrp
      useradd -g gdsgrp gds_user

#. Change the owner of the GDS package directory and source data file directory to the GDS user.

   .. code-block::

      chown -R gds_user:gdsgrp /opt/bin/dws/gds
      chown -R gds_user:gdsgrp /input_data

#. Switch to user **gds_user**.

   .. code-block::

      su - gds_user

   If the current cluster version is 8.0.\ *x* or earlier, skip :ref:`9 <en-us_topic_0000001503363660__en-us_topic_0000001188163708_li14421575388>` and go to :ref:`10 <en-us_topic_0000001503363660__en-us_topic_0000001188163708_li22515537437>`.

   If the current cluster version is 8.1.\ *x*, go to the next step.

#. .. _en-us_topic_0000001503363660__en-us_topic_0000001188163708_li14421575388:

   Execute the script on which the environment depends (applicable only to 8.1.\ *x*).

   .. code-block::

      cd /opt/bin/dws/gds/bin
      source gds_env

#. .. _en-us_topic_0000001503363660__en-us_topic_0000001188163708_li22515537437:

   Start GDS.

   GDS is green software and can be started after being decompressed. There are two ways to start GDS. One is to run the **gds** command to configure startup parameters. The other is to write the startup parameters into the **gds.conf** configuration file and run the **gds_ctl.py** command to start GDS.

   The first method is recommended when you do not need to import data again. The second method is recommended when you need to import data regularly.

   -  Method 1: Run the **gds** command to start GDS.

      -  If data is transmitted in non-SSL mode, run the following command to start GDS:

         .. code-block::

            gds -d dir -p ip:port -H address_string -l log_file -D -t worker_num

         Example:

         .. code-block::

            /opt/bin/dws/gds/bin/gds -d /input_data/ -p 192.168.0.90:5000 -H 10.10.0.1/24 -l /opt/bin/dws/gds/gds_log.txt -D -t 2

      -  If data is transmitted in SSL mode, run the following command to start GDS:

         .. code-block::

            gds -d dir -p ip:port -H address_string -l log_file -D
            -t worker_num --enable-ssl --ssl-dir Cert_file

         Example:

         Run the following command to upload the SSL certificate mentioned in :ref:`4 <en-us_topic_0000001503363660__en-us_topic_0000001188163708_li16883354813>` to **/opt/bin**:

         .. code-block::

            /opt/bin/dws/gds/bin/gds -d /input_data/ -p 192.168.0.90:5000 -H 10.10.0.1/24 -l /opt/bin/dws/gds/gds_log.txt -D --enable-ssl --ssl-dir /opt/bin/

      Replace the information in italic as required.

      -  **-d** *dir*: directory for storing data files that contain data to be imported. This tutorial uses **/input_data/** as an example.

      -  **-p** *ip:port*: listening IP address and port for GDS. The default value is **127.0.0.1**. Replace it with the IP address of a 10GE network that can communicate with GaussDB(DWS). The port number ranges from 1024 to 65535. The default port is **8098**. This tutorial uses **192.168.0.90:5000** as an example.

      -  **-H** *address_string*: specifies the hosts that are allowed to connect to and use GDS. The value must be in CIDR format. Configure this parameter to enable a GaussDB(DWS) cluster to access GDS for data import. Ensure that the network segment covers all hosts in a GaussDB(DWS) cluster.

      -  **-l** *log_file*: GDS log directory and log file name. This tutorial uses **/opt/bin/dws/gds/gds_log.txt** as an example.

      -  **-D**: GDS in daemon mode. This parameter is used only in Linux.

      -  **-t** *worker_num*: number of concurrent GDS threads. If the data server and GaussDB(DWS) have available I/O resources, you can increase the number of concurrent GDS threads.

         GDS determines the number of threads based on the number of concurrent import transactions. Even if multi-thread import is configured before GDS startup, the import of a single transaction will not be accelerated. By default, an **INSERT** statement is an import transaction.

      -  **--enable-ssl**: enables SSL for data transmission.

      -  **--ssl-dir** *Cert_file*: SSL certificate directory. Set this parameter to the certificate directory in :ref:`4 <en-us_topic_0000001503363660__en-us_topic_0000001188163708_li16883354813>`.

      -  For details about GDS parameters, see "GDS - Parallel Data Loader > gds" in the *Data Warehouse Service (DWS) Tool Guide*.

   -  Method 2: Write the startup parameters into the **gds.conf** configuration file and run the **gds_ctl.py** command to start GDS.

      a. Run the following command to go to the **config** directory of the GDS package and modify the **gds.conf** configuration file. For details about the parameters in the **gds.conf** configuration file, see :ref:`Table 1 <en-us_topic_0000001503363660__en-us_topic_0000001188163708_t051f8c4ef816412c85e082e7fb7297dd>`.

         .. code-block::

            vim /opt/bin/dws/gds/config/gds.conf

         Example:

         The **gds.conf** configuration file contains the following information:

         .. code-block::

            <?xml version="1.0"?>
            <config>
            <gds name="gds1" ip="192.168.0.90" port="5000" data_dir="/input_data/" err_dir="/err" data_seg="100MB" err_seg="100MB" log_file="/log/gds_log.txt" host="10.10.0.1/24" daemon='true' recursive="true" parallel="32"></gds>
            </config>

         Information in the configuration file is described as follows:

         -  The data server IP address is **192.168.0.90** and the GDS listening port is **5000**.
         -  Data files are stored in the **/input_data/** directory.
         -  Error log files are stored in the **/err** directory. The directory must be created by a user who has the GDS read and write permissions.
         -  The size of a single data file is 100 MB.
         -  The size of a single error log file is 100 MB.
         -  Logs are stored in the **/log/gds_log.txt** file. The directory must be created by a user who has the GDS read and write permissions.
         -  Only nodes with the IP address **10.10.0.**\ ``*`` can be connected.
         -  The GDS process is running in daemon mode.
         -  Recursive data file directories are used.
         -  The number of concurrent import threads is 2.

      b. Start GDS and check whether it has been started.

         .. code-block::

            python3 gds_ctl.py start

         Example:

         .. code-block::

            cd /opt/bin/dws/gds/bin
            python3 gds_ctl.py start
            Start GDS gds1                  [OK]
            gds [options]:
             -d dir            Set data directory.
             -p port           Set GDS listening port.
                ip:port        Set GDS listening ip address and port.
             -l log_file       Set log file.
             -H secure_ip_range
                               Set secure IP checklist in CIDR notation. Required for GDS to start.
             -e dir            Set error log directory.
             -E size           Set size of per error log segment.(0 < size < 1TB)
             -S size           Set size of data segment.(1MB < size < 100TB)
             -t worker_num     Set number of worker thread in multi-thread mode, the upper limit is 200. If without setting, the default value is 8.
             -s status_file    Enable GDS status report.
             -D                Run the GDS as a daemon process.
             -r                Read the working directory recursively.
             -h                Display usage.

gds.conf Parameter Description
------------------------------

.. _en-us_topic_0000001503363660__en-us_topic_0000001188163708_t051f8c4ef816412c85e082e7fb7297dd:

.. table:: **Table 1** gds.conf configuration description

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | Attribute             | Description                                                                                                                           | Value Range                                              |
   +=======================+=======================================================================================================================================+==========================================================+
   | name                  | Identifier                                                                                                                            | ``-``                                                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | ip                    | Listening IP address                                                                                                                  | The IP address must be valid.                            |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | Default value: **127.0.0.1**                             |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | port                  | Listening port                                                                                                                        | Value range: 1024 to 65535 (integer)                     |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | Default value: **8098**                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | data_dir              | Data file directory                                                                                                                   | ``-``                                                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | err_dir               | Error log file directory                                                                                                              | Default value: data file directory                       |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | log_file              | Log file Path                                                                                                                         | ``-``                                                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | host                  | Host IP address allowed to be connected to GDS (The value must in CIDR format and this parameter is available for the Linux OS only.) | ``-``                                                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | recursive             | Whether the data file directories are recursive                                                                                       | Value range:                                             |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | -  **true**: recursive                                   |
   |                       |                                                                                                                                       | -  **false**: not recursive                              |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | Default value: **false**                                 |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | daemon                | Whether the process is running in daemon mode                                                                                         | Value range:                                             |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | -  **true**: The process is running in daemon mode.      |
   |                       |                                                                                                                                       | -  **false**: The process is not running in daemon mode. |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | Default value: **false**                                 |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | parallel              | Number of concurrent data import threads                                                                                              | Value range: 0 to 200 (integer)                          |
   |                       |                                                                                                                                       |                                                          |
   |                       |                                                                                                                                       | Default value: **8**                                     |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
