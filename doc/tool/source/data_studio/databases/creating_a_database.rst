:original_name: DWS_DS_41.html

.. _DWS_DS_41:

Creating a Database
===================

A relational database is a database that has a set of tables which is manipulated in accordance with the relational model of data. It contains a set of data objects used to store, manage, and access data. Examples of such data objects are tables, views, indexes, functions and so on.

Follow the steps below to create a database:

#. In the **Object Browser** pane, right-click the selected **Databases** group and select **Create Database**.

   .. note::

      This operation can be performed only when there is at least one active database.

   A **Create Database** dialog box is displayed prompting you to provide the necessary information to create the database.

#. Enter the database name. Refer to the server manual for database naming rules.

#. Select the required type of encoding character set from the **Database Encoding** drop-down list.

   The database supports **UTF-8**, **GBK**, **SQL_ASCII**, and **LATIN1** types of encoding character sets. Creating the database with other encoding character sets may result in erroneous operations.

#. Select the **Connect to the DB** check box and click **OK**.

   The status bar displays the status of the completed operation.

   You can view the created database in the **Object Browser**. The system related schema present in the server is automatically added to the new database.

   .. note::

      Data Studio allows you to login even if the password has expired with a message informing that some operations may not work as expected when no other database is connected in that connection profile. Refer to :ref:`Password Expiry <en-us_topic_0000001234042179__en-us_topic_0185264975_section56881736153111>` for information to change this behavior.

.. _en-us_topic_0000001233922161__en-us_topic_0185264253_section143932723614:

Cancelling Connection
---------------------

Follow the steps below to cancel the connection operation:

#. Double-click the status bar to open the **Progress View** tab.

#. In the **Progress View** tab, click |image1|.

#. In the **Cancel Operation** dialog box, click **Yes**.

   The status bar displays the status of the cancelled operation.

.. |image1| image:: /_static/images/en-us_image_0000001188362858.jpg
