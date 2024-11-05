:original_name: dws_04_0531.html

.. _dws_04_0531:

Dynamically Calling Anonymous Blocks
====================================

This section describes how to execute anonymous blocks in dynamic statements. Append **IN** and **OUT** behind the **EXECUTE IMMEDIATE...USING** statement to input and output parameters.

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001460882408__fa37441543aa3495d84b876c58afc0a2d>` shows the syntax diagram.

.. _en-us_topic_0000001460882408__fa37441543aa3495d84b876c58afc0a2d:

.. figure:: /_static/images/en-us_image_0000001510522789.png
   :alt: **Figure 1** call_anonymous_block::=

   **Figure 1** call_anonymous_block::=

:ref:`Figure 2 <en-us_topic_0000001460882408__f1cfe980b10ab4ce88a9b967d374ec40e>` shows the syntax diagram for **using_clause**.

.. _en-us_topic_0000001460882408__f1cfe980b10ab4ce88a9b967d374ec40e:

.. figure:: /_static/images/en-us_image_0000001510163121.png
   :alt: **Figure 2** using_clause-4

   **Figure 2** using_clause-4

The above syntax diagram is explained as follows:

-  The execute part of an anonymous block starts with a **BEGIN** statement, has a break with an **END** statement, and ends with a semicolon (;).
-  **USING [IN|OUT|IN OUT]bind_argument**: specifies where the variable passed to the stored procedure parameter value is stored. The modifiers in front of **bind_argument** and of the corresponding parameter are the same.
-  The input and output parameters in the middle of an anonymous block are designated by placeholders. The numbers of the placeholders and the parameters are the same. The sequences of the parameters corresponding to the placeholders and the USING parameters are the same.
-  Currently in GaussDB(DWS), when dynamic statements call anonymous blocks, placeholders cannot be used to pass input and output parameters in an **EXCEPTION** statement.

Example
-------

::

   --Create the stored procedure dynamic_proc.
   CREATE OR REPLACE PROCEDURE dynamic_proc
   AS
      staff_id     NUMBER(6) := 200;
      first_name   VARCHAR2(20);
      salary       NUMBER(8,2);
   BEGIN
   --Execute the anonymous block.
       EXECUTE IMMEDIATE 'begin select first_name, salary into :first_name, :salary from staffs where staff_id= :dno; end;'
          USING OUT first_name, OUT salary, IN staff_id;
      dbms_output.put_line(first_name|| ' ' || salary);
   END;
   /

   -- Invoke the stored procedure.
   CALL dynamic_proc();

   -- Delete the stored procedure.
   DROP PROCEDURE dynamic_proc;
