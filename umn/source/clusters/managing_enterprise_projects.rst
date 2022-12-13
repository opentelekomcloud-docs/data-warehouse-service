:original_name: dws_01_0113.html

.. _dws_01_0113:

Managing Enterprise Projects
============================

An enterprise project is a cloud resource management mode. Enterprise Management provides users with comprehensive management in cloud-based resources, personnel, and permissions. Unlike common management consoles that feature independent control and configuration of cloud products, the Enterprise Management console is oriented to resource management. It helps enterprises with cloud-based management in resources, personnel, and permissions in the hierarchy of companies, departments, and projects.

Binding an Enterprise Project
-----------------------------

You can select an enterprise project during cluster creation to associate it with the cluster. For details, see :ref:`Creating a Cluster <dws_01_0019>`. The **Enterprise Project** drop-down list displays the projects you created. In addition, the system has a built-in enterprise project (**default**). If you do not select an enterprise project for the cluster, the default project is used.

During cluster creation, if the cluster is successfully bound to an enterprise project, the cluster will be successfully created. If the binding fails, the system sends an alarm and the cluster fails to be created.

Snapshots of a cluster retain the association between the cluster and its enterprise project. When the cluster is restored, the association is also restored.

When you delete a cluster, the association between the cluster and its enterprise project is automatically deleted.

Viewing Enterprise Projects
---------------------------

After a cluster is created, you can view the associated enterprise project in the cluster list and **Basic Information** page. You can query only the cluster resources of the project on which you have the access permission.

In the cluster list on the **Clusters** page, view the enterprise project to which the cluster belongs.


.. figure:: /_static/images/en-us_image_0000001180320431.png
   :alt: **Figure 1** Viewing the enterprise project

   **Figure 1** Viewing the enterprise project

In the cluster list, find the target cluster and click the cluster name. The **Basic Information** page is displayed, on which you can view the enterprise project associated with the cluster. Click the enterprise project name to view and edit it on the Enterprise Management console.


.. figure:: /_static/images/en-us_image_0000001180440371.png
   :alt: **Figure 2** Viewing the enterprise project

   **Figure 2** Viewing the enterprise project

When querying the resource list of a specified project on the Enterprise Management console, you can also query the GaussDB(DWS) resources.

Searching for Clusters by Enterprise Project
--------------------------------------------

Log in to the GaussDB(DWS) management console, choose **Clusters**, click **All projects** above the cluster list, and select the required project name from the drop-down list to view all clusters associated with the project.


.. figure:: /_static/images/en-us_image_0000001134560786.png
   :alt: **Figure 3** Search by enterprise projects

   **Figure 3** Search by enterprise projects

Migrating a Cluster to or Out of an Enterprise Project
------------------------------------------------------

A GaussDB(DWS) cluster can be associated with only one enterprise project. After a cluster is created, you can migrate it from its current enterprise project to another one on the Enterprise Management console, or migrate the cluster from another enterprise project to a specified enterprise project. After the migration, the cluster is associated with the new enterprise project. The association between the cluster and the original enterprise project is automatically released. For details, see "Resource Management > Managing Enterprise Project Resources" in the *Enterprise Management User Guide*.
