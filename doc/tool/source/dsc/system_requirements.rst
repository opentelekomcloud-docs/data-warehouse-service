:original_name: dws_mt_0014.html

.. _dws_mt_0014:

System Requirements
===================

Supported Databases
-------------------

:ref:`Table 1 <en-us_topic_0000001188202622__en-us_topic_0218440498_table293193041118>` lists the source databases supported by DSC.

.. _en-us_topic_0000001188202622__en-us_topic_0218440498_table293193041118:

.. table:: **Table 1** Supported source databases

   +-----------------------------------+-----------------------------------+
   | Database Name                     | Database Version                  |
   +===================================+===================================+
   | Netezza                           | 7.0                               |
   +-----------------------------------+-----------------------------------+
   | Teradata                          | 13.1                              |
   +-----------------------------------+-----------------------------------+
   | Oracle                            | 11g Release 2                     |
   |                                   |                                   |
   |                                   | 12c Release 1                     |
   +-----------------------------------+-----------------------------------+
   | MySQL                             | 5.6                               |
   +-----------------------------------+-----------------------------------+

.. _en-us_topic_0000001188202622__en-us_topic_0218440498_section65156403:

Software Requirements
---------------------

OS requirements

:ref:`Table 2 <en-us_topic_0000001188202622__en-us_topic_0218440498_table1750314290446>` lists the operating systems compatible with DSC.

.. _en-us_topic_0000001188202622__en-us_topic_0218440498_table1750314290446:

.. table:: **Table 2** Compatible OSs

   =========== =============================== =====================
   Server      Operating System                Version
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
   \           Windows                         7.0, 10
   =========== =============================== =====================

Other software requirements

:ref:`Table 3 <en-us_topic_0000001188202622__en-us_topic_0218440498_table27159916>` lists the requirements of DSC on other software versions.

.. _en-us_topic_0000001188202622__en-us_topic_0218440498_table27159916:

.. table:: **Table 3** Requirements of DSC on other software versions

   ====================== ======================================
   Software               Description
   ====================== ======================================
   JDK 1.8.0_141 or later Used to run DSC.
   Perl 5.8.8             Used to migrate Perl files.
   Perl 5.28 or later     Used to migrate Perl files in Windows.
   ====================== ======================================

.. note::

   Perl 5.8.8 is a fixed version. To ensure syntax compatibility, do not use a later version.
