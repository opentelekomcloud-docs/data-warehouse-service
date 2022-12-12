:original_name: dws_02_0036.html

.. _dws_02_0036:

Getting Started
===============

This section describes how to use GaussDB(DWS) APIs to manage clusters. The procedure of the management clusters is as follows:

#. Call the API in :ref:`Authentication <dws_02_0064>` to obtain the user token, which will be put into the request header for authentication in a subsequent request.
#. Call the API in :ref:`Querying the Supported Node Types <dws_02_0022>` to obtain the supported node types.
#. Call the API in :ref:`Creating a Cluster <dws_02_0020>` to create a cluster.
#. Call the API in :ref:`Querying the Cluster List <dws_02_0018>` to obtain the cluster information.
#. Call the API in :ref:`Querying Cluster Details <dws_02_0019>` to view cluster details.
#. Call the API in :ref:`Creating a Snapshot <dws_02_0026>` to create a snapshot.
#. Call the API in :ref:`Querying the Snapshot List <dws_02_0024>` to check whether the snapshot is successfully created.
#. Call the API in :ref:`Restoring a Cluster <dws_02_0032>` to restore a cluster using its snapshot.
#. Call the API in :ref:`Deleting a Manual Snapshot <dws_02_0027>` to delete an unwanted snapshot.
#. Call the API in :ref:`Deleting a Cluster <dws_02_0021>` to delete a finished or unwanted cluster.

Prerequisites
-------------

-  You have created a VPC, subnet, and security group and obtained their IDs. For details, see :ref:`Creating a VPC <dws_02_0040>`.
-  You have obtained the endpoints of IAM and GaussDB(DWS). For details, see "Regions and Endpoints".
-  You have obtained the project ID. For details, see :ref:`Obtaining a Project ID <dws_02_0011>`.

Managing a Cluster
------------------

The following values are examples (replace them based on the actual situation).

-  IAM endpoint: **iam_endpoint**
-  GaussDB(DWS) endpoint: **dws\_endpoint**
-  VPC ID: **219ab8a0-1272-4049-a383-8ad0b770fa11**
-  Subnet ID: **d23ef2e9-8b90-49b3-bc4a-fd7d6bea6bec**
-  Security group ID: **12e3c23a-8710-4b75-95e4-5c8d7f68ef3c**
-  Project ID: **9bc552e6-19af-4326-800d-281a92984636**

Perform the following operations to manage the cluster.

#. Before calling other APIs, call the API in :ref:`Authentication <dws_02_0064>` to obtain the token and set it as an environment variable.

   .. code-block::

      curl -H "Content-type:application/json" https://{iam_endpoint}/v3/auth/tokens -X POST -d '{
          "auth": {
              "identity": {
                  "methods": [
                      "password"
                  ],
                  "password": {
                      "user": {
                          "name": "testname",
                          "domain": {
                              "name": "testname"
                          },
                          "password": "Passw0rd"
                      }
                  }
              },
              "scope": {
                  "project": {
                      "name": "eu-de"
                  }
              }
          }
      }' -v -k

   a. Obtain the value of **X-Subject-Token** from the response header as follows. **X-Subject-Token** indicates the token.

      .. code-block::

         X-Subject-Token:MIidkgYJKoZIhvcNAQcCoIidgzCCA38CAQExDTALBglghkgBZQMEAgEwgXXXXX...

   b. Run the following command to set the token as an environment variable:

      **export Token={X-Subject-Token}**

      **X-Subject-Token** is the token obtained in the preceding step.

      .. code-block::

         export Token=MIidkgYJKoZIhvcNAQcCoIidgzCCA38CAQExDTALBglghkgBZQMEAgEwgXXXXX...

#. Call the API in :ref:`Querying the Supported Node Types <dws_02_0022>` to obtain the supported node types.

   .. code-block::

      curl -X GET -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/node_types -v -k

   The request response is as follows:

   .. code-block::

      status CODE 200
      {
          "node_types": [
              {
                  "spec_name": "dws.d1.xlarge",
                  "id": "ebe532d6-665f-40e6-a4d4-3c51545b6a67",
                  "detail": [
                      {
                          "type": "vCPU",
                          "value": "4"
                      },
                      {
                          "value": "1675",
                          "type": "LOCAL_DISK",
                          "unit": "GB"
                      },
                      {
                          "type": "mem",
                          "value": "32",
                          "unit": "GB"
                      }
                  ]
              },
              {
                  "spec_name": "dws.m1.xlarge.ultrahigh",
                  "id": "ebe532d6-665f-40e6-a4d4-3c51545b4f71",
                  "detail": [
                      {
                          "type": "vCPU",
                          "value": "4"
                      },
                      {
                          "value": "512",
                          "type": "SSD",
                          "unit": "GB"
                      },
                      {
                          "type": "mem",
                          "value": "32",
                          "unit": "GB"
                      }
                  ]
              }
          ]
      }

