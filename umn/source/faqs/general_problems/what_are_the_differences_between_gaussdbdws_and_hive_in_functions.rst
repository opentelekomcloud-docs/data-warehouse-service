:original_name: dws_03_0037.html

.. _dws_03_0037:

What Are the Differences Between GaussDB(DWS) and Hive in Functions?
====================================================================

GaussDB(DWS) and Hive have different functions in the following aspects:

#. Hive is a data warehouse based on Hadoop MapReduce. GaussDB(DWS) is a data warehouse based on Postgres MPP.
#. Hive data is stored on HDFS. GaussDB(DWS) data can be stored locally or on OBS in foreign table form.
#. Hive does not support indexes. GaussDB(DWS) supports indexes, so querying is faster.
#. Hive does not support stored procedures. GaussDB(DWS) does, so it has more extensive application scenarios.
#. Hive supports fewer SQL statements than GaussDB(DWS), including functions, customized functions, and stored procedures.
#. Hive does not support transactions. GaussDB(DWS) supports complete transactions.
#. Both Hive and GaussDB(DWS) support backups, so the reliability is the same.
#. GaussDB(DWS) delivers much better performance than Hive.

Based on their respective functions, Hive is useful for offline analysis while GaussDB(DWS) is useful for both online analysis and ad-hoc query.
