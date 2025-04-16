:original_name: dws_16_0027.html

.. _dws_16_0027:

.. _en-us_topic_0000001860318449:

Teradata Perl Migration
=======================

Overview
--------

This section describes how to migrate Teradata Perl files.

Run the **runDSC.sh** or **runDSC.bat** command and set **--application-lang** to **perl** to migrate Teradata **BTEQ** or **SQL_LANG** scripts in Perl files to Perl-compatible GaussDB(DWS). After migrating Perl files, you can verify the migration by comparing the output file with its input file using a comparison tool.

The process of migrating Perl files is as follows:

#. Complete the steps in section :ref:`Prerequisites <en-us_topic_0000001860198829>`.
#. Create an input folder and copy the Perl files to be migrated to the folder. For example, create a **/migrationfiles/perlfiles** folder.
#. Execute DSC to migrate Perl scripts and set :ref:`db-bteq-tag-name <en-us_topic_0000001813439212__en-us_topic_0000001443528629_li147773406162>` to **BTEQ** or :ref:`db-tdsql-tag-name <en-us_topic_0000001813439212__en-us_topic_0000001443528629_li132693018413>` to **SQL_LANG**.

   a. The DSC extracts the **BTEQ** or **SQL_LANG** scripts from the Perl files.

      #. BTEQ is the tag name, which contains a set of BTEQ scripts. This tag name is configurable using the **db-bteq-tag-name** configuration parameter in **perl-migration.properties** file.
      #. SQL_LANG is another tag name, which contains Teradata SQL statements. This is also configurable using the **db-tdsql-tag-name** configuration parameter.

   b. DSC invokes the Teradata SQL to migrate the extracted SQL scripts. For details about Teradata SQL migration, see :ref:`Teradata SQL Migration <en-us_topic_0000001860198977>`.
   c. Perl files are embedded in the migrated scripts.

#. DSC creates the migrated files in the specified output folder. If no output folder is specified, DSC creates an output folder named **converted** in the input folder, for example, **/migrationfiles/perlfiles/converted**.

   .. note::

      -  Perl variables containing SQL statements can also be migrated to SQL by setting the :ref:`migrate-variables <en-us_topic_0000001813439212__en-us_topic_0000001443528629_li1148411265916>` parameter.
      -  For perl v 5.10.0 and later are compatible.


Teradata Perl Migration
-----------------------

To migrate Perl files, execute DSC with **--source-db Teradata** and **--application-lang Perl** parameter values. DSC supports the migration of **BTEQ** and **SQL_LANG** scripts. You can specify the scripts to be migrated by setting :ref:`db-bteq-tag-name <en-us_topic_0000001813439212__en-us_topic_0000001443528629_li147773406162>` or :ref:`db-tdsql-tag-name <en-us_topic_0000001813439212__en-us_topic_0000001443528629_li132693018413>`.

Run the following commands to set the source database, input and output folder paths, log paths, and application language.

**Linux**:

.. code-block::

   ./runDSC.sh
   --source-db|-S Teradata
   [--application-lang|-A Perl]
   [--input-folder|-I <input-script-path>]
   [--output-folder|-O <output-script-path>]
   [--conversion-type|-M <Bulk or BLogic>]
   [--log-folder|-L <log-path>]

**Windows**:

.. code-block::

   runDSC.bat
   --source-db|-S Teradata
   [--application-lang|-A Perl]
   [--input-folder|-I <input-script-path>]
   [--output-folder|-O <output-script-path>]
   [--conversion-type|-M <Bulk or BLogic>]
   [--log-folder|-L <log-path>]

For example:

.. code-block::

   ./runDSC.sh --input-folder /opt/DSC/DSC/input/teradata_perl/ --output-folder /opt/DSC/DSC/output/ --source-db teradata --conversion-type Bulk --application-lang PERL

During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console.

.. code-block::

   ********************** Schema Conversion Started *************************
   DSC process start time : Mon Jan 20 17:24:49 IST 2020
   Statement count progress 100% completed [FILE(1/1)]
   Schema Conversion Progress 100% completed
   **************************************************************************
   Total number of files in input folder : 1
   **************************************************************************
   Log file path :....../DSC/DSC/log/dsc.log
   DSC process end time : Mon Jan 20 17:24:49 IST 2020
   DSC total process time : 0 seconds
    ********************* Schema Conversion Completed ************************

For details about the parameters for Teradata Perl migration, see :ref:`Teradata Perl Configuration <en-us_topic_0000001813439212>`.

For details about CLI parameters, see :ref:`Database Schema Conversion <en-us_topic_0000001813438760>`.

.. note::

   -  DSC formats the input files and saves them in the output folder. You can compare the formatted input files with the output files.

   -  Ensure that there are no spaces in the input path. If there is a space, DSC throws an error. For details, see :ref:`Troubleshooting <en-us_topic_0000001813598928>`.

   -  For details about logs, see :ref:`Log Reference <en-us_topic_0000001813598960>`.

   -  If the output folder contains subfolders or files, DSC deletes the subfolders and files or overwrites them based on parameter settings in the **application.properties** configuration file in the **config** folder before the migration. Deleted or overwritten subfolders and files cannot be restored by DSC.

   -  **Process start time** indicates the migration start time and **Process end time** indicates the migration end time. **Process total time** indicates the total migration duration, in milliseconds. In addition, the total number of migrated files, total number of processors, number of used processors, log file path, and error log file path are also displayed on the console.

   -  Set **--add-timing-on** to **true** in the **perl-migration.properties** file to add a custom script to calculate statement execution time.

      Example:

      **Input**

      ::

         $V_SQL2 = "SELECT T1.userTypeInd FROM T07_EBM_CAMP T1  WHERE T1.Camp_List_Id = '$abc'";
         $STH = $dbh->prepare($V_SQL2);
         $sth->execute();
         @rows = $sth->fetchrow();

      **Output**

      .. code-block::

         $V_SQL2 = "SELECT T1.userTypeInd FROM T07_EBM_CAMP T1  WHERE T1.Camp_List_Id = '$abc'";
         $STH = $dbh->prepare($V_SQL2);
         use Time::HiRes qw/gettimeofday/;
         my $start = [Time::HiRes::gettimeofday()];
         $sth->execute();
         my $elapsed = Time::HiRes::tv_interval($start);
         $elapsed = $elapsed * 1000;
         printf("Time: %.3f ms\n", $elapsed);
         @rows = $sth->fetchrow();

   -  GROUP and OTHERS must not have write permission for the files or folders specified by\ **--input-folder**. That is, the privilege for the folder specified by **--input-folder** must not be higher than **755**. For security purposes, DSC will not be executed if the input files or folders have the write permission.

   -  If migration tasks are executed concurrently, the input folder must be unique for each task.

Best Practices
--------------

To optimize the migration, you are advised to follow the standard practices:

-  **BTEQ** scripts must be in the following format:

   .. code-block::

      print BTEQ <<ENDOFINPUT;
      TRUNCATE TABLE employee;
      ENDOFINPUT
      close(BTEQ);

-  **SQL_LANG** scripts must be in the following format:

   ::

      my $sSQL=<<SQL_LANG;
      TRUNCATE TABLE employee;
      SQL_LANG

-  Comment must not contain the following information:

   -  print BTEQ <<ENDOFINPUT
   -  ENDOFINPUT
   -  close(BTEQ)
   -  my $sSQL=<<SQL_LANG
   -  SQL_LANG
