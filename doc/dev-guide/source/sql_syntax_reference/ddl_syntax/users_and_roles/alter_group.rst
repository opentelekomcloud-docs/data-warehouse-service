:original_name: dws_06_0127.html

.. _dws_06_0127:

ALTER GROUP
===========

Function
--------

This syntax modifies the attributes of a user group.

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

For details about more parameters, see :ref:`Parameter Description <en-us_topic_0000001764516506__se56072ec00a3453892cbc30531d221f5>` in **ALTER ROLE**.

.. table:: **Table 1** ALTER GROUP parameters

   +------------+--------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                  | Value Range                                                                                                      |
   +============+==============================================================+==================================================================================================================+
   | group_name | Specifies the name of the user group to be modified.         | A string that indicates the name of an existing group.                                                           |
   +------------+--------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | user_name  | Specifies the user to be added to or removed from the group. | Name of an existing user.                                                                                        |
   +------------+--------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_name   | Specifies the new name of a group.                           | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +------------+--------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+

Helpful Links
-------------

:ref:`CREATE GROUP <dws_06_0164>`, :ref:`DROP GROUP <dws_06_0194>`, and :ref:`ALTER ROLE <dws_06_0134>`
