:original_name: dws_04_0858.html

.. _dws_04_0858:

REDACTION_POLICIES
==================

**REDACTION_POLICIES** displays information about all redaction objects in the current database.

.. table:: **Table 1** **REDACTION_POLICIES** columns

   +--------------------+---------+----------------------------------------------------------------------+
   | Name               | Type    | Description                                                          |
   +====================+=========+======================================================================+
   | object_owner       | name    | Owner of the object to be redacted.                                  |
   +--------------------+---------+----------------------------------------------------------------------+
   | object_name        | name    | Redacted object name                                                 |
   +--------------------+---------+----------------------------------------------------------------------+
   | policy_name        | name    | Name of the redact policy                                            |
   +--------------------+---------+----------------------------------------------------------------------+
   | expression         | text    | Policy effective expression (for users)                              |
   +--------------------+---------+----------------------------------------------------------------------+
   | enable             | boolean | Policy status (enabled or disabled)                                  |
   +--------------------+---------+----------------------------------------------------------------------+
   | policy_description | text    | Description of a policy                                              |
   +--------------------+---------+----------------------------------------------------------------------+
   | inherited          | bool    | Whether a redacted column is inherited from another redacted column. |
   +--------------------+---------+----------------------------------------------------------------------+
