:original_name: DWS_DS_006.html

.. _DWS_DS_006:

Downloading and Installing the Data Studio Client
=================================================

#. GaussDB(DWS) provides a Windows-based Data Studio client and the tool depends on the JDK. You need to install the JDK on the client host first.

   .. important::

      Only JDK 1.8 is supported.

   In the Windows operating system, you can download the required JDK version from the official website of SDK, and install it by following the installation guidance.

#. Log in to the GaussDB(DWS) management console. In the navigation pane, choose **Management** > **Client Connections**. The **Download Client and Driver** page is displayed.

#. On the **Download Client and Driver** page, download **Data Studio GUI Client**.

   -  Select **Windows x86** or **Windows x64** based on the OS type and click **Download** to download a Data Studio version that matches the current cluster.
   -  Click **Historical Version** to download the corresponding Data Studio version. You are advised to download Data Studio based on the cluster version.


   .. figure:: /_static/images/en-us_image_0000002092006144.png
      :alt: **Figure 1** Downloading the DSC client

      **Figure 1** Downloading the DSC client

#. Decompress the downloaded client software package (32-bit or 64-bit) to the installation directory.

#. Open the installation directory and double-click **Data Studio.exe** to start the Data Studio client.


   .. figure:: /_static/images/en-us_image_0000001860199277.png
      :alt: **Figure 2** Starting the client

      **Figure 2** Starting the client

   .. table:: **Table 1** Data Studio installation package structure

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Folders/Files                     | Description                                                                                                                                                                                                                              |
      +===================================+==========================================================================================================================================================================================================================================+
      | *configuration*                   | Contains information about the application startup and the required **Eclipse** plug-in path.                                                                                                                                            |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *db_assistant*                    | Contains files related to the SQL Assistant function.                                                                                                                                                                                    |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *docs*                            | -  Contains *Data Studio User Manual*.                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                          |
      |                                   | -  Contains copyright notices, licenses, and written offer of the open source software used in Data Studio.                                                                                                                              |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *features*                        | Contains Eclipse (rich client protocol-GUI) and Data Studio features.                                                                                                                                                                    |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *p2*                              | Contains files required for providing and managing Eclipse- and Equinox-based applications.                                                                                                                                              |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *plugins*                         | Contains the required Eclipse and Data Studio plug-ins.                                                                                                                                                                                  |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *tools*                           | Contains tools that Data Studio depends on.                                                                                                                                                                                              |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *UserData/*                       | Contains folders of each OS user of Data Studio.                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                          |
      | -  Autosave                       | **Autosave**: contains the information of queries and functions/procedures that are automatically saved.                                                                                                                                 |
      | -  *Logs/*                        |                                                                                                                                                                                                                                          |
      | -  *Preferences/*                 | **Logs**: contains the **Data Studio.log** file that logs all the operations performed in Data Studio.                                                                                                                                   |
      | -  *Profile/*                     |                                                                                                                                                                                                                                          |
      |                                   | **Preferences**: contains the **Preferences.prefs** file that stores the custom preferences.                                                                                                                                             |
      |    -  *History/*                  |                                                                                                                                                                                                                                          |
      |                                   | **Profile**: contains the **connection.properties** and **Profiles.txt** files, as well as SQL History, to manage connection information in Data Studio.                                                                                 |
      | -  *Security/*                    |                                                                                                                                                                                                                                          |
      |                                   | **Security**: contains the files required for the management security of Data Studio.                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                          |
      |                                   | .. note::                                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                          |
      |                                   |    -  The **UserData** folder is created after the first user opens the instance using Data Studio.                                                                                                                                      |
      |                                   |    -  The **Logs** folder, language, memory settings and log level are common for all users.                                                                                                                                             |
      |                                   |    -  After Data Studio is started, the **Logs**, **Preferences**, **Profile**, and **Security** folders, as well as the **Data Studio.log**, **Preferences.prefs**, **connection.properties**, and **Profiles.txt** files, are created. |
      |                                   |    -  If the **Logs** folder path is specified in the **Data Studio.ini** file, logs are created in the specified path.                                                                                                                  |
      |                                   |    -  When you cannot log in to Data Studio due to a damaged security key, perform the following steps to generate a new security key:                                                                                                   |
      |                                   |                                                                                                                                                                                                                                          |
      |                                   |       a. Delete the **security** folder in **Data Studio** > **UserData**.                                                                                                                                                               |
      |                                   |       b. Restart Data Studio.                                                                                                                                                                                                            |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *artifacts.xml*                   | Contains the product build information.                                                                                                                                                                                                  |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *changelog.txt*                   | Contains the detailed log changes of the current release.                                                                                                                                                                                |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *Data Studio.exe/DataStudio.sh*   | Allows you to connect to a server and perform operations, such as managing database objects, editing or executing PL/SQL programs.                                                                                                       |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *Data Studio.ini*                 | Contains the configuration information for running Data Studio.                                                                                                                                                                          |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | *readme.txt*                      | Contains the features or fixed issues of the current release.                                                                                                                                                                            |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      If your computer blocks the running of the application, you can unlock the **Data Studio.exe** file to start the application.

#. .. _en-us_topic_0000001860198941__en-us_topic_0000001533190636_en-us_topic_0000001523826708_en-us_topic_0000001514019285_li7537173111372:

   After the installation is complete, double-click the **StartDataStudio.bat** file in installation directory\ **/tools** to check the versions of the OS, Java, and Data Studio.

   The batch file checks the version compatibility and will launch Data Studio or display appropriate message based on the OS, Java and Data Studio version installed.

   If the Java version installed is earlier than 1.8, then an :ref:`error message <en-us_topic_0000001813439016__en-us_topic_0185264982_li107293206554>` may be displayed.

   The batch file determines the versions of the OS and Java for Data Studio based on the following scenarios:

   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
   | DS Installation (32-bit/64-bit) | OS (Bit) | Java (bit) | Result                                                                                                           |
   +=================================+==========+============+==================================================================================================================+
   | 32                              | 32       | 32         | Data Studio is launched.                                                                                         |
   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
   | 32                              | 64       | 32         | Data Studio is launched.                                                                                         |
   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
   | 32                              | 64       | 64         | An :ref:`error message <en-us_topic_0000001813439016__en-us_topic_0185264982_li107293206554>` will be displayed. |
   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
   | 64                              | 32       | 32         | An :ref:`error message <en-us_topic_0000001813439016__en-us_topic_0185264982_li107293206554>` will be displayed. |
   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
   | 64                              | 64       | 32         | An :ref:`error message <en-us_topic_0000001813439016__en-us_topic_0185264982_li107293206554>` will be displayed. |
   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
   | 64                              | 64       | 64         | Data Studio is launched.                                                                                         |
   +---------------------------------+----------+------------+------------------------------------------------------------------------------------------------------------------+
