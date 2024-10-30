:original_name: dws_04_0552.html

.. _dws_04_0552:

DBMS_RANDOM
===========

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001460563132__t8a5bcdf2282f4e64a850b8ae4a7f7076>` provides all interfaces supported by the **DBMS_RANDOM** package.

.. _en-us_topic_0000001460563132__t8a5bcdf2282f4e64a850b8ae4a7f7076:

.. table:: **Table 1** DBMS_RANDOM interface parameters

   +--------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | API                                                                                        | Description                                                             |
   +============================================================================================+=========================================================================+
   | :ref:`DBMS_RANDOM.SEED <en-us_topic_0000001460563132__l4aad3da7b38a4442af63e28faf51e0ba>`  | Sets a seed for a random number.                                        |
   +--------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | :ref:`DBMS_RANDOM.VALUE <en-us_topic_0000001460563132__l605ebfc282024f8e8c0c32fc70b7bb67>` | Generates a random number between a specified low and a specified high. |
   +--------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+

-  .. _en-us_topic_0000001460563132__l4aad3da7b38a4442af63e28faf51e0ba:

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

-  .. _en-us_topic_0000001460563132__l605ebfc282024f8e8c0c32fc70b7bb67:

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

Example
-------

Generate a random number between 0 and 1:

::

   SELECT DBMS_RANDOM.VALUE(0,1);

To get a random integer in a range, use a low and high as the lower and upper bounds. The result will be greater than or equal to low, but less than high. To get an integer ranging from 0 to 99, run the following command:

::

   SELECT TRUNC(DBMS_RANDOM.VALUE(0,100));
