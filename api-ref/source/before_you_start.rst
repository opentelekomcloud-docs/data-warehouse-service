:original_name: dws_02_0057.html

.. _dws_02_0057:

Before You Start
================

Overview
--------

Welcome to GaussDB(DWS). GaussDB(DWS) is a fully-managed and enterprise-level cloud data warehouse service. It is O&M-free, compatible with the PostgreSQL ecosystem, and supports online cluster scale-out and efficient loading of multiple data sources. It helps enterprises efficiently analyze and monetize massive amounts of data online.

This guide explains how to use APIs to manage GaussDB(DWS) clusters, including creating, querying, and deleting tags and snapshots. For details about all supported operations, see :ref:`API Overview <dws_02_0034>`.

Before calling an API, get familiar with related concepts of GaussDB(DWS). For details, see "Service Overview" in the *Data Warehouse Service User Guide*.

GaussDB(DWS) supports Representational State Transfer (REST) APIs, allowing you to call APIs using HTTPS. For details about API calling, see :ref:`Calling APIs <dws_02_0062>`.

Endpoints
---------

An endpoint is the **request address** for calling an API. Endpoints vary depending on services and regions. For the endpoints of all services, see "Regions and Endpoints".

Basic Concepts
--------------

-  Account

   An account has full access permissions for all the resources and cloud services under it. It can reset user passwords and grant users permissions. For security purposes, create IAM users and assign them permissions for routine management.

-  User

   An IAM user is created by an account to use cloud services. Each IAM user has its own identity credentials (password or access keys).

   The account name, username, and password will be required for API authentication.

-  Region

   A region is a geographic area where cloud resources are deployed. Availability zones (AZs) in the same region can communicate with each other over an intranet, while AZs in different regions are isolated from each other. By creating cloud resources in different regions, you can better meet customer requirements and comply with local laws and regulations.

-  AZ

   An AZ contains one or more physical data centers. Each AZ has independent power and network devices. Within an AZ, computing, network, storage, and other resources are logically divided into multiple clusters. AZs within a region are interconnected using high-speed optical fibers to support cross-AZ high-availability systems.

-  Project

   A project corresponds to a region. Default projects are defined to group and physically isolate resources (including compute, storage, and network resources) between different regions. Users can be granted permissions in a default project to access all resources under their accounts in the region associated with the project. For more refined access control, create subprojects under a project and apply for resources in the subprojects. Users can then be assigned permissions to access only specific resources in the subprojects.


   .. figure:: /_static/images/en-us_image_0000002138022226.png
      :alt: **Figure 1** Project isolating model

      **Figure 1** Project isolating model

-  Enterprise project

   Enterprise projects group and logically isolate resources. An enterprise project can contain resources from different regions, and resources can be transferred between enterprise projects.
