:original_name: dws_02_0048.html

.. _dws_02_0048:

Querying Resources by Tag
=========================

Function
--------

This API is used to query resource instances that meet the specified tag filtering criteria.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/clusters/resource_instances/action

.. table:: **Table 1** URI parameters

   +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                  |
   +============+===========+========+==============================================================================================================+
   | project_id | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   +=================+=================+=======================================================================+====================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | tags            | No              | List<:ref:`tag <en-us_topic_0000001186151632__table11169125892211>`>  | The resources to be queried contain tags listed in **tags**. Each resource to be queried contains a maximum of 10 keys. Each tag key can have a maximum of 10 tag values. The tag value corresponding to each tag key can be an empty array but the structure cannot be missing. Each tag key must be unique, and each tag value in a tag must be unique. The response returns resources containing all tags in this list. Keys in this list are in an AND relationship while values in each key-value structure are in an OR relationship. If no tag filtering condition is specified, full data is returned.                     |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags_any        | No              | List<:ref:`tag <en-us_topic_0000001186151632__table11169125892211>`>  | The resources to be queried contain any tags listed in **tags_any**. Each resource to be queried contains a maximum of 10 keys. Each tag key can have a maximum of 10 tag values. The tag value corresponding to each tag key can be an empty array but the structure cannot be missing. Each tag key must be unique, and each tag value in a tag must be unique. The response returns resources containing the tags in this list. Keys in this list are in an OR relationship and values in each key-value structure are also in an OR relationship. If no tag filtering condition is specified, full data is returned.           |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | not_tags        | No              | List<:ref:`tag <en-us_topic_0000001186151632__table11169125892211>`>  | The resources to be queried do not contain tags listed in **not_tags**. Each resource to be queried contains a maximum of 10 keys. Each tag key can have a maximum of 10 tag values. The tag value corresponding to each tag key can be an empty array but the structure cannot be missing. Each tag key must be unique, and each tag value in a tag must be unique. The response returns resources containing no tags in this list. Keys in this list are in an AND relationship while values in each key-value structure are in an OR relationship. If no tag filtering condition is specified, full data is returned.           |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | not_tags_any    | No              | List<:ref:`tag <en-us_topic_0000001186151632__table11169125892211>`>  | The resources to be queried do not contain any tags listed in **not_tags_any**. Each resource to be queried contains a maximum of 10 keys. Each tag key can have a maximum of 10 tag values. The tag value corresponding to each tag key can be an empty array but the structure cannot be missing. Each tag key must be unique, and each tag value in a tag must be unique. The response returns resources containing no tags in this list. Keys in this list are in an OR relationship and values in each key-value structure are also in an OR relationship. If no tag filtering condition is specified, full data is returned. |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | limit           | No              | String                                                                | Maximum number of records returned in the query result. This parameter is not displayed when **action** is set to **count**. If **action** is set to **filter**, this parameter takes effect. Its value ranges from 1 to 1000 (default).                                                                                                                                                                                                                                                                                                                                                                                           |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | offset          | No              | String                                                                | Start location of pagination query. The query starts from the next resource of the specified location. When querying the data on the first page, you do not need to specify this parameter. When querying the data on subsequent pages, set this parameter to the value in the response body returned by querying data of the previous page. This parameter is not displayed when **action** is set to **count**. If **action** is set to **filter**, this parameter takes effect. Its value can be 0 (default) or a positive integer.                                                                                             |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | action          | Yes             | String                                                                | Identifies the operation. The value can be **filter** or **count**.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                 |                 |                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                 |                 |                                                                       | -  **filter**: indicates filtering. If both **limit** and **offset** are specified, the return result is paginated. If **limit** and **offset** are not specified, return result is paginated only when the number of returned records exceeds 1000.                                                                                                                                                                                                                                                                                                                                                                               |
   |                 |                 |                                                                       | -  **count** indicates the total number of returned records that meet the query criteria.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | matches         | No              | List<:ref:`match <en-us_topic_0000001186151632__table1117295882218>`> | Search field. **key** indicates the field to be matched, for example, **resource_name**. **value** indicates the fuzzy match result.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   +-----------------+-----------------+-----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001186151632__table11169125892211:

.. table:: **Table 3** **tag** field description

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                       |
   +=================+=================+=================+===================================================================================================================================================================================================================================================================+
   | key             | Yes             | String          | Tag key. A tag key can contain a maximum of 127 Unicode characters, which cannot be null. The first and last characters cannot be spaces.                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                                                                   |
   |                 |                 |                 | It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_).                                                                                                                                            |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | values          | Yes             | List<String>    | Tag value. A tag value can contain a maximum of 255 Unicode characters, which can be null. The first and last characters cannot be spaces. It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_). |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001186151632__table1117295882218:

