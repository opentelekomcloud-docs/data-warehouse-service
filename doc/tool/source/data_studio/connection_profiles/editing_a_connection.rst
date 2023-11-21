:original_name: DWS_DS_36.html

.. _DWS_DS_36:

Editing a Connection
====================

Follow the steps below to edit the database connection properties:

#. In the **Object Browser** pane, right-click the selected connection name and select **Edit** **Connection**.

   Editing an active connection will require closing the connection and then reopening the connection with the new properties. A warning message about connections being reset is shown.

   The **Edit Connection** dialog box is displayed.

#. Click **OK** to proceed or **Cancel** to exit the operation.

   .. note::

      The **Connection Name** field cannot be modified.

#. Edit the connection parameters. Connection parameters are explained in :ref:`Adding a Connection <dws_ds_34>`.

#. Click **OK** to save the updated connection information.

   .. note::

      -  You can click **Clear** to clear all fields in the **Edit Database Connection** dialog box.
      -  If you click **OK** without modifying any connection parameters, a dialog box is displayed, indicating that the modification is not saved. After the connection parameters are modified, a dialog box is displayed.
      -  You can still log in to Data Studio even if the password has expired, but a message indicating that some operations may not be performed normally will be displayed. For details, see :ref:`Password Expiry <en-us_topic_0000001234042179__en-us_topic_0185264975_section56881736153111>`.
      -  Refer to :ref:`Canceling the Connection <en-us_topic_0000001188521154__en-us_topic_0185264624_section12413942172715>` to cancel the connection.

   If SSL is not enabled, a **Connection Security Alert** dialog box is displayed.

#. Click **Continue** to proceed with insecure connections or click **Cancel** to return to the **Edit Connection** dialog box to enable SSL.

   .. note::

      **Do not show again** option is used to hide the **Connection Security Alert** dialog box for subsequent connections.

   The **Remove** **Server Confirmation** is displayed to confirm closing databases for the edited connection.

#. Click **Yes** to proceed to updating the connection information and reconnecting the connection with the updated parameters.

   The status bar displays the status of the completed operation.
