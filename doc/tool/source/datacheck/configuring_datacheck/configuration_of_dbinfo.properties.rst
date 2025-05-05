:original_name: dws_07_0207.html

.. _dws_07_0207:

Configuration of dbinfo.properties
==================================

The **dbinfo.properties** file contains a series of application configuration parameters, which are used to connect the source database and the destination GaussDB(DWS) database. The parameters in this file are general parameters.

Perform the following steps to configure the parameters:

#. Open the **dbinfo.properties** file in the **conf** folder.

#. Change the values of the parameters in the **dbinfo.properties** file according to the actual situation.

   For the description of the parameters in the **dbinfo.properties** file, see :ref:`Table 1 <en-us_topic_0000001813598816__en-us_topic_0000001382367698_table60771938143352>`.

   .. note::

      -  Parameter values are case-insensitive.
      -  Do not modify any other parameter except the listed ones.

#. Save the configuration and exit.

.. _en-us_topic_0000001813598816__en-us_topic_0000001382367698_table60771938143352:

.. table:: **Table 1** Parameters in the dbinfo.properties file

   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | Parameter              | Description                                         | Value Range | Default Value | Example                   |
   +========================+=====================================================+=============+===============+===========================+
   | src.dbtype             | Source database type                                | -  mysql    | MySQL         | src.dbtype =mysql         |
   |                        |                                                     | -  pg       |               |                           |
   |                        |                                                     | -  dws_src  |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | src.dbname             | Source database name                                | N/A         | sys           | src.dbname=sys            |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | src.ip                 | IP address of the source database                   | N/A         | N/A           | src.ip=100.xx.xx.47       |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | src.port               | Source database port                                | N/A         | 3306          | src.port=3306             |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | src.username           | Source database username                            | N/A         | root          | src.username=root         |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | src.passwd             | Source database password                            | N/A         | N/A           | src.passwd=123456         |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | dws.dbtype             | Database type of the GaussDB(DWS) destination end   | dws         | dws           | dws.dbtype=dws            |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | dws.dbname             | Name of the GaussDB(DWS) destination end            | N/A         | gaussdb       | dws.dbname=gaussdb        |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | dws.ip                 | IP address of the GaussDB(DWS) destination end      | N/A         | N/A           | dws.ip=100.xx.xx.186      |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | dws.port               | Port of the GaussDB(DWS) destination end            | N/A         | 8000          | dws.port=8000             |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | dws.username           | Username of the GaussDB(DWS) destination end        | N/A         | dbadmin       | dws.username=dbadmin      |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | dws.passwd             | Password of the GaussDB(DWS) destination end        | N/A         | N/A           | dws.passwd=123456         |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.sum.switch      | Value check sum function switch                     | -  on       | on            | config.sum.switch=on      |
   |                        |                                                     | -  off      |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.avg.switch      | Average value function switch for value check       | -  on       | on            | config.avg.switch=on      |
   |                        |                                                     | -  off      |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.data.min.switch | Minimum value function switch for value check       | -  on       | on            | config.data.min.switch=on |
   |                        |                                                     | -  off      |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.data.max.switch | Maximum value function switch for value check       | -  on       | on            | config.data.max.switch=on |
   |                        |                                                     | -  off      |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.date.min.switch | Minimum value function switch for date check        | -  on       | on            | config.date.min.switch=on |
   |                        |                                                     | -  off      |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.date.max.switch | Maximum value function switch for date check        | -  on       | on            | config.date.max.switch=on |
   |                        |                                                     | -  off      |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.collate.switch  | Indicates whether to enable a collate rule.         | -  on       | on            | config.collate.switch=on  |
   |                        |                                                     | -  off      |               |                           |
   |                        | **on**: enable; **off**: disable.                   |             |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
   | config.dws.collate     | Collate rule value: **C**, **zh_CN**, or **en_US**. | -  C        | C             | config.dws.collate=C      |
   |                        |                                                     | -  zh_CN    |               |                           |
   |                        |                                                     | -  en_US    |               |                           |
   +------------------------+-----------------------------------------------------+-------------+---------------+---------------------------+
