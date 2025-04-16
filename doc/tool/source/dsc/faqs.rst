:original_name: dws_mt_0192.html

.. _dws_mt_0192:

FAQs
====

This section covers the frequently asked questions.

**Q1: During installation, I get an error "Root privileged users are not allowed to install the DSC for Linux." What is the solution?**

**Answer:** A root privileged user must not be used for installation and execution of the DSC for Linux. It is recommended that a user without root privileges be used to install and operate the DSC.

**Question 2: How do I configure DSC to enable Teradata to support version GaussDB 200 V100R002C60?**

**Answer:** Perform the following steps to set table variables to support the GaussDB 200 V100R002C60 version:

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
