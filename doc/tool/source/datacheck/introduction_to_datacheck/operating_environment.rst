:original_name: dws_07_0203.html

.. _dws_07_0203:

Operating Environment
=====================

Supported Databases
-------------------

The following table lists the source databases supported by DataCheck.

.. table:: **Table 1** Supported source databases

   ============ ==============
   Database     Version
   ============ ==============
   MySQL        8.0
   PostgreSQL   42.6.0
   GaussDB(DWS) 8.1.0 or later
   ============ ==============

The following table lists the destination databases supported by DataCheck.

.. table:: **Table 2** Supported destination databases

   ============ ==============
   Database     Version
   ============ ==============
   GaussDB(DWS) 8.1.0 or later
   ============ ==============

**Hardware requirements**

:ref:`Table 3 <en-us_topic_0000001860318861__table15632152814519>` lists the hardware requirements of DataCheck.

.. _en-us_topic_0000001860318861__table15632152814519:

.. table:: **Table 3** DataCheck hardware requirements

   ========== =================================================
   Hardware   Configuration
   ========== =================================================
   CPU        AMD or Intel Pentium (minimum frequency: 500 MHz)
   Min memory 1 GB
   Disk space 1 GB
   ========== =================================================

**Software requirements**

OS requirements

:ref:`Table 4 <en-us_topic_0000001860318861__table130908195514>` lists the OSs that are compatible with DataCheck.

.. _en-us_topic_0000001860318861__table130908195514:

.. table:: **Table 4** Compatible OSs

   +----------------------------+-------------------------------------------+----------------------------------------------+
   | Server                     | OS                                        | Version                                      |
   +============================+===========================================+==============================================+
   | General-purpose x86 server | SUSE Linux Enterprise Server 11 and later | SUSE Linux Enterprise Server 11 SP1 or later |
   +----------------------------+-------------------------------------------+----------------------------------------------+
   |                            | RHEL                                      | Red Hat 6.4 or later                         |
   +----------------------------+-------------------------------------------+----------------------------------------------+
   |                            | CentOS                                    | CentOS 6.4 or later                          |
   +----------------------------+-------------------------------------------+----------------------------------------------+
   |                            | Windows                                   | 7.0, 10, 11                                  |
   +----------------------------+-------------------------------------------+----------------------------------------------+

Requirements of DataCheck on other software versions

:ref:`Table 5 <en-us_topic_0000001860318861__table1185824916>` lists other software versions required by DataCheck.

.. _en-us_topic_0000001860318861__table1185824916:

.. table:: **Table 5** Requirements of DataCheck on other software versions

   ================== =======================
   Software           Purpose
   ================== =======================
   JDK 1.8 or JRE 1.8 Run the DataCheck tool.
   ================== =======================
