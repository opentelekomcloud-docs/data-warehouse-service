:original_name: DWS_DS_19.html

.. _DWS_DS_19:

Getting Started
===============

This section describes the steps to be followed to start Data Studio.

Prerequisites
-------------

The **StartDataStudio.bat** batch file checks the version of Operating System (OS), Java and Data Studio as a prerequisite to run Data Studio.

#. In the :ref:`Release package <dws_ds_13>` navigate to Tools folder, locate and double-click *StartDataStudio.bat* to execute and check Java version compatibility.

   The batch file checks the version compatibility and will launch Data Studio or display appropriate message based on OS, Java and Data Studio version installed.

   If the Java version installed is earlier than 1.8, then :ref:`error message <en-us_topic_0000001145913163__en-us_topic_0185264982_li107293206554>` is displayed.

   The scenarios checked by the batch file to confirm the required versions of the OS and Java for DS.

   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
   | DS **Installation** **(32/64bit)** | OS **(bit)** | Java **(bit)** | Outcome                                                                                                 |
   +====================================+==============+================+=========================================================================================================+
   | 32                                 | 32           | 32             | Launches Data Studio                                                                                    |
   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
   | 32                                 | 64           | 32             | Launches Data Studio                                                                                    |
   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
   | 32                                 | 64           | 64             | :ref:`Error message <en-us_topic_0000001145913163__en-us_topic_0185264982_li107293206554>` is displayed |
   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
   | 64                                 | 32           | 32             | :ref:`Error message <en-us_topic_0000001145913163__en-us_topic_0185264982_li107293206554>` is displayed |
   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
   | 64                                 | 64           | 32             | :ref:`Error message <en-us_topic_0000001145913163__en-us_topic_0185264982_li107293206554>` is displayed |
   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
   | 64                                 | 64           | 64             | Launches Data Studio                                                                                    |
   +------------------------------------+--------------+----------------+---------------------------------------------------------------------------------------------------------+
