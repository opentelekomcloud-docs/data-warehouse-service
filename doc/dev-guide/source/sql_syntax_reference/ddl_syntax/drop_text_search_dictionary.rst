:original_name: dws_06_0211.html

.. _dws_06_0211:

DROP TEXT SEARCH DICTIONARY
===========================

Function
--------

**DROP** **TEXT SEARCH** **DICTIONARY** deletes a full-text retrieval dictionary.

Precautions
-----------

-  **DROP** is not supported by predefined dictionaries.
-  Only the owner of a dictionary can do **DROP** to the dictionary. System administrators have this permission by default.
-  Execute **DROP...CASCADE** only when necessary because this operation will delete the text search configuration that uses this dictionary.

Syntax
------

::

   DROP TEXT SEARCH DICTIONARY [ IF EXISTS ] name [ CASCADE | RESTRICT ]

Parameter Description
---------------------

-  **IF EXISTS**

   Reports a notice instead of throwing an error if the specified full-text retrieval dictionary does not exist.

-  *name*

   Specifies the name of a dictionary to be deleted. (If you do not specify a schema name, the dictionary in the current schema will be deleted by default.)

   Value range: name of an existing dictionary

-  **CASCADE**

   Automatically deletes dependent objects of a dictionary and then deletes all dependent objects of these objects in sequence.

   If any text search configuration that uses the dictionary exists, **DROP** execution will fail. You can add **CASCADE** to delete all text search configurations and dictionaries that use the dictionary.

-  **RESTRICT**

   Rejects the deletion of a dictionary if any object depends on the dictionary. This is the default.

Examples
--------

Delete the **english** dictionary:

::

   DROP TEXT SEARCH DICTIONARY english;

Helpful Links
-------------

:ref:`ALTER TEXT SEARCH DICTIONARY <dws_06_0146>`, :ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>`
