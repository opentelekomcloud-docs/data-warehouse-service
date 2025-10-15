:original_name: dws_06_0210.html

.. _dws_06_0210:

DROP TEXT SEARCH CONFIGURATION
==============================

Function
--------

**DROP TEXT SEARCH CONFIGURATION** deletes an existing text search configuration.

Precautions
-----------

To run the **DROP TEXT SEARCH CONFIGURATION** command, you must be the owner of the text search configuration.

Syntax
------

::

   DROP TEXT SEARCH CONFIGURATION [ IF EXISTS ] name [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified text search configuration does not exist.

-  **name**

   Specifies the name (optionally schema-qualified) of a text search configuration to be deleted.

-  **CASCADE**

   Automatically deletes objects that depend on the text search configuration to be deleted.

-  **RESTRICT**

   Refuses to delete the text search configuration if any objects depend on it. This is the default.

Examples
--------

Delete the text search configuration **ngram1**:

::

   DROP TEXT SEARCH CONFIGURATION ngram1;

Helpful Links
-------------

:ref:`ALTER TEXT SEARCH CONFIGURATION <dws_06_0145>`, :ref:`CREATE TEXT SEARCH CONFIGURATION <dws_06_0182>`
