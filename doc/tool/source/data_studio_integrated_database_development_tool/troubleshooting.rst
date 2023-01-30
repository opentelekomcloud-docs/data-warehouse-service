:original_name: DWS_DS_145.html

.. _DWS_DS_145:

Troubleshooting
===============

#. The Data Studio cannot be opened for a long time.

   Solution: Check whether JRE is found. Verify the Java path configured in the environment. For details about the supported Java JDK versions, see :ref:`System Requirements <dws_ds_14>`.

#. After you double-click the Data Studio.exe file, the Data Studio cannot be opened and a Java running error is displayed.

   **Solution**

   -  Without JRE:

      |image1|

      Check whether the Java Runtime Environment (JRE) or Java Development Kit (JDK) version 1.8 that matches the bit version of the operating system has been installed in the system, and set the Java Home path. If multiple Java versions are installed, set the -vm parameter in the configuration file by referring to :ref:`Installing and Configuring Data Studio <dws_ds_16>`. This is the prerequisite for running Data Studio.

   -  Old JRE version:

      |image2|

      Query the version of the installed JRE or JDK. If an earlier version is installed in the system, this error is reported. Upgrade the JRE version to 1.8 that matches the number of bits of the operating system.

   -  Incompatible with Java

      |image3|

      Check the version of the JRE or JDK installed in the system. If the installed Java version is incompatible with the system, this error occurs. Upgrade the JRE version to 1.8 that matches the number of bits of the operating system.

      You are advised to run the BAT file to check the Java version compatibility, and then open Data Studio. For details, see :ref:`Getting Started <dws_ds_19>`.

#. .. _en-us_topic_0000001145913163__en-us_topic_0185264982_li107293206554:

   The following information is displayed when you run the StartDataStudio.bat file:

   **Solution**

   +-------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | information                                                                                                             | Solution                                                                  |
   +=========================================================================================================================+===========================================================================+
   | You are trying to run 32-bit Data Studio in the following environment:                                                  | Install 32-bit Java 1.8.                                                  |
   |                                                                                                                         |                                                                           |
   | -  64-bit OS                                                                                                            |                                                                           |
   |                                                                                                                         |                                                                           |
   | -  Microsoft Windows 7 Professional                                                                                     |                                                                           |
   |                                                                                                                         |                                                                           |
   | -  64-bit Java 1.8 JDK (incompatible)                                                                                   |                                                                           |
   |                                                                                                                         |                                                                           |
   |    Install 32-bit Java 1.8.                                                                                             |                                                                           |
   +-------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | The Java version supported by Data Studio must be 1.8 or later. Before using Data Studio, you need to install Java 1.8. | Install Java 1.8 that matches the number of bits of the operating system. |
   +-------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | You are trying to run 64-bit Data Studio in the following environment:                                                  | Install 64-bit Java 1.8.                                                  |
   |                                                                                                                         |                                                                           |
   | -  64-bit OS                                                                                                            |                                                                           |
   |                                                                                                                         |                                                                           |
   | -  Microsoft Windows 7 Professional                                                                                     |                                                                           |
   |                                                                                                                         |                                                                           |
   | -  32-bit Java 1.8 JDK (incompatible): Install 64-bit Java 1.8.                                                         |                                                                           |
   +-------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | You are trying to run 64-bit Data Studio in the following environment:                                                  | Install the 32-bit Data Studio.                                           |
   |                                                                                                                         |                                                                           |
   | -  For a 32-bit OS, run the following command:                                                                          |                                                                           |
   |                                                                                                                         |                                                                           |
   | -  Microsoft Windows 7 Professional                                                                                     |                                                                           |
   |                                                                                                                         |                                                                           |
   | -  32-bit Java 1.8 JDK (incompatible)                                                                                   |                                                                           |
   |                                                                                                                         |                                                                           |
   |    Install the 32-bit Data Studio.                                                                                      |                                                                           |
   +-------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+

#. All entered information is correct, but the Data Studio cannot connect to the server.

   Solution: Check whether the server is running on the specified IP address and port. Use gsql to connect to a specified user and check the user availability.

#. Connection problems occur during the use of Data Studio.

   Solution: If a connection problem occurs during the use of Data Studio, see the following example.

   Create a database connection.

   Used for executing queries.

   When a connection exception occurs in any database (PostgreSQL), the connection is closed. When the database connection is closed, all open procedure and function windows are closed.

   The system displays an error message. The Object Browser navigation tree displays the database status.

   .. note::

      Only the current database is interrupted. Other databases remain connected or are reconnected.

   Reconnect to the database and continue the query.

#. When a Java application is used to obtain a process that contains Chinese comments, the Chinese characters are invisible. What should we do?

   Solution: Choose Preferences > Session Settings > Data Studio Encoding and set the encoding format to GBK so that Chinese characters can be displayed properly.

