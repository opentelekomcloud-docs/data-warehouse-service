:original_name: dws_07_6818.html

.. _dws_07_6818:

LONG RAW
========

"Data type LONG RAW" is not supported in the CREATE TABLE statement. LONG RAW data type needs to be replaced with Byte.

**Input**

::

   CREATE TABLE SAD.WORKFLOWDEFS
      ( ID NUMBER(*,0),
    WF_NAME VARCHAR2(200),
    WF_DEFINITION LONG RAW,
    WF_VERSION NUMBER(*,0),
    WF_PUBLISH CHAR(1),
    WF_MAINFLOW CHAR(1),
    WF_APP_NAME VARCHAR2(20),
    CREATED_BY NUMBER,
    CREATION_DATE DATE,
    LAST_UPDATED_BY NUMBER,
    LAST_UPDATE_DATE DATE,
    WFDESC VARCHAR2(2000)
      );

**Output**

::

   CREATE TABLE sad.workflowdefs
     (
        id                    NUMBER (38, 0),
        wf_name          VARCHAR2 (200),
        wf_definition     BYTEA,
        wf_version       NUMBER (38, 0),
        wf_publish       CHAR(1),
        wf_mainflow     CHAR(1),
        wf_app_name   VARCHAR2 (20),
        created_by       NUMBER,
        creation_date    DATE,
        last_updated_by  NUMBER,
        last_update_date DATE,
        wfdesc               VARCHAR2 (2000)
     );
