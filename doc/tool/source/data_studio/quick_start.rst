:original_name: DWS_DS_19.html

.. _DWS_DS_19:

Quick Start
===========

This section describes how to start Data Studio.

**Prerequisites**
-----------------

The **StartDataStudio.bat** batch file checks the version of Operating System (OS), Java and Data Studio as a prerequisite to run Data Studio.

#. In the :ref:`Release package <dws_ds_13>` navigate to **Tools** folder, locate and double-click **StartDataStudio.bat** to execute and check Java version compatibility.

   The batch file checks the version compatibility and will launch Data Studio or display appropriate message based on the OS, Java and Data Studio version installed.

   If the Java version installed is earlier than 1.8, then :ref:`error message <en-us_topic_0000001188362524__en-us_topic_0185264982_li107293206554>` is displayed.

   The batch file determines the versions of the OS and Java for Data Studio based on the following scenarios:

   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
   | DS Installation (32-bit/64-bit) | OS (Bit) | Java (bit) | Outcome                                                                                                  |
   +=================================+==========+============+==========================================================================================================+
   | 32                              | 32       | 32         | Data Studio is launched.                                                                                 |
   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
   | 32                              | 64       | 32         | Data Studio is launched.                                                                                 |
   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
   | 32                              | 64       | 64         | :ref:`Error Message <en-us_topic_0000001188362524__en-us_topic_0185264982_li107293206554>` is displayed. |
   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
   | 64                              | 32       | 32         | :ref:`Error Message <en-us_topic_0000001188362524__en-us_topic_0185264982_li107293206554>` is displayed. |
   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
   | 64                              | 64       | 32         | :ref:`Error Message <en-us_topic_0000001188362524__en-us_topic_0185264982_li107293206554>` is displayed. |
   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
   | 64                              | 64       | 64         | Data Studio is launched.                                                                                 |
   +---------------------------------+----------+------------+----------------------------------------------------------------------------------------------------------+
