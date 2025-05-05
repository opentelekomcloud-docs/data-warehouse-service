:original_name: dws_mt_0135.html

.. _dws_mt_0135:

.. _en-us_topic_0000002076353973:

Date Functions
==============

This section describes the following date functions:

-  :ref:`ADD_MONTHS <en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section1351029182812>`
-  :ref:`DATE_TRUNC <en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section1521011916174>`
-  :ref:`LAST_DAY <en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section79301937142811>`
-  :ref:`MONTHS_BETWEEN <en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section47599521288>`
-  :ref:`SYSTIMESTAMP <en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section12301686294>`

.. _en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section1351029182812:

ADD_MONTHS
----------

**ADD_MONTHS** is an Oracle system function and is not implicitly supported by GaussDB(DWS).

.. note::

   Before using this function, perform the following operations:

   #. Create and use the **MIG_ORA_EXT** schema.
   #. Copy the content of the custom script and execute the script in all target databases for which migration is to be performed. For details, see :ref:`Migration Process <dws_16_0017>`.

   ADD_MONTHS returns a date with the month.

-  Data type of the **date** parameter is DATETIME.
-  Data type of the **integer** parameter is INTEGER.

The return type is DATE.

**Input** - **ADD_MONTHS**

::

   SELECT
             TO_CHAR( ADD_MONTHS ( hire_date ,1 ) ,'DD-MON-YYYY' ) "Next month"
        FROM
             employees
        WHERE
             last_name = 'Baer'
   ;

**Output**

::

   SELECT
             TO_CHAR( MIG_ORA_EXT.ADD_MONTHS ( hire_date ,1 ) ,'DD-MON-YYYY' ) "Next month"
        FROM
             employees
        WHERE
             last_name = 'Baer'
   ;

TO_DATE with Third Parameter
----------------------------

In TO_DATE(' 2019-05-02 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'), the third parameter should be commented.

**Input**

::

    CREATE TABLE PRODUCT
        ( prod_id       INTEGER
        , prod_code     VARCHAR(5)
        , prod_name     VARCHAR(100)
        , unit_price    NUMERIC(6,2) NOT NULL
        , manufacture_date  DATE DEFAULT sysdate )
    PARTITION BY RANGE (manufacture_date)
    (PARTITION "P_20190501"  VALUES LESS THAN (TO_DATE(' 2019-05-02 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
     );

**Output**

::

    CREATE TABLE PRODUCT
        ( prod_id       INTEGER
        , prod_code     VARCHAR(5)
        , prod_name     VARCHAR(100)
        , unit_price    NUMERIC(6,2) NOT NULL
        , manufacture_date  DATE DEFAULT sysdate )
    PARTITION BY RANGE (manufacture_date)
    (PARTITION "P_20190501"  VALUES LESS THAN (TO_DATE(' 2019-05-02 00:00:00', 'YYYY-MM-DD HH24:MI:SS'/* , 'NLS_CALENDAR=GREGORIAN' */))
     );

.. _en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section1521011916174:

DATE_TRUNC
----------

The DATE_TRUNC function returns a date with the time portion of the day truncated to the unit specified by the format model **fmt**.

**Input**

::

   select trunc(to_char(trunc(add_months(sysdate,-12),'MM'),'YYYYMMDD')/100) into v_start_date_s from dual;
   select trunc(to_char(trunc(sysdate,'mm'),'YYYYMMDD')/100) into v_end_date_e from dual;
   ID_MNTH>=TRUNC(TO_CHAR(ADD_MONTHS(to_date(to_char('||v_curr_date||'),''YYYYMMDD''),-12),''YYYYMMDD'')/100))
   AND ID_MNTH>=TRUNC(TO_CHAR(ADD_MONTHS(to_date(to_char('||v_curr_date||'),''YYYYMMDD''),-12),''YYYYMMDD'')/100))

   select TRUNC(to_char(add_months(trunc(TO_DATE(TO_CHAR(P_DATE),'YYYYMMDD'),'MM')-1,-2),'YYYYMMDD')/100) INTO START_MONTH from dual;
   select TRUNC(TO_CHAR(trunc(TO_DATE(TO_CHAR(P_DATE),'YYYYMMDD'),'MM')-1,'YYYYMMDD')/100) INTO END_MONTH from dual;

**Output**

