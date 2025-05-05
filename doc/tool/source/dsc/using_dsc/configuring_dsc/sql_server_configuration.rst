:original_name: dws_07_0186.html

.. _dws_07_0186:

SQL Server Configuration
========================

You can customize the behavior of the migration tool when migrating SQL Server database scripts by setting SQL Server configuration parameters.

Open the **features-mysql.properties** file in the **config** folder. When migrating SQL Server databases, change the data type of the source database to SQL Server and set the parameters listed in :ref:`Table 1 <en-us_topic_0000002160142072__table1669410624015>` as required.

.. _en-us_topic_0000002160142072__table1669410624015:

.. table:: **Table 1** Parameters in the **features-mysql.properties** file

   +----------------------------+---------------------------+------------------------------------+---------------+--------------------------------------+
   | Parameter                  | Description               | Value Range                        | Default Value | Example                              |
   +============================+===========================+====================================+===============+======================================+
   | table.origin.database.type | Source database type      | bigquery,doris,sqlserver,mysql,adb | mysql         | table.origin.database.type=sqlserver |
   +----------------------------+---------------------------+------------------------------------+---------------+--------------------------------------+
   | table.orientation          | Default data storage mode | ROW,COLUMN                         | ROW           | table.orientation=ROW                |
   +----------------------------+---------------------------+------------------------------------+---------------+--------------------------------------+
   | table.type                 | Default table type        | REPLICATION,HASH,ROUND-ROBIN       | HASH          | table.type=ROUND-ROBIN               |
   +----------------------------+---------------------------+------------------------------------+---------------+--------------------------------------+
