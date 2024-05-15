:original_name: dws_06_0185.html

.. _dws_06_0185:

CREATE TYPE
===========

Function
--------

**CREATE TYPE** defines a new data type in the current database. The user who defines a new data type becomes its owner. Types are designed only for row-store tables.

Four types of data can be created by using **CREATE TYPE**: composite data, base data, a shell data, and enumerated data.

-  Composite types

   A composite type is specified by a list of attribute names and data types. If the data type of an attribute is collatable, the attribute's collation rule can also be specified. A composite type is essentially the same as the row type of a table. However, using **CREATE TYPE** avoids the need to create an actual table when only a type needs to be defined. In addition, a standalone composite type is useful, for example, as the parameter or return type of a function.

   To create a composite type, you must have the **USAGE** permission for all its attribute types.

-  Base types

   You can customize a new base type (scalar type). Generally, functions required for base types must be coded in C or another low-level language.

-  Shell types

   This parameter allows you to create a shell type, which is a type name that has no definition yet. You can use CREATE TYPE with only the type name to make a shell type. Shell types are needed as forward references when base types are created.

-  Enumerated types

   An enumerated type is a list of enumerated values. Each value is a non-empty string with the maximum length of 64 bytes.

Precautions
-----------

If a schema name is given, the type will be created in the specified schema. Otherwise, it will be created in the current schema. The type name must be distinct from the name of any existing type or domain in the same schema. (Because tables have associated data types, the type name must also be distinct from the name of any existing table in the same schema.)

Syntax
------

::

   CREATE TYPE name AS
       ( [ attribute_name data_type [ COLLATE collation ] [, ... ] ] )

   CREATE TYPE name (
       INPUT = input_function,
       OUTPUT = output_function
       [ , RECEIVE = receive_function ]
       [ , SEND = send_function ]
       [ , TYPMOD_IN = type_modifier_input_function ]
       [ , TYPMOD_OUT = type_modifier_output_function ]
       [ , ANALYZE = analyze_function ]
       [ , INTERNALLENGTH = { internallength | VARIABLE } ]
       [ , PASSEDBYVALUE ]
       [ , ALIGNMENT = alignment ]
       [ , STORAGE = storage ]
       [ , LIKE = like_type ]
       [ , CATEGORY = category ]
       [ , PREFERRED = preferred ]
       [ , DEFAULT = default ]
       [ , ELEMENT = element ]
       [ , DELIMITER = delimiter ]
       [ , COLLATABLE = collatable ]
   )

   CREATE TYPE name

   CREATE TYPE name AS ENUM
       ( [ 'label' [, ... ] ] )

Parameter Description
---------------------

Composite types

-  **name**

   Specifies the name of the type to be created. It can be schema-qualified.

-  **attribute_name**

   Specifies the name of an attribute (column) for the composite type.

-  **data_type**

   Specifies the name of an existing data type to become a column of the composite type.

-  **collation**

   Specifies the name of an existing collation rule to be associated with a column of the composite type.

Base types

When creating a base type, you can place parameters in any order. The **input_function** and **output_function** parameters are mandatory, and other parameters are optional.

-  **input_function**

   Specifies the name of a function that converts data from the external text format of a type to its internal format.

   An input function can be declared as taking one parameter of the cstring type or taking three parameters of the cstring, oid, and integer types.

   -  The cstring-type parameter is the input text as a C string.
   -  The oid-type parameter is the OID of the type (except for array types, where the parameter is the element type OID of an array type).
   -  The integer-type parameter is typmod of the destination column, if known (**-1** will be passed if not known).

   An input function must return a value of the data type itself. Generally, an input function must be declared as **STRICT**. If it is not, it will be called with a **NULL** parameter coming first when the system reads a **NULL** input value. In this case, the function must still return **NULL** unless an error raises. (This mechanism is designed for supporting domain input functions, which may need to reject **NULL** input values.)

   .. note::

      Input and output functions can be declared to have the results or parameters of a new type because they have to be created before the new type is created. The new type should first be defined as a shell type, which is a placeholder type that has no attributes except a name and an owner. This can be done by delivering the **CREATE TYPE** *name* statement, with no additional parameters. Then, the C I/O functions can be defined as referencing the shell type. Finally, **CREATE TYPE** with a full definition replaces the shell type with a complete, valid type definition. After that, the new type can be used normally.

