:original_name: dws_06_0369.html

.. _dws_06_0369:

Enumeration Type
================

An enumeration (enum) type is a data type consisting of a static, ordered set of values. They are equivalent to the enum types used in many programming languages. The enumeration type can be a date in a week or a set of status values.

Declaration of Enumeration Types
--------------------------------

Enumeration types can be created using the **CREATE TYPE** command. For example:

::

   CREATE TYPE mood AS ENUM ('sad', 'ok', 'happy');

After an enumeration type is created, it can be used in table and function definitions.

::

   CREATE TYPE mood AS ENUM ('sad', 'ok', 'happy');
   CREATE TABLE person (name text, current_mood mood);
   INSERT INTO person VALUES ('Moe', 'happy');
   SELECT * FROM person WHERE current_mood = 'happy';
    name | current_mood
   ------+--------------
    Moe  | happy
   (1 row)

Sorting
-------

The order of enumeration values is the order of the values listed when the type is created.

::

   INSERT INTO person VALUES ('Larry', 'sad');
   INSERT INTO person VALUES ('Curly', 'ok');

   SELECT * FROM person WHERE current_mood > 'sad';
    name  | current_mood
   -------+--------------
    Moe   | happy
    Curly | ok
   (2 rows)

   SELECT * FROM person WHERE current_mood > 'sad' ORDER BY current_mood;
    name  | current_mood
   -------+--------------
    Curly | ok
    Moe   | happy
   (2 rows)

   SELECT name FROM person WHERE current_mood = (SELECT MIN(current_mood) FROM person);
    name
   -------
    Larry
   (1 row)

Security of Enumeration Types
-----------------------------

Each enumeration data type is independent and cannot be compared with other enumeration types.

::

   CREATE TYPE happiness AS ENUM ('happy', 'very happy', 'ecstatic');
   CREATE TABLE holidays (num_weeks integer, happiness happiness);
   INSERT INTO holidays(num_weeks,happiness) VALUES (4, 'happy');
   INSERT INTO holidays(num_weeks,happiness) VALUES (6, 'very happy');
   INSERT INTO holidays(num_weeks,happiness) VALUES (8, 'ecstatic');

   INSERT INTO holidays(num_weeks,happiness) VALUES (2, 'sad');
   ERROR:  invalid input value for enum happiness: "sad"

   SELECT person.name, holidays.num_weeks FROM person, holidays
          WHERE person.current_mood = holidays.happiness;
   ERROR:  operator does not exist: mood = happiness

If comparison is required, you can use a customized operator or add an explicit type to the query.

::

    SELECT person.name, holidays.num_weeks FROM person, holidays
          WHERE person.current_mood::text = holidays.happiness::text;
    name | num_weeks
   ------+-----------
    Moe  |         4
   (1 row)

Precautions
-----------

-  Enumeration tags are case sensitive. 'happy' is different from 'HAPPY'. Spaces in tags also have meanings.
-  Although enumeration types are primarily intended for static collections of values, there are ways to add new and renamed values to existing enumeration types (see :ref:`ALTER TYPE <dws_06_0148>`). Existing values cannot be removed from an enumeration type, and the order of these values cannot be changed unless the enumeration type is deleted and recreated.
-  The mapping between enumerated values and text tags is stored in the **PG_ENUM** system catalog.
