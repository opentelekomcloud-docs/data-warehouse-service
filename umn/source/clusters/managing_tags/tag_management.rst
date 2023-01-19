:original_name: dws_01_0105.html

.. _dws_01_0105:

Tag Management
==============

This section describes how to search for clusters based on tags and how to add, modify, and delete tags for clusters.

.. _en-us_topic_0000001180440147__section77515910494:

Adding a Tag to a Cluster
-------------------------

#. On the **Clusters** page, click the name of the cluster to which a tag is to be added, and click the **Tags** tab.


   .. figure:: /_static/images/en-us_image_0000001134400956.png
      :alt: **Figure 1** Tags page

      **Figure 1** Tags page

#. Click **Add Tag**.

#. Configure the tag parameters in the displayed dialog box.


   .. figure:: /_static/images/en-us_image_0000001134400958.png
      :alt: **Figure 2** Adding a tag to a cluster

      **Figure 2** Adding a tag to a cluster

   .. table:: **Table 1** Tag parameters

      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                                                                                                                    | Example Value         |
      +=======================+================================================================================================================================================================================================================================================================================================================================================================================================+=======================+
      | Tag key               | You can:                                                                                                                                                                                                                                                                                                                                                                                       | key01                 |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       | -  Select a predefined tag key or an existing resource tag key from the drop-down list of the text box.                                                                                                                                                                                                                                                                                        |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    .. note::                                                                                                                                                                                                                                                                                                                                                                                   |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |       To add a predefined tag, you need to create one on TMS and select it from the drop-down list of **Tag key**. You can click **View predefined tags** to enter the **Predefined Tags** page of TMS. Then, click **Create Tag** to create a predefined tag. For more information, see "Management > Predefined Tags > Creating Predefined Tags" in the *Tag Management Service User Guide*. |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       | -  Enter a tag key in the text box. The tag key can contain a maximum of 36 characters and cannot be an empty string.                                                                                                                                                                                                                                                                          |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    Only digits, letters, underscores (_), and hyphens (-) are allowed.                                                                                                                                                                                                                                                                                                                         |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    .. note::                                                                                                                                                                                                                                                                                                                                                                                   |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |       A key must be unique in a given cluster.                                                                                                                                                                                                                                                                                                                                                 |                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Tag value             | You can:                                                                                                                                                                                                                                                                                                                                                                                       | value01               |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       | -  Select a predefined tag value or resource tag value from the drop-down list of the text box.                                                                                                                                                                                                                                                                                                |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       | -  Enter a tag value in the text box. The tag key can contain a maximum of 43 characters and cannot be an empty string.                                                                                                                                                                                                                                                                        |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    Only digits, letters, underscores (_), periods (.), and hyphens (-) are allowed.                                                                                                                                                                                                                                                                                                            |                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

.. _en-us_topic_0000001180440147__section20922320396:

Searching for Clusters Based on Tags
------------------------------------

You can quickly locate a tagged cluster using tags.

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**.

#. Click **Search by Tag** on the upper right of the cluster list to expand the tab page.


   .. figure:: /_static/images/en-us_image_0000001180440309.png
      :alt: **Figure 3** Search by Tag

      **Figure 3** Search by Tag

#. In the **Search by Tag** area, click the **Tag Key** text box to select a tag key from the drop-down list and then click the **Tag Value** text box to select the corresponding tag value.

   You can only enter a tag key or value that exists in the drop-down list. If no tag key or value is available, create a tag for the cluster. For details, see :ref:`Adding a Tag to a Cluster <en-us_topic_0000001180440147__section77515910494>`.

#. Click |image1| to add the selected tag to the area under the text boxes.

   -  Select another tag in the text boxes and click |image2| to generate a tag combination for cluster search. You can add a maximum of 10 tags to search for data warehouse clusters. If you specify more than one tag, clusters containing all the specified tags will be displayed.

   -  To delete an existing tag, click |image3| next to the tag.

   -  You can click **Reset** to clear all added tags.


      .. figure:: /_static/images/en-us_image_0000001180320401.png
         :alt: **Figure 4** Adding the tag key and value

         **Figure 4** Adding the tag key and value

#. Click **Search**. The target cluster will be displayed in the cluster list.

Modifying a Tag
---------------

#. On the **Clusters** page, click the name of the cluster for which a tag is to be modified, and click the **Tags** tab.

#. Locate the row that contains the tag to be modified, and click **Edit** in the **Operation** column. The **Edit Tag** dialog box is displayed.


   .. figure:: /_static/images/en-us_image_0000001134400952.png
      :alt: **Figure 5** Editing a tag

      **Figure 5** Editing a tag

#. Enter the new key value in the **Value** text box.

#. Click **OK**.

Deleting a Tag
--------------

#. On the **Clusters** page, click the name of the cluster from which a tag is to be deleted, and click the **Tags** tab.

#. Locate the row that contains the tag to be deleted, click **Delete** in the **Operation** column. The **Delete Tag** dialog box is displayed.


   .. figure:: /_static/images/en-us_image_0000001134560732.png
      :alt: **Figure 6** Deleting a tag

      **Figure 6** Deleting a tag

#. Click **Yes** to delete the tag.

.. |image1| image:: /_static/images/en-us_image_0000001180320375.png
.. |image2| image:: /_static/images/en-us_image_0000001180320375.png
.. |image3| image:: /_static/images/en-us_image_0000001134560752.png
