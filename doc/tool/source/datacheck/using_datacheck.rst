:original_name: dws_07_0209.html

.. _dws_07_0209:

Using DataCheck
===============

Notes
-----

-  Before starting DataCheck, configure the **dbinfo.properties** and **check_input.xlsx** files in the **config** folder. Incorrect parameter settings will cause DataCheck execution errors.
-  If you are running DataCheck tasks concurrently on the same server (by the same or different tasks), it is important to use separate **check_input.xlsx** files for each task.
-  After running DataCheck, a **logs** folder is created. You can open this folder to review the logs generated during the execution of DataCheck, which can assist you in identifying any problems.

Using DataCheck on Linux
------------------------

#. Upload the tool package to the Linux server and decompress it.

   |image1|

#. Generate the ciphertext of the database login password.

   Go to the **bin** directory.

   |image2|

   Run the script for generating the ciphertext. Run this script to generate ciphertext for the login passwords of the source and destination databases.

   sh encryption.sh [password]

   |image3|

#. Configure the **conf/dbinfo.properties** file.

   Go to the Datacheck directory and run the **vi conf/dbinfo.properties** command.

   |image4|

   Configure the database connection information of the source and destination ends. Use the ciphertext generated in the previous step for the password in the configuration file.

   |image5|

#. Edit the **input/check_input.xlsx** file.

   Copy the **check_input.xlsx** file to the Windows server, use Excel to edit the file, enter the table information to be verified, save the file, and upload the file to the Linux server to overwrite the original file.

   |image6|

#. Run the data check tool.

   Go to the bin directory and run the **sh datacheck.sh** script.

   |image7|

#. View the result **output/check_input_result.xlsx**.

   |image8|

#. Check result analysis:

   a. If **Status** is **No Pass**, the check fails.
   b. The **Check Result Diff** column displays the items that fail to pass the check. You can view the column that fails to pass the check.
   c. The **Check SQL** area displays the query SQL statements that are executed in the database.

Using DataCheck on Windows
--------------------------

#. Upload the tool package to the Windows server and decompress it.

   |image9|

#. Generate the ciphertext of the database login password.

   Go to the **bin** directory and start the CMD tool.

   |image10|

   Run the script for generating the ciphertext. Run this script to generate ciphertext for the login passwords of the source and destination databases.

   encryption.bat [password]

   |image11|

#. Configure the **conf/dbinfo.properties** file.

   Edit the **dbinfo.properties** file in the **conf** directory, configure the database connection information of the source and destination ends, and use the ciphertext generated in the previous step as the password in the configuration file.

   |image12|

#. Edit the **input/check_input.xlsx** file and save it.

   Open the **input/check_input.xlsx** file using Excel, input the table information that needs to be verified, and save the file.

   |image13|

#. Run the data check tool **datacheck.bat**.

   |image14|

#. View the result **output/check_input_result.xlsx**. (The result analysis is the same as that in the Linux scenario.)

   |image15|

   |image16|

#. Check result analysis:

   a. If **Status** is **No Pass**, the check fails.
   b. The **Check Result Diff** column displays the items that fail to pass the check. You can view the column that fails to pass the check.
   c. The **Check SQL** area displays the query SQL statements that are executed in the database.

.. |image1| image:: /_static/images/en-us_image_0000002114138993.png
.. |image2| image:: /_static/images/en-us_image_0000002114139545.png
.. |image3| image:: /_static/images/en-us_image_0000002114139789.png
.. |image4| image:: /_static/images/en-us_image_0000002078549004.png
.. |image5| image:: /_static/images/en-us_image_0000002114144325.png
.. |image6| image:: /_static/images/en-us_image_0000002114220901.png
.. |image7| image:: /_static/images/en-us_image_0000002078705432.png
.. |image8| image:: /_static/images/en-us_image_0000002114228013.png
.. |image9| image:: /_static/images/en-us_image_0000002114760145.png
.. |image10| image:: /_static/images/en-us_image_0000002079322158.png
.. |image11| image:: /_static/images/en-us_image_0000002079011850.png
.. |image12| image:: /_static/images/en-us_image_0000002079013756.png
.. |image13| image:: /_static/images/en-us_image_0000002114856101.png
.. |image14| image:: /_static/images/en-us_image_0000002079018184.png
.. |image15| image:: /_static/images/en-us_image_0000002079331782.png
.. |image16| image:: /_static/images/en-us_image_0000002084159912.png
