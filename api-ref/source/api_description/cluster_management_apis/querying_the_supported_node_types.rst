:original_name: dws_02_0022.html

.. _dws_02_0022:

Querying the Supported Node Types
=================================

Function
--------

This API is used to query the node types supported by GaussDB(DWS).

URI
---

-  URI format

   .. code-block:: text

      GET /v2/{project_id}/node-types

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                          |
      +============+===========+========+======================================================================================================+
      | project_id | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
      +------------+-----------+--------+------------------------------------------------------------------------------------------------------+

Request Message
---------------

-  Request example

   .. code-block:: text

      GET /v2/89cd04f168b84af6be287f71730fdb4b/node-types

Response Message
----------------

-  Example response

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

-  Parameter description

   .. table:: **Table 2** Response parameter description

      +------------+--------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Parameter  | Type                                                                                                               | Description               |
      +============+====================================================================================================================+===========================+
      | node_types | Array of :ref:`NodeTypes <en-us_topic_0000001224645685__en-us_topic_0000001098976622_response_node_types>` objects | List of node type objects |
      +------------+--------------------------------------------------------------------------------------------------------------------+---------------------------+

   .. _en-us_topic_0000001224645685__en-us_topic_0000001098976622_response_node_types:

   .. table:: **Table 3** NodeTypes

      +-----------+-------------------------------------------------------------------------------------------------------------+---------------------+
      | Parameter | Type                                                                                                        | Description         |
      +===========+=============================================================================================================+=====================+
      | spec_name | String                                                                                                      | Name of a node type |
      +-----------+-------------------------------------------------------------------------------------------------------------+---------------------+
      | detail    | Array of :ref:`Detail <en-us_topic_0000001224645685__en-us_topic_0000001098976622_response_detail>` objects | Node type details   |
      +-----------+-------------------------------------------------------------------------------------------------------------+---------------------+
      | id        | String                                                                                                      | Node type ID        |
      +-----------+-------------------------------------------------------------------------------------------------------------+---------------------+

   .. _en-us_topic_0000001224645685__en-us_topic_0000001098976622_response_detail:

   .. table:: **Table 4** Detail

      ========= ====== ===============
      Parameter Type   Description
      ========= ====== ===============
      type      String Attribute type
      value     String Attribute value
      unit      String Attribute unit
      ========= ====== ===============

Status Code
-----------

-  Normal

   200

-  Exception

   .. table:: **Table 5** Returned values

      ========================= ===========================
      Returned Value            Description
      ========================= ===========================
      400 Bad Request           Request error.
      401 Unauthorized          Authorization failed.
      403 Forbidden             No operation permission.
      404 Not Found             No resources found.
      500 Internal Server Error Internal service error.
      503 Service Unavailable   The service is unavailable.
      ========================= ===========================
