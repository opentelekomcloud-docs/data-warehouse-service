:original_name: dws_03_0022.html

.. _dws_03_0022:

Is Data Secure in GaussDB(DWS)?
===============================

Yes. In the big data era, data has become a core asset. The public cloud will adhere to the commitment made over the years that we do not touch your applications or data, helping you protect your core assets. This is our commitment to users and the society, laying the foundation for the business success of the public cloud and their partners.

GaussDB(DWS) is a data warehousing system with telecom-class security to safeguard your data and privacy. Moreover, the public cloud GaussDB(DWS) delivers carrier-class quality, which can satisfy data security and privacy requirements of governments, financial organizations, and carriers. Therefore, it is widely used by various industries. GaussDB(DWS) of the public cloud won the following security authentication:

-  Internal Cyber Security Lab (ICSL) in compliance with cyber security standards issued by the UK authorities.
-  Privacy and Security Assessment (PSA) to meet EU requirements of data security and privacy.

Service Data Security
---------------------

GaussDB(DWS) is built on public cloud software infrastructure, including ECS and OBS.

Service data of GaussDB(DWS) users is stored in the ECSs in the cluster. Neither users nor public cloud O&M administrators can log in to these ECSs.

ECSs have their operating systems hardened through various measures such as kernel hardening, patch updates, access controls, port management, and protocol and port attack defense.

GaussDB(DWS) provides comprehensive security measures, such as password policies, authentication, session management, user permissions management, and database auditing.

Snapshot Data Security
----------------------

GaussDB(DWS) stores its backup data in OBS as snapshots. OBS supports access permission control, key access, and data encryption features. GaussDB(DWS) snapshots can be used for data backup and restoration only and cannot be accessed by any user. The GaussDB (DWS) system administrator can view the OBS storage space occupied by snapshots on the GaussDB(DWS) console and through and public cloud bills.

Network Access Security
-----------------------

The L2 and L3 networks of GaussDB(DWS) can be fully isolated to meet the security requirements of government and financial customers.

-  GaussDB(DWS) is deployed in a dedicated ECS environment, which is not shared with any other tenant. This eliminates the possibility of data leakage caused by computing resource sharing.
-  ECSs in a GaussDB(DWS) cluster are isolated through VPCs, preventing the ECSs from being discovered and accessed by other tenants.
-  The network is divided into the service plane and management plane. The two planes are physically isolated, ensuring network security.
-  The tenants can flexibly customize the security group and access rules.

-  External application software access GaussDB(DWS) over SSL.
-  Data imported from OBS is encrypted.
