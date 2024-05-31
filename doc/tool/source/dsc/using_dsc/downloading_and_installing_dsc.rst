:original_name: dws_16_0212.html

.. _dws_16_0212:

Downloading and Installing DSC
==============================

Before using DSC, install it on a Linux or Windows server. DSC supports 64-bit Linux OSs. For details about other OSs supported by DSC, see :ref:`Table 4 <en-us_topic_0000001772696052__en-us_topic_0000001706105073_en-us_topic_0000001392380920_table1750314290446>`.

Prerequisites
-------------

-  In Linux, do not install or perform operations on DSC as the **root** user. To execute the **install.sh** script, you must have the permission for creating folders.

-  The size of the target folder must be at least four times that of the SQL files in the input folder.

   For example, if the size of the SQL files in the input folder is 100 KB, the target folder must have available disk space of at least 400 KB to process the SQL files.

   .. note::

      -  To query the available disk space of the target folder in Linux, run the following command:

         .. code-block::

            df -P <Folder path>

      -  To query the size of the input file in Linux, run the following command in the directory where the file resides:

         .. code-block::

            ls -l

-  JRE 1.8 or later and Perl have been installed. For details about the hardware and software requirements, see :ref:`Operating Environment <en-us_topic_0000001772696052>`.

   To check the installed Java version and set the Java path, perform the following steps:

   #. Check whether the Java version meets requirements.

      .. code-block::

         java -version

   #. Ensure that the Java path is correctly set.

      -  Linux

         a. Check whether the Java path is correctly set.

            .. code-block::

               echo $JAVA_HOME

         b. If no information is returned, add the following two lines to the **.bashrc** file of the current user, save the modification, and exit.

            Assume that the Java installation path is **/home/user/Java/jdk1.8.0_141**.

            .. code-block::

               export JAVA_HOME=/home/user/Java/jdk1.8.0_141
               export PATH=$JAVA_HOME/bin:$PATH

         c. Activate Java environment variables.

            .. code-block::

               source ~/.bashrc

      -  Windows

         a. Right-click **My Computer** and choose **Properties** from the shortcut menu. The **System** window is displayed.

         b. Click **Advanced System Settings**. The **System Properties** dialog box is displayed.

         c. On the **Advanced** tab page, click **Environment Variables**. The **Environment Variables** dialog box is displayed.

         d. Select **Path** in the **System variables** area. Click **Edit** and check the Java installation path.

            If the Java installation path does not exist or is incorrect, add the Java path of this PC to **Path**.

            Assume that the Java installation path is **C:\\Program Files\\Java\\jdk1.8.0_141\\bin** and the environment variable in **Path** is *c:\\windows\\system32;*. Set **Path** to *c:\\windows\\system32;*\ **C:\\Program Files\\Java\\jdk1.8.0_141\\bin;**.

Downloading DSC
---------------

#. Log in to the GaussDB(DWS) management console. In the navigation tree on the left, choose **Connections**.

#. On the **Download Client and Driver** page, click **here** to download the **DSC** software package.


   .. figure:: /_static/images/en-us_image_0000001706105425.png
      :alt: **Figure 1** Downloading the DSC client

      **Figure 1** Downloading the DSC client

#. Decompress the downloaded client software package to a user-defined path.

Installing DSC
--------------

DSC is a command line tool running on the Linux or Windows operating system. It can be used without installation. After downloading the software package, you can decompress it to use it.

Windows:

#. Decompress the **DSC.zip** package.

   The **DSC** folder is generated.

   .. note::

      You can decompress **DSC.zip** to any folder you need.

#. Go to the **DSC** directory.

#. Find and check the files in the **DSC** directory.

   :ref:`Table 1 <en-us_topic_0000001819336053__en-us_topic_0000001706224301_en-us_topic_0000001290072612_en-us_topic_0218440254_table9595434171818>` describes the obtained folders and files.

**Linux:**

#. Extract files from **DSC.zip**.

   .. code-block::

      sh install.sh

#. Go to the **DSC** directory.

   .. code-block::

      cd DSC

#. Check the files in the **DSC** directory.

   .. code-block::

      ls
      config   lib   scripts   bin  input output runDSC.sh  runDSC.bat

.. _en-us_topic_0000001819336053__en-us_topic_0000001706224301_en-us_topic_0000001290072612_en-us_topic_0218440254_table9595434171818:

.. table:: **Table 1** DSC directory

   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Folder or File |            | Description                                                                                                                    |
   +================+============+================================================================================================================================+
   | DSC            | bin        | DSC-related JAR package (executable)                                                                                           |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | config     | Configuration file of DSC                                                                                                      |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | input      | Input folder                                                                                                                   |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | lib        | Library files required for the normal running of DSC                                                                           |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | output     | Output folder                                                                                                                  |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | scripts    | Customized configuration scripts for Oracle and Teradata migration, which can be executed to implement corresponding functions |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | runDSC.sh  | Application executed on the Linux OS                                                                                           |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   |                | runDSC.bat | Application executed on the Windows OS                                                                                         |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   | changelog      |            | To notify users of the current modifications                                                                                   |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Install.sh     |            | To set the file permissions for DSC                                                                                            |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+
   | readme.txt     |            | Instructions of installation and configuration                                                                                 |
   +----------------+------------+--------------------------------------------------------------------------------------------------------------------------------+

.. note::

   If you do not need DSC, you can uninstall it by deleting the **DSC** folder.
