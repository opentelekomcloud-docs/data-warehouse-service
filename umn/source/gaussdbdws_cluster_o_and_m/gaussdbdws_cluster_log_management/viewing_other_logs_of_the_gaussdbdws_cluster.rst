:original_name: dws_01_01111.html

.. _dws_01_01111:

Viewing Other Logs of the GaussDB(DWS) Cluster
==============================================

Overview
--------

Cluster logs are collected and sent to Log Tank Service (LTS). You can check or dump the collected cluster logs on LTS.

The following log types are supported: CN logs, DN logs, OS messages logs, audit logs, cms logs, gtm logs, Roach client logs, Roach server logs, upgrade logs, and scale-out logs.

.. note::

   -  Only 8.1.1.300 and later versions support cluster log management.
   -  Only 8.3.0 and later versions support CMS logs, GTM logs, Roach client logs, Roach server logs, scaling logs, and upgrade logs.

Enabling LTS
------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. Choose **Logs**.

   |image1|

#. On the **Logs** tab, enable LTS. If LTS is enabled for the first time, the following dialog box will be displayed. Confirm the information and click **Yes**.

   .. note::

      -  If LTS has been enabled and authorized to create an agency, no authorization is required when LTS is enabled again.
      -  By default, only cloud accounts or users with **Security Administrator** permissions can query and create agencies. IAM users under an account do not have the permission to query or create agencies by default. Contact a user with that permission and complete the authorization on the current page.
      -  When interconnecting with LTS, you need to grant LTS-related permission policies **(LTS Admin**, **LTS Administrator**, **LTS FullAccess**, and **LTS ReadOnlyAccess**) to users.

.. _en-us_topic_0000002167905796__section1600157575:

Checking Cluster Logs
---------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. Choose **Logs**.

#. On the **Logs** tab, click **View Log** in the **Operation** column of a log type to go to the Log Tank Service (LTS) page and view logs.

   |image2|

Disabling LTS
-------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.
#. Click the name of the target cluster. Choose **Logs**.
#. Toggle off the LTS switch.
#. Click **OK** in the dialog box.

.. |image1| image:: /_static/images/en-us_image_0000002167906100.png
.. |image2| image:: /_static/images/en-us_image_0000002203426797.png
