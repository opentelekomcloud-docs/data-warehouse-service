:original_name: dws_01_0152.html

.. _dws_01_0152:

Modifying Database Parameters
=============================

After a cluster is created, you can modify the cluster's database parameters as required. On the GaussDB(DWS) console, you can configure common database parameters. For details, see :ref:`Modifying Parameters <en-us_topic_0000001952008165__section10522114017574>`. You can also view the parameter modification history. For details, see :ref:`Viewing Parameter Change History <en-us_topic_0000001952008165__section1987072615553>`. You can run SQL commands to view or set other database parameters. For details, see **Setting Configuration Parameters** in the *Data Warehouse Service Database Development Guide*.

Prerequisites
-------------

You can modify parameters only when no task is running in the cluster.

.. _en-us_topic_0000001952008165__section10522114017574:

Modifying Parameters
--------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.

#. Click the **Parameters** tab and modify the parameter values. Then click **Save**.

   |image1|

#. In the **Modification Preview** dialog box, confirm the modifications and click **Save**.

#. You can determine whether you need to restart the cluster after parameter modification based on the **Restart Cluster** column.

   |image2|

   .. note::

      -  If cluster restart is not required for a parameter, the parameter modification takes effect immediately.
      -  If cluster restart is required for parameter modifications to take effect, the new parameter values will be displayed on the page after the modification, but will not take effect until the cluster is restarted. Before a restart, the cluster status is **To be restarted**, and some O&M operations are disabled.

.. _en-us_topic_0000001952008165__section1987072615553:

Viewing Parameter Change History
--------------------------------

Perform the following steps to view the parameter modification history and check whether the modifications have taken effect:

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.

#. Click the **Modify Records** tab.

   |image3|

   .. note::

      -  If a parameter can take effect immediately after modification, its status will change to **Synchronized** after you modify it.
      -  If a parameter can take effect only after a cluster restart, its status will change to **To be restarted** after you modify it. You can click the expansion icon on the left to view the parameters that have not taken effect. After the cluster is restarted, the status of the record will change to **Synchronized**.

#. By default, only the change history within a specified period is displayed. To check the entire change history of a parameter, search for it in the search box in the upper right corner.

.. _en-us_topic_0000001952008165__section926416313488:

Parameter Description
---------------------

There are a large number of database parameters. You can search for and view the parameters on the **Parameter Modification** page. For details, see :ref:`Modifying Parameters <en-us_topic_0000001952008165__section10522114017574>`. The default values of the parameters are for reference only. For more information, see "Setting GUC Parameters".

.. |image1| image:: /_static/images/en-us_image_0000001924569748.png
.. |image2| image:: /_static/images/en-us_image_0000001924569744.png
.. |image3| image:: /_static/images/en-us_image_0000001924569752.png
