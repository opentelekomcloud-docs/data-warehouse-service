:original_name: dws_06_0311.html

.. _dws_06_0311:

date_part
=========

The **date_part** function is modeled on the traditional Ingres equivalent to the SQL-standard function **extract**:

::

   date_part('field', source)

Note that the **field** must be a string, rather than a name. The valid field names are the same as those for **extract**. For details, see :ref:`EXTRACT <dws_06_0310>`.

Example:

::

   SELECT date_part('day', TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           16
   (1 row)

::

   SELECT date_part('hour', interval '4 hours 3 minutes');
    date_part
   -----------
            4
   (1 row)
