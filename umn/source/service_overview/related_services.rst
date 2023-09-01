:original_name: dws_01_0003.html

.. _dws_01_0003:

Related Services
================

IAM
---

GaussDB(DWS) uses Identity and Access Management (IAM) for authentication and authorization.

Users who have the **DWS Administrator** permissions can fully utilize GaussDB(DWS). To obtain the permissions, contact a user with the **Security Administrator** permissions or directly create a user with the **DWS Administrator** permissions. Users granted the **DWS Database Access** permissions can generate temporary database user credentials based on IAM users to connect to databases in the data warehouse clusters.

ECS
---

GaussDB(DWS) uses an ECS as a cluster node.

VPC
---

GaussDB(DWS) uses the Virtual Private Cloud (VPC) service to provide a network topology for clusters to isolate clusters and control access.

OBS
---

GaussDB(DWS) uses OBS to convert cluster data and external data, satisfying the requirements for secure, reliable, and cost-effective storage.

MRS
---

Data can be migrated from MRS to GaussDB(DWS) clusters for analysis after the data is processed by Hadoop.

DRS
---

You can use Data Replication Service (DRS) to synchronize stream data to GaussDB(DWS) in real time.

Cloud Eye
---------

GaussDB(DWS) uses Cloud Eye to monitor cluster performance metrics, delivering status information in a concise and efficient manner. Cloud Eye supports alarm customization so that you are notified of the exception instantly.

CTS
---

GaussDB(DWS) uses Cloud Trace Service (CTS) to audit your non-query operations on the management console to ensure that no invalid or unauthorized operations are performed, enhancing service security management.

LTS
---

GaussDB(DWS) users can view collected cluster logs or dump logs on the Log Tank Service (LTS) console.

TMS
---

With Tag Management Service (TMS), GaussDB(DWS) can provide centralized tag management and resource classification functions across regions and services. You can customize tags to classify and locate resources.

DNS
---

GaussDB(DWS) uses Domain Name Service (DNS) to provide the cluster IP addresses mapped from domain names.

ELB
---

With Elastic Load Balance (ELB) health checks, the CN requests of a cluster can be quickly forwarded to normal CNs. If a CN is faulty, the workload can be immediately shifted to a healthy node, minimizing cluster access faults.
