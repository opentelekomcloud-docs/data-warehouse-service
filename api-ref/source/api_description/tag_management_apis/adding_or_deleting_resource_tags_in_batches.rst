:original_name: dws_02_0047.html

.. _dws_02_0047:

Adding or Deleting Resource Tags in Batches
===========================================

Function
--------

This API is used to add or delete tags in batches for a specified resource. A maximum of 10 tags can be added for one resource.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/clusters/{resource_id}/tags/action

.. table:: **Table 1** URI parameters

   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                                  |
   +=============+===========+========+==============================================================================================================+
   | project_id  | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | resource_id | Yes       | String | Resource ID, for example, **7d85f602-a948-4a30-afd4-e84f47471c15**.                                          |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+---------------------------------------------------------------------------+----------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                                                                      | Description                                                          |
   +=================+=================+===========================================================================+======================================================================+
   | tags            | Yes             | List<:ref:`ResourceTag <en-us_topic_0000001185673164__table98198573205>`> | Tag list.                                                            |
   +-----------------+-----------------+---------------------------------------------------------------------------+----------------------------------------------------------------------+
   | action          | Yes             | String                                                                    | Identifies the operation. The value can be **create** or **delete**. |
   |                 |                 |                                                                           |                                                                      |
   |                 |                 |                                                                           | -  **create**: adds tags in batches.                                 |
   |                 |                 |                                                                           | -  **delete**: deletes tags in batches.                              |
   +-----------------+-----------------+---------------------------------------------------------------------------+----------------------------------------------------------------------+

.. _en-us_topic_0000001185673164__table98198573205:

.. table:: **Table 3** resource_tag

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                               |
   +=================+=================+=================+===========================================================================================================================================+
   | key             | Yes             | String          | Tag key. A tag key can contain a maximum of 36 Unicode characters, which cannot be null. The first and last characters cannot be spaces.  |
   |                 |                 |                 |                                                                                                                                           |
   |                 |                 |                 | It can contain only letters, digits, hyphens (-), and underscores (_).                                                                    |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Key value. A tag value can contain a maximum of 43 Unicode characters, which can be null. The first and last characters cannot be spaces. |
   |                 |                 |                 |                                                                                                                                           |
   |                 |                 |                 | It can contain only letters, digits, hyphens (-), and underscores (_).                                                                    |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

None

Example Request
---------------

-  Sample request for adding tags in batches

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/7d85f602-a948-4a30-afd4-e84f47471c15/tags/action
      {
          "action": "create",
          "tags": [
              {
                  "key": "Flower",
                  "value": "rose"
              },
              {
                  "key": "Food",
                  "value": "pie"
              }
          ]
      }

-  Sample request for deleting tags in batches

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/7d85f602-a948-4a30-afd4-e84f47471c15/tags/action
      {
          "action": "delete",
          "tags": [
              {
                  "key": "Flower",
                  "value": "rose"
               },
              {
                  "key": "Food",
                  "value": "pie"
              }
          ]
      }

Response Message
----------------

None

Status Code
-----------

============== =====================================
Returned Value Description
============== =====================================
204            Tags are added or deleted in batches.
400            Invalid tag.
401            Authentication failed.
403            Insufficient permission.
404            No resources found.
500            Internal service error.
============== =====================================
