:original_name: dws_01_0149.html

.. _dws_01_0149:

Syntax of Fine-Grained Permissions Policies
===========================================

In actual services, you may need to grant different operation permissions on resources to users of different roles. The IAM service provides fine-grained access control. An IAM administrator (a user in the **admin** group) can create a custom policy containing required permissions. After a policy is granted to a user group, users in the group can obtain all permissions defined by the policy. In this way, IAM implements fine-grained permission management.

To control the GaussDB(DWS) operations on resources more precisely, you can use the user management function of IAM to grant different operation permissions to users of different roles for fine-grained permission control.

Policy Structure
----------------

A fine-grained policy consists of a Version and a Statement. Each policy can have multiple statements.


.. figure:: /_static/images/en-us_image_0000002167906624.jpg
   :alt: **Figure 1** Policy structure

   **Figure 1** Policy structure

Policy Syntax
-------------

In the navigation pane on the IAM console, click **Policies** and then click the name of a policy to view its details. The **DWS ReadOnlyAccess** policy is used as an example to describe the syntax of fine-grained policies.

.. code-block::

   {
           "Version": "1.1",
           "Depends": [],
           "Statement": [
                   {
                           "Effect": "Allow",
                           "Action": [
                                   "dws:*:get*",
                                   "dws:*:list*",
                                   "ecs:*:get*",
                                   "ecs:*:list*",
                                   "vpc:*:get*",
                                   "vpc:*:list*",
                                   "evs:*:get*",
                                   "evs:*:list*",
                                   "mrs:*:get*",
                                   "bss:*:list*",
                                   "bss:*:get*"
                           ]
                   }
           ]
   }

-  **Version**: Distinguishes between role-based access control (RBAC) and fine-grained policies.

   -  **1.0**: RBAC policies. An RBAC policy consists of permissions for an entire service. Users in a group with such a policy assigned are granted all of the permissions required for that service.
   -  **1.1**: Fine-grained policies. A fine-grained policy consists of API-based permissions for operations on specific resource types. Fine-grained policies, as the name suggests, allow for more fine-grained control than RBAC policies. Users granted permissions of such a policy can only perform specific operations on the corresponding service. Fine-grained policies include system and custom policies.

-  **Depends**: dependency item.
-  **Statement**: Permissions defined by a policy, including Effect and Action.

   -  Effect

      The value of **Effect** can be **Allow** or **Deny**. System policies contain only **Allow** statements. For custom policies containing both **Allow** and **Deny** statements, **Deny** statements take precedence over **Allow** statements.

   -  Action

      Actions allowed on resources. An action is in the format of *Service name*:*Resource type*:*Action*. A policy can contain one or more actions. You can use a wildcard (``*``) to indicate all services, resource types, or actions.

      Example: **dws:cluster:create** (permission for create data warehouse clusters)

.. _en-us_topic_0000002168065764__section89181381475:

List of Supported Actions
-------------------------

When creating a custom policy on IAM, you can add the operations on GaussDB(DWS) resources or the permissions corresponding to RESTful APIs to the action list of the policy authorization statement so that the policy contains the operation permissions. The following table lists the GaussDB(DWS) permissions.

-  **REST API**

   For details about REST API actions supported by GaussDB(DWS), see "Permissions Policies and Supported Actions" in *Data Warehouse Service (DWS) API Reference*.

-  **Management console operations**

   :ref:`Table 1 <en-us_topic_0000002168065764__table42061239124614>` describes the GaussDB(DWS) operations on resources and corresponding permissions.

   .. note::

      -  Some GaussDB(DWS) permissions depend on the actions of ECS, VPC, EVS, ELB, MRS, and OBS. Grant GaussDB(DWS) the required service admin permissions.
      -  The table shows frequently used GaussDB(DWS) APIs, but some only allow project-based authentication (IAM authentication) and not enterprise project authentication. To use these APIs, they must be configured on the IAM authentication page.

.. _en-us_topic_0000002168065764__table42061239124614:

