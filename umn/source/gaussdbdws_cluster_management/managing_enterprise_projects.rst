:original_name: dws_01_0113.html

.. _dws_01_0113:

Managing Enterprise Projects
============================

An enterprise project is a cloud resource management mode. Enterprise Management provides users with comprehensive management in cloud-based resources, personnel, and permissions. The Enterprise Management console differs from typical management consoles as it focuses on resource management rather than independent control and configuration of cloud products. It assists enterprises in managing resources, personnel, and permissions within the hierarchy of companies, departments, and projects.

Binding an Enterprise Project
-----------------------------

You can select an enterprise project during cluster creation to associate it with the cluster. For details, see :ref:`Creating a GaussDB(DWS) Storage-Compute Coupled Cluster <dws_01_0019>`. The **Enterprise Project** drop-down list displays the projects you created. In addition, the system has a built-in enterprise project (**default**). If you do not select an enterprise project for the cluster, the default project is used.

During cluster creation, if the cluster is successfully bound to an enterprise project, the cluster will be successfully created. If the binding fails, the system sends an alarm and the cluster fails to be created.

Snapshots of a cluster retain the association between the cluster and its enterprise project. When the cluster is restored, the association is also restored.

When you delete a cluster, the association between the cluster and its enterprise project is automatically deleted.

Viewing Enterprise Projects
---------------------------

After a cluster is created, you can view the associated enterprise project in the cluster list and **Cluster Information** page. You can query only the cluster resources of the project on which you have the access permission.

-  In the cluster list on the **Clusters** page, view the enterprise project to which the cluster belongs.
-  In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed, on which you can view the enterprise project associated with the cluster. Click the enterprise project name to view and edit it on the Enterprise Management console.

-  When querying the resource list of a specified project on the Enterprise Management console, you can also query the GaussDB(DWS) resources.

Searching for Clusters by Enterprise Project
--------------------------------------------

Log in to the GaussDB(DWS) console and choose **Dedicated Clusters** > **Clusters**. Click the search box above the cluster list and select **Enterprise Project**. Enter the project name and click the search button to view all clusters associated with the project.

Migrating a Cluster to or Out of an Enterprise Project
------------------------------------------------------

A GaussDB(DWS) cluster can be associated with only one enterprise project. After a cluster is created, you can migrate it from its current enterprise project to another one on the Enterprise Management console, or migrate the cluster from another enterprise project to a specified enterprise project. After the migration, the cluster is associated with the new enterprise project. The association between the cluster and the original enterprise project is automatically released. For details, see "Resource Management" > "Managing Enterprise Project Resources" in the *Enterprise Management User Guide*.

Enterprise Project-Level Authorization
--------------------------------------

If permissions preset in the system cannot meet requirements, you can customize policies and grant the policies to user groups for refined access control. As an independent managed object, the enterprise project can be bound to a user group, and the customized policy can be granted to the user group. This implements refined authorization at the enterprise project level.

#. Log in to the IAM console and create a custom policy.

   For details, see the *Identity and Access Management User Guide*.

   Refer to the following to create the policy:

   -  Use the IAM administrator account, that is, the user in the admin user group, because only the IAM administrator has the permissions to create users and user groups and modify user group permissions.

   -  GaussDB(DWS) is a project-level service, so its **Scope** must be set to **Project-level services**. If this policy is required to take effect for multiple projects, authorization is required to each project.

   -  Some GaussDB(DWS) policy templates are preconfigured on IAM. When creating a custom policy, you can select one of the following templates and modify the policy authorization statement based on the template:

      -  **DWS FullAccess**: all execution permissions for GaussDB(DWS)
      -  **DWS ReadOnlyAccess**: read-only permission for GaussDB(DWS)
      -  **DWS Administrator**: all execution permissions for GaussDB(DWS)
      -  **DWS Database Access**: Users granted this permission can generate temporary database user credentials based on IAM users to connect to databases in the data warehouse clusters.

   -  You can add permissions corresponding to GaussDB(DWS) operations or RESTful APIs listed in :ref:`List of Supported Actions <en-us_topic_0000002168065764__section89181381475>` to the action list in the policy authorization statement, so that the policy can obtain the permissions.

      For example, if **dws:cluster:create** is added to the action list of a policy statement, the policy has the permission to create or restore clusters.

   -  If you want to use other services, grant related operation permissions on these services. For details, see the help documents of related services.

      For example, when creating a data warehouse cluster, you need to configure the VPC to which the cluster belongs. To obtain the VPC list, add permission **vpc:*:get\*** to the policy statement.

   Policy example:

   -  Example in which multiple operation permissions are supported

      The following policy has the permissions to create/restore/restart/delete a cluster, set security parameters, and reset passwords.

      .. code-block::

         {
               "Version": "1.1",
               "Statement": [
                     {
                           "Effect": "Allow",
                           "Action": [
                                 "dws:cluster:create",
                                 "dws:cluster:restart",
                                 "dws:cluster:delete",
                                 "dws:cluster:setParameter",
                                 "dws:cluster:resetPassword",
                                 "ecs:*:get*",
                                 "ecs:*:list*",
                                 "vpc:*:get*",
                                 "vpc:*:list*"
                           ]
                     }
               ]
         }

   -  Example of wildcard (*) usage

      The following policy has all operation permissions on GaussDB(DWS) snapshots.

      .. code-block::

         {
               "Version": "1.1",
               "Statement": [
                     {
                           "Effect": "Allow",
                           "Action": [
                                 "dws:snapshot:*",
                                 "ecs:*:get*",
                                 "ecs:*:list*",
                                 "vpc:*:get*",
                                 "vpc:*:list*"
                           ]
                     }
               ]
         }

#. Click the username in the upper right corner of the management console and select **Enterprise Management** from the drop-down list to enter the Enterprise Management console.

#. Choose **Personnel Management > User Group Management** in the left navigation tree. Then, create a user group and add users to it, add the user group to a project, and grant the newly created custom policy to the group so that users in the group can obtain the permissions defined by the policy.

   For details, see "Project Management > Personnel Management > Managing User Groups in an Enterprise Project" in the *Enterprise Management User Guide*.