::

   SELECT Trunc(To_char(Date_trunc ('MONTH', mig_ora_ext.Add_months (SYSDATE, -12)) , 'YYYYMMDD') / 100)
   INTO   v_start_date_s
   FROM   dual;

   SELECT Trunc(To_char(Date_trunc ('MONTH', SYSDATE), 'YYYYMMDD') / 100)
   INTO   v_end_date_e
   FROM   dual;

   SELECT Trunc(To_char(mig_ora_ext.Add_months (Date_trunc ('MONTH', To_date(To_char(p_date), 'YYYYMMDD' )) - 1 , -2), 'YYYYMMDD') / 100)
   INTO   start_month
   FROM   dual;

   SELECT Trunc(To_char(Date_trunc ('MONTH', To_date(To_char(p_date), 'YYYYMMDD')) - 1, 'YYYYMMDD') / 100)
   INTO   end_month
   FROM   dual;

.. _en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section79301937142811:

LAST_DAY
--------

The Oracle LAST_DAY function returns the last day of the month based on a date value.

.. code-block::

   LAST_DAY(date)

The return type is always DATE, regardless of the data type of the date.

**LAST_DAY** is an Oracle system function and is not implicitly supported by GaussDB(DWS). To support this function, DSC creates a LAST_DAY function in the **MIG_ORA_EXT** schema. The migrated statements will use the new function MIG_ORA_EXT.LAST_DAY as shown in the following example.

.. note::

   Before using this function, perform the following operations:

   #. Create and use the **MIG_ORA_EXT** schema.
   #. Copy the content of the custom script and execute the script in all target databases for which migration is to be performed. For details, see :ref:`Migration Process <dws_16_0017>`.

**Input** - **LAST_DAY**

::

    SELECT
             to_date( '01/' || '07/' || to_char( sysdate ,'YYYY' ) ,'dd/mm/yyyy' ) FIRST
             ,last_day( to_date( '01/' || '07/' || to_char( sysdate ,'YYYY' ) ,'dd/mm/yyyy' ) ) last__day
      FROM
             dual;

**Output**

::

   SELECT
             to_date( '01/' || '07/' || to_char( sysdate ,'YYYY' ) ,'dd/mm/yyyy' ) FIRST
             ,MIG_ORA_EXT.LAST_DAY (
                  to_date( '01/' || '07/' || to_char( sysdate ,'YYYY' ) ,'dd/mm/yyyy' )
             ) last__day
     FROM
             dual;

.. _en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section47599521288:

MONTHS_BETWEEN
--------------

The MONTHS_BETWEEN function returns the number of months between two dates.

**MONTHS_BETWEEN** is an Oracle system function and is not implicitly supported by GaussDB(DWS). To support this function, use DSC to create a MONTHS_BETWEEN function in the **MIG_ORA_EXT** schema. The migrated statements will use the new function MIG_ORA_EXT.MONTHS_BETWEEN as shown in the following example.

.. note::

   Before using this function, perform the following operations:

   #. Create and use the **MIG_ORA_EXT** schema.
   #. Copy the content of the custom script and execute the script in all the target databases to which data is to be migrated. For details, see :ref:`Migration Process <dws_16_0017>`.

**Input** - **MONTHS_BETWEEN**

.. code-block::

   Select Months_Between(to_date('2017-06-20', 'YYYY-MM-DD'), to_date('2011-06-20', 'YYYY-MM-DD')) from dual;

**Output**

.. code-block::

   Select MIG_ORA_EXT.MONTHS_BETWEEN(to_date('2017-06-20', 'YYYY-MM-DD'), to_date('2011-06-20', 'YYYY-MM-DD')) from dual;

.. _en-us_topic_0000002076353973__en-us_topic_0000001657865290_en-us_topic_0238518396_en-us_topic_0237362331_en-us_topic_0202727110_section12301686294:

SYSTIMESTAMP
------------

The SYSTIMESTAMP function returns the system date, including fractional seconds and time zones, of the system on which the database resides. The return type is **TIMESTAMP WITH TIME ZONE**.


.. figure:: /_static/images/en-us_image_0000001658025146.png
   :alt: **Figure 1** **Input** - **SYSTIMESTAMP**

   **Figure 1** **Input** - **SYSTIMESTAMP**


.. figure:: /_static/images/en-us_image_0000001657865822.png
   :alt: **Figure 2** **Output** - **SYSTIMESTAMP**

   **Figure 2** **Output** - **SYSTIMESTAMP**
