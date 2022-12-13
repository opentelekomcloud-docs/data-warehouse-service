:original_name: dws_01_0128.html

.. _dws_01_0128:

Preparing an ECS as the gsql Client Host
========================================

The gsql command line client provided by GaussDB(DWS) runs on the Linux OS. Before using it to remotely connect to a GaussDB(DWS) cluster, you need to prepare a Linux host for installing and running the gsql client. If you use a public network address to access the cluster, you can install the gsql client on your own Linux host. Ensure that the Linux host has a public network address. For your convenience, you are advised to create a Linux ECS. This section describes how to prepare an ECS. If you already have a qualified ECS, skip this section.

Preparing an ECS
----------------

For details about how to create an ECS, see "Getting Started > Creating an ECS" in the *Elastic Cloud Server User Guide*.

The created ECS must meet the following requirements:

-  The ECS and data warehouse cluster must belong to the same region and AZ.

-  If you use the gsql client provided by GaussDB(DWS) to connect to the GaussDB(DWS) cluster, the ECS image must meet the following requirements:

   There is no special requirement for the image's specifications. The image's OS must be one of the following Linux OSs supported by the gsql client:

   -  The **Redhat x86_64** client can be used on the following OSs:

      -  RHEL 6.4 to RHEL 7.6
      -  CentOS 6.4 to CentOS 7.4
      -  EulerOS 2.3

   -  The **SUSE x86_64** client can be used on the following OSs:

      -  SLES 11.1 to SLES 11.4
      -  SLES 12.0 to SLES 12.3

-  If the client accesses the cluster using the private network address, ensure that the created ECS is in the same VPC as the GaussDB(DWS) cluster.

   For details about VPC operations, see "VPC and Subnet" in the *Virtual Private Cloud User Guide*.

-  If the client accesses the cluster using the public network address, ensure that both the created ECS and GaussDB(DWS) cluster have an EIP.

   When creating an ECS, set **EIP** to **Automatically assign** or **Specify**.

-  The security group rules of the ECS must enable communication between the ECS and the port that the GaussDB(DWS) cluster uses to provide services.

   For details about security group operations, see "Security Group" in the *Virtual Private Cloud User Guide*.

   Ensure that the security group of the ECS contains rules meeting the following requirements. If the rules do not exist, add them to the security group:

   -  **Transfer Direction**: **Outbound**
   -  **Protocol/Application**: The value must contain **TCP**, for example, **TCP** and **All**.
   -  **Port**: The value must contain the database port that provides services in the GaussDB(DWS) cluster. For example, set this parameter to **1-65535** or a specific GaussDB(DWS) database port.
   -  **Destination:** The IP address set here must contain the IP address of the cluster to be connected. For example, set this parameter to **0.0.0.0/0** or the specific connection address of the GaussDB(DWS) cluster.

-  The security group rules of the GaussDB(DWS) cluster must ensure that GaussDB(DWS) can receive network access requests from clients.

   Ensure that the cluster's security group contains rules meeting the following requirements. If the rules do not exist, add them to the security group:

   -  **Transfer Direction**: **Inbound**
   -  **Protocol/Application**: The value must contain **TCP**, for example, **TCP** and **All**.
   -  **Port**: Set this parameter to the database port that provides services in the GaussDB(DWS) cluster, for example, **8000**.
   -  **Source**: The IP address set here must contain the IP address of the GaussDB(DWS) client host, for example, **192.168.0.10/32**.
