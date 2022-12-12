:original_name: dws_02_0056.html

.. _dws_02_0056:

Permissions Policies and Supported Actions
==========================================

This section describes fine-grained permissions management for your GaussDB(DWS) service using IAM. You can skip this section if your cloud account already satisfies your needs.

By default, new IAM users do not have permissions assigned. You need to add the users to one or more groups, and attach permissions policies or roles to these groups. Users inherit permissions from the groups to which they are added and can perform specified operations on cloud services based on the permissions.

You can grant users permissions by using roles and policies. Roles are provided by IAM to define service-based permissions depending on users' job responsibilities. Policies define API-based permissions for operations on specific resources under certain conditions, allowing for more fine-grained, secure access control of cloud resources.

.. note::

   Policy-based authorization is useful if you want to allow or deny the access to an API.

An account has all of the permissions required to call all APIs, but IAM users must have the required permissions specifically assigned. The permissions required for calling an API are determined by the actions supported by the API. Only users who have been granted permissions allowing the actions can call the API successfully. For example, if an IAM user wants to query the GaussDB(DWS) cluster list using an API, the user must have been granted permissions that allow the **dws:openAPICluster:list** action.

Supported Actions
-----------------

DWS provides system-defined policies that can be directly used in IAM. Database administrators can also create custom policies and use them to supplement system-defined policies, implementing more refined access control. Actions supported by policies are specific to APIs. The following are common concepts related to policies:

-  **Permissions**: Allow or deny operations on specified resources under specific conditions.
-  **APIs**: RESTful APIs that can be called in a custom policy.
-  **Actions**: Added to a custom policy to control permissions for specific operations.
-  **IAM or enterprise projects**: Type of projects for which an action will take effect. Policies that contain actions supporting both IAM and enterprise projects can be assigned to user groups and take effect in both IAM and Enterprise Management. Policies that only contain actions supporting IAM projects can be assigned to user groups and only take effect for IAM. Such policies will not take effect if they are assigned to user groups in Enterprise Management.

   .. note::

      The check mark (Y) indicates that an action takes effect. The cross mark (x) indicates that an action does not take effect.

GaussDB(DWS) supports the following actions that can be defined in custom policies:

-  :ref:`Managing Clusters <en-us_topic_0000001134564664__section15829194192310>`
-  :ref:`Managing Snapshots <en-us_topic_0000001134564664__section936022411233>`
-  :ref:`Managing Tags <en-us_topic_0000001134564664__section1916320474239>`

.. _en-us_topic_0000001134564664__section15829194192310:

Managing Clusters
-----------------

+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Permissions                                  | APIs                                                         | Actions                             | IAM Project | Enterprise Project |
+==============================================+==============================================================+=====================================+=============+====================+
| Creating clusters                            | POST /v1.0/{project_id}/clusters                             | dws:openAPICluster:create           | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Querying the cluster list                    | GET /v1.0/{project_id}/clusters                              | dws:openAPICluster:list             | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Querying cluster details                     | GET /v1.0/{project_id}/clusters/{cluster_id}                 | dws:openAPICluster:getDetail        | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Querying the node type                       | GET /v2/{project_id}/node-types                              | dws:openAPIFlavors:get              | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Deleting clusters                            | DELETE /v1.0/{project_id}/clusters/{cluster_id}              | dws:openAPICluster:delete           | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Restarting clusters                          | POST /v1.0/{project_id}/clusters/{cluster_id}/restart        | dws:openAPICluster:restart          | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Scales out a cluster.                        | POST /v1.0/{project_id}/clusters/{cluster_id}/resize         | dws:cluster:scaleOutOrOpenAPIResize | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+
| Resetting the cluster administrator password | POST /v1.0/{project_id}/clusters/{cluster_id}/reset-password | dws:openAPICluster:resetPassword    | Y           | Y                  |
+----------------------------------------------+--------------------------------------------------------------+-------------------------------------+-------------+--------------------+

.. _en-us_topic_0000001134564664__section936022411233:

Managing Snapshots
------------------

+----------------------------+---------------------------------------------------------+-----------------------------+-------------+--------------------+
| Permissions                | APIs                                                    | Actions                     | IAM Project | Enterprise Project |
+============================+=========================================================+=============================+=============+====================+
| Creating snapshots         | POST /v1.0/{project_id}/snapshots                       | dws:openAPISnapshot:create  | Y           | Y                  |
+----------------------------+---------------------------------------------------------+-----------------------------+-------------+--------------------+
| Querying the snapshot list | GET /v1.0/{project_id}/snapshots                        | dws:openAPISnapshot:list    | Y           | Y                  |
+----------------------------+---------------------------------------------------------+-----------------------------+-------------+--------------------+
| Querying snapshot details  | GET /v1.0/{project_id}/snapshots/{snapshot_id}          | dws:openAPISnapshot:detail  | Y           | Y                  |
+----------------------------+---------------------------------------------------------+-----------------------------+-------------+--------------------+
| Deleting snapshots         | DELETE /v1.0/{project_id}/snapshots/{snapshot_id}       | dws:openAPISnapshot:delete  | Y           | Y                  |
+----------------------------+---------------------------------------------------------+-----------------------------+-------------+--------------------+
| Restoring clusters         | POST /v1.0/{project_id}/snapshots/{snapshot_id}/actions | dws:openAPISnapshot:restore | Y           | Y                  |
+----------------------------+---------------------------------------------------------+-----------------------------+-------------+--------------------+

.. _en-us_topic_0000001134564664__section1916320474239:

Managing Tags
-------------

+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
| Permissions                                  | APIs                                                        | Actions                         | IAM Project | Enterprise Project |
+==============================================+=============================================================+=================================+=============+====================+
| Adding a resource tag                        | POST /v1.0/{project_id}/clusters/{resource_id}/tags         | dws:openAPITag:add              | Y           | Y                  |
+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
| Adding and deleting resource tags in batches | POST /v1.0/{project_id}/clusters/{resource_id}/tags/action  | dws:openAPITag:update           | Y           | Y                  |
+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
| Querying resources by tag                    | POST /v1.0/{project_id}/clusters/resource_instances/action  | dws:openAPITag:getResourceByTag | Y           | Y                  |
+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
| Querying resource tags                       | GET /v1.0/{project_id}/clusters/{resource_id}/tags          | dws:openAPITag:getResourceTag   | Y           | Y                  |
+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
| Querying tags in a specified project         | GET /v1.0/{project_id}/clusters/tags                        | dws:openAPITag:list             | Y           | Y                  |
+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
| Deleting a tag                               | DELETE /v1.0/{project_id}/clusters/{resource_id}/tags/{key} | dws:openAPITag:delete           | Y           | Y                  |
+----------------------------------------------+-------------------------------------------------------------+---------------------------------+-------------+--------------------+
