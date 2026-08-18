:original_name: dws_04_1213.html

.. _dws_04_1213:

Accessing Hudi Tables on MRS
============================

SQL on Hudi supports access to hudi tables stored on MRS.

.. note::

   In DWS, a Hudi foreign table that uses the schema evolution feature cannot be read. Schema evolution enables you to modify a Hudi table's schema throughout its lifecycle, ensuring the data remains adaptable to changing requirements. This includes adding, deleting, or modifying fields within the table.

Prerequisites
-------------

You have created an MRS data source. For details, see "MRS Data Sources" in *Data Warehouse Service (DWS) User Guide*.

.. note::

   SQL on Hudi can read hudi tables stored on MRS. The only difference in usage compared to OBS is when creating data sources.

Accessing Multiple MRS Clusters Concurrently
--------------------------------------------

Due to JDK restrictions, one JVM can store only one Kerberos configuration file at a time. As a result, one DWS cluster cannot concurrently access Hudi tables in multiple MRS clusters through SQL on Hudi. However, you can perform the following operations to access Hudi tables in multiple MRS clusters.

#. Obtain the krb5.conf file of each MRS cluster from the downloaded client.

#. Use the krb5.conf file of any MRS cluster as the file to be combined (cluster A for short).

#. Add the KDC domain information of cluster B to **realms** in cluster A's configuration file.

   .. caution::

      In practice, you need to combine the actual KDC domain information in **realms** or **domain_realm** of the cluster.

   Example:

   .. code-block::

      [realms]
      CLUSTER.A.COM = {
      admin_server =  ClusterA_SERVER_IP:PORT
      kdc = ClusterA_KDC_IP:PORT
      kdc = ClusterA_KDC_IP:PORT
      }
      CLUSTER.B.COM = {
      admin_server = ClusterB_SERVER_IP:PORT
      kdc = ClusterB_KDC_IP:PORT
      kdc = ClusterB_KDC_IP:PORT
      }

#. Add the domain information of cluster B to **domain_realm** in cluster A's configuration file.

   Example:

   .. code-block::

      [domain_realm]
      .cluster.a.com = CLUSTER.A.COM
      .cluster.b.com = CLUSTER.B.COM

#. Replace the original **krb5.conf** file with the combined one in the original path of each node in each cluster.
