:original_name: dws_02_00068.html

.. _dws_02_00068:

Obtaining the Cluster ID
========================

A cluster ID (**cluster_id**) is required for some URLs when an API is called. To obtain the cluster ID, perform the following steps:

Obtaining Cluster ID Using APIs
-------------------------------

You can obtain the cluster ID by calling the API for :ref:`creating a cluster <createcluster>`.

The API used to obtain a cluster ID is **GET https://{Endpoint}/v1.0/{project_id}/clusters**. *Endpoint* indicates the IAM endpoint, which can be obtained from the Regions and Endpoints. For details about Project ID, see :ref:`Obtaining Project ID <dws_02_0011>`. For details about interface authentication, see :ref:`Authentication <dws_02_0064>`.

The following is an example response. The value of **id** under the specified clusters is the cluster ID.

.. code-block::

   {
     "clusters" : [ {
       "id" : "7d85f602-a948-4a30-afd4-e84f47471c15",
       "name" : "dws-1",
       "status" : "AVAILABLE",
       "version" : "1.2.0",
       "updated" : "2016-02-10T14:28:14Z",
       "created" : "2016-02-10T14:26:14Z",
       "port" : 8000,
       "endpoints" : [ {
         "connect_info" : "192.168.0.12:8000",
         "jdbc_url" : "jdbc:postgresql://192.168.0.12:8000/<YOUR_DATABASE_name>"
       } ],
       "nodes" : [ {
         "id" : "acaf62a4-41b3-4106-bf6b-2f669d88291e",
         "status" : "200"
       }, {
         "id" : "d32de51e-4fcd-4e5a-a9dc-bb903abb494b",
         "status" : "200"
       }, {
         "id" : "d71a4a25-c9bc-4ffd-9f4a-e422aef327f9",
         "status" : "200"
       } ],
       "tags" : [ {
         "key" : "key1",
         "value" : "value1"
       }, {
         "key" : "key2",
         "value" : "value2"
       } ],
       "user_name" : "dbadmin",
       "number_of_node" : 3,
       "recent_event" : 6,
       "availability_zone" : "eu-de-01",
       "enterprise_project_id" : "0",
       "node_type" : "dws.d1.xlarge.ultrahigh",
       "vpc_id" : "85b20d7e-9eb7-4b2a-98f3-3c8843ea3574",
       "subnet_id" : "374eca02-cfc4-4de7-8ab5-dbebf7d9a720",
       "public_ip" : {
         "public_bind_type" : "auto_assign",
         "eip_id" : "85b20d7e-9eb7-4b2a-98f3-3c8843ea3574"
       },
       "public_endpoints" : [ {
         "public_connect_info" :"192.168.0.12:8000",
         "jdbc_url" : "jdbc:postgresql://192.168.0.12:8000/<YOUR_DATABASE_name>"
       } ],
       "action_progress" : {
         "SNAPSHOTTING" : "20%"
       },
       "sub_status" : "READONLY",
       "task_status" : "SNAPSHOTTING",
       "security_group_id" : "dc3ec145-9029-4b39-b5a3-ace5a01f772b"
     } ]
   }

Obtaining Cluster ID from DWS Console
-------------------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, click **Clusters**.

#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.

#. In the Basic Information area, view the cluster ID, as shown in the following figure.


   .. figure:: /_static/images/en-us_image_0000002531894009.png
      :alt: **Figure 1** Viewing the cluster ID

      **Figure 1** Viewing the cluster ID
