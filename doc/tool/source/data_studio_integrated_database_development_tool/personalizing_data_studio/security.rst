:original_name: DWS_DS_142.html

.. _DWS_DS_142:

Security
========

This section describes how to customize the display of passwords and security disclaimers.

.. _en-us_topic_0000001098673336__en-us_topic_0185264975_section1327033652211:

Saving Password Permanently
---------------------------

You can configure whether to display the option of saving password permanently in the **Connection** pane.

Perform the following steps to modify the display of the option of saving password permanently:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Security** > **Password**.

   The **Password** pane is displayed.

#. Select the required option. For details, see the following table.

   +--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option | Description                                                                                                                                                        |
   +========+====================================================================================================================================================================+
   | Yes    | The option of saving password permanently is displayed in the **Save Password** drop-down list in the **Connection** pane.                                         |
   +--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No     | The option of saving password permanently is displayed in the **Save Password** drop-down list in the **Connection** pane and the saved passwords will be deleted. |
   +--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, the **Process is running** dialog box is displayed.

   Click **Force Restart** to cancel the operations and restart Data Studio.

#. Restart Data Studio, or click **OK** to proceed with the current operations.

   .. note::

      Click **Restore Defaults** in **Password** to restore the default value. The default value is **No**.

.. _en-us_topic_0000001098673336__en-us_topic_0185264975_section56881736153111:

Password Expiry
---------------

This topic describes how to continue or stop using Data Studio after password expires using the Password setting.

Perform the following steps to modify the behavior of Data Studio upon password expiry:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Security > Password**.

   The **Password** pane is displayed.

#. Select the required option. For details, see the following table.

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option                            | Description                                                                                                                                              |
   +===================================+==========================================================================================================================================================+
   | Yes                               | You can log in to Data Studio after the password expired.                                                                                                |
   |                                   |                                                                                                                                                          |
   |                                   | .. note::                                                                                                                                                |
   |                                   |                                                                                                                                                          |
   |                                   |    A message is displayed notifying you that the password has expired and that some operations may not be performed properly in the following scenarios: |
   |                                   |                                                                                                                                                          |
   |                                   |    -  Creating a connection                                                                                                                              |
   |                                   |    -  Editing a connection                                                                                                                               |
   |                                   |    -  Connecting to a database during its creation when no other database is connected in that connection profile                                        |
   |                                   |    -  Connecting to a database when no other database is connected in that connection profile                                                            |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No                                | You cannot log in to Data Studio after the password expired. A message is displayed notifying you that the password has expired.                         |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, the **Process is running** dialog box is displayed.

#. Click **Force Restart** to cancel the operations and restart Data Studio. Click **OK** to proceed with the current operations.

   .. note::

      The default value is **Yes**.

.. _en-us_topic_0000001098673336__en-us_topic_0185264975_section8272203611226:

Security Disclaimer
-------------------

You can configure whether to display the security disclaimer for any insecure connection or file operation.

Perform the following steps to modify the display of the security disclaimer:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Security** > **Security Disclaimer**.

   The **Security Disclaimer** pane is displayed.

#. Select the required option. For details, see the following table.

   +---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option  | Description                                                                                                                                                                                                           |
   +=========+=======================================================================================================================================================================================================================+
   | Enable  | The security disclaimer is displayed each time you try to establish an insecure connection or perform a file operation.                                                                                               |
   +---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Disable | The security disclaimer is not displayed each time you try to establish an insecure connection or perform a file operation. You need to agree to the security implications that may arise due to insecure connection. |
   +---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, the **Process is running** dialog box is displayed.

#. Click **Force Restart** to cancel the operations and restart Data Studio. Click **OK** to proceed with the current operations.

   .. note::

      Click **Restore Defaults** in **Security Disclaimer** to restore the default value. The default value is **Enable**.
