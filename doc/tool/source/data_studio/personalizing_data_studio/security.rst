:original_name: DWS_DS_142.html

.. _DWS_DS_142:

Security
========

This section provides details on how to personalize password and security disclaimer display.

.. _en-us_topic_0000001234042179__en-us_topic_0185264975_section1327033652211:

Save Password Permanently
-------------------------

You can enable/disable to display the permanent option to save password in the connection window.

Follow the steps below to modify display of permanent save password option:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Security > Password**.

   The **Password** pane is displayed.

#. Select the required option. Refer table below to understand the customization options available:

   +--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option | Outcome                                                                                                                                                                  |
   +========+==========================================================================================================================================================================+
   | Yes    | Selecting this option enables you to view the "Permanently" save password option from the **Save Password** drop-down list in the connection window.                     |
   +--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No     | Selecting this option removes the "Permanently" save password option from the **Save Password** drop-down list in the connection window and removes the saved passwords. |
   +--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The **Restart** **Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, then Data Studio displays the **Process is running** dialog box.

   Click **Force Restart** to discard operations and restart Data Studio.

#. Click **OK** to continue performing operations.

   .. note::

      Click **Restore Defaults** from **Password** pane to reset to default values. The default value is **No**.

.. _en-us_topic_0000001234042179__en-us_topic_0185264975_section56881736153111:

Password Expiry
---------------

This section provides details on how to continue/discontinue working with Data Studio once password expires using the Password setting.

Follow the steps below to modify the behavior of Data Studio once password expires:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Security > Password**.

   The **Password** pane is displayed.

#. Select the required option. Refer table below to understand the customization options available:

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option                            | Outcome                                                                                                                                                         |
   +===================================+=================================================================================================================================================================+
   | Yes                               | Selecting this option allows you to login to Data Studio after the password has expired.                                                                        |
   |                                   |                                                                                                                                                                 |
   |                                   | .. note::                                                                                                                                                       |
   |                                   |                                                                                                                                                                 |
   |                                   |    A message displays informing you that the password has expired and some operations may not work as expected in the following scenarios:                      |
   |                                   |                                                                                                                                                                 |
   |                                   |    -  Establishing a new connection.                                                                                                                            |
   |                                   |    -  Editing a connection.                                                                                                                                     |
   |                                   |    -  Connecting to a database while creating the database when no other database is connected in that connection profile.                                      |
   |                                   |    -  Connecting to a database when no other database is connected in that connection profile.                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No                                | Selecting this option will not allow you to login to Data Studio once the password has expired. A message displays informing you that the password has expired. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, then Data Studio displays the **Process is running** dialog box.

#. Click **Force Restart** to discard operations and restart Data Studio. Click **OK** to continue performing operations.

   .. note::

      The default value is **Yes**.

.. _en-us_topic_0000001234042179__en-us_topic_0185264975_section8272203611226:

Security Disclaimer
-------------------

You can enable/disable to display the security disclaimer for any unsecured connection/file operations.

Follow the steps below to modify the display of security disclaimer:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Security > Security Disclaimer**.

   The **Security Disclaimer** pane is displayed.

#. Select the required option. Refer table below to understand the customization options available:

   +---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option  | Outcome                                                                                                                                                                                                                            |
   +=========+====================================================================================================================================================================================================================================+
   | Enable  | Selecting this option displays the security disclaimer each time you try to establish an unsecure connection or perform a file operation.                                                                                          |
   +---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Disable | Selecting this option will not display the security disclaimer while establishing an unsecure connection or performing a file operation. You need to agree to the security implications that may arise due to unsecure connection. |
   +---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The **Restart** **Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, then Data Studio displays the **Process is running** dialog box.

#. Click **Force Restart** to discard operations and restart Data Studio. Click **OK** to continue performing operations.

   .. note::

      Click **Restore Defaults** from **Security Disclaimer** pane to reset to default values. The default value is **Enable**.
