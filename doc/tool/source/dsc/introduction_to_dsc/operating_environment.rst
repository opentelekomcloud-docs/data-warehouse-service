:original_name: dws_16_0008.html

.. _dws_16_0008:

.. _en-us_topic_0000001772696052:

Operating Environment
=====================

Supported Databases
-------------------

The following table lists the source databases supported by DSC.

.. table:: **Table 1** Source databases supported by DSC

   ======== =======
   Database Version
   ======== =======
   Teradata 13.1
   MySQL    8.0
   ======== =======

The following table lists the destination databases supported by DSC.

.. table:: **Table 2** Destination databases supported by DSC

   ============ ===========================
   Database     Version
   ============ ===========================
   GaussDB(DWS) GaussDB(DWS) 8.1.0 or later
   ============ ===========================

Hardware Requirements
---------------------

:ref:`Table 3 <en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table48268587161954>` lists the hardware requirements of DSC.

.. _en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table48268587161954:

.. table:: **Table 3** Hardware requirements of DSC

   ========== =================================================
   Hardware   Configuration
   ========== =================================================
   CPU        AMD or Intel Pentium (minimum frequency: 500 MHz)
   Min memory 1 GB
   Disk space 1 GB
   ========== =================================================

Software Requirements
---------------------

**OS**

:ref:`Table 4 <en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table1750314290446>` lists the operating systems compatible with DSC.

.. _en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table1750314290446:

.. table:: **Table 4** OSs compatible with DSC

   =========== =============================== =====================
   Server      OS                              Version
   =========== =============================== =====================
   x86 servers SUSE Linux Enterprise Server 11 SP1 (SUSE 11.1)
   \                                           SP2 (SUSE 11.2)
   \                                           SP3 (SUSE 11.3)
   \                                           SP4 (SUSE 11.4)
   \           SUSE Linux Enterprise Server 12 SP0 (SUSE 12.0)
   \                                           SP1 (SUSE 12.1)
   \                                           SP2 (SUSE 12.2)
   \                                           SP3 (SUSE 12.3)
   \           RHEL                            6.4-x86_64 (RHEL 6.4)
   \                                           6.5-x86_64 (RHEL 6.5)
   \                                           6.6-x86_64 (RHEL 6.6)
   \                                           6.7-x86_64 (RHEL 6.7)
   \                                           6.8-x86_64 (RHEL 6.8)
   \                                           6.9-x86_64 (RHEL 6.9)
   \                                           7.0-x86_64 (RHEL 7.0)
   \                                           7.1-x86_64 (RHEL 7.1)
   \                                           7.2-x86_64 (RHEL 7.2)
   \                                           7.3-x86_64 (RHEL 7.3)
   \                                           7.4-x86_64 (RHEL 7.4)
   \           CentOS                          6.4 (CentOS 6.4)
   \                                           6.5 (CentOS 6.5)
   \                                           6.6 (CentOS 6.6)
   \                                           6.7 (CentOS 6.7)
   \                                           6.8 (CentOS 6.8)
   \                                           6.9 (CentOS 6.9)
   \                                           7.0 (CentOS 7.0)
   \                                           7.1 (CentOS 7.1)
   \                                           7.2 (CentOS 7.2)
   \                                           7.3 (CentOS 7.3)
   \                                           7.4 (CentOS 7.4)
   \           Windows                         7.0, 10, 11
   =========== =============================== =====================

**Other software requirements**

:ref:`Table 5 <en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table27159916>` lists the requirements of DSC on other software versions.

.. _en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table27159916:

.. table:: **Table 5** Requirements of DSC on other software versions

   ====================== ======================================
   Software               Description
   ====================== ======================================
   JDK 1.8.0_141 or later Used to run DSC.
   Perl 5.8.8             Used to migrate Perl files.
   Perl 5.28.2 and later  Used to migrate Perl files in Windows.
   Python 3.8.2           Used to verify post migration script.
   ====================== ======================================
