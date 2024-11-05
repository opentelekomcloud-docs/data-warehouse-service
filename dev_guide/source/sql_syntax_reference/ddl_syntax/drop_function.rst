:original_name: dws_06_0193.html

.. _dws_06_0193:

DROP FUNCTION
=============

Function
--------

Deletes an existing function.

Precautions
-----------

-  To delete an overloaded function, you must specify the function's parameter (argument) type. For non-overloaded functions, you can delete them by just specifying the function name.
-  If a function involves operations on temporary tables, the function cannot be deleted by running **DROP FUNCTION**.

Syntax
------

::

   DROP FUNCTION [ IF EXISTS ] function_name
   [ ( [ {[ argmode ] [ argname ] argtype} [, ...] ] ) [ CASCADE | RESTRICT ] ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified function does not exist.

-  **function_name**

   Specifies the name of the function to be deleted.

   Value range: an existing function name.

-  **argmode**

   Specifies the mode of a function parameter.

-  **argname**

   Specifies the name of a function parameter.

-  **argtype**

   Specifies the data type of a function parameter.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes all objects that depend on the function to be deleted (such as operators).
   -  **RESTRICT**: refuses to delete the function if any objects depend on it. This is the default.

Examples
--------

Delete a function named **add_two_number**:

::

   DROP FUNCTION add_two_number;

Helpful Links
-------------

:ref:`ALTER FUNCTION <dws_06_0126>`, :ref:`CREATE FUNCTION <dws_06_0163>`
