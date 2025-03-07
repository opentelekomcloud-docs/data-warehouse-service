:original_name: dws_04_0611.html

.. _dws_04_0611:

PG_REDACTION_POLICY
===================

**PG_REDACTION_POLICY** records information about the object to be redacted.

.. table:: **Table 1** PG_REDACTION_POLICY columns

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                                               |
   +=======================+=======================+===========================================================================================+
   | object_oid            | oid                   | OID of the object to be redacted.                                                         |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | policy_name           | name                  | Name of the redaction policy.                                                             |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | enable                | boolean               | Policy status (enabled or disabled).                                                      |
   |                       |                       |                                                                                           |
   |                       |                       | .. note::                                                                                 |
   |                       |                       |                                                                                           |
   |                       |                       |    The value can be:                                                                      |
   |                       |                       |                                                                                           |
   |                       |                       |    -  **true**: enabled                                                                   |
   |                       |                       |    -  **false**: disabled                                                                 |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | expression            | pg_node_tree          | Policy effective expression (for users).                                                  |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | policy_description    | text                  | Description of a policy.                                                                  |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | inherited             | bool                  | Whether a redaction policy is inherited from another redaction policy.                    |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
   | policy_order          | float4                | Masking policy sequence. This field is supported by 8.2.1.100 and later cluster versions. |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------+
