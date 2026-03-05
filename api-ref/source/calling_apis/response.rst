:original_name: dws_02_0065.html

.. _dws_02_0065:

Response
========

After sending a request, you will receive a response containing the status code, response header, and response body.

Status Code
-----------

A status code is a group of digits, ranging from 1xx to 5xx. It indicates the status of a request. For more information, see :ref:`Status Code <dws_02_0038>`.

Response Header
---------------

Similar to a request, a response also has a header, for example, **content-type**.


.. figure:: /_static/images/en-us_image_0000002531894007.png
   :alt: **Figure 1** Response headers

   **Figure 1** Response headers

Response Body
-------------

The body of a response is often returned in structured format (for example, JSON or XML) as specified in the **Content-type** header field. The response body transfers content except the response header.

.. code-block::

   {
       "user": {
           "id": "c131886aec...",
           "name": "IAMUser",
           "description": "IAM User Description",
           "areacode": "",
           "phone": "",
           "email": "***@***.com",
           "status": null,
           "enabled": true,
           "pwd_status": false,
           "access_mode": "default",
           "is_domain_owner": false,
           "xuser_id": "",
           "xuser_type": "",
           "password_expires_at": null,
           "create_time": "2024-05-21T09:03:41.000000",
           "domain_id": "d78cbac1..........",
           "xdomain_id": "30086000........",
           "xdomain_type": "",
           "default_project_id": null
       }
   }

If an error occurs during API calling, an error code and a message will be displayed. The following shows an error response body.

.. code-block::

   {
       "error_msg": "The format of message is error",
        "error_code": "AS.0001"
   }

In the response body, **error_code** is an error code, and **error_msg** provides information about the error.