.. table:: **Table 1** GaussDB(DWS) permissions

   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Operation                                                                        | Permission                                    | Dependent Permission                       | Scope                     |
   +==================================================================================+===============================================+============================================+===========================+
   | Creating a cluster                                                               | "``dws:cluster:create``"                      | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:securityGroupRules:delete``",       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:ports:update``",                    |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining the cluster list                                                       | "``dws:cluster:list``"                        | --                                         | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining the details of a cluster                                               | "``dws:cluster:getDetail``"                   | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``vpc:vpcs:list``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:securityGroups:get``"               |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Setting automated snapshot policy                                                | "``dws:cluster:setAutomatedSnapshot``"        | "``dws:backupPolicy:list``"                | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Setting security parameters/parameter groups                                     | "``dws:cluster:setSecuritySettings``"         | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Restarting a Cluster                                                             | "``dws:cluster:restart``"                     | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Scaling out clusters                                                             | "``dws:cluster:scaleOut``"                    | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:cluster:scaleOutOrOpenAPIResize``", |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:update*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Scaling out or resizing a cluster via API                                        | "``dws:cluster:scaleOutOrOpenAPIResize``"     | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``vpc:vpcs:list``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:ports:create``",                    |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:ports:get``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:ports:update``",                    |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:subnets:get``",                     |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:subnets:update``",                  |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:subnets:create``",                  |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:routers:get``",                     |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:routers:update``",                  |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:networks:create``",                 |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:networks:get``",                    |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:networks:update``",                 |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:serverInterfaces:use``",            |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:serverInterfaces:get``",            |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:cloudServerFlavors:get``"           |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Resetting Your Password                                                          | "``dws:cluster:resetPassword``"               | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a cluster                                                               | "``dws:cluster:delete``"                      | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:delete*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Configuring maintenance windows                                                  | "``dws:cluster:setMaintainceWindow``"         | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Binding EIPs                                                                     | "``dws:eip:operate``"                         | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``eip:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``eip:*:list*``"                          |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Unbinding EIPs                                                                   | "``dws:eip:operate``"                         | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``eip:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``eip:*:list*``"                          |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating MRS connections                                                         | "``dws:MRSConnection:create``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``mrs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:cluster:create``",                  |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Updating MRS connections                                                         | "``dws:MRSConnection:update``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``mrs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:cluster:create``",                  |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting MRS connections                                                         | "``dws:MRSConnection:delete``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``mrs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:cluster:create``"                   |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:delete*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | MRS data source list                                                             | "``dws:MRSSource:list``"                      | "``mrs:cluster:list``",                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``mrs:tag:listResource``",                |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``mrs:tag:list``",                        |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Adding/Deleting tags                                                             | "``dws:tag:addAndDelete``"                    | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:openAPITag:update``",               |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:openAPITag:getResourceTag``",       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Editing tags                                                                     | "``dws:tag:edit``"                            | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:openAPITag:update``",               |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:openAPITag:getResourceTag``",       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating a snapshot                                                              | "``dws:snapshot:create``"                     | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining the snapshot list                                                      | "``dws:snapshot:list``"                       | --                                         | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Viewing the snapshot list of a cluster                                           | "``dws:clusterSnapshot:list``"                | "``dws:cluster:list``",                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:openAPICluster:getDetail``"         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting snapshots                                                               | "``dws:snapshot:delete``"                     | "``dws:snapshot:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Copying snapshots                                                                | "``dws:snapshot:copy``"                       | "``dws:snapshot:list``",                   | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:snapshot:create``"                  |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Restoring data to a new cluster                                                  | "``dws:cluster:restore``"                     | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Resizing a cluster                                                               | "``dws:cluster:resize``"                      | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:delete*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Switchback                                                                       | "``dws:cluster:switchover``"                  | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying the ELB list                                                            | "``dws:elb:list``"                            | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:list*``",                         |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Associating ELB                                                                  | "``dws:elb:bind``"                            | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:create*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Disassociating ELB                                                               | "``dws:elb:unbind``"                          | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``elb:*:delete*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying snapshot configurations                                                 | "``dws:snapshotConfig:list``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Updating a snapshot policy                                                       | "``dws:backupPolicyDetail:update``"           | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a snapshot policy                                                       | "``dws:backupPolicy:delete``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying a snapshot policy                                                       | "``dws:backupPolicy:list``"                   | "``dws:cluster:list``"                     | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying cluster encryption information                                          | "``dws:clusterEncryptInfo:list``"             | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "KMS Administrator"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating an agent                                                                | "``dws:createAgency:create``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "security administrator"                   |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying OBS bucket information                                                  | "``dws:queryBuckets:list``"                   | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Adding a node                                                                    | "``dws:expandWithExistedNodes:update``"       | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:update*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a DR backup                                                             | "``dws:disasterRecovery:delete``"             | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:delete*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:delete*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating a DR backup                                                             | "``dws:disasterRecovery:create``"             | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Other DR and backup operations                                                   | "``dws:disasterRecovery:otherOperate``"       | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying DR and backup operations                                                | "``dws:disasterRecovery:get``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``"                          |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Adding a CN                                                                      | "``dws:module:install``"                      | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a CN                                                                    | "``dws:module:uninstall``"                    | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Removing nodes                                                                   | "``dws:clusterNodes:operate``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Updating the node alias                                                          | dws:instanceAliasName:update                  | dws:cluster:list                           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Redistributing data                                                              | "``dws:redistribution:operate``"              | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying redistribution                                                          | "``dws:redistributionInfo:list``"             | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Stopping redistribution                                                          | "``dws:redistribution:suspend``"              | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Resuming redistribution                                                          | "``dws:redistribution:recover``"              | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying product specifications                                                  | "``dws:specProduct:list``"                    | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``"                          |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Performing a check before cluster creation                                       | "``dws:checkCluster:create``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``vpc:*:create*``",                       |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``evs:*:create*``",                       |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Binding the management plane IP address                                          | "``dws:bindManageIp:operate``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining user authorization                                                     | "``dws:checkAuthorize:operate``"              | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:checkSupport:operate``"             |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Authorizing a user                                                               | "``dws:authorize:operate``"                   | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:checkSupport:operate``"             |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying user databases                                                          | "``dws:userDatabase:list``"                   | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:checkSupport:operate``"             |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying user schemas                                                            | "``dws:schemas:list``"                        | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:checkSupport:operate``"             |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying user tables                                                             | "``dws:tables:list``"                         | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Restoring tables                                                                 | "``dws:tableRestore:operate``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Checking the name of the table to be restored                                    | "``dws:tableRestoreCheck:operate``"           | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Checking whether a cluster supports fine-grained backup                          | "``dws:checkSupport:operate``"                | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying the list of flavors that can be changed                                 | "``dws:supportFlavors:list``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Changing the node flavor                                                         | "``dws:specResize:operate``"                  | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``ecs:*:get*``",                          |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:create*``"                        |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Stopping snapshot creation                                                       | "``dws:snapshot:stop``"                       | "``dws:snapshot:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Terminating a session                                                            | "``dws:dmsSession:terminate``"                | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Workload report operations                                                       | "``dws:dmsWorkloadDiagnosisReport:create``"   | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Modifying an alarm rule                                                          | "``dws:dmsAlarmRule:update``"                 | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Enabling an alarm rule                                                           | "``dws:dmsAlarmRule:enable``"                 | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Enabling a cluster alarm                                                         | "``dws:dmsClusterAlarm:enable``"              | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Disabling a cluster alarm                                                        | "``dws:dmsClusterAlarm:disable``"             | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | gRPC external service                                                            | "``dws:dmsGrpcOuter:operation``"              | "``dws:dmsQuery:list``",                   | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:cluster:setSecuritySettings``",     |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``obs:bucket:ListAllMyBuckets``"          |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Adding a SQL probe                                                               | "``dws:dmsProbe:add``"                        | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Modifying a SQL probe                                                            | "``dws:dmsProbe:update``"                     | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a SQL probe                                                             | "``dws:dmsProbe:delete``"                     | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Enabling or disabling a SQL probe                                                | "``dws:dmsProbe:enable``"                     | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating a User panel                                                            | "``dws:dmsUserBoard:create``"                 | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Modifying a user panel                                                           | "``dws:dmsUserBoard:update``"                 | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a user panel                                                            | "``dws:dmsUserBoard:delete``"                 | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Terminating a query                                                              | "``dws:dmsQuery:terminate``"                  | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Enabling or disabling DMS                                                        | "``dws:dmsService:enableOrDisable``"          | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Modifying DMS storage configurations                                             | "``dws:dmsStorageConfig:modify``"             | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining, or creating a DDL review                                              | "``dws:dmsDdlExamine:getOrCreate``"           | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Workload snapshot operations                                                     | "``dws:dmsWorkloadDiagnosisSnapshot:create``" | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating an alarm rule                                                           | "``dws:dmsAlarmRule:add``"                    | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting an alarm rule                                                           | "``dws:dmsAlarmRule:delete``"                 | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Executing a SQL probe                                                            | "``dws:dmsProbe:execute``"                    | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting a monitoring item                                                       | "``dws:dmsPerformanceMonitor:delete``"        | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Enabling or disabling DMS monitoring metrics                                     | "``dws:dmsCollectItem:enableOrDisable``"      | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Modifying DMS monitoring configurations                                          | "``dws:dmsCollectConfig:modify``"             | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | OpenAPI Conditional Query                                                        | "``dws:dmsOpenapiQuery:list``"                | "``dws:cluster:list``"                     | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise projects |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Disabling an alarm rule                                                          | "``dws:dmsAlarmRule:disable``"                | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting an alarm record                                                         | "``dws:dmsAlarmRecord:delete``"               | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Checking SQL probes                                                              | "``dws:dmsProbe:check``"                      | "``dws:dmsGrpcOuter:operation``"           | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Adding a monitoring item                                                         | "``dws:dmsPerformanceMonitor:add``"           | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Modifying monitoring metrics                                                     | "``dws:dmsPerformanceMonitor:update``"        | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Downloading historical monitoring trend                                          | "``dws:dmsTrendHistory:down``"                | "``dws:dmsQuery:list``"                    | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining cluster ring information                                               | "``dws:ring:list``"                           | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining the cluster process topology                                           | "``dws:processTopo:list``"                    | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying intelligent O&M information                                             | "``dws:operationalTask:get``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Intelligent O&M Operations                                                       | "``dws:operationalTask:operate``"             | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Adding, deleting, and modifying a logical cluster                                | "``dws:logicalCluster:operate``"              | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying a logical cluster                                                       | "``dws:logicalCluster:get``"                  | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Elastic logical cluster planning                                                 | "``dws:logicalClusterPlan:operate``"          | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               | "``dws:logicalCluster:*``",                |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:cluster:scaleOut``",                |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``iam:agencies:*``",                      |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``iam:permissions:*Agency*``"             |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating an endpoint service                                                     | "``dws:vpcEndpointService:create``"           | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying the resource management list                                            | "``dws:workLoadManager:get``"                 | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Resource management operations                                                   | "``dws:workLoadManager:operate``"             | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | LTS operations                                                                   | "``dws:ltsAccess:operate``"                   | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying LTS Information                                                         | "``dws:ltsAccess:get``"                       | "``dws:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Project             |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying events                                                                  | "``dws:event:list``"                          | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying event specifications                                                    | "``dws:event:list``"                          | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying event subscriptions                                                     | "``dws:eventSub:list``"                       | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating an event subscription                                                   | "``dws:eventSub:create``"                     | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Updating an event subscription                                                   | "``dws:eventSub:update``"                     | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting an event subscription                                                   | "``dws:eventSub:delete``"                     | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying alarm statistics                                                        | "``dws:alarmStatistic:list``"                 | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying alarm details                                                           | "``dws:alarmDetail:list``"                    | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying alarm configurations                                                    | "``dws:alarmConfig:list``"                    | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying alarm subscriptions                                                     | "``dws:alarmSub:list``"                       | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Creating an alarm subscription                                                   | "``dws:alarmSub:create``"                     | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Updating an alarm subscription                                                   | "``dws:alarmSub:update``"                     | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Deleting an alarm subscription                                                   | "``dws:alarmSub:delete``"                     | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Delivering cluster upgrade operations (upgrade, rollback, submission, and retry) | "``dws:cluster:doUpdate``"                    | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying the available upgrade paths of a cluster                                | "``dws:cluster:getUpgradePaths``"             | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Querying cluster upgrade records                                                 | "``dws:cluster:getUpgradeRecords``"           | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``"                          |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Starting a cluster                                                               | "``dws:cluster:startCluster``"                | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:start``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:stop``"                           |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Stopping a cluster                                                               | "``dws:cluster:stopCluster``"                 | "``dws:*:get*``",                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``dws:*:list*``",                         |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:get*``",                          | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:list*``",                         |    -  Project             |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:start``",                         |                           |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               | "``ecs:*:stop``"                           |                           |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining tags                                                                   | "``dws:openAPItag:list``"                     | "``dws:*:list*``"                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Service EPS list                                                                 | "``dws:service:listEps``"                     | "``dws:*:list*``"                          | -  Supported:             |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining the DR information                                                     | "``dws:disasterRecovery:get``"                | "``dws:*:*``"                              | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Cluster restoration check                                                        | "``dws:cluster:checkRestore``"                | "``dws:*:*``"                              | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Static alarm list                                                                | "``dws:alarmStatistic:list``"                 | "``dws:*:list*``"                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining static resource information                                            | "``dws:service:getResourceStatistics``"       | "``dws:*:*``"                              | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Alarm details list                                                               | "``dws:alarmDetail:list``"                    | "``dws:*:list*``"                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Obtaining the cluster details                                                    | "``dws:openAPICluster:getDetail``"            | "``dws:*:*``"                              | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Cluster event specifications                                                     | "``dws:eventSpec:list``"                      | "``dws:*:list*``"                          | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Project             |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Cluster DR list                                                                  | "``dws:cluster:listDisasterRecovery``"        | "``dws:*:list*``",                         | -  Not supported          |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise Project  |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Scope:                 |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Projects            |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+
   | Checking the alarm data overview                                                 | "``dws:alarm:listStatistics``"                | "``dws:*:list*``",                         | -  Not supported:         |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Enterprise projects |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            | -  Supported:             |
   |                                                                                  |                                               |                                            |                           |
   |                                                                                  |                                               |                                            |    -  Projects            |
   +----------------------------------------------------------------------------------+-----------------------------------------------+--------------------------------------------+---------------------------+

