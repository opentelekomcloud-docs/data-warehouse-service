:original_name: dws_02_0064.html

.. _dws_02_0064:

Authentication
==============

You can use either of the following authentication methods when calling APIs:

-  AK/SK authentication: Requests are encrypted using an AK/SK.
-  Token authentication: Requests are authenticated using a token.

.. _en-us_topic_0000002500174024__section19316110123319:

AK/SK-based Authentication
--------------------------

.. note::

   -  AK/SK-based authentication supports API requests with a body no larger than 12 MB. For API requests with a larger body, you should use token-based authentication.
   -  You can use the AK/SK in a permanent or temporary access key. The **X-Security-Token** field must be configured if the AK/SK in a temporary access key is used, and the field value is **security_token** of the temporary access key.

In AK/SK-based authentication, the AK/SK is used to sign requests and the signature is then added to the requests for authentication.

-  AK: access key ID, which is a unique identifier used with a secret access key to sign requests cryptographically.
-  SK: secret access key. It is used together with an AK to sign requests. They can identify request senders and prevent requests from being modified.

In AK/SK-based authentication, you can use the AK/SK to sign requests based on the signature algorithm or use a dedicated signing SDK to sign requests. For details about how to sign requests and use the signing SDK, see the AK/SK Signing and Authentication Guide.

.. important::

   The signing SDKs are only used for signing requests and different from the SDKs provided by services.

Token-based Authentication
--------------------------

A token specifies temporary permissions in a computer system. During API authentication using a token, the token is added to request headers to get permissions for calling the API.

.. note::

   -  Cache the token for authentication to prevent frequently calling the IAM API to obtain the token. A token is valid for 24 hours.
   -  Ensure that the token is valid while you use it. Using a token that will soon expire may cause API calling failures.

When calling the API to obtain a user token, you must set **auth.scope** in the request body to **project**.

.. code-block::

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
                   "name": "xxxxxxxx"
               }
           }
       }
   }

After a token is obtained, the **X-Auth-Token** header field must be added to requests to specify the token when calling other APIs. For example, if the token is **ABCDEFG...**, add **X-Auth-Token: ABCDEFG....** to the request header.
