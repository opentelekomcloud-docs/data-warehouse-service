:original_name: dws_04_0552.html

.. _dws_04_0552:

DBMS_RANDOM
===========

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001233681769__t8a5bcdf2282f4e64a850b8ae4a7f7076>` provides all interfaces supported by the **DBMS_RANDOM** package.

.. _en-us_topic_0000001233681769__t8a5bcdf2282f4e64a850b8ae4a7f7076:

.. table:: **Table 1** DBMS_RANDOM interface parameters

   +-------------------+-------------------------------------------------------------------------+
   | Interface         | Description                                                             |
   +===================+=========================================================================+
   | DBMS_RANDOM.SEED  | Sets a seed for a random number.                                        |
   +-------------------+-------------------------------------------------------------------------+
   | DBMS_RANDOM.VALUE | Generates a random number between a specified low and a specified high. |
   +-------------------+-------------------------------------------------------------------------+

-  DBMS_RANDOM.SEED

   The stored procedure SEED is used to set a seed for a random number. The DBMS_RANDOM.SEED function prototype is:

   ::

      DBMS_RANDOM.SEED (seed IN INTEGER);

   .. table:: **Table 2** DBMS_RANDOM.SEED interface parameters

      ========= =====================================
      Parameter Description
      ========= =====================================
      seed      Generates a seed for a random number.
      ========= =====================================

-  DBMS_RANDOM.VALUE

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

Example
-------

Generate a random number between 0 and 1.

::

   SELECT DBMS_RANDOM.VALUE(0,1);

Specify the low and high parameters to an integer within the specified range and intercept smaller values from the result. (The maximum value cannot be a possible value.) Therefore, use the following code for an integer between 0 and 99:

::

   SELECT TRUNC(DBMS_RANDOM.VALUE(0,100));