Authorization Using the Fine-Grained Permission Policy
------------------------------------------------------

#. Log in to the IAM console as and create a user-defined policy.

   For details, see "Fine-Grained Policy Management > Creating a Custom Policy" in the *Identity and Access Management User Guide*.

   Refer to the following to create the policy:

   -  Use the IAM administrator account, that is, the user in the admin user group, because only the IAM administrator has the permissions to create users and user groups and modify user group permissions.

   -  GaussDB(DWS) is a project-level service, so its **Scope** must be set to **Project-level service**. If this policy is required to take effect for multiple projects, authorization is required to each project.

   -  Two GaussDB(DWS) policy templates are preconfigured on IAM. When creating a custom policy, you can select either of the following templates and modify the policy authorization statement based on the template:

      -  **DWS Admin**: has all execution permissions on GaussDB(DWS).
      -  **DWS Viewer**: has the read-only permission on GaussDB(DWS).

   -  You can add permissions corresponding to GaussDB(DWS) operations or RESTful APIs listed in :ref:`List of Supported Actions <en-us_topic_0000002168065764__section89181381475>` to the action list in the policy authorization statement, so that the policy can obtain the permissions.

      For example, if **dws:cluster:create** is added to the action list of a policy statement, the policy has the permission to create clusters.

   -  If you want to use other services, grant related operation permissions on these services. For details, see the help documents of related services.

      For example, when creating a data warehouse cluster, you need to configure the VPC to which the cluster belongs. To obtain the VPC list, add permission **vpc:*:get\*** to the policy statement.

