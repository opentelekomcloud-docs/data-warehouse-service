:original_name: dws_16_0010.html

.. _dws_16_0010:

Overview
========

This chapter describes how to use DSC, including how to install and configure DSC and how to use DSC to migrate data.

.. important::

   Use the latest patches to update the operating system and related software to prevent vulnerabilities and other security issues.

Security in DSC is managed by access control to the files and folders that are created by the tool. To access these files and folders, you must have the required permissions. For example, you need the permission 600/400 to access target files and log files, and the permission 700 to access target folders and log folders. The tool also ensures data security by not saving sensitive data in the log files.

The file or folder specified in **--input-folder** must not have write privileges to GROUP and/or OTHERS. For security reasons, the tool will not execute if the input files/folders have write privileges.

It is required that the root privileged user must not be used for installation and execution of the DSC for Linux.

The **umask** value provided in the **DSC** file is a set value that is related to file permissions. It is recommended that users do not modify this value. Modifying this value will affect file permissions.

.. note::

   DSC is a standalone application that can run without any network or database connection. It can run on any machine that is isolated from any network.
