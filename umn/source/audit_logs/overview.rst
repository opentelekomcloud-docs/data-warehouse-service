:original_name: dws_01_1911.html

.. _dws_01_1911:

Overview
========

-  Tenant database audit logs:

   GaussDB(DWS) allows you to record the audit logs of specific operations, involving audit log retention policies, unauthorized access, as well as DML, DDL, **SELECT** and **COPY** operations performed on the stored procedures and database objects.

   After configuring audit logs, you can query audit information to troubleshoot, or historical operation records for a malfunctioning GaussDB(DWS) cluster.

   For details about how to view the database audit logs, see "Database Security Management > Querying Audit Results" in the *Data Warehouse Service (DWS) Developer Guide*.

   .. important::

      -  You can configure database audit logs on the **Security Settings** page of the cluster. For details, see :ref:`Configuring the Database Audit Logs <dws_01_0075>`.
      -  Enabling audit logs will affect performance. The degree of impact depends on the number of audit items enabled.

-  Management console audit logs:

   GaussDB(DWS) allows you to record key operation events of the management console, such as creating a cluster, creating a snapshot, and restarting a cluster. The logs can be used in common application scenarios such as security analysis, compliance audit, resource tracing, and fault locating.

   For details about how to view the audit information on the management console, see :ref:`Viewing Audit Logs of Key Operations on the Management Console <dws_01_0118>`.
