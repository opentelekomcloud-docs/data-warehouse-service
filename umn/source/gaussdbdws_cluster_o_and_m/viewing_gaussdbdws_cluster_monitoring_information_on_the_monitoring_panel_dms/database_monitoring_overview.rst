:original_name: dws_01_00131.html

.. _dws_01_00131:

Database Monitoring Overview
============================

Overview
--------

DMS is provided by GaussDB(DWS) to ensure the fast and stable running of databases. It collects, monitors, and analyzes the disk, network, and OS metric data used by the service database, as well as key performance metric data of cluster running. It also diagnoses database hosts, instances, and service SQL statements based on the collected metrics to expose key faults and performance problems in a database in a timely manner, and guides customers to optimize and resolve the problems.

.. note::

   -  The database monitoring function supports only cluster versions 8.1.1.200 and later.
   -  The database monitoring function and Cloud Eye monitor different data sources. In database monitoring, the size of a database is the total disk space used by the database, including the space occupied due to bloating.

Entering the Database Monitoring Page
-------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.
#. In the **Operation** column of the target cluster, choose **Monitoring Panel**. The database monitoring page is displayed.
