:original_name: dws_01_0003.html

.. _dws_01_0003:

Related Services
================

ECS
---

DWS uses an ECS as a cluster node.

VPC
---

DWS uses the Virtual Private Cloud (VPC) service to provide a network topology for clusters to isolate clusters and control access.

OBS
---

DWS uses OBS to convert cluster data and external data, satisfying the requirements for secure, reliable, and cost-effective storage.

MRS
---

Data can be migrated from MRS to DWS clusters for analysis after the data is processed by Hadoop.

Cloud Eye
---------

DWS uses Cloud Eye to monitor cluster performance metrics, delivering status information in a concise and efficient manner. Cloud Eye supports alarm customization so that you are notified of the exception instantly.

CTS
---

DWS uses Cloud Trace Service (CTS) to audit your non-query operations on the management console to ensure that no invalid or unauthorized operations are performed, enhancing service security management.

LTS
---

DWS users can view collected cluster logs or dump logs on the Log Tank Service (LTS) console.

TMS
---

With Tag Management Service (TMS), DWS can provide centralized tag management and resource classification functions across regions and services. You can customize tags to classify and locate resources.

DNS
---

DWS uses Domain Name Service (DNS) to provide the cluster IP addresses mapped from domain names.

ELB
---

With Elastic Load Balance (ELB) health checks, the CN requests of a cluster can be quickly forwarded to normal CNs. If a CN is faulty, the workload can be immediately shifted to a healthy node, minimizing cluster access faults.
