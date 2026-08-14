:original_name: dws_06_0415.html

.. _dws_06_0415:

Deleting a Specified Character
==============================

trim([leading \|trailing \|both] [characters] from string)
----------------------------------------------------------

Description: Deletes specific characters from the beginning, end, or both ends of a string.

Parameters:

-  **leading**: deletes spaces or specified characters at the beginning of a string.
-  **trailing**: deletes spaces or specified characters at the end of a string.
-  **both**: deletes spaces or specified characters at the beginning and end of a string (default behavior).
-  **characters**: (optional) specifies specific characters to be deleted. If this parameter is not specified, spaces are deleted by default.
-  **string**: original string to be processed.

Return type: varchar

Examples:

Delete the characters **x** at the beginning and end of the string **xTomxx**.

::

   SELECT trim(BOTH 'x' FROM 'xTomxx');
    btrim
   -------
    Tom
   (1 row)

   SELECT trim('x' FROM 'xTomxx');
    btrim
   -------
    Tom
   (1 row)

Delete the character **x** at the beginning of the string **xTomxx**.

::

   SELECT trim(LEADING 'x' FROM 'xTomxx');
    ltrim
   -------
    Tomxx
   (1 row)

Delete the character **x** at the end of the string **xTomxx**.

::

   SELECT trim(TRAILING 'x' FROM 'xTomxx');
    rtrim
   -------
    xTom
   (1 row)

rtrim(string [, characters])
----------------------------

Description: Deletes the specified characters from the end of a string.

Parameters:

-  **string**: original string to be processed.
-  characters (optional): specifies the set of characters to be deleted from the end of a string. By default, all blank characters (for example, spaces) are deleted.

Return type: varchar

Example:

::

   SELECT rtrim('DataxBasexx','x');
      rtrim
   -----------
    DataxBase
   (1 row)

   SELECT rtrim('   Data  ');
     rtrim
   ---------
       Data
   (1 row)

ltrim(string [, characters])
----------------------------

Description: Deletes the specified strings from the beginning of a string.

Parameters:

-  **string**: original string to be processed.
-  **characters** (optional): specifies the set of characters to be deleted from the beginning of a string. By default, all blank characters (for example, spaces) are deleted.

Return type: varchar

Example:

::

   SELECT ltrim('xxDataxBase','x');
      ltrim
   -----------
    DataxBase
   (1 row)

   SELECT ltrim('   Data  ');
    ltrim
   --------
    Data
   (1 row)
