:original_name: dws_mt_0192.html

.. _dws_mt_0192:

FAQs
====

This section covers the frequently asked questions.

**Q1: During installation, I get an error "Root privileged users are not allowed to install the DSC for Linux." What is the solution?**

**Answer:** A root privileged user must not be used for installation and execution of the DSC for Linux. It is recommended that a user without root privileges be used to install and operate the DSC.

**Q2: How do I configure the DSC to support** **GaussDB T, GaussDB A and GaussDB(DWS)** **version V100R002C60 for Teradata?**

**Answer:** Perform the following steps to configure the DSC to support GaussDB T, GaussDB A and GaussDB(DWS) version V100R002C60 for Teradata:

#. Open the *features-teradata.properties* file in the *config* subfolder of TOOL_HOME.
#. Change the following variable values based on the requirement.

   -  VOLATILE

   -  PRIMARY INDEX

      For example,

      .. code-block::

         VOLATILE=UNLOGGED | LOCAL TEMPORARY

      .. code-block::

         PRIMARY INDEX=ONE | MANY

.. note::

   The default variable value for **VOLATILE** is *LOCAL TEMPORARY* and for **PRIMARY INDEX** it is *MANY*.