#. When you connect to the database and load a large number of SQL queries and data on the SQL terminal, the "Out Of Memory" or "Java Heap Error" error may occur in Data Studio. How to solve this problem?

   Solution: When the Data Studio has used up the allocated maximum Java memory, the message "Out of Memory" or "Java Heap Error" is displayed. By default, the Data Studio.ini configuration file (in the Data Studio installation path) contains the entry -Xmx1200m. 1200m indicates 1200 MB, which is the maximum Java memory that can be used by Data Studio. The memory usage of Data Studio depends on the size of data obtained by users during the use of Data Studio.

   To solve this problem, you can expand the Java memory size to an ideal value. For example, change -Xmx1200m to -Xmx2000m and restart Data Studio. If the updated memory is used up, the same problem may occur again.

   .. note::

      -  For the 32-bit Data Studio with 8 GB RAM, the value of Xmx cannot exceed 2044. For the 64-bit Data Studio with 8 GB RAM, the value of Xmx cannot exceed 6000. The upper limit may vary with the current memory usage of the user.

         Examples are as follows:

         -Xms1024m

         -Xmx1800m

      -  The maximum file size supported by Data Studio on the SQL terminal depends on the value of Xmx in the Data Studio.ini file and the available memory.

#. If a large amount of data is returned after the SQL query is executed, the Data Studio displays the "Insufficient Memory" error. What should we do?

   Solution: Data Studio disconnects from the database specified in the file. Re-establish the connection and continue the operation.

#. Why do I receive an export failure message when exporting DDL or data?

   Solution: The possible causes are as follows:

   -  An invalid client SSL certificate and/or client SSL key file was selected. Select a correct file and try again. For details, see :ref:`Adding a Connection <dws_ds_34>`.
   -  The identity of the object in the database may have changed. Check whether the identity of the object has changed and try again.
   -  You do not have the required permissions. Contact the database administrator to obtain required permissions.

#. Why does the system receive a message indicating that the DDL operation fails when the DDL operation is performed?

   Solution: The possible causes are as follows:

   -  An invalid client SSL certificate and/or client SSL key file was selected. Select a correct file and try again. For details, see :ref:`Adding a Connection <dws_ds_34>`.
   -  The identity of the object in the database may have changed. Check whether the identity of the object has changed and try again.
   -  You do not have the required permissions. Contact the database administrator to obtain required permissions.

#. .. _en-us_topic_0000001145913163__en-us_topic_0185264982_li1161793674918:

   Why do I receive the following error message when performing a Show DDL or Export DDL operation?

   "Can't start this program because MSVCRT100.dll is missing on your computer. Try reinstalling the program to resolve the problem."

   Solution: gs_dump.exe needs to be executed to display or export DDL, which requires the Microsoft VC Runtime Library file msvcrt100.dll.

   To resolve this issue, copy the msvcrt100.dll file from the \\Windows\\System32 folder to the \\Windows\\SysWOW64 folder.

#. Why is the saved connection details not displayed when I try to establish a connection?

   Solution: If the Profile folder in the User Data folder is unavailable or has been manually modified, this problem may occur. Ensure that the Profile folder exists and its name meets the requirements.

#. Why Are Historical SQL Query Records Lost When I Close and Restart Data Studio?

   Solution: If the Profile folder in the User Data folder is lost or manually modified, this problem may occur. Ensure that the Profile folder exists and its name meets the requirements.

#. Why does the system display a message indicating that the modification fails to be saved when I attempt to modify the syntax highlighting setting?

   Solution: This problem may occur if the Preferences file does not exist or its name has been changed. Restart Data Studio.

#. What should I do if the Data Studio is in the idle state but the Data Studio.log file is in the No more handles state?

   Solution: Restart Data Studio.

#. What happens if I send a 303 error after editing a table and I cannot continue to modify the table?

   Solution: All edited data will be lost. Close the Edit Data dialog box and modify the data again.

#. Why is the message "The number of pasted cells does not match the number of selected cells" displayed when the operation is correct?

   Solution: This problem occurs if you choose Preferences > Query Results and set the column headers to be included. The selected cell also contains the column header cell. Modify the settings to disable the Include column headers option and try again.

#. Why can't I edit temporary tables when the Reuse Connections option is disabled?

   Answer: After the Reuse Connection option is disabled, the tool creates a new session, but the temporary table can be edited only in the existing connection. To edit temporary tables, enable the Reuse Connections option. For details, see :ref:`Managing SQL Terminal Connections <dws_ds_130>`.

#. What happens when you add the same column multiple times in a multi-column sort dialog box?

   Answer: If you add the same column multiple times in the multi-column sorting dialog box and click Apply, the following message is displayed. You need to click OK and select non-duplicate columns for sorting.

   |image4|

#. What happens when no column name is specified and Apply is clicked?

   Answer: The following message is displayed. You need to set a valid column name and click Apply again. Then, the message is not displayed.

   |image5|

#. What happens when I click Cancel while multiple table queries are running in the SQL terminal window?

   Answer: Canceling a table query that is being executed may cause the console to display the names of tables that are not created. In this case, you are advised to delete the table so that you can perform operations on tables with the same name.

#. What Should I Do If I Cannot Log In to Data Studio Because the Security Key Is Cracked?

   Solution: Perform the following steps to generate a new security key:

   a. Choose Datastudio > Userdata and delete Security folder.
   b. Restart Data Studio.
   c. Create a new security folder and generate a new key.
   d. Enter the password again to log in to Data Studio.

.. |image1| image:: /_static/images/en-us_image_0000001098673452.png
.. |image2| image:: /_static/images/en-us_image_0000001145913243.png
.. |image3| image:: /_static/images/en-us_image_0000001099153260.png
.. |image4| image:: /_static/images/en-us_image_0000001098833282.png
.. |image5| image:: /_static/images/en-us_image_0000001145833137.png
