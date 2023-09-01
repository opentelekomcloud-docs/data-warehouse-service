:original_name: dws_03_2101.html

.. _dws_03_2101:

How Do I Use a User-Defined Function to Rewrite the CRC32() Function?
=====================================================================

Currently, GaussDB(DWS) does not have a built-in **CRC32()** function. Instead, you can use the user-defined function of GaussDB(DWS) to rewrite the **CRC32()** function.

-  CRC32(expr)
-  Description: Calculates the cyclic redundancy. The input parameter **expr** is a string. If the parameter is NULL, NULL is returned. Otherwise, a 32-bit unsigned value is returned after redundancy calculation.

Example of rewriting the **CRC32()** function using the user-defined function statement of GaussDB(DWS):

::

   CREATE OR REPLACE FUNCTION crc32(text_string text) RETURNS bigint AS $$
   DECLARE
       val bigint;
       i int;
       j int;
       byte_length int;
       binary_string bytea;
   BEGIN
       IF text_string is null THEN
           RETURN null;
       ELSIF text_string = '' THEN
           RETURN 0;
       END IF;

       i = 0;
       val = 4294967295;
       byte_length = bit_length(text_string) / 8;
       binary_string = decode(replace(text_string, E'\\', E'\\\\'), 'escape');
       LOOP
           val = (val # get_byte(binary_string, i))::bigint;
           i = i + 1;
           j = 0;
           LOOP
               val = ((val >> 1) # (3988292384 * (val & 1)))::bigint;
               j = j + 1;
               IF j >= 8 THEN
                   EXIT;
               END IF;
           END LOOP;
           IF i >= byte_length THEN
               EXIT;
           END IF;
       END LOOP;
       RETURN (val # 4294967295);
   END
   $$ IMMUTABLE LANGUAGE plpgsql;

Verify the rewriting result.

::

   select crc32(null),crc32(''),crc32('1');
    crc32 | crc32 |   crc32
   -------+-------+------------
          |     0 | 2212294583
   (1 row)

For details about how to use user-defined functions, see .
