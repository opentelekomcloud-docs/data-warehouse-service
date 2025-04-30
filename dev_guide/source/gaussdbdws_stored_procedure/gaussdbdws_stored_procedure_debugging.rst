:original_name: dws_04_0558.html

.. _dws_04_0558:

GaussDB(DWS) Stored Procedure Debugging
=======================================

Syntax
------

RAISE has the following five syntax formats:


.. figure:: /_static/images/en-us_image_0000001811491549.png
   :alt: **Figure 1** raise_format::=

   **Figure 1** raise_format::=


.. figure:: /_static/images/en-us_image_0000001764651228.png
   :alt: **Figure 2** raise_condition::=

   **Figure 2** raise_condition::=


.. figure:: /_static/images/en-us_image_0000001811491545.png
   :alt: **Figure 3** raise_sqlstate::=

   **Figure 3** raise_sqlstate::=


.. figure:: /_static/images/en-us_image_0000001764651232.png
   :alt: **Figure 4** raise_option::=

   **Figure 4** raise_option::=

.. _en-us_topic_0000001764491712__f83a9e69cf9474bd5a885130e5c140da8:

.. figure:: /_static/images/en-us_image_0000001764651224.png
   :alt: **Figure 5** raise::=

   **Figure 5** raise::=

Parameter description:

-  The level option is used to specify the error level, that is, **DEBUG**, **LOG**, **INFO**, **NOTICE**, **WARNING**, or **EXCEPTION** (default). **EXCEPTION** reports an error that normally terminates the current transaction and the others only generate information at their levels. The :ref:`log_min_messages <en-us_topic_0000001811490985__s1ffb0797361d413d875381200fed970b>` and :ref:`client_min_messages <en-us_topic_0000001811490985__sbd8ad9bb6b9b48ba97f998f060dc56f3>` parameters control whether the error messages of specific levels are reported to the client and are written to the server log.

-  **format**: specifies the error message text to be reported, a format character string. The format character string can be appended with an expression for insertion to the message text. In a format character string, **%** is replaced by the parameter value attached to format and **%%** is used to print **%**. For example:

   .. code-block::

      --v_job_id replaces % in the character string.
      RAISE NOTICE 'Calling cs_create_job(%)',v_job_id;

-  option = expression: inserts additional information to an error report. The keyword option can be **MESSAGE**, **DETAIL**, **HINT**, or **ERRCODE**, and each expression can be any character string.

   -  MESSAGE: specifies the error message text. This option cannot be used in a RAISE statement that contains a format character string in front of USING.
   -  DETAIL: specifies detailed information of an error.
   -  HINT: prints hint information.
   -  **ERRCODE**: designates an error code (SQLSTATE) to a report. A condition name or a five-character SQLSTATE error code can be used.

-  condition_name: specifies the condition name corresponding to the error code.

-  sqlstate: specifies the error code.

If neither a condition name nor an **SQLSTATE** is designated in a **RAISE EXCEPTION** command, the **RAISE EXCEPTION (P0001)** is used by default. If no message text is designated, the condition name or SQLSTATE is used as the message text by default.

.. important::

   If the **SQLSTATE** designates an error code, the error code is not limited to a defined error code. It can be any error code containing five digits or ASCII uppercase rather than **00000**. Avoid using error codes that end with three zeros as they are type codes and can be captured by the entire category.

.. note::

   The syntax described in :ref:`Figure 5 <en-us_topic_0000001764491712__f83a9e69cf9474bd5a885130e5c140da8>` does not append any parameter. This form is used only for the **EXCEPTION** statement in a **BEGIN** block so that the error can be re-processed.

Examples
--------

Display error and hint information when a transaction terminates:

::

   CREATE OR REPLACE PROCEDURE proc_raise1(user_id in integer)
   AS
   BEGIN
   RAISE EXCEPTION 'Noexistence ID --> %',user_id USING HINT = 'Please check your user ID';
   END;
   /

   call proc_raise1(300011);

   -- Execution result:
   ERROR:  Noexistence ID --> 300011
   HINT:  Please check your user ID

Two methods are available for setting **SQLSTATE**:

::

   CREATE OR REPLACE PROCEDURE proc_raise2(user_id in integer)
   AS
   BEGIN
   RAISE 'Duplicate user ID: %',user_id USING ERRCODE = 'unique_violation';
   END;
   /

   \set VERBOSITY verbose
   call proc_raise2(300011);

   -- Execution result:
   ERROR:  Duplicate user ID: 300011
   SQLSTATE: 23505
   LOCATION:  exec_stmt_raise, pl_exec.cpp:3482

If the main parameter is a condition name or **SQLSTATE**, the following applies:

RAISE division_by_zero;

RAISE SQLSTATE '22012';

For example:

.. code-block::

   CREATE OR REPLACE PROCEDURE division(div in integer, dividend in integer)
   AS
   DECLARE
   res int;
       BEGIN
       IF dividend=0 THEN
           RAISE division_by_zero;
           RETURN;
       ELSE
           res := div/dividend;
           RAISE INFO 'division result: %', res;
           RETURN;
       END IF;
       END;
   /
   call division(3,0);

   -- Execution result:
   ERROR:  division_by_zero

Alternatively:

::

   RAISE unique_violation USING MESSAGE = 'Duplicate user ID: ' || user_id;
