:original_name: dws_06_0219.html

.. _dws_06_0219:

RESET
=====

Function
--------

**RESET** restores run-time parameters to their default values. The default values are parameter default values complied in the **postgresql.conf** configuration file.

**RESET** is an alternative spelling for:

**SET configuration_parameter TO DEFAULT**

Precautions
-----------

**RESET** and **SET** have the same transaction behavior. Their impact will be rolled back.

Syntax
------

.. code-block::

   RESET {configuration_parameter | CURRENT_SCHEMA | TIME ZONE | TRANSACTION ISOLATION LEVEL | SESSION AUTHORIZATION | ALL };

.. _en-us_topic_0000001145510787__se65334e5a0844cf2926813c622b3fc24:

Parameter Description
---------------------

-  **configuration_parameter**

   Specifies the name of a settable run-time parameter.

   Value range: Run-time parameters. You can view them by running the **SHOW ALL** command.

   .. note::

      Some parameters that viewed by **SHOW ALL** cannot be set by **SET**. For example, **max_datanodes**.

-  **CURRENT_SCHEMA**

   Specifies the current schema.

-  **TIME ZONE**

   Specifies the time zone.

-  **TRANSACTION ISOLATION LEVEL**

   Specifies the transaction isolation level.

-  **SESSION AUTHORIZATION**

   Specifies the session authorization.

-  **ALL**

   Resets all settable run-time parameters to default values.

Examples
--------

Reset **timezone** to the default value.

::

   RESET timezone;

Set all parameters to their default values.

::

   RESET ALL;

Helpful Links
-------------

:ref:`SET <dws_06_0220>`, :ref:`SHOW <dws_06_0224>`
