:original_name: DWS_DS_36.html

.. _DWS_DS_36:

Editing a Connection
====================

Perform the following steps to edit the properties of a database connection.

#. In the **Object Browser** pane, right-click the selected connection name and select **Edit Connection**.

   To edit an active connection, you need to disable the connection and then open the connection with the new properties. A warning message about connection resetting is displayed.

   The **Edit Connection** dialog box is displayed.

#. Click **OK** to proceed or **Cancel** to exit the operation.

   .. note::

      The **Connection Name** cannot be modified.

#. Edit the connection parameters. For details, see :ref:`Adding a Connection <dws_ds_34>`.

#. Click **OK** to save the updated connection information.

   .. note::

      -  You can click **Clear** to clear all fields in the **Edit Database Connection** dialog box.
      -  If you click **OK** without modifying any connection parameters, a dialog box is displayed, indicating that the modification is not saved. After the connection parameters are modified, a dialog box is displayed.
      -  You can still log in to Data Studio even if the password has expired, but a message indicating that some operations may not be performed normally will be displayed. For details, see :ref:`Password Expiry <en-us_topic_0000001098673336__en-us_topic_0185264975_section56881736153111>`.
      -  Cancel the connection by referring to :ref:`Canceling the Connection <en-us_topic_0000001145713081__en-us_topic_0185264624_section12413942172715>`.

   If SSL is not enabled, a **Connection Security Alert** dialog box is displayed.

#. Click **Continue** to proceed with insecure connections or click **Cancel** to return to the **Edit Connection** dialog box to enable SSL.

   .. note::

      If you select **Do not show again**, the **Connection Security Alert** dialog box is not displayed for the subsequent connections of logged Data Studio instances.

   A dialog box is displayed asking users to confirm whether the database whose connection has been edited is deleted.

#. Click **Yes** to update the connection information and reconnect the connection with the updated parameters.

   The status of the completed operation is displayed in the status bar.
