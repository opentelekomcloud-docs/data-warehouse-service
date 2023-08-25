:original_name: dws_07_0001.html

.. _dws_07_0001:

Overview
========

This document describes how to use GaussDB(DWS) tools, including client tools, as shown in :ref:`Table 1 <en-us_topic_0000001233800667__table138932093610>`, and server tools, as shown in :ref:`Table 2 <en-us_topic_0000001233800667__table1139657195412>`.

The client tools can be obtained by referring to :ref:`Downloading Related Tools <dws_07_0002>`.

The server tools are stored in the **$GPHOME/script** and **$GAUSSHOME/bin** paths on the database server.

.. _en-us_topic_0000001233800667__table138932093610:

.. table:: **Table 1** Client Tools

   +-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Tool        | Description                                                                                                                                                                                                                                                                                   |
   +=============+===============================================================================================================================================================================================================================================================================================+
   | gsql        | A command-line interface (CLI) SQL client tool running on the Linux OS. It is used to connect to the database in a GaussDB(DWS) cluster and perform operation and maintenance on the database.                                                                                                |
   +-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data Studio | A client tool used to connect to a database. It provides a GUI for managing databases and objects, editing, executing, and debugging SQL scripts, and viewing execution plans. Data Studio can run on a 32-bit or 64-bit Windows OS. You can use it after decompression without installation. |
   +-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | GDS         | A CLI tool running on the Linux OS. It works with foreign tables to quickly import and export data. The GDS tool package needs to be installed on the server where the data source file is located. This server is called the data server or the GDS server.                                  |
   +-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DSC         | A CLI tool used for migrating SQL scripts from Teradata or Oracle to GaussDB(DWS) to rebuild a database on GaussDB(DWS). DSC runs on the Linux OS. You can use it after decompression without installation.                                                                                   |
   +-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001233800667__table1139657195412:

.. table:: **Table 2** Server Tools

   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Tool                               | Description                                                                                                                                                                                                                                                                                    |
   +====================================+================================================================================================================================================================================================================================================================================================+
   | :ref:`gs_dump <dws_07_0101>`       | gs_dump exports database information, such as the complete and consistent data of database objects (including databases, schemas, tables, and views), without affecting the normal access of users to the database.                                                                            |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gs_dumpall <dws_07_0102>`    | gs_dumpall exports database information, such as the complete and consistent data of database objects, without affecting the normal access of users to the database.                                                                                                                           |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gs_restore <dws_07_0103>`    | gs_restore is a tool provided by GaussDB(DWS) to import data that is exported using gs_dump. It can also be used to import files that were exported using **gs_dump**.                                                                                                                         |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gds_check <dws_07_0104>`     | gds_check is used to check the GDS deployment environment, including the OS parameters, network environment, and disk usage. It also supports the recovery of system parameters. This helps detect potential problems during GDS deployment and running, improving the execution success rate. |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gds_install <dws_07_0106>`   | gds_install is a script tool used to install GDS in batches, improving GDS deployment efficiency.                                                                                                                                                                                              |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gds_uninstall <dws_07_0107>` | gds_uninstall is a script tool used to uninstall GDS in batches.                                                                                                                                                                                                                               |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gds_ctl <dws_07_0105>`       | gds_ctl is a script tool used for starting or stopping GDS service processes in batches. You can start or stop GDS service processes, which use the same port, on multiple nodes at a time, and set a daemon for each GDS process during the startup.                                          |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`gs_sshexkey <dws_07_0108>`   | During cluster installation, you need to execute commands and transfer files among hosts in the cluster. gs_sshexkey is used to help users establish mutual trust.                                                                                                                             |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
