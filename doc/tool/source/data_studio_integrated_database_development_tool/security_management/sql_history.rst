:original_name: DWS_DS_153.html

.. _DWS_DS_153:

SQL History
===========

The following information is critical to manage security for Data Studio:

-  SQL History scripts are not encrypted.
-  The **SQL History** list does not display sensitive queries that contain the following keywords:

   -  Alter Role
   -  Alter User
   -  Create Role
   -  Create User
   -  Identified by
   -  Password

-  Few query syntax examples are listed below:

   -  ALTER USER name [[ WITH ] option [ ... ]]
   -  CREATE USER name [ [ WITH ] option [ ... ] ]
   -  CREATE ROLE name [ [ WITH ] option [ ... ] ]
   -  ALTER ROLE name [ [ WITH ] option [ ... ] ]