-  **output_function**

   Specifies the name of a function that converts data from the internal format of a type to its external text format.

   An output function must be declared as taking one parameter of a new data type. It must return data of the cstring type. Output functions are not invoked for **NULL** values.

-  **receive_function**

   (Optional) Specifies the name of a function that converts data from the external binary format of a type to its internal format.

   If this function is not used, the type cannot participate in binary input. It costs lower to convert the binary format to the internal format, more portable. (For example, the standard integer data types use the network byte order as an external binary representation, whereas the internal representation is in the machine's native byte order.) This function should perform adequate checks to ensure a valid value.

   Also, this function can be declared as taking one parameter of the internal type or taking three parameters of the internal, oid, and integer types.

   -  The internal-type parameter is a pointer to a StringInfo buffer holding received byte strings.
   -  The oid- and integer-type parameters are the same as those of the text input function.

   A receive function must return a value of the data type itself. Generally, a receive function must be declared as **STRICT**. If it is not, it will be called with a **NULL** parameter coming first when the system reads a **NULL** input value. In this case, the function must still return **NULL** unless an error raises. (This mechanism is designed for supporting domain receive functions, which may need to reject **NULL** input values.)

-  **send_function**

   (Optional) Specifies the name of a function that converts data from the internal format of a type to its external binary format.

   If this function is not used, the type cannot participate in binary output. A send function must be declared as taking one parameter of a new data type. It must return data of the bytea type. Send functions are not invoked for **NULL** values.

-  **type_modifier_input_function**

   (Optional) Specifies the name of a function that converts an array of modifiers for a type to its internal format.

-  **type_modifier_output_function**

   (Optional) Specifies the name of a function that converts the internal format of modifiers for a type to its external text format.

   .. note::

      **type_modifier_input_function** and **type_modifier_output_function** are needed if a type supports modifiers, that is, optional constraints attached to a type declaration, such as char(5) or numeric(30,2). GaussDB(DWS) allows user-defined types to take one or more simple constants or identifiers as modifiers. However, this information must be capable of being packed into a single non-negative integer value for storage in system catalogs. Declared modifiers are passed to **type_modifier_input_function** in the cstring array format. The parameter must check values for validity, throwing an error if they are wrong. If they are correct, the parameter will return a single non-negative integer value, which will be stored as typmod in a column. If the type does not have **type_modifier_input_function**, type modifiers will be rejected. **type_modifier_output_function** converts the internal integer typmod value back to a correct format for user display. It must return a cstring value, which is the exact string appending to the type name. For example, a numeric function may return (30,2). If the default display format is enclosing a stored typmod integer value in parentheses, you can omit **type_modifier_output_function**.

-  **analyze_function**

   (Optional) Specifies the name of a function that performs statistical analysis for a data type.

   By default, if there is a default B-tree operator class for a type, **ANALYZE** will attempt to gather statistics by using the "equals" and "less-than" operators of the type. This behavior is inappropriate for non-scalar types, and can be overridden by specifying a custom analysis function. The analysis function must be declared to take one parameter of the internal type and return a boolean result.

-  **internallength**

   (Optional) Specifies a numeric constant for specifying the length in bytes of the internal representation of a new type. By default, it is variable-length.

   Although the details of the new type's internal representation are only known to I/O functions and other functions that you create to work with the type, there are still some attributes of the internal representation that must be declared to GaussDB(DWS). The most important one is **internallength**. Base data types can be fixed-length (when **internallength** is a positive integer) or variable-length (when **internallength** is set to **VARIABLE**; internally, this is represented by setting **typlen** to **-1**). The internal representation of all variable-length types must start with a 4-byte integer. **internallength** defines the total length.

-  **PASSEDBYVALUE**

   (Optional) Specifies that values of a data type are passed by value, rather than by reference. Types passed by value must be fixed-length, and their internal representation cannot be larger than the size of the Datum type (4 bytes on some machines, and 8 bytes on others).

-  **alignment**

   (Optional) Specifies the storage alignment required for a data type. It supports values **char**, **int2**, **int4**, and **double**. The default value is **int4**.

   The allowed values equate to alignment on 1-, 2-, 4-, or 8-byte boundaries. Note that variable-length types must have an alignment of at least 4 since they must contain an int4 value as their first component.

-  **storage**

   (Optional) Specifies the storage strategy for a data type.

   It supports values **plain**, **external**, **extended**, and **main**. The default value is **plain**.

   -  **plain** specifies that data of a type will always be stored in-line and not compressed. (Only **plain** is allowed for fixed-length types.)

   -  **extended** specifies that the system will first try to compress a long data value and will then move the value out of the main table row if it is still too long.

   -  **external** allows a value to be moved out of the main table, but the system will not try to compress it.

   -  **main** allows for compression, but discourages moving a value out of the main table. (Data items with this storage strategy might still be moved out of the main table if there is no other way to make a row fit. However, they will be kept in the main table preferentially over **extended** and **external** items.)

      All **storage** values except **plain** imply that the functions of the data type can handle values that have been toasted. A given value merely determines the default **TOAST** storage strategy for columns of a toastable data type. Users can choose other strategies for individual columns by using **ALTER TABLE SET STORAGE**.

-  **like_type**

   (Optional) Specifies the name of an existing data type that has the same representation as a new type. The values of **internallength**, **passedbyvalue**, **alignment**, and **storage** are copied from this type, unless they are overridden by explicit specifications elsewhere in the **CREATE TYPE** command.

   Specifying representation in this way is especially useful when the low-level implementation of a new type references an existing type.

-  **category**

   (Optional) Specifies the category code (a single ASCII character) for a type. The default value is **U** for a user-defined type. You can also choose other ASCII characters to create custom categories.

-  **preferred**

   (Optional) Specifies whether a type is preferred within its type category. If it is, the value will be **TRUE**, else **FALSE**. The default value is **FALSE**. Be cautious when creating a new preferred type within an existing type category because this could cause great changes in behavior.

   .. note::

      The **category** and **preferred** parameters can be used to help determine which implicit cast excels in ambiguous situations. Each data type belongs to a category named by a single ASCII character, and each type is either preferred or not within its category. If this rule is helpful in resolving overloaded functions or operators, the parser will prefer casting to preferred types (but only from other types within the same category). For types that have no implicit casts to or from any other types, it is sufficient to leave these parameters at their default values. However, for a group of types that have implicit casts, mark them all as belonging to a category and select one or two of the most general types as being preferred within the category. The **category** parameter is helpful in adding a user-defined type to an existing built-in category, such as the numeric or string type. However, you can also create new entirely-user-defined type categories. Select any ASCII character other than an uppercase letter to name such a category.

-  **default**

   (Optional) Specifies the default value for a data type. If this parameter is omitted, the default value will be **NULL**.

   A default value can be specified if you expect the columns of a data type to default to something other than the **NULL** value. You can also specify a default value using the **DEFAULT** keyword. (Such a default value can be overridden by an explicit **DEFAULT** clause attached to a particular column.)

-  **element**

   (Optional) Specifies the type of an array element when an array type is created. For example, to define an array of 4-byte integers (int4), set **ELEMENT** to **int4**.

-  **delimiter**

   (Optional) Specifies the delimiter character to be used between values in arrays made of a type.

   **delimiter** can be set to a specific character. The default delimiter is a comma (,). Note that a delimiter is associated with the array element type, instead of the array type itself.

-  **collatable**

   (Optional) Specifies whether a type's operations can use collation information. If they can, the value will be **TRUE**, else **FALSE** (default).

   If **collatable** is **TRUE**, column definitions and expressions of a type may carry collation information by using the **COLLATE** clause. It is the implementations of functions operating on the type that actually use the collation information. This use cannot be achieved merely by marking the type collatable.

-  **lable**

   (Optional) Specifies a text label associated with an enumerated value. It is a non-empty string of up to 64 characters.

.. note::

   Whenever a user-defined type is created, GaussDB(DWS) automatically creates an associated array type whose name consists of the element type name prepended with an underscore (_).

Example
-------

Example 1: Create a composite type, create a table, insert data, and make a query.

::

   CREATE TYPE compfoo AS (f1 int, f2 text);
   CREATE TABLE t1_compfoo(a int, b compfoo);
   CREATE TABLE t2_compfoo(a int, b compfoo);
   INSERT INTO t1_compfoo values(1,(1,'demo'));
   INSERT INTO t2_compfoo select * from t1_compfoo;
   SELECT (b).f1 FROM t1_compfoo;
   SELECT * FROM t1_compfoo t1 join t2_compfoo t2 on (t1.b).f1=(t1.b).f1;

Example 2: Create an enumeration type and use it in the table definition.

::

   CREATE TYPE bugstatus AS ENUM ('create', 'modify', 'closed');
   CREATE TABLE customer (name text,current_bugstatus bugstatus);
   INSERT INTO customer VALUES ('type','create');
   SELECT * FROM customer WHERE current_bugstatus = 'create';

Example 3: Compile a .so file and create the shell type.

::

   CREATE TYPE complex;

This statement creates a placeholder for the type to be created, which can then be referenced when defining its I/O functions. Now you can define an I/O function. Note that the function must be declared in NOT FENCED mode when it is created.

::

   CREATE FUNCTION
   complex_in(cstring)
       RETURNS complex
       AS 'filename'
       LANGUAGE C IMMUTABLE STRICT not fenced;

   CREATE FUNCTION
   complex_out(complex)
       RETURNS cstring
       AS 'filename'
       LANGUAGE C IMMUTABLE STRICT not fenced;

   CREATE FUNCTION
   complex_recv(internal)
       RETURNS complex
       AS 'filename'
       LANGUAGE C IMMUTABLE STRICT not fenced;

   CREATE FUNCTION
   complex_send(complex)
       RETURNS bytea
       AS 'filename'
       LANGUAGE C IMMUTABLE STRICT not fenced;

-- Finally, provide a complete definition of the data type:

::

   CREATE TYPE complex (
   internallength = 16,
   input = complex_in,
   output = complex_out,
   receive = complex_recv,
   send = complex_send,
   alignment = double
   );

The C functions corresponding to the input, output, receive, and send functions are defined as follows:

::

   -- Define a structure body Complex:
   typedef struct Complex {
       double      x;
       double      y;
   } Complex;

   -- Define an input function:
   PG_FUNCTION_INFO_V1(complex_in);

   Datum
   complex_in(PG_FUNCTION_ARGS)
   {
       char       *str = PG_GETARG_CSTRING(0);
       double      x,
                   y;
       Complex    *result;

       if (sscanf(str, " ( %lf , %lf )", &x, &y) != 2)
           ereport(ERROR,
                   (errcode(ERRCODE_INVALID_TEXT_REPRESENTATION),
                    errmsg("invalid input syntax for complex: \"%s\"",
                           str)));

       result = (Complex *) palloc(sizeof(Complex));
       result->x = x;
       result->y = y;
       PG_RETURN_POINTER(result);
   }

   -- Define an output function:
   PG_FUNCTION_INFO_V1(complex_out);

   Datum
   complex_out(PG_FUNCTION_ARGS)
   {
           Complex    *complex = (Complex *) PG_GETARG_POINTER(0);
           char       *result;

           result = (char *) palloc(100);
           snprintf(result, 100, "(%g,%g)", complex->x, complex->y);
           PG_RETURN_CSTRING(result);
   }

   -- Define a receive function:
   PG_FUNCTION_INFO_V1(complex_recv);

   Datum
   complex_recv(PG_FUNCTION_ARGS)
   {
       StringInfo  buf = (StringInfo) PG_GETARG_POINTER(0);
       Complex    *result;

       result = (Complex *) palloc(sizeof(Complex));
       result->x = pq_getmsgfloat8(buf);
       result->y = pq_getmsgfloat8(buf);
       PG_RETURN_POINTER(result);
   }

   -- Define a send function:
   PG_FUNCTION_INFO_V1(complex_send);

   Datum
   complex_send(PG_FUNCTION_ARGS)
   {
       Complex    *complex = (Complex *) PG_GETARG_POINTER(0);
       StringInfoData buf;

       pq_begintypsend(&buf);
       pq_sendfloat8(&buf, complex->x);
       pq_sendfloat8(&buf, complex->y);
       PG_RETURN_BYTEA_P(pq_endtypsend(&buf));
   }

Helpful Links
-------------

:ref:`ALTER TYPE <dws_06_0148>`, :ref:`DROP TYPE <dws_06_0213>`
