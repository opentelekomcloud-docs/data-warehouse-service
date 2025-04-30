:original_name: dws_02_0064.html

.. _dws_02_0064:

Authentication
==============

Calling an API can be authenticated using tokens.

Token-based Authentication
--------------------------

A token specifies temporary permissions in a computer system. During API authentication using a token, the token is added to request headers to get permissions for calling the API.

.. note::

   The validity period of a token is 24 hours. When using a token for authentication, cache it to prevent frequently calling the IAM API used to obtain a user token.

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
                   "id": "xxxxxxxx"
               }
           }
       }
   }

After a token is obtained, the **X-Auth-Token** header field must be added to requests to specify the token when calling other APIs. If the token is **ABCDEFG....**, add **X-Auth-Token: ABCDEFG....** to a request.
