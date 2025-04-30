:original_name: dws_01_1911.html

.. _dws_01_1911:

Log Types Supported by GaussDB(DWS) Clusters
============================================

GaussDB(DWS) provides database audit logs, management console audit logs, and other logs for users to query service logs, analyze problems, and learn product security and performance.

Database Audit Logs
-------------------

If the **Security** function is enabled, GaussDB(DWS) records any DML and DDL operations performed by the database. You can locate and analyze faults based on the database audit logs, and perform behavior analysis and security auditing on historical database operations to improve GaussDB(DWS) security.

For how to enable and view database audit logs, see :ref:`Viewing GaussDB(DWS) Database Audit Logs <dws_01_0183>`.

Management Console Audit Logs
-----------------------------

GaussDB(DWS) uses Cloud Trace Service (CTS) to record mission-critical operations performed on the GaussDB(DWS) console, such as cluster creation, snapshot creation, cluster scale-out, and cluster restart. The logs can be used in purposes such as security analysis, compliance audit, resource tracing, and fault locating.

For how to enable and view management console audit logs, see :ref:`Viewing Operation Logs on the GaussDB(DWS) Console <dws_01_0118>`.

Other Logs
----------

GaussDB(DWS) interconnects with Log Tank Service (LTS). You can view collected cluster logs or dump logs on the LTS console.

The following log types are supported: CN logs, DN logs, OS messages logs, audit logs, cms logs, gtm logs, Roach client logs, Roach server logs, upgrade logs, and scale-out logs.
