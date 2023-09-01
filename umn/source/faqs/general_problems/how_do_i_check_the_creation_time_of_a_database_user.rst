:original_name: dws_03_0063.html

.. _dws_03_0063:

How Do I Check the Creation Time of a Database User?
====================================================

**Method 1**:

When you create a GaussDB(DWS) database user, if the time when the user takes effect (**VALID BEGIN**) is the same as the creation time of the user, and the time when the user takes effect has not been changed, you can check the **valbegin** column in the **PG_USER** view to check the user creation time.

The following is an example:

Create user **jerry** and set its validity start time to its current creation time.

::

   CREATE USER jerry PASSWORD 'password' VALID BEGIN '2022-05-19 10:31:56';

View users in the **PG_USER** view. The **valbegin** column indicates the time when **jerry** took effect, that is, the time when jerry was created.

::

   SELECT * FROM PG_USER;
    usename | usesysid | usecreatedb | usesuper | usecatupd | userepl |  passwd  |        valbegin        | valuntil |   respool    | parent | spacelimit | useconfig | nodegroup | tempspacelimit |
    spillspacelimit
   ---------+----------+-------------+----------+-----------+---------+----------+------------------------+----------+--------------+--------+------------+-----------+-----------+----------------+
   -----------------
    Ruby    |       10 | t           | t        | t         | t       | ******** |                        |          | default_pool |      0 |            |           |           |                |

    dbadmin |    16393 | f           | f        | f         | f       | ******** |                        |          | default_pool |      0 |            |           |           |                |

    jack    |   451897 | f           | f        | f         | f       | ******** |                        |          | default_pool |      0 |            |           |           |                |

    emma    |   451910 | f           | f        | f         | f       | ******** |                        |          | default_pool |      0 |            |           |           |                |

    jerry   |   457386 | f           | f        | f         | f       | ******** | 2022-05-19 10:31:56+08 |          | default_pool |      0 |            |           |           |                |
   (5 rows)

**Method 2:**

Check the **passwordtime** column in the **PG_AUTH_HISTORY** system catalog. This column indicates the time when the user's initial password was created. Only users with system administrator permissions can access the catalog.

::

   select roloid, min(passwordtime) as create_time from pg_auth_history group by roloid order by roloid;

The following is an example:

Query the **PG_USER** view to obtain the OID of user **jerry**, which is **457386**. Query the **passwordtime** column to obtain the creation time of user **jerry**, which is **2022-05-19 10:31:56**.

::

   select roloid, min(passwordtime) as create_time from pg_auth_history group by roloid order by roloid;
    roloid |          create_time
   --------+-------------------------------
        10 | 2022-02-25 09:53:38.711785+08
     16393 | 2022-02-25 09:55:17.992932+08
    451897 | 2022-05-18 09:42:26.897855+08
    451910 | 2022-05-18 09:46:33.152354+08
    457386 | 2022-05-19 10:31:56.037706+08
   (5 rows)
