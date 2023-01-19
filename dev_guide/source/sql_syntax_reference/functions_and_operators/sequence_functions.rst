:original_name: dws_06_0043.html

.. _dws_06_0043:

SEQUENCE Functions
==================

The sequence functions provide a simple method to ensure security of multiple users for users to obtain sequence values from sequence objects.

.. note::

   -  The hybrid data warehouse (standalone) does not support **SEQUENCE** and related functions.

-  nextval(regclass)

   Specifies an increasing sequence and returns a new value.

   .. note::

      -  To avoid blocking of concurrent transactions that obtain numbers from the same sequence, a nextval operation is never rolled back; that is, once a value has been fetched it is considered used, even if the transaction that did the nextval later aborts. This means that aborted transactions may leave unused "holes" in the sequence of assigned values. Therefore, sequences in GaussDB(DWS) cannot be used to obtain sequence without gaps.
      -  If the nextval function is pushed to DNs, each DN will automatically connect to the GTM and requests the next value. For example, in the **insert into t1 select xxx** statement, a column in table **t1** needs to invoke the nextval function. If maximum number of connections on the GTM is 8192, this type of pushed statements occupies too many GTM connections. Therefore, the number of concurrent connections for these statements is limited to 7000 divided by the number of cluster DNs. The other 1192 connections are reserved for other statements.

   Return type: bigint

   The **nextval** function can be invoked in either of the following ways: (In example 2, the Oracle syntax is supported. Currently, the sequence name cannot contain a dot.)

   Example 1:

   ::

      select nextval('seqDemo');
       nextval
      ---------
             2
      (1 row)

   Example 2:

   ::

      select seqDemo.nextval;
       nextval
      ---------
             2
      (1 row)

-  currval(regclass)

   Returns the last value of **nextval** for a specified sequence in the current session. If **nextval** has not been invoked for the specified sequence in the current session, an error is reported when **currval** is invoked. By default, **currval** is disabled. To enable it, set **enable_beta_features** to **true**. After **currval** is enabled, **nextval** will not be pushed down.

   Return type: bigint

   The **currval** function can be invoked in either of the following ways: (In example 2, the Oracle syntax is supported. Currently, the sequence name cannot contain a dot.)

   Example 1:

   ::

      select currval('seq1');
       currval
      ---------
             2
      (1 row)

   Example 2:

   ::

      select seq1.currval seq1;
       currval
      ---------
             2
      (1 row)

-  lastval()

   Returns the last value of **nextval** in the current session. This function is equivalent to **currval**, but **lastval** does not have a parameter. If **nextval** has not been invoked in the current session, an error is reported when **lastval** is invoked.

   By default, **lastval** is disabled. To enable it, set **enable_beta_features** or **lastval_supported** to **true**. After **lastval** is enabled, **nextval** will not be pushed down.

   Return type: bigint

   For example:

   ::

      select lastval();
       lastval
      ---------
             2
      (1 row)

-  setval(regclass, bigint)

   Sets the current value of a sequence.

   Return type: bigint

   For example:

   ::

      select setval('seqDemo',1);
       setval
      --------
            1
      (1 row)

-  setval(regclass, bigint, boolean)

   Sets the current value of a sequence and the is_called sign.

   Return type: bigint

   For example:

   ::

      select setval('seqDemo',1,true);
       setval
      --------
            1
      (1 row)

   .. note::

      The current session and GTM will take effect immediately after **setval** is performed. If other sessions have buffered sequence values, **setval** will take effect only after the values are used up. Therefore, to prevent sequence value conflicts, you are advised to use **setval** with caution.

      Because the sequence is non-transactional, changes made by **setval** will not be canceled when a transaction rolled back.
