:original_name: dws_06_0127.html

.. _dws_06_0127:

ALTER GROUP
===========

Function
--------

**ALTER GROUP** modifies the attributes of a user group.

Precautions
-----------

**ALTER GROUP** is an alias for **ALTER ROLE**, and it is not a standard SQL command and not recommended. Users can use **ALTER ROLE** directly.

Syntax
------

-  Add users to a group.

   ::

      ALTER GROUP group_name
          ADD USER user_name [, ... ];

-  Remove users from a group.

   ::

      ALTER GROUP group_name
          DROP USER user_name [, ... ];

-  Change the name of the group.

   ::

      ALTER GROUP group_name
          RENAME TO new_name;

Parameter Description
---------------------

See the :ref:`Example <en-us_topic_0000001098830946__s8302a739997543e0a22f9ee43ce9bfbf>` in **ALTER ROLE**.

Helpful Links
-------------

:ref:`CREATE GROUP <dws_06_0164>`, :ref:`DROP GROUP <dws_06_0194>`, :ref:`ALTER ROLE <dws_06_0134>`
