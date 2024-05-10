:original_name: dws_02_0065.html

.. _dws_02_0065:

Response
========

Status Code
-----------

After sending a request, you will receive a response, including a status code, response header, and response body.

A status code is a group of digits, ranging from 1xx to 5xx. It indicates the status of a request. For more information, see :ref:`Status Code <dws_02_0038>`.

For example, if status code **201** is returned for calling the API used to obtain a user token, the request is successful.

Response Header
---------------

Similar to a request, a response also has a header, for example, **content-type**.

:ref:`Figure 1 <en-us_topic_0000001185833138__f84b86cb0c7364361a19a908603852b51>` shows the response header fields for the API used to obtain a user token. The **x-subject-token** header field is the desired user token. You can use this token to authenticate the calling of other APIs.

.. _en-us_topic_0000001185833138__f84b86cb0c7364361a19a908603852b51:

.. figure:: /_static/images/en-us_image_0000001185833158.png
   :alt: **Figure 1** Header fields of the response to the request for obtaining a user token

   **Figure 1** Header fields of the response to the request for obtaining a user token

Response Body
-------------

The body of a response is often returned in structured format (for example, JSON or XML) as specified in the **Content-type** header field. The response body transfers content except the response header.

The following is part of the response body for the API used to obtain a user token.

.. code-block::

   {
       "token": {
           "expires_at": "2019-02-13T06:52:13.855000Z",
           "methods": [
               "password"
           ],
           "catalog": [
               {
                   "endpoints": [
                       {
                           "region_id": "eu-de",
   ......

If an error occurs during API calling, an error code and a message will be displayed. The following shows an error response body.

.. code-block::

   {
       "error_msg": "The format of message is error",
        "error_code": "AS.0001"
   }

In the response body, **error_code** is an error code, and **error_msg** provides information about the error.
