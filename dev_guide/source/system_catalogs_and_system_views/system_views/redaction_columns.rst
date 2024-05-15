:original_name: dws_04_0857.html

.. _dws_04_0857:

REDACTION_COLUMNS
=================

**REDACTION_COLUMNS** displays information about all redaction columns in the current database.

.. table:: **Table 1** **REDACTION_COLUMNS** columns

   +------------------------+---------+--------------------------------------------------------------------------------------+
   | Name                   | Type    | Description                                                                          |
   +========================+=========+======================================================================================+
   | object_owner           | name    | Owner of the object to be redacted.                                                  |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | object_name            | name    | Redacted object name                                                                 |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | column_name            | name    | Redacted column name                                                                 |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | function_type          | integer | Redaction type                                                                       |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | function_parameters    | text    | Parameter used when the redaction type is **partial** (reserved)                     |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | regexp_pattern         | text    | Pattern string when the redaction type is **regexp** (reserved)                      |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | regexp_replace_string  | text    | Replacement string when the redaction type is **regexp** (reserved)                  |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | regexp_position        | integer | Start and end replacement positions when the redaction type is **regexp** (reserved) |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | regexp_occurrence      | integer | Replacement times when the redaction type is **regexp** (reserved)                   |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | regexp_match_parameter | text    | Regular control parameter used when the redaction type is **regexp** (reserved)      |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | function_info          | text    | Redaction function information                                                       |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | column_description     | text    | Description of the redacted column                                                   |
   +------------------------+---------+--------------------------------------------------------------------------------------+
   | inherited              | bool    | Whether a redacted column is inherited from another redacted column.                 |
   +------------------------+---------+--------------------------------------------------------------------------------------+
