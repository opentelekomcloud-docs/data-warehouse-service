:original_name: dws_06_0141.html

.. _dws_06_0141:

ALTER SYSTEM KILL SESSION
=========================

Function
--------

**ALTER SYSTEM KILL SESSION** ends a session.

Precautions
-----------

None

Syntax
------

::

   ALTER SYSTEM KILL SESSION 'session_sid, serial' [ IMMEDIATE ];

Parameter Description
---------------------

-  **session_sid, serial**

   Specifies **SID** and **SERIAL** of a session (see examples for format).

   Value range: The **SID**\ s and **SERIAL**\ s of all sessions that can be queried from the system catalog **V$SESSION**.

-  **IMMEDIATE**

   Indicates that a session will be ended instantly after the command is executed.

Examples
--------

Query session information.

::

   SELECT sid,serial#,username FROM V$SESSION;

          sid       | serial# | username
   -----------------+---------+----------
    140131075880720 |       0 |
    140131025549072 |       0 |
    140131073779472 |       0 |
    140131071678224 |       0 |
    140131125774096 |       0 |
    140131127875344 |       0 |
    140131113629456 |       0 |
    140131094742800 |       0 |
   (8 rows)

End the session whose SID is 140131075880720.

::

   ALTER SYSTEM KILL SESSION '140131075880720,0' IMMEDIATE;
