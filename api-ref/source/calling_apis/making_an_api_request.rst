:original_name: dws_02_0063.html

.. _dws_02_0063:

Making an API Request
=====================

This section describes the structure of a REST API request, and describes how to call an API by obtaining a user token of the IAM service. The obtained token can then be used to authenticate the calling of other APIs.

Request URI
-----------

A request URI is in the following format:

**{URI-scheme}://{Endpoint}/{resource-path}?{query-string}**

Although a request URI is included in the request header, most programming languages or frameworks require the request URI to be transmitted separately.

.. table:: **Table 1** URI parameter description

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                        |
   +===================================+====================================================================================================================================================================================================================================================================+
   | URI-scheme                        | Protocol used to transmit requests. All APIs use HTTPS.                                                                                                                                                                                                            |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Endpoint                          | Domain name or IP address of the server bearing the REST service. The endpoint varies between services in different regions. It can be obtained from the **Regions and Endpoints** section.                                                                        |
   |                                   |                                                                                                                                                                                                                                                                    |
   |                                   | For example, the endpoint of IAM in the **eu-de** region is **iam.eu-de.otc.t-systems.com**.                                                                                                                                                                       |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource-path                     | Access path of an API for performing a specified operation. Obtain the path from the URI of an API. For example, the **resource-path** of the API used to obtain a user token is **/v3/auth/tokens**.                                                              |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query-string                      | Query parameter, which is optional. Ensure that a question mark (?) is included before each query parameter that is in the format of "*Parameter name=Parameter value*". For example, **?limit=10** indicates that a maximum of 10 data records will be displayed. |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   To simplify the URI display in this document, each API is provided only with a **resource-path** and a request method. The **URI-scheme** of all APIs is **HTTPS**, and the endpoints of all APIs in the same region are identical.

Request Methods
---------------

The HTTP protocol defines the following request methods that can be used to send a request to the server:

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

For example, in the case of the API used to obtain a user token, the request method is POST. The request is as follows:

.. code-block:: text

   POST https://iam.eu-de.otc.t-systems.com/v3/auth/tokens

Request Header
--------------

You can also add additional header fields to a request, such as the fields required by a specified URI or HTTP method. For example, to request for the authentication information, add **Content-type**, which specifies the request body type.

For details about common request headers, see :ref:`Table 3 <en-us_topic_0000001180444265__t771a2351c332419f86fd66136fc5ebae>`.

.. _en-us_topic_0000001180444265__t771a2351c332419f86fd66136fc5ebae:

.. table:: **Table 3** Common request header fields

   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | Field           | Description                                                                                                                                                                                                                                                                  | Mandatory                                                   | Example                                    |
   +=================+==============================================================================================================================================================================================================================================================================+=============================================================+============================================+
   | x-sdk-date      | Time when the request is sent. The time is in **YYYYMMDD'T'HHMMSS'Z'** format.                                                                                                                                                                                               | No                                                          | 20150907T101459Z                           |
   |                 |                                                                                                                                                                                                                                                                              |                                                             |                                            |
   |                 | The value is the current GMT time of the system.                                                                                                                                                                                                                             |                                                             |                                            |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | Host            | Server information of the resource being requested. The value can be obtained from the URL of the service API. The value is in the format of *hostname[:port]*. If the port number is not specified, the default port is used. The default port number for HTTPS is **443**. | No                                                          | code.test.com                              |
   |                 |                                                                                                                                                                                                                                                                              |                                                             |                                            |
   |                 |                                                                                                                                                                                                                                                                              |                                                             | or                                         |
   |                 |                                                                                                                                                                                                                                                                              |                                                             |                                            |
   |                 |                                                                                                                                                                                                                                                                              |                                                             | code.test.com:443                          |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | Content-Type    | Request body MIME type. You are advised to use the default value **application/json**. For APIs used to upload objects or images, the value can vary depending on the flow type.                                                                                             | Yes                                                         | application/json                           |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | Content-Length  | Length of the request body. The unit is byte.                                                                                                                                                                                                                                | No                                                          | 3495                                       |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | X-Project-id    | Project ID. Obtain the project ID by following the instructions in :ref:`Obtaining a Project ID <dws_02_0011>`.                                                                                                                                                              | No                                                          | e9993fc787d94b6c886cbaa340f9c0f4           |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | X-Auth-Token    | User token.                                                                                                                                                                                                                                                                  | No                                                          | The following is part of an example token: |
   |                 |                                                                                                                                                                                                                                                                              |                                                             |                                            |
   |                 | The user token is a response to the API used to obtain a user token. This API is the only one that does not require authentication.                                                                                                                                          | This parameter is mandatory for token-based authentication. | MIIPAgYJKoZIhvcNAQcCo...ggg1BBIINPXsidG9rZ |
   |                 |                                                                                                                                                                                                                                                                              |                                                             |                                            |
   |                 | The **X-Subject-Token** value contained in the returned message header is the token.                                                                                                                                                                                         |                                                             |                                            |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+
   | X-Language      | Request language.                                                                                                                                                                                                                                                            | No                                                          | en_us                                      |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+--------------------------------------------+

The API used to obtain a user token does not require authentication. Therefore, only the **Content-type** field needs to be added to requests for calling the API. An example of such requests is as follows:

.. code-block:: text

   POST https://iam.eu-de.otc.t-systems.com/v3/auth/tokens
   Content-type: application/json

Request Body
------------

The body of a request is often sent in a structured format (JSON or XML) as specified in the **Content-type** header field. The request body transfers content except the request header. If the request body contains Chinese characters, these characters must be coded in UTF-8.

The request body varies between APIs. Some APIs do not require the request body, such as the APIs requested using the GET and DELETE methods.

In the case of the API used to obtain a user token, the request parameters and parameter description can be obtained from the API request. The following provides an example request with a body included. Replace **user_name**, **domainname** (account name), **\*******\*** (login password), and **xxxxxxxxxxxxxxxxxx** (project ID) with actual ones. Obtain the project ID from the database administrator.

.. note::

   The **scope** parameter specifies where a token takes effect. In the example, the token takes effect only on the resources specified by the project. In the following example, the token takes effect only for the resources in a specified project. For more information about this API, see "Obtaining a User Token".

.. code-block:: text

   POST https://iam.eu-de.otc.t-systems.com/v3/auth/tokens
   Content-type: application/json

   {
       "auth": {
           "identity": {
               "methods": [
                   "password"
               ],
               "password": {
                   "user": {
                       "name": "user_name",
                       "password": "********",
                       "domain": {
                           "name": "domainname"
                       }
                   }
               }
           },
           "scope": {
               "project": {
                   "id": "xxxxxxxxxxxxxxxxxx"
               }
           }
       }
   }

If all data required for the API request is available, you can send the request to call the API through `curl <https://curl.haxx.se/>`__, `Postman <https://www.getpostman.com/>`__, or coding. In the response to the API used to obtain a user token, **x-subject-token** is the desired user token. This token can then be used to authenticate the calling of other APIs.
