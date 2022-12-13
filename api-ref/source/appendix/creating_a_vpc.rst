:original_name: dws_02_0040.html

.. _dws_02_0040:

Creating a VPC
==============

Background
----------

Before creating a cluster, you need to create a VPC to provide a secure and isolated network environment for using GaussDB(DWS).

If you have already created a VPC, you do not need to create it again.

.. note::

   For details about how to create a VPC, see **Creating a VPC** in the *Virtual Private Cloud User Guide*.

Procedure
---------

#. Log in to the management console.
#. Under **Network**, click **Virtual Private Cloud**.
#. On the **Virtual Private Cloud** page, click **Create VPC** to create a VPC.
#. Obtain the VPC and subnet ID for subsequent use in :ref:`Creating a Cluster <dws_02_0020>`.
#. On the **Virtual Private Cloud** page, choose **Access Control** > **Security Groups** in the navigation tree on the left, and click **Create Security Group** to create a security group.
#. Obtain the security group ID for subsequent use in :ref:`Creating a Cluster <dws_02_0020>`.
