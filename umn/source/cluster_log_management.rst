:original_name: dws_01_01111.html

.. _dws_01_01111:

Cluster Log Management
======================

Overview
--------

Cluster logs are collected and sent to Log Tank Service (LTS). You can check or dump the collected cluster logs on LTS.

Currently, the following log types are supported:

-  CN logs
-  DN logs
-  OS messages logs
-  Audit logs

.. note::

   -  Cluster log management depends on LTS.
   -  Only 8.1.1.300 and later versions support cluster log management.

Enabling LTS
------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. Choose **Logs**.

   |image1|

#. On the **Logs** tab, enable LTS. If LTS is enabled for the first time, the following dialog box will be displayed. Confirm the information and click **Yes**.

   |image2|

   .. note::

      -  If LTS has been enabled and authorized to create an agency, no authorization is required when LTS is enabled again.
      -  By default, only cloud accounts or users with Security Administrator permissions can query and create agencies. IAM users under an account do not have the permission to query or create agencies by default. Contact a user with that permission and complete the authorization on the current page.

#. Check the LTS status.

   |image3|

.. _en-us_topic_0000001466914082__section1600157575:

Checking Cluster Logs
---------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. Choose **Logs**.

#. On the **Logs** tab, click **View Log** in the **Operation** column of a log type to go to the Log Tank Service (LTS) page and view logs.

   |image4|

Disabling LTS
-------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. Choose **Logs**.

#. Toggle off the LTS switch.

   |image5|

#. Click **OK** in the dialog box.

   |image6|

.. |image1| image:: /_static/images/en-us_image_0000001466914210.png
.. |image2| image:: /_static/images/en-us_image_0000001517754285.png
.. |image3| image:: /_static/images/en-us_image_0000001466594930.png
.. |image4| image:: /_static/images/en-us_image_0000001517754281.png
.. |image5| image:: /_static/images/en-us_image_0000001466594934.png
.. |image6| image:: /_static/images/en-us_image_0000001466754590.png