.. table:: **Table 4** Description of field **Match**

   +-----------+-----------+--------+------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                            |
   +===========+===========+========+========================================================================+
   | key       | Yes       | String | Key. Currently, it can only be **resource_name**.                      |
   +-----------+-----------+--------+------------------------------------------------------------------------+
   | value     | Yes       | String | Key value. Each value can contain a maximum of 255 Unicode characters. |
   +-----------+-----------+--------+------------------------------------------------------------------------+

Response Parameters
-------------------

.. table:: **Table 5** Response body parameters

   +-------------+-----------+--------------------------------------------------------------------------+------------------------------------------+
   | Parameter   | Mandatory | Type                                                                     | Description                              |
   +=============+===========+==========================================================================+==========================================+
   | resources   | Yes       | List<:ref:`resource <en-us_topic_0000001186151632__table6260232102319>`> | Resources that meet the search criteria. |
   +-------------+-----------+--------------------------------------------------------------------------+------------------------------------------+
   | total_count | Yes       | Integer                                                                  | Total number of queried records.         |
   +-------------+-----------+--------------------------------------------------------------------------+------------------------------------------+

.. _en-us_topic_0000001186151632__table6260232102319:

.. table:: **Table 6** resource

   +----------------+-----------+-------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type                                                                          | Description                                                                                                |
   +================+===========+===============================================================================+============================================================================================================+
   | resource_id    | Yes       | String                                                                        | Resource ID.                                                                                               |
   +----------------+-----------+-------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | resouce_detail | Yes       | Object                                                                        | Resource details. The value is a resource object, used for extension. This value is left empty by default. |
   +----------------+-----------+-------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | tags           | Yes       | List<:ref:`resource_tag <en-us_topic_0000001186151632__table42631432172318>`> | List of tags. If no tag is matched, an empty array is returned.                                            |
   +----------------+-----------+-------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | resource_name  | Yes       | String                                                                        | Resource name. This parameter is an empty string by default if the resource name is not specified.         |
   +----------------+-----------+-------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001186151632__table42631432172318:

.. table:: **Table 7** **resource_ag** field description

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                      |
   +=================+=================+=================+==================================================================================================================================================================================================================================================================+
   | key             | Yes             | String          | Tag key. A tag key can contain a maximum of 36 Unicode characters, which cannot be null. The first and last characters cannot be spaces.                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                                                                  |
   |                 |                 |                 | It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_).                                                                                                                                           |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Key value. A tag value can contain a maximum of 43 Unicode characters, which can be null. The first and last characters cannot be spaces. It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_). |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

-  Sample request when **action** is set to **filter**

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/resource_instances/action
      {
        "offset": "100",
        "limit": "100",
        "action": "filter",
        "matches":[
            {
              "key": "resource_name",
              "value": "dws"
             }
        ],

        "tags": [
          {
            "key": "Flower",
            "values": [
              "rose",
              "holly"
            ]
          }
        ],
        "tags_any": [
          {
            "key": "Food",
            "values": [
              "pie"
            ]
          }
        ],
        "not_tags": [
          {
            "key": "juice",
            "values": [
              "Apple"

            ]
          }
        ],
        "not_tags_any": [
          {
            "key": "color",
            "values": [
              "Red",
              "Green"
            ]
          }
        ]

      }

-  Sample request when **Action** is set to **count**

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/resource_instances/action
      {
        "action": "count",

        "tags": [
          {
            "key": "Flower",
            "values": [
              "rose",
              "holly"
            ]
      },
      {
            "key": "Food",
            "values": [
              "pie",
              "rice"
            ]
          }
        ],
        "tags_any": [
          {
            "key": "Food",
            "values": [
              "pie"
            ]
          }
        ],
        "not_tags": [
          {
            "key": "juice",
            "values": [
              "apple"

            ]
          }
        ],
        "not_tags_any": [
          {
            "key": "color",
            "values": [
              "red",
              "green"
            ]
          }
        ],

      "matches":[
      {
              "key": "resource_name",
              "value": "GaussDB(DWS) "
             }
      ]
      }

Example Responses
-----------------

Response body when **action** is set to **filter**

.. code-block::

   {
         "resources": [
            {
               "resource_detail": null,
               "resource_id": "4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90",
               "resource_name": "dws-Flower-Food",
               "tags": [
                   {
                      "key": "Flower",
                      "value": "rose"
                   },
                   {
                      "key": "Flower",
                      "value": "holly"
                   }
                ]
            }
          ],
         "total_count": 1
   }

Response body when **action** is set to **count**

.. code-block::

   {
          "total_count": 1
   }

Status Code
-----------

============== ========================
Returned Value Description
============== ========================
200            Query succeeded.
400            Invalid parameter.
401            Authentication failed.
403            Insufficient permission.
404            No resources found.
500            Internal service error.
============== ========================