#. Create a user group.

   For details, see "User and User Group Management > Viewing or Modifying User Group Information > Creating a User Group" in the *Identity and Access Management User Guide*.

#. Add users to the user group and grant the new custom policy to the user group so that users in it can obtain the permissions defined by the policy.

   For details, see "User and User Group Management > Viewing or Modifying User Group Information" in the *Identity and Access Management User Guide*.

Authentication Logic
--------------------

If a user is granted permissions of multiple policies or of only one policy containing both Allow and Deny statements, then authentication starts from the Deny statements. The following figure shows the authentication logic for resource access.


.. figure:: /_static/images/en-us_image_0000002203427309.jpg
   :alt: **Figure 2** Authentication logic

   **Figure 2** Authentication logic

.. note::

   The actions in each policy bear the OR relationship.

#. A user accesses the system and makes an operation request.
#. The system evaluates all the permissions policies assigned to the user.
#. In these policies, the system looks for explicit deny permissions. If the system finds an explicit deny that applies, it returns a decision of Deny, and the authentication ends.
#. If no explicit deny is found, the system looks for allow permissions that would apply to the request. If the system finds an explicit allow permission that applies, it returns a decision of Allow, and the authentication ends.
#. If no explicit allow permission is found, IAM returns a decision of Deny, and the authentication ends.
