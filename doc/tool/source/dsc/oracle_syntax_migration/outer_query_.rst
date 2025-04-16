:original_name: dws_mt_0215.html

.. _dws_mt_0215:

OUTER QUERY (+)
===============

Join is supported by GaussDB(DWS), so parameter **supportJoinOperator** is added.

**OUTER QUERY (+)** can be migrated when **supportJoinOperator** is set to **false**.

**Input-OUTER QUERY(+)**

::

   SELECT PP.PUBLISH_NO
                        FROM SPMS_PARAM_PUBLISH PP
                          WHERE PP.PUBLISH_ID(+) = TB2.PUBLISH_ID;

                  SELECT  I.APP_CHNAME, I.APP_SHORTNAME
                        FROM SPMS_APPVERSION SA, SPMS_APP_INFO I
                         WHERE SA.APP_ID = I.APP_ID(+)
                          AND SA.DELIVERY_USER = IN_USERID
                          ORDER BY  APPVER_ID DESC ;

**Output**

::

   SELECT
             PP.PUBLISH_NO
        FROM
             SPMS_PARAM_PUBLISH PP
        WHERE
             PP.PUBLISH_ID (+) = TB2.PUBLISH_ID
   ;

   SELECT
             I.APP_CHNAME
             ,I.APP_SHORTNAME
        FROM
             SPMS_APPVERSION SA
             ,SPMS_APP_INFO I
        WHERE
             SA.APP_ID = I.APP_ID (+)
             AND SA.DELIVERY_USER = IN_USERID
        ORDER BY
             APPVER_ID DESC
   ;
