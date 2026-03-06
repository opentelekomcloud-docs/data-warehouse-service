:original_name: dws_02_0063.html

.. _dws_02_0063:

Making an API Request
=====================

This section describes the structure of a RESTful API request, and uses the IAM API for creating an IAM user as an example to describe how to call an API.

Request URI
-----------

A request URI is in the following format:

**{URI-scheme}://{Endpoint}/{resource-path}?{query-string}**

Although a request URI is included in the request header, most programming languages or frameworks require the request URI to be transmitted separately.

.. table:: **Table 1** URI parameter description

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                 |
   +===================================+=============================================================================================================================================================================================================================================================+
   | URI-scheme                        | Protocol used to transmit requests. All APIs use HTTPS.                                                                                                                                                                                                     |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Endpoint                          | Domain name or IP address of the server bearing the REST service endpoint. The endpoint varies between services in different regions. It can be obtained from Regions and Endpoints.                                                                        |
   |                                   |                                                                                                                                                                                                                                                             |
   |                                   | For example, the endpoint of IAM in the **eu-de** region is **iam.eu-de.otc.t-systems.com**.                                                                                                                                                                |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource-path                     | Access path of an API for performing a specified operation. Obtain the value from the URI of an API. For example, **resource-path** of the API used to create an IAM user is **/v3.0/OS-USER/users**.                                                       |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query-string                      | Query parameter, which is optional. The query parameter must be in the format of *parameter-name*\ **=**\ *parameter-value* and prefixed with a question mark (?). For example, **limit=10** indicates that a maximum of 10 data records will be displayed. |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   To simplify the URI display in this document, each API is provided only with a **resource-path** and a request method. The **URI-scheme** of all APIs is **HTTPS**, and the endpoints of all APIs in the same region are identical.

Request Methods
---------------

The HTTP protocol defines the following request methods that can be used to send a request to the server.

.. table:: **Table 2** HTTP methods

   +-----------------------------------+----------------------------------------------------------------------------+
   | Method                            | Description                                                                |
   +===================================+============================================================================+
   | GET                               | Requests the server to return specified resources.                         |
   +-----------------------------------+----------------------------------------------------------------------------+
   | PUT                               | Requests the server to update specified resources.                         |
   +-----------------------------------+----------------------------------------------------------------------------+
   | POST                              | Requests the server to add resources or perform special operations.        |
   +-----------------------------------+----------------------------------------------------------------------------+
   | DELETE                            | Requests the server to delete specified resources, for example, an object. |
   +-----------------------------------+----------------------------------------------------------------------------+
   | HEAD                              | Same as GET except that the server must return only the response header.   |
   +-----------------------------------+----------------------------------------------------------------------------+
   | PATCH                             | Requests the server to update partial content of a specified resource.     |
   |                                   |                                                                            |
   |                                   | If the resource does not exist, a new resource will be created.            |
   +-----------------------------------+----------------------------------------------------------------------------+

Request Header
--------------

You can also add additional header fields to a request, such as the fields required by a specified URI or HTTP method. For example, to request for the authentication information, add **Content-type**, which specifies the request body type.

Common request headers include:

-  **Content-Type**: specifies the request body type or format. This field is mandatory and its default value is **application/json**. Other values of this field (if any) will be provided for specific APIs.
-  **Authorization**: specifies signature authentication information. This field is optional. When AK/SK authentication is enabled, this field is automatically specified when SDK is used to sign the request. For more information, see :ref:`AK/SK-based Authentication <en-us_topic_0000002500174024__section19316110123319>`.
-  **X-Sdk-Date**: specifies the time when a request is sent. This field is optional. When AK/SK authentication is enabled, this field is automatically specified when SDK is used to sign the request. For more information, see :ref:`AK/SK-based Authentication <en-us_topic_0000002500174024__section19316110123319>`.
-  **X-Auth-Token**: specifies a user token only for token-based API authentication.
-  **X-Project-ID**: specifies subproject ID. This field is optional and can be used in multi-project scenarios. This field is mandatory in the request header for accessing resources in a sub-project through AK/SK-based authentication.
-  **X-Domain-ID**: specifies the account ID, which is optional. When you call APIs of global services using AK/SK-based authentication, **X-Domain-ID** needs to be configured in the request header.

Request Body
------------

The body of a request is often sent in a structured format (JSON or XML) as specified in the **Content-type** header field. The request body transfers content except the request header. If the request body contains Chinese characters, these characters must be coded in UTF-8. Specify the character encoding mode in **Content-type**, for example, **Content-Type: application/json; charset=utf-8**.

The request body varies between APIs. Some APIs do not require the request body, such as the APIs requested using the GET and DELETE methods.

In the case of the API used to , the request parameters and parameter description can be obtained from the API request. The following provides an example request with a body included.

-  **accountid**: ID of the account to which the IAM user belongs.
-  **username**: IAM username to be created.
-  **email**: email address of the IAM user.
-  **\*********\***: password of the IAM user.

.. code-block::

   Content-type: application/json
   X-Sdk-Date: 20240416T095341Z
    Authorization: SDK-HMAC-SHA256 Access=****************, SignedHeaders=content-type;host;x-sdk-date, Signature=****************
   {
         "user": {
             "domain_id": "accountid",
             "name": "username",
             "password": "**********",
             "email": "email",
             "description": "IAM User Description"
         }
     }

The content required by an API request is ready. You can use `curl <https://curl.haxx.se/>`__, `Postman <https://www.getpostman.com/>`__, or write code to send the request to call the API.
