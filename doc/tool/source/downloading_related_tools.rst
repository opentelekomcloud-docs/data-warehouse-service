:original_name: dws_07_0002.html

.. _dws_07_0002:

Downloading Related Tools
=========================

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. In the navigation tree on the left, choose **Management** > **Client Connections**.

#. In the **Download Client and Driver** area, select the tools corresponding to the cluster version based on the computer OS and download them.

   You can download the following tools:

   -  CLI client: The gsql tool package contains the gsql client tool, GDS (parallel data loading tool), **gs_dump**, **gs_dumpall**, and **gs_restore** tools.
   -  Data Studio GUI client
   -  DSC migration tool

   The gsql and Data Studio client tools have multiple historical versions. You can click **Historical Version** to download the tools based on the cluster version. GaussDB(DWS) clusters are compatible with earlier versions of gsql and Data Studio tools. You are advised to download the matching tool version based on the cluster version.


   .. figure:: /_static/images/en-us_image_0000002232440573.png
      :alt: **Figure 1** Downloading the client

      **Figure 1** Downloading the client

GaussDB(DWS) also supports the open source ODBC driver: PostgreSQL ODBC 09.01.0200 or later.

.. table:: **Table 1** ODBC driver download links

   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | OS            | Download Link                                                                                                                      | Verification File                                                                                                                                |
   +===============+====================================================================================================================================+==================================================================================================================================================+
   | Windows       | `dws_9.1.x_odbc_driver_for_windows.zip <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_windows.zip>`__       | `dws_9.1.x_odbc_driver_for_windows.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_windows.zip.sha256>`__       |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.2.x_odbc_driver_for_windows.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_windows.zip>`__       | `dws_8.2.x_odbc_driver_for_windows.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_windows.zip.sha256>`__       |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.1.x_odbc_driver_for_windows.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_windows.zip>`__       | `dws_8.1.x_odbc_driver_for_windows.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_windows.zip.sha256>`__       |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | EulerOS Arm   | `dws_9.1.x_odbc_driver_for_arm_euler.zip <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_arm_euler.zip>`__   | `dws_9.1.x_odbc_driver_for_arm_euler.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_arm_euler.zip.sha256>`__   |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.2.x_odbc_driver_for_arm_euler.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_arm_euler.zip>`__   | `dws_8.2.x_odbc_driver_for_arm_euler.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_arm_euler.zip.sha256>`__   |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.1.x_odbc_driver_for_arm_euler.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_arm_euler.zip>`__   | `dws_8.1.x_odbc_driver_for_arm_euler.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_arm_euler.zip.sha256>`__   |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Red Hat Arm   | `dws_9.1.x_odbc_driver_for_arm_redhat.zip <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_arm_redhat.zip>`__ | `dws_9.1.x_odbc_driver_for_arm_redhat.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_arm_redhat.zip.sha256>`__ |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.2.x_odbc_driver_for_arm_redhat.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_arm_redhat.zip>`__ | `dws_8.2.x_odbc_driver_for_arm_redhat.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_arm_redhat.zip.sha256>`__ |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.1.x_odbc_driver_for_arm_redhat.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_arm_redhat.zip>`__ | `dws_8.1.x_odbc_driver_for_arm_redhat.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_arm_redhat.zip.sha256>`__ |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Redhat x86_64 | `dws_9.1.x_odbc_driver_for_x86_redhat.zip <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_x86_redhat.zip>`__ | `dws_9.1.x_odbc_driver_for_x86_redhat.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_x86_redhat.zip.sha256>`__ |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.2.x_odbc_driver_for_x86_redhat.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_x86_redhat.zip>`__ | `dws_8.2.x_odbc_driver_for_x86_redhat.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_x86_redhat.zip.sha256>`__ |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.1.x_odbc_driver_for_x86_redhat.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_x86_redhat.zip>`__ | `dws_8.1.x_odbc_driver_for_x86_redhat.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_x86_redhat.zip.sha256>`__ |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | SUSE x86_64   | `dws_9.1.x_odbc_driver_for_x86_suse.zip <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_x86_suse.zip>`__     | `dws_9.1.x_odbc_driver_for_x86_suse.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_9.1.x_odbc_driver_for_x86_suse.zip.sha256>`__     |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.2.x_odbc_driver_for_x86_suse.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.2.x_odbc_driver_for_x86_suse.zip>`__     | `dws_8.1.x_odbc_driver_for_x86_suse.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_x86_suse.zip.sha256>`__     |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |               | `dws_8.1.x_odbc_driver_for_x86_suse.zip <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_x86_suse.zip>`__     | `dws_8.1.x_odbc_driver_for_x86_suse.zip.sha256 <https://dws.obs.myhuaweicloud.com/download/dws_8.1.x_odbc_driver_for_x86_suse.zip.sha256>`__     |
   +---------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
