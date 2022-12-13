:original_name: dws_03_0082.html

.. _dws_03_0082:

How Can I View Database Operation Logs?
=======================================

GaussDB(DWS) allows you to record the audit logs of specific operations, involving audit log retention policies, unauthorized access, as well as DML, DDL, **SELECT** and **COPY** operations performed on stored procedures and database objects.

After configuring audit logs, you can query audit information to troubleshoot, or historical operation records for a malfunctioning GaussDB(DWS) cluster.

The audit logs are stored in the database by default. Dump them to OBS to give users who are responsible for monitoring the database easier access.

For details, see "Audit Logs" in the *Management Guide*.
