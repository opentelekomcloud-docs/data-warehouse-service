:original_name: dws_04_0530.html

.. _dws_04_0530:

Dynamically Calling Stored Procedures
=====================================

This section describes how to dynamically call store procedures. You must use anonymous statement blocks to package stored procedures or statement blocks and append **IN** and **OUT** behind the **EXECUTE IMMEDIATE...USING** statement to input and output parameters.

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001098655136__fd10dcc31d2a145cab7c077a74e08a456>` shows the syntax diagram.

.. _en-us_topic_0000001098655136__fd10dcc31d2a145cab7c077a74e08a456:

.. figure:: /_static/images/en-us_image_0000001098815232.png
   :alt: **Figure 1** call_procedure::=

   **Figure 1** call_procedure::=

:ref:`Figure 2 <en-us_topic_0000001098655136__f26eaef86e5e1488c92f4579266153d2f>` shows the syntax diagram for **using_clause**.

.. _en-us_topic_0000001098655136__f26eaef86e5e1488c92f4579266153d2f:

.. figure:: /_static/images/en-us_image_0000001098975228.png
   :alt: **Figure 2** using_clause-3

   **Figure 2** using_clause-3

The above syntax diagram is explained as follows:

-  **CALL procedure_name**: calls the stored procedure.
-  **[:placeholder1,:placeholder2,...]**: specifies the placeholder list of the stored procedure parameters. The numbers of the placeholders and the parameters are the same.
-  **USING [IN|OUT|IN OUT]bind_argument**: specifies where the variable passed to the stored procedure parameter value is stored. The modifiers in front of **bind_argument** and of the corresponding parameter are the same.

Examples
--------

::

   --Create the stored procedure proc_add:
   CREATE OR REPLACE PROCEDURE proc_add
   (
       param1    in   INTEGER,
       param2    out  INTEGER,
       param3    in   INTEGER
   )
   AS
   BEGIN
      param2:= param1 + param3;
   END;
   /

   DECLARE
       input1 INTEGER:=1;
       input2 INTEGER:=2;
       statement  VARCHAR2(200);
       param2     INTEGER;
   BEGIN
      --Declare the call statement:
       statement := 'call proc_add(:col_1, :col_2, :col_3)';
      --Execute the statement:
       EXECUTE IMMEDIATE statement
           USING IN input1, OUT param2, IN input2;
       dbms_output.put_line('result is: '||to_char(param2));
   END;
   /

   -- Delete the stored procedure.
   DROP PROCEDURE proc_add;
