:original_name: dws_01_0144.html

.. _dws_01_0144:

GaussDB(DWS) Permissions Management
===================================

If you need to assign different permissions to employees in your enterprise to access your GaussDB(DWS) resources on cloud, IAM is a good choice for fine-grained permissions management. IAM provides identity authentication, permissions management, and access control, helping you secure access to your cloud resources.

With IAM, you can use your cloud account to create IAM users for your employees, and assign permissions to the users to control their access to specific resource types. Assume you want to allow software developers in your enterprise to use GaussDB(DWS) resources, but forbid them from deleting the resources or performing any high-risk operations. To this end, you can create IAM users for these developers and grant them only the permissions required for using GaussDB(DWS) resources.

If your cloud account does not need individual IAM users for permissions management, you may skip this section.

IAM can be used free of charge. You pay only for the resources in your account. For more information about IAM, see "Service Overview" in *Identity and Access Management User Guide*.

Supported System Policies
-------------------------

By default, new IAM users do not have permissions assigned. You need to add a user to one or more groups, and attach permissions policies or roles to these groups. Users inherit permissions from the groups to which they are added and can perform specified operations on cloud services.

GaussDB(DWS) is a project-level service deployed and accessed in specific physical regions. To assign GaussDB(DWS) permissions to a user group, specify the scope as region-specific projects and select projects for the permissions to take effect. If **All projects** is selected, the permissions will take effect for the user group in all region-specific projects. When accessing GaussDB(DWS), the users need to switch to a region where they have been authorized to use GaussDB(DWS).

-  **Role**: IAM initially provides a coarse-grained authorization mechanism to define permissions based on users' job responsibilities. This mechanism provides only a limited number of service-level roles for authorization. When using roles to grant permissions, you must also assign other roles on which the permissions depend to take effect. However, roles are not an ideal choice for fine-grained authorization and secure access control.
-  **Policies**: A type of fine-grained authorization mechanism that defines permissions required to perform operations on specific cloud resources under certain conditions. This mechanism allows for more flexible policy-based authorization, meeting requirements for secure access control. For example, you can grant GaussDB(DWS) users only the permissions for managing a certain type of GaussDB(DWS) resources.

:ref:`Table 1 <en-us_topic_0000001951848289__table1945683962711>` lists all the system-defined roles and policies supported by GaussDB(DWS).

.. _en-us_topic_0000001951848289__table1945683962711:

.. table:: **Table 1** GaussDB(DWS) system permissions

   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Role/Policy Name    | Description                                                                                                                                                                                           | Category              | Dependencies                                                                                                                                             |
   +=====================+=======================================================================================================================================================================================================+=======================+==========================================================================================================================================================+
   | DWS ReadOnlyAccess  | Read-only permissions for GaussDB(DWS). Users granted these permissions can only view GaussDB(DWS) data.                                                                                              | System-defined policy | N/A                                                                                                                                                      |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DWS FullAccess      | Database administrator permissions for GaussDB(DWS). Users granted these permissions can perform all operations on GaussDB(DWS).                                                                      | System-defined policy | N/A                                                                                                                                                      |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DWS Administrator   | Database administrator permissions for GaussDB(DWS). Users granted these permissions can perform operations on all GaussDB(DWS) resources.                                                            | System-defined role   | Dependent on the **Tenant Guest** and **Server Administrator** policies, which must be assigned in the same project as the **DWS Administrator** policy. |
   |                     |                                                                                                                                                                                                       |                       |                                                                                                                                                          |
   |                     | -  Users granted permissions of the **VPC Administrator** policy can create VPCs and subnets.                                                                                                         |                       |                                                                                                                                                          |
   |                     | -  Users granted permissions of the **Cloud Eye Administrator** policy can view monitoring information of data warehouse clusters.                                                                    |                       |                                                                                                                                                          |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DWS Database Access | GaussDB(DWS) database access permission. Users with this permission can generate the temporary database user credentials based on IAM users to connect to the database in the data warehouse cluster. | System-defined role   | Dependent on the **DWS Administrator** policy, which must be assigned in the same project as the **DWS Database Access** policy.                         |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

:ref:`Table 2 <en-us_topic_0000001951848289__table175152005317>` lists the common operations supported by each system-defined policy or role of GaussDB(DWS). Choose appropriate policies or roles as required.

.. note::

   -  If you use the EIP for the first time for a project in a region, the system prompts you to create the **DWSAccessVPC** agency to authorize GaussDB(DWS) to access VPC. After the authorization is successful, GaussDB(DWS) can switch to a healthy VM when the VM bound with the EIP is faulty.
   -  In addition to policy permissions, you may need to grant different operation permissions on resources to users of different roles. For details about operations, such as creating snapshots and restarting clusters, see "Syntax of Fine-Grained Permissions Policies" in *Data Warehouse Service (DWS) User Guide*.
   -  By default, only cloud accounts or users with Security Administrator permissions can query and create agencies. By default, the IAM users in those accounts cannot query or create agencies. When the users use the EIP, the system makes the binding function unavailable. Contact a user with the **DWS Administrator** permissions to authorize the agency on the current page.

.. _en-us_topic_0000001951848289__table175152005317:

.. table:: **Table 2** Common operations supported by each system-defined policy or role of GaussDB(DWS)

   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Operation                                    | DWS FullAccess | DWS ReadOnlyAccess | DWS Administrator | DWS Database Access |
   +==============================================+================+====================+===================+=====================+
   | Creating/Restoring clusters                  | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Obtaining the cluster list                   | Y              | Y                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Obtaining the details of a cluster           | Y              | Y                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Setting automated snapshot policy            | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Setting security parameters/parameter groups | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Restarting clusters                          | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Scaling out clusters                         | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Resetting passwords                          | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Deleting clusters                            | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Configuring maintenance windows              | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Binding EIPs                                 | x              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Unbinding EIPs                               | x              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Creating DNS domain names                    | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Releasing DNS domain names                   | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Modifying DNS domain names                   | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Creating MRS connections                     | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Updating MRS connections                     | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Deleting MRS connections                     | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Adding/Deleting tags                         | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Editing tags                                 | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Creating snapshots                           | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Obtaining the snapshot list                  | Y              | Y                  | Y                 | Y                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Deleting snapshots                           | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
   | Copying snapshots                            | Y              | x                  | Y                 | x                   |
   +----------------------------------------------+----------------+--------------------+-------------------+---------------------+
