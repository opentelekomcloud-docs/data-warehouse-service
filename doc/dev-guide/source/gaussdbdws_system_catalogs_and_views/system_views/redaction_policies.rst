:original_name: dws_04_0858.html

.. _dws_04_0858:

REDACTION_POLICIES
==================

**REDACTION_POLICIES** displays information about all redaction objects in the current database.

.. table:: **Table 1** **REDACTION_POLICIES** columns

   +--------------------+---------+----------------------------------------------------------------------+
   | Name               | Type    | Description                                                          |
   +====================+=========+======================================================================+
   | object_owner       | Name    | Owner of the object to be redacted.                                  |
   +--------------------+---------+----------------------------------------------------------------------+
   | object_name        | Name    | Redacted object name.                                                |
   +--------------------+---------+----------------------------------------------------------------------+
   | policy_name        | Name    | Name of the redaction policy.                                        |
   +--------------------+---------+----------------------------------------------------------------------+
   | expression         | Text    | Policy effective expression (for users).                             |
   +--------------------+---------+----------------------------------------------------------------------+
   | enable             | Boolean | Policy status (enabled or disabled).                                 |
   +--------------------+---------+----------------------------------------------------------------------+
   | policy_description | Text    | Policy description.                                                  |
   +--------------------+---------+----------------------------------------------------------------------+
   | inherited          | Bool    | Whether a redacted column is inherited from another redacted column. |
   +--------------------+---------+----------------------------------------------------------------------+
