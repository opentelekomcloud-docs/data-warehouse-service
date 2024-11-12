:original_name: dws_01_0105.html

.. _dws_01_0105:

Managing Tags
=============

This section describes how to search for clusters based on tags and how to add, modify, and delete tags.

.. _en-us_topic_0000001924569400__section77515910494:

Adding a Tag to a Cluster
-------------------------

#. On the **Clusters** > **Dedicated Clusters** page, click the name of the cluster to which a tag is to be added, and choose **Tag**.
#. Click **Add Tag**.
#. Configure tag information in the **Add Tag** dialog box. The value of a key cannot be left blank.

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
      |                       | -  Enter a tag key in the text box. The tag key can contain a maximum of 128 characters and cannot be an empty string. It cannot start with **\_sys\_**.                                                                                                                                                                                                                                       |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    Only letters, digits, spaces, and the following characters are allowed: \_ . : = + - @                                                                                                                                                                                                                                                                                                      |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    .. note::                                                                                                                                                                                                                                                                                                                                                                                   |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |       A key must be unique in a given cluster.                                                                                                                                                                                                                                                                                                                                                 |                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Tag value             | You can:                                                                                                                                                                                                                                                                                                                                                                                       | value01               |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       | -  Select a predefined tag value or resource tag value from the drop-down list of the text box.                                                                                                                                                                                                                                                                                                |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       | -  Enter a tag value in the text box. The tag key can contain a maximum of 255 characters and cannot be an empty string.                                                                                                                                                                                                                                                                       |                       |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                |                       |
      |                       |    Only letters, digits, spaces, and the following characters are allowed: \_ . : = + - @                                                                                                                                                                                                                                                                                                      |                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

.. _en-us_topic_0000001924569400__section20922320396:

Searching for Clusters Based on Tags
------------------------------------

You can quickly locate a tagged cluster using tags.

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters** > **Dedicated Clusters**.

#. Click the search box above the cluster list and select the **Resource Tag** filter.

#. Click the tag key to be searched for and select the corresponding tag value. Click the search box again to add more tag filters.

   Search by tag supports only the keys and values that exist in the drop-down list. If no tag key or value is available, create a tag for the cluster. For details, see :ref:`Adding a Tag to a Cluster <en-us_topic_0000001924569400__section77515910494>`.

#. Click **Search**. The target cluster will be displayed in the cluster list.

Modifying a Tag
---------------

#. On the **Clusters** > **Dedicated Clusters** page, click the name of the cluster to which a tag is to be added, and choose **Tag**.
#. Locate the row that contains the tag to be modified, and click **Edit** in the **Operation** column. The **Edit Tag** dialog box is displayed.
#. Enter the new key value in the **Value** text box.
#. Click **OK**.

Deleting a Tag
--------------

#. On the **Clusters** > **Dedicated Clusters** page, click the name of the cluster from which a tag is to be deleted, and click the **Tags** tab.
#. Locate the row that contains the tag to be deleted, click **Delete** in the **Operation** column. The **Delete Tag** dialog box is displayed.
#. Click **Yes** to delete the tag.
