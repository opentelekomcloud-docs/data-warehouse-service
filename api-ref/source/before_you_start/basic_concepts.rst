:original_name: dws_02_0061.html

.. _dws_02_0061:

Basic Concepts
==============

-  Account

   This account has full access to all cloud services and resources associated with it. It can be used to reset user passwords and grant user permissions. The domain should not be used directly to perform routine management. For security purposes, create users and grant them permissions for routine management.

-  Users

   An IAM user is created using an account for cloud services. Each IAM user has its own identity credentials (password and access keys).

   The account name, username, and password are required for API authentication.

-  Region

   A region is a geographic area in which cloud resources are deployed. Availability zones (AZs) in the same region can communicate with each other over an intranet, while AZs in different regions are isolated from each other. Deploying cloud resources in different regions can better suit certain user requirements or comply with local laws or regulations.

-  AZ

   An AZ contains one or more physical data centers. Each AZ has independent power and network devices. Within an AZ, computing, network, storage, and other resources are logically divided into multiple clusters. AZs within a region are interconnected using high-speed optical fibers to support cross-AZ high-availability systems.

-  Item

   Projects group and isolate resources (including compute, storage, and network resources) across physical regions. A default project is provided for each service region, and subprojects can be created under each default project. Users can be granted permissions to access all resources in a specific project. For more refined access control, create subprojects under a project and apply for resources in the subprojects. IAM users can then be assigned permissions to access only specific resources in the subprojects.


   .. figure:: /_static/images/en-us_image_0000001231391281.png
      :alt: **Figure 1** Project isolating model

      **Figure 1** Project isolating model

-  Enterprise project

   Enterprise projects group and manage resources across regions. Resources in enterprise projects are logically isolated from each other. An enterprise project can contain resources of multiple regions, and resources can be added to or removed from enterprise projects.
