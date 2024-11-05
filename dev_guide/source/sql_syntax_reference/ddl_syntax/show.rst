:original_name: dws_06_0224.html

.. _dws_06_0224:

SHOW
====

Function
--------

Shows the current value of a runtime parameter. You can use the **SET** statement to set these parameters.

Precautions
-----------

Some parameters that can be viewed by running the **SHOW** command are read-only. You can view but cannot modify their values.

Syntax
------

::

   SHOW
     {
       configuration_parameter |
       CURRENT_SCHEMA |
       TIME ZONE |
       TRANSACTION ISOLATION LEVEL |
       SESSION AUTHORIZATION |
       ALL
     };

Parameter Description
---------------------

See :ref:`Parameter Description <en-us_topic_0000001510161381__se65334e5a0844cf2926813c622b3fc24>` in **RESET**.

Examples
--------

Show the value of **timezone**.

::

   SHOW timezone;

Show the current setting of the **DateStyle** parameter.

::

   SHOW DateStyle;

Show the current setting of all parameters.

::

   SHOW ALL;

Helpful Links
-------------

:ref:`SET <dws_06_0220>`, :ref:`RESET <dws_06_0219>`
