:original_name: dws_02_0011.html

.. _dws_02_0011:

Obtaining a Project ID
======================

Obtaining a Project ID by Calling the API
-----------------------------------------

You can obtain the project ID by calling the IAM API used to query project information based on the specified criteria.

The API used to obtain a project ID is **GET https://{**\ *Endpoint*\ **}/v3/projects/**. {*Endpoint*} indicates the IAM endpoint and can be obtained from "Regions and Endpoints". For details about API authentication, see :ref:`Authentication <dws_02_0064>`.

The following is an example response. The value of **id** of **projects** indicates the project ID.

.. code-block::

   {
       "projects": [
           {
               "domain_id": "65382450e8f64ac0870cd180d14e684b",
               "is_domain": false,
               "parent_id": "65382450e8f64ac0870cd180d14e684b",
               "name": "eu-de-01",
               "description": "",
               "links": {
                   "next": null,
                   "previous": null,
                   "self": "https://www.example.com/v3/projects/a4a5d4098fb4474fa22cd05f897d6b99"
               },
               "id": "a4a5d4098fb4474fa22cd05f897d6b99",
               "enabled": true
           }
       ],
       "links": {
           "next": null,
           "previous": null,
           "self": "https://www.example.com/v3/projects"
       }
   }

Obtaining a Project ID from the Console
---------------------------------------

A project ID is required for some URLs when an API is called. To obtain a project ID, perform the following operations:

#. Log in to the management console.

#. Click the username and select **My Credential** from the drop-down list.

   On the **My Credential** page, view the project ID in the project list.


.. figure:: /_static/images/en-us_image_0000001186151638.jpg
   :alt: **Figure 1** Viewing project IDs

   **Figure 1** Viewing project IDs
