:original_name: dws_16_0098.html

.. _dws_16_0098:

TIMESTAMP
=========

**Input - TIMESTAMP with FORMAT**

The FORMAT phrase sets the format for a specific TIME or TIMESTAMP column or value. A FORMAT phrase overrides the system format.

::

   SELECT 'StartDTTM' as a
                ,CURRENT_TIMESTAMP (FORMAT 'HH:MI:SSBMMMBDD,BYYYY');

**Output**

::

   SELECT 'StartDTTM' AS a
                ,TO_CHAR( CURRENT_TIMESTAMP ,'HH:MI:SS MON DD, YYYY' ) ;

TIMESTAMP Typecasting
---------------------

**Input**

::

   COALESCE( a.Snd_Tm ,TIMESTAMP '0001-01-01 00:00:00' )

**Output**

::

   COALESCE( a.Snd_Tm , CAST('0001-01-01 00:00:00' AS TIMESTAMP) )
