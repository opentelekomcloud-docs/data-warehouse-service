:original_name: dws_04_0552.html

.. _dws_04_0552:

DBMS_RANDOM
===========

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001145494453__t93766423da5a44c296f075b7dfc7af5a>` provides all interfaces supported by the **DBMS_RANDOM** package.

.. _en-us_topic_0000001145494453__t93766423da5a44c296f075b7dfc7af5a:

.. table:: **Table 1** DBMS_RANDOM interface parameters

   +--------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | API                                                                                        | Description                                                             |
   +============================================================================================+=========================================================================+
   | :ref:`DBMS_RANDOM.SEED <en-us_topic_0000001145494453__lac6fccaa336645a8b4a6d52272b2af1e>`  | Sets a seed for a random number.                                        |
   +--------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | :ref:`DBMS_RANDOM.VALUE <en-us_topic_0000001145494453__lf224bfe4ed9b4cab864ddace5779ab58>` | Generates a random number between a specified low and a specified high. |
   +--------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494453__lac6fccaa336645a8b4a6d52272b2af1e:

   DBMS_RANDOM.SEED

   The stored procedure SEED is used to set a seed for a random number. The DBMS_RANDOM.SEED function prototype is:

   ::

      DBMS_RANDOM.SEED (seed  IN  INTEGER);

   .. table:: **Table 2** DBMS_RANDOM.SEED interface parameters

      ========= =====================================
      Parameter Description
      ========= =====================================
      seed      Generates a seed for a random number.
      ========= =====================================

-  .. _en-us_topic_0000001145494453__lf224bfe4ed9b4cab864ddace5779ab58:

   DBMS_RANDOM.VALUE

   The stored procedure VALUE generates a random number between a specified low and a specified high. The DBMS_RANDOM.VALUE function prototype is:

   ::

      DBMS_RANDOM.VALUE(
      low  IN  NUMBER,
      high IN  NUMBER)
      RETURN NUMBER;

   .. table:: **Table 3** DBMS_RANDOM.VALUE interface parameters

      +-----------+----------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                              |
      +===========+==========================================================================================================+
      | low       | Sets the low bound for a random number. The generated random number is greater than or equal to the low. |
      +-----------+----------------------------------------------------------------------------------------------------------+
      | high      | Sets the high bound for a random number. The generated random number is less than the high.              |
      +-----------+----------------------------------------------------------------------------------------------------------+

.. note::

   The only requirement is that the parameter type is **NUMERIC** regardless of the right and left bound values.

Examples
--------

::

   -- Generate a random number between 0 and 1:
   SELECT DBMS_RANDOM.VALUE(0,1);

   -- Add the low and high parameters to an integer within the specified range and intercept smaller values from the result. (The maximum value cannot be a possible value.) Therefore, use the following code for an integer between 0 and 99:
   SELECT TRUNC(DBMS_RANDOM.VALUE(0,100));