#. Call the API in :ref:`Creating a Cluster <dws_02_0020>` to create a cluster.

   The examples for configuring the cluster are as follows:

   -  Cluster name: **dws-demo**
   -  Administrator username: **dbadmin**
   -  Administrator password: **Dws2017demo!**
   -  Port: **8000**
   -  Node type: **dws.d1.xlarge**
   -  Number of nodes: **3**
   -  Elastic IP (EIP): **auto_assign**

   .. code-block::

      curl -X POST -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" -d '{
          "node_type": "dws.d1.xlarge",
              "number_of_node": 3,
              "subnet_id": "d23ef2e9-8b90-49b3-bc4a-fd7d6bea6bec",
              "security_group_id": "12e3c23a-8710-4b75-95e4-5c8d7f68ef3c",
              "vpc_id": "219ab8a0-1272-4049-a383-8ad0b770fa11",
              "port": 8000,
              "name": "dws-demo",
              "user_name": "dbadmin",
              "user_pwd": "Dws2017demo!",
              "public_ip": {
                  "public_bind_type": "auto_assign"
              }
      }' https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/clusters -v -k

   If status code **200** is returned, the request for creating a cluster is successfully sent.

#. Call the API in :ref:`Querying the Cluster List <dws_02_0018>` to obtain the cluster information.

   .. code-block::

      curl -X GET -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/clusters -k -v

   The request response is as follows:

   .. code-block::

      {
              "clusters": [
              {
              "id": "7ba031f6-81f4-4670-ad20-c490b91877e5",
              "status": "AVAILABLE",
              "sub_status": "NORMAL",
              "task_status": null,
              "action_progress": null,
              "node_type":  "dws.d1.xlarge",
              "subnet_id": "d23ef2e9-8b90-49b3-bc4a-fd7d6bea6bec",
              "security_group_id": "12e3c23a-8710-4b75-95e4-5c8d7f68ef3c",
              "number_of_node": 3,
              "availability_zone": "eu-de-01",
              "port": 8000,
              "name": "dws-demo",
              "version": "1.1.0",
              "vpc_id": "219ab8a0-1272-4049-a383-8ad0b770fa11",
              "user_name": "dbadmin",
              "public_ip": {
                  "public_bind_type": "auto_assign",
                  "eip_id": "85b20d7e-9eb7-4b2a-98f3-3c8843ea3574"
               },
              "public_endpoints": [
                  {
                      "public_connect_info": "10.0.0.8:8000",
                      "jdbc_url": "jdbc:postgresql://10.0.0.8:8000/<YOUR_DATABASE_name>"
                  }
               ],
              "endpoints": [
                  {
                      "connect_info": "192.168.0.10:8000",
                      "jdbc_url": "jdbc:postgresql://192.168.0.10:8000/<YOUR_DATABASE_name>"
                  },
                  {
                      "connect_info": "192.168.0.12:8000",
                      "jdbc_url": "jdbc:postgresql://192.168.0.12:8000/<YOUR_DATABASE_name>"
                  }
               ] ,
              "updated": "2018-01-15T12:50:06",
              "created": "2018-01-15T12:50:06",
              "recent_event": 1
              }
          ]
      }

   -  If **status** is **CREATING**, the cluster is being created. If **status** is **AVAILABLE**, the cluster is successfully created.
   -  The UUID of cluster **dws-demo** is **7ba031f6-81f4-4670-ad20-c490b91877e5**. Record the UUID for subsequent use.

