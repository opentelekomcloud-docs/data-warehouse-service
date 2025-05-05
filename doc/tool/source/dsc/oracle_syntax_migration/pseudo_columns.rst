:original_name: dws_mt_0128.html

.. _dws_mt_0128:

Pseudo Columns
==============

This section contains the migration syntax of Oracle Pseudo Columns. The migration syntax decides how the keywords/features are migrated.

A **pseudo column** is similar to a table column, but is not actually stored in the table. User can select values from pseudo columns, but cannot insert, update, or delete values in the pseudo columns.

ROWID
-----

The **ROWID** pseudo column returns the address of a specific row.


.. figure:: /_static/images/en-us_image_0000001658025254.png
   :alt: **Figure 1** Input - ROWID

   **Figure 1** Input - ROWID


.. figure:: /_static/images/en-us_image_0000001657865930.png
   :alt: **Figure 2** Output - ROWID

   **Figure 2** Output - ROWID

ROWNUM
------

For each row of data returned by the query, the **ROWNUM** pseudo column returns a number, indicating the order with which the Oracle database selects rows from a table or a group of joined rows. The value of **ROWNUM** in the first row is **1**, the value of **ROWNUM** in the second row is **2**, and so on.


.. figure:: /_static/images/en-us_image_0000001658025258.png
   :alt: **Figure 3** Input - ROWNUM

   **Figure 3** Input - ROWNUM


.. figure:: /_static/images/en-us_image_0000001657865926.png
   :alt: **Figure 4** Output - ROWNUM

   **Figure 4** Output - ROWNUM

**Input-ROWNUM with UPDATE**

When executing **UPDATE**, if ROWNUM with some value (integer) is used, the system will UPDATE records using the operator near ROWNUM accordingly.

::

   UPDATE SCMS_MSGPOOL_LST
                SET MSG_STD = '11'
              WHERE UNISEQNO = IN_OUNISEQNO
                AND MSG_TYP1 IN ('MT103', 'MT199')
                AND ROWNUM = 1;

**Output**

::

    UPDATE SCMS_MSGPOOL_LST
      SET MSG_STD = '11'
    WHERE (xc_node_id,ctid) in (select xc_node_id, ctid
           from SCMS_MSGPOOL_LST
           where UNISEQNO = IN_OUNISEQNO
           AND MSG_TYP1 IN ('MT103', 'MT199')
           LIMIT 1)

**Input-ROWNUM with DELETE**

When executing DELETED, if ROWNUM with some value (integer) is used, system will DELETE records using the operator near ROWNUM accordingly.

::

   delete from test1
   where c1='abc' and rownum = 1;

**Output**

::

   delete from test1 where (xc_node_id,ctid) in (select xc_node_id, ctid from test1 where c1='abc' limit 1);

**Input - UPDATE with ROWNUM**

The UPDATE and DELETE scripts migrated using **ROWNUM** contain **LIMIT**, which is not supported by GaussDB(DWS).

::

   UPDATE SCMS_MSGPOOL_LST
    SET MSG_STD = '11'
    WHERE UNISEQNO = IN_OUNISEQNO
    AND MSG_TYP1 IN ('MT103', 'MT199')
    AND ROWNUM = 1;

**Output**

::

   UPDATE SCMS_MSGPOOL_LST
      SET MSG_STD = '11'
    WHERE (xc_node_id, ctid) = ( SELECT xc_node_id, ctid
              FROM SCMS_MSGPOOL_LST
             WHERE UNISEQNO = IN_OUNISEQNO
            AND MSG_TYP1 IN ('MT103', 'MT199')
             LIMIT 1
          );

**Input - DELETE with ROWNUM**

.. code-block:: text

   DELETE FROM SPMS_APP_PUBLISH
    WHERE NOVA_NO = IN_NOVA_NO
      AND DELIVERY_TYPE = '1'
      AND PUBLISH_DATE = IN_PUBLISH_DATE
      AND ROWNUM = 1;

**Output**

.. code-block:: text

   DELETE FROM SPMS_APP_PUBLISH
    WHERE (xc_node_id, ctid) IN (SELECT xc_node_id, ctid
              FROM SPMS_APP_PUBLISH
             WHERE NOVA_NO = IN_NOVA_NO
            AND DELIVERY_TYPE = '1'
            AND PUBLISH_DATE = IN_PUBLISH_DATE
             LIMIT 1
           );
