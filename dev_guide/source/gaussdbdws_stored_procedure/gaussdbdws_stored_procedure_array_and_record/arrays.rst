:original_name: dws_04_0517.html

.. _dws_04_0517:

Arrays
======

Use of Array Types
------------------

Before the use of arrays, an array type needs to be defined:

Define an array type immediately after the **AS** keyword in a stored procedure. Run the following statement:

.. code-block::

   TYPE array_type IS VARRAY(size) OF data_type [NOT NULL];

Its parameters are as follows:

-  **array_type**: indicates the name of the array type to be defined.
-  **VARRAY**: indicates the array type to be defined.
-  **size**: indicates the maximum number of members in the array type to be defined. The value is a positive integer.
-  **data_type**: indicates the types of members in the array type to be created.
-  **NOT NULL**: an optional constraint. It can be used to ensure that none of the elements in the array is **NULL**.

.. note::

   -  In GaussDB(DWS), an array automatically increases. If an access violation occurs, a null value will be returned, and no error message will be reported. If out-of-bounds write occurs in an array, the message **Subscript outside of limit** is displayed.
   -  The scope of an array type defined in a stored procedure takes effect only in this storage process.
   -  It is recommended that you use one of the preceding methods to define an array type. If both methods are used to define the same array type, GaussDB(DWS) prefers the array type defined in a stored procedure to declare array variables.

In GaussDB(DWS) 8.1.0 and earlier versions, the system does not verify the length of array elements and out-of-bounds write because the array can automatically increase. This version adds related constraints to be compatible with Oracle databases. If out-of-bounds write exists, you can configure **varray_verification** in the parameter :ref:`behavior_compat_options <en-us_topic_0000001510522261__section1980124735516>` to be compatible with previously unverified operations.

Example:

::

   -- Declare an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE array_proc
   AS
          TYPE ARRAY_INTEGER IS VARRAY(1024) OF INTEGER;--Define the array type.
          TYPE ARRAY_INTEGER_NOT_NULL IS VARRAY(1024) OF INTEGER NOT NULL;-- Defines non-null array types.
          ARRINT ARRAY_INTEGER: = ARRAY_INTEGER();  --Declare the variable of the array type.
   BEGIN
          ARRINT.extend(10);
          FOR I IN 1..10 LOOP
                  ARRINT(I) := I;
          END LOOP;
          DBMS_OUTPUT.PUT_LINE(ARRINT.COUNT);
          DBMS_OUTPUT.PUT_LINE(ARRINT(1));
          DBMS_OUTPUT.PUT_LINE(ARRINT(10));
          DBMS_OUTPUT.PUT_LINE(ARRINT(ARRINT.FIRST));
          DBMS_OUTPUT.PUT_LINE(ARRINT(ARRINT.last));
   END;
   /

   -- Invoke the stored procedure.
   CALL array_proc();
   10
   1
   10
   1
   10
   -- Delete the stored procedure.
   DROP PROCEDURE array_proc;

Declaration and Use of Rowtype Arrays
-------------------------------------

In addition to the declaration and use of common arrays and non-null arrays in the preceding example, the array also supports the declaration and use of rowtype arrays.

Example:

::

   -- Use the COUNT function on an array in a stored procedure.
   CREATE TABLE tbl (a int, b int);
   INSERT INTO tbl VALUES(1, 2),(2, 3),(3, 4);
   CREATE OR REPLACE PROCEDURE array_proc
   AS
       CURSOR all_tbl IS SELECT * FROM tbl ORDER BY a;
       TYPE tbl_array_type IS varray(50) OF tbl%rowtype; -- Defines the array of the rowtype type. tbl indicates any table.
       tbl_array tbl_array_type;
       tbl_item tbl%rowtype;
       inx1 int;
   BEGIN
       tbl_array := tbl_array_type();
       inx1 := 0;
       FOR tbl_item IN all_tbl LOOP
           inx1 := inx1 + 1;
           tbl_array(inx1) := tbl_item;
       END LOOP;
       WHILE inx1 IS NOT NULL LOOP
           DBMS_OUTPUT.PUT_LINE('tbl_array(inx1).a=' || tbl_array(inx1).a || ' tbl_array(inx1).b=' || tbl_array(inx1).b);
           inx1 := tbl_array.PRIOR(inx1);
       END LOOP;
   END;
   /

The execution output is as follows:

::

   call array_proc();
   tbl_array(inx1).a=3 tbl_array(inx1).b=4
   tbl_array(inx1).a=2 tbl_array(inx1).b=3
   tbl_array(inx1).a=1 tbl_array(inx1).b=2

Array Related Functions
-----------------------

GaussDB(DWS) supports Oracle-related array functions. You can use the following functions to obtain array attributes or perform operations on the array content.

COUNT
-----

Returns the number of elements in the current array. Only the initialized elements or the elements extended by the EXTEND function are counted.

Use:

*varray*\ **.COUNT** or *varray*\ **.COUNT()**

Example:

::

   -- Use the COUNT function on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3);
       DBMS_OUTPUT.PUT_LINE('v_varray.count=' || v_varray.count);
       v_varray.extend;
       DBMS_OUTPUT.PUT_LINE('v_varray.count=' || v_varray.count);
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.count=3
   v_varray.count=4

FIRST and LAST
--------------

The FIRST function can return the subscript of the first element. The LAST function can return the subscript of the last element.

Use:

*varray*\ **.FIRST** or *varray*\ **.FIRST()**

