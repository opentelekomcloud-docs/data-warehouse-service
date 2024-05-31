:original_name: dws_04_0117.html

.. _dws_04_0117:

ODBC-Based Development
======================

Open Database Connectivity (ODBC) is an MS API for accessing databases based on the X/OPEN CLI. The ODBC API alleviates applications from directly operating in databases, and enhances the database portability, extensibility, and maintainability.

:ref:`Figure 1 <en-us_topic_0000001233563131__f08f2cb1be20447fc8206f122126d1ec3>` shows the system structure of ODBC.

.. _en-us_topic_0000001233563131__f08f2cb1be20447fc8206f122126d1ec3:

.. figure:: /_static/images/en-us_image_0000001188323782.jpg
   :alt: **Figure 1** ODBC system structure

   **Figure 1** ODBC system structure

GaussDB(DWS) supports ODBC 3.5 in the following environments.

.. table:: **Table 1** OSs Supported by ODBC

   +--------------------------------------------------------------------------+-----------------------------------+
   | OS                                                                       | Platform                          |
   +==========================================================================+===================================+
   | SUSE Linux Enterprise Server 11 SP1/SP2/SP3/SP4                          | x86_64                            |
   |                                                                          |                                   |
   | SUSE Linux Enterprise Server 12 and SP1/SP2/SP3/SP5                      |                                   |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Red Hat Enterprise Linux 6.4/6.5/6.6/6.7/6.8/6.9/7.0/7.1/7.2/7.3/7.4/7.5 | x86_64                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Red Hat Enterprise Linux 7.5                                             | ARM64                             |
   +--------------------------------------------------------------------------+-----------------------------------+
   | CentOS 6.4/6.5/6.6/6.7/6.8/6.9/7.0/7.1/7.2/7.3/7.4                       | x86_64                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | CentOS 7.6                                                               | ARM64                             |
   +--------------------------------------------------------------------------+-----------------------------------+
   | EulerOS 2.0 SP2/SP3                                                      | x86_64                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | EulerOS 2.0 SP8                                                          | ARM64                             |
   +--------------------------------------------------------------------------+-----------------------------------+
   | NeoKylin 7.5/7.6                                                         | ARM64                             |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Oracle Linux R7U4                                                        | x86_64                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Windows 7                                                                | 32-bit                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Windows 7                                                                | 64-bit                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Windows Server 2008                                                      | 32-bit                            |
   +--------------------------------------------------------------------------+-----------------------------------+
   | Windows Server 2008                                                      | 64-bit                            |
   +--------------------------------------------------------------------------+-----------------------------------+

The operating systems listed above refer to the operating systems on which the ODBC program runs. They can be different from the operating systems where databases are deployed.

The ODBC Driver Manager running on UNIX or Linux can be unixODBC or iODBC. Select unixODBC-2.3.0 here as the component for connecting the database.

Windows has a native ODBC Driver Manager. You can locate **Data Sources (ODBC)** by choosing **Control Panel > Administrative Tools**.

.. note::

   The current database ODBC driver is based on an open source version and may be incompatible with GaussDB(DWS) data types, such as tinyint, smalldatetime, and nvarchar2.

-  :ref:`ODBC Package and Its Dependent Libraries and Header Files <dws_04_0118>`
-  :ref:`Configuring a Data Source in the Linux OS <dws_04_0119>`
-  :ref:`Configuring a Data Source in the Windows OS <dws_04_0120>`
-  :ref:`ODBC Development Example <dws_04_0123>`
-  :ref:`ODBC Interfaces <dws_04_0124>`

.. toctree::
   :maxdepth: 1
   :hidden: 

   odbc_package_and_its_dependent_libraries_and_header_files
   configuring_a_data_source_in_the_linux_os
   configuring_a_data_source_in_the_windows_os
   odbc_development_example
   odbc_interfaces/index
