:original_name: dws_04_0521.html

.. _dws_04_0521:

Anonymous Block
===============

An anonymous block applies to a script infrequently executed or a one-off activity. An anonymous block is executed in a session and is not stored.

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001145494671__f0d3f58494e2241b8867d81e8cc747d36>` shows the syntax diagrams for an anonymous block.

.. _en-us_topic_0000001145494671__f0d3f58494e2241b8867d81e8cc747d36:

.. figure:: /_static/images/en-us_image_0000001145495161.png
   :alt: **Figure 1** anonymous_block::=

   **Figure 1** anonymous_block::=

Details about the syntax diagram are as follows:

-  The execute part of an anonymous block starts with a **BEGIN** statement, has a break with an **END** statement, and ends with a semicolon (;). Type a slash (/) and press **Enter** to execute the statement.

   .. important::

      The terminator "/" must be written in an independent row.

-  The declaration section includes the variable definition, type, and cursor definition.
-  A simplest anonymous block does not execute any commands. At least one statement, even a null statement, must be presented in any implementation blocks.

Examples
--------

The following lists basic anonymous block programs:

::

   -- Null statement block:
   BEGIN
        NULL;
   END;
   /

   -- Print information to the console:
   BEGIN
        dbms_output.put_line('hello world!');
   END;
   /

   -- Print variable contents to the console:
   DECLARE
        my_var VARCHAR2(30);
   BEGIN
        my_var :='world';
        dbms_output.put_line('hello'||my_var);
   END;
   /