*varray*\ **.LAST** or *varray*\ **.LAST()**

Example:

::

   -- Use the FIRST and LAST functions on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3);
       DBMS_OUTPUT.PUT_LINE('v_varray.first=' || v_varray.first);
       DBMS_OUTPUT.PUT_LINE('v_varray.last=' || v_varray.last);
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.first=1
   v_varray.last=3

EXTEND
------

.. note::

   The EXTEND function is used to be compatible with two Oracle database operations. In GaussDB(DWS), an array automatically grows, and the EXTEND function is not necessary. For a newly written stored procedure, you do not need to use the EXTEND function.

The EXTEND function can extend arrays. The EXTEND function can be invoked in either of the following ways:

-  Method 1:

   EXTEND contains an integer input parameter, indicating that the array size is extended by the specified length. After executing the EXTEND function, the values of the COUNT and LAST functions change accordingly.

   Use:

   *varray*.EXTEND(size)

   By default, one bit is added to the end of *varray*\ **.EXTEND**, which is equivalent to *varray*\ **.EXTEND(1)**.

-  Method 2:

   EXTEND contains two integer input parameters. The first parameter indicates the length of the extended size. The second parameter indicates that the value of the extended array element is the same as that of the element with the **index** subscript.

   Use:

   *varray*.EXTEND(size, index)

Example:

::

   -- Use the EXTEND function on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3);
       v_varray.extend(3);
       DBMS_OUTPUT.PUT_LINE('v_varray.count=' || v_varray.count);
       v_varray.extend(2,3);
       DBMS_OUTPUT.PUT_LINE('v_varray.count=' || v_varray.count);
       DBMS_OUTPUT.PUT_LINE('v_varray(7)=' || v_varray(7));
       DBMS_OUTPUT.PUT_LINE('v_varray(8)=' || v_varray(7));
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.count=6
   v_varray.count=8
   v_varray(7)=3
   v_varray(8)=3

NEXT and PRIOR
--------------

The NEXT and PRIOR functions are used for cyclic array traversal. The NEXT function returns the subscript of the next array element based on the input parameter **index**. If the subscript reaches the maximum value, **NULL** is returned. The PRIOR function returns the subscript of the previous array element based on the input parameter **index**. If the minimum value of the array subscript is reached, **NULL** is returned.

Use:

*varray*.NEXT(index)

*varray*.PRIOR(index)

Example:

::

   -- Use the NEXT and PRIOR functions on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
       i int;
   BEGIN
       v_varray := varray_type(1, 2, 3);

       i := v_varray.COUNT;
       WHILE i IS NOT NULL LOOP
           DBMS_OUTPUT.PUT_LINE('test prior v_varray('||i||')=' || v_varray(i));
           i := v_varray.PRIOR(i);
       END LOOP;

       i := 1;
       WHILE i IS NOT NULL LOOP
           DBMS_OUTPUT.PUT_LINE('test next v_varray('||i||')=' || v_varray(i));
           i := v_varray.NEXT(i);
       END LOOP;
   END;
   /

The execution output is as follows:

::

   call test_varray();
   test prior v_varray(3)=3
   test prior v_varray(2)=2
   test prior v_varray(1)=1
   test next v_varray(1)=1
   test next v_varray(2)=2
   test next v_varray(3)=3

EXISTS
------

Determines whether an array subscript exists.

Use:

*varray*.EXISTS(index)

Example:

::

   -- Use the EXISTS function on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3);
       IF v_varray.EXISTS(1) THEN
           DBMS_OUTPUT.PUT_LINE('v_varray.EXISTS(1)');
       END IF;
       IF NOT v_varray.EXISTS(10) THEN
           DBMS_OUTPUT.PUT_LINE('NOT v_varray.EXISTS(10)');
       END IF;
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.EXISTS(1)
   NOT v_varray.EXISTS(10)

TRIM
----

Deletes a specified number of elements from the end of an array.

Use:

*varray*.TRIM(size)

*varray*\ **.TRIM** is equivalent to *varray*\ **.TRIM(1)**, because the default input parameter is **1**.

Example:

::

   -- Use the TRIM function on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3, 4, 5);
       v_varray.trim(3);
       DBMS_OUTPUT.PUT_LINE('v_varray.count' || v_varray.count);
       v_varray.trim;
       DBMS_OUTPUT.PUT_LINE('v_varray.count:' || v_varray.count);
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.count:2
   v_varray.count:1

DELETE
------

Deletes all elements from an array.

Use:

*varray*\ **.DELETE** or *varray*\ **.DELETE()**

Example:

::

   -- Use the DELETE function on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3, 4, 5);
       v_varray.delete;
       DBMS_OUTPUT.PUT_LINE('v_varray.count:' || v_varray.count);
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.count:0

LIMIT
-----

Returns the allowed maximum length of an array.

Use:

*varray*\ **.LIMIT** or *varray*\ **.LIMIT()**

Example:

::

   -- Use the LIMIT function on an array in a stored procedure.
   CREATE OR REPLACE PROCEDURE test_varray
   AS
       TYPE varray_type IS VARRAY(20) OF INT;
       v_varray varray_type;
   BEGIN
       v_varray := varray_type(1, 2, 3, 4, 5);
       DBMS_OUTPUT.PUT_LINE('v_varray.limit:' || v_varray.limit);
   END;
   /

The execution output is as follows:

::

   call test_varray();
   v_varray.limit:20
