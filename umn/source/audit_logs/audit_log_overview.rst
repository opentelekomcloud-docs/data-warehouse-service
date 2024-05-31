:original_name: dws_01_1911.html

.. _dws_01_1911:

Audit Log Overview
==================

GaussDB(DWS) provides management console audit logs and database audit logs for users to query service logs, analyze problems, and learn product security and performance status.

Management Console Audit Logs
-----------------------------

GaussDB(DWS) uses Cloud Trace Service (CTS) to record mission-critical operations performed on the GaussDB (DWS) management console, such as cluster creation, snapshot creation, cluster scale-out, and cluster restart. The logs can be used in purposes such as security analysis, compliance audit, resource tracing, and fault locating.

For details about how to enable and view management console audit logs, see :ref:`Management Console Audit Logs <dws_01_0118>`.

Database Audit Logs
-------------------

If the **Security** function is enabled, GaussDB(DWS) records any DML and DDL operations performed by the database. You can locate and analyze faults based on the database audit logs, and perform behavior analysis and security auditing on historical database operations to improve GaussDB (DWS) security.

For details about how to enable and view database audit logs, see :ref:`Configuring the Database Audit Logs <dws_01_0075>` and :ref:`Viewing Database Audit Logs <dws_01_0183>`.