#. Call the API in :ref:`Querying Cluster Details <dws_02_0019>` to view cluster details.

   .. code-block::

      curl -X GET -H "Content-type:application/json" -H "X-Auth-Token:$Token"
       https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/clusters/7ba031f6-81f4-4670-ad20-c490b91877e5 -k -v

   The request response is as follows:

   .. code-block::

      {
          "cluster": {
              "id": "7ba031f6-81f4-4670-ad20-c490b91877e5",
              "status": "AVAILABLE",
              "name": "dws-demo",
              "updated": "2018-01-15T12:50:06",
              "created": "2018-01-15T12:50:06",
              "user_name": "dbadmin",
              "sub_status": "NORMAL",
              "task_status": null,
              "action_progress": null,
              "node_type":  "dws.d1.xlarge",
              "node_type_id": "5ddb1071-c5d7-40e0-a874-8a032e81a697",
              "subnet_id": "d23ef2e9-8b90-49b3-bc4a-fd7d6bea6bec",
              "security_group_id": "12e3c23a-8710-4b75-95e4-5c8d7f68ef3c",
              "number_of_node": 3,
              "availability_zone": "eu-de-01",
              "port": 8000,
              "vpc_id": "219ab8a0-1272-4049-a383-8ad0b770fa11",
              "public_ip": {
                  "public_bind_type": "auto_assign",
                  "eip_id": "85b20d7e-9eb7-4b2a-98f3-3c8843ea3574"
              },
              "public_endpoints": [
              {
                      "public_connect_info": "10.0.0.8:8000",
                      "jdbc_url": "jdbc:postgresql://10.0.0.8:8000/<YOUR_DATABASE_name>"
               }
               ],
              "endpoints": [
              {
                      "connect_info": "192.168.0.10:8000",
                      "jdbc_url": "jdbc:postgresql://192.168.0.10:8000/<YOUR_DATABASE_name>"
              },
              {
                      "connect_info": "192.168.0.12:8000",
                      "jdbc_url": "jdbc:postgresql://192.168.0.12:8000/<YOUR_DATABASE_name>"
              }
               ],
              "version": "1.1.0",
              "maintain_window": {
                  "day": "Wed",
                  "start_time": "22:00",
                  "end_time": "02:00"
              },
              "recent_event": 1,
              "tags": null,
              "parameter_group": {
                    "id": "157e9cc4-64a8-11e8-adc0-fa7ae01bbebc",               "name": "Default-Parameter-Group-dws ",               "status": "In-Sync"
              }
          }
      }

   **public_endpoints** and **endpoints** can be queried from the response. After the cluster is successfully created, you can use **public_endpoints** or **endpoints** to access the cluster from an external source.

#. Call the API in :ref:`Creating a Snapshot <dws_02_0026>` to create a snapshot.

   Create snapshot **snapshotForDemoCluster** for cluster **dws-demo**.

   .. code-block::

      curl -X POST -H "Content-type:application/json" -H "X-Auth-Token:$Token" -d '{
          "snapshot": {
              "name": "snapshotForDemoCluster",
              "cluster_id": "7ba031f6-81f4-4670-ad20-c490b91877e5",
              "description": "Snapshot description"
          }
      }' https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/snapshots -k -v

   The request response is as follows:

   .. code-block::

      {
        "snapshot": {
            "id": "2a4d0f86-67cd-408a-8b66-017454fb7793"
        }
      }

   If status code **200** is returned, the request for creating a snapshot is successfully sent. Record **id** so that the ID can be used when you query the snapshot details later.

#. Call the API in :ref:`Querying the Snapshot List <dws_02_0024>` to check whether the snapshot is successfully created.

   .. code-block::

      curl -X GET -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/snapshots/2a4d0f86-67cd-408a-8b66-017454fb7793 -k -v

   If the snapshot status in the response is **AVAILABLE**, the snapshot is successfully created. If the snapshot status is **CREATING**, the snapshot is being created.

   .. code-block::

      {
          "snapshot": {
              "id": "2a4d0f86-67cd-408a-8b66-017454fb7793",
              "name": "snapshotForDemoCluster",
              "description": "Snapshot description",
              "started": "2018-01-18T13:59:23Z",
              "finished": "2018-01-18T13:01:40Z",
              "size": 500,
              "status": "AVAILABLE",
              "type": "MANUAL",
              "cluster_id": "4f87d3c4-9e33-482f-b962-e23b30d1a18c"
          }
      }

#. Call the API in :ref:`Restoring a Cluster <dws_02_0032>` to restore a cluster using its snapshot.

   Restore snapshot **snapshotForDemoCluster** to new cluster **dws-restore**.

   .. code-block::

      curl -X POST -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" -d '{
          "restore": {
              "name": "dws-restore"
          }
      }' https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/snapshots/2a4d0f86-67cd-408a-8b66-017454fb7793/actions -v -k

   If status code **200** is returned, the cluster is successfully restored. You can check the cluster restoration status by performing operations in :ref:`Querying Snapshot Details <dws_02_0025>`.

#. Call the API in :ref:`Deleting a Manual Snapshot <dws_02_0027>` to delete an unwanted snapshot.

   .. code-block::

      curl -X DELETE -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/snapshots/2a4d0f86-67cd-408a-8b66-017454fb7793 -v -k

   If status code **202** is returned, the snapshot is successfully deleted.

#. Call the API in :ref:`Deleting a Cluster <dws_02_0021>` to delete an unwanted cluster.

   .. code-block::

      curl -X DELETE -H 'Content-type:application/json;charset=utf-8' -H "X-Auth-Token:$Token" -d '{
          "keep_last_manual_snapshot":0
      }' https://{dws_endpoint}/v1.0/9bc552e6-19af-4326-800d-281a92984636/clusters/7ba031f6-81f4-4670-ad20-c490b91877e5 -v -k

   If status code **202** is returned, the cluster is successfully deleted.
