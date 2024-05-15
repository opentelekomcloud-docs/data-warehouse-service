:original_name: dws_06_0318.html

.. _dws_06_0318:

Network Address Functions
=========================

The **abbrev**, **host**, and **text** functions are primarily intended to offer alternative display formats.

Any **cidr** value can be cast to **inet** implicitly or explicitly; therefore, the functions shown above as operating on **inet** also work on **cidr** values. An **inet** value can be cast to **cidr**. After the conversion, any bits to the right of the subnet mask are silently zeroed to create a valid **cidr** value. In addition, you can cast a text string to **inet** or **cidr** using normal casting syntax. For example, **inet(expression)** or **colname::cidr**.

abbrev(inet)
------------

Description: Abbreviated display format as text

Return type: text

Example:

::

   SELECT abbrev(inet '10.1.0.0/16') AS RESULT;
      result
   -------------
    10.1.0.0/16
   (1 row)

abbrev(cidr)
------------

Description: Abbreviated display format as text

Return type: text

Example:

::

   SELECT abbrev(cidr '10.1.0.0/16') AS RESULT;
    result
   ---------
    10.1/16
   (1 row)

broadcast(inet)
---------------

Description: Broadcast address for network

Return type: inet

Example:

::

   SELECT broadcast('192.168.1.5/24') AS RESULT;
         result
   ------------------
    192.168.1.255/24
   (1 row)

family(inet)
------------

Description: Extracts family of address; **4** for IPv4, **6** for IPv6

Return type: int

Example:

::

   SELECT family('::1') AS RESULT;
    result
   --------
         6
   (1 row)

host(inet)
----------

Description: Extracts IP address as text.

Return type: text

Example:

::

   SELECT host('192.168.1.5/24') AS RESULT;
      result
   -------------
    192.168.1.5
   (1 row)

hostmask(inet)
--------------

Description: Constructs host mask for network.

Return type: inet

Example:

::

   SELECT hostmask('192.168.23.20/30') AS RESULT;
    result
   ---------
    0.0.0.3
   (1 row)

masklen(inet)
-------------

Description: Extracts subnet mask length.

Return type: int

Example:

::

   SELECT masklen('192.168.1.5/24') AS RESULT;
    result
   --------
        24
   (1 row)

netmask(inet)
-------------

Description: Constructs a subnet mask for the network.

Return type: inet

Example:

::

   SELECT netmask('192.168.1.5/24') AS RESULT;
       result
   ---------------
    255.255.255.0
   (1 row)

network(inet)
-------------

Description: Extracts network part of address.

Return type: cidr

Example:

::

   SELECT network('192.168.1.5/24') AS RESULT;
        result
   ----------------
    192.168.1.0/24
   (1 row)

set_masklen(inet, int)
----------------------

Description: Sets subnet mask length for **inet** value.

Return type: inet

Example:

::

   SELECT set_masklen('192.168.1.5/24', 16) AS RESULT;
        result
   ----------------
    192.168.1.5/16
   (1 row)

set_masklen(cidr, int)
----------------------

Description: Sets subnet mask length for **cidr** value.

Return type: cidr

Example:

::

   SELECT set_masklen('192.168.1.0/24'::cidr, 16) AS RESULT;
        result
   ----------------
    192.168.0.0/16
   (1 row)

text(inet)
----------

Description: Extracts IP address and subnet mask length as text.

Return type: text

Example:

::

   SELECT text(inet '192.168.1.5') AS RESULT;
        result
   ----------------
    192.168.1.5/32
   (1 row)

trunc(macaddr)
--------------

The function **trunc(macaddr)** returns a MAC address with the last 3 bytes set to zero.

Description: Sets last 3 bytes to zero.

Return type: macaddr

Example:

::

   SELECT trunc(macaddr '12:34:56:78:90:ab') AS RESULT;
         result
   -------------------
    12:34:56:00:00:00
   (1 row)

The **macaddr** type also supports the standard relational operators (such as **>** and **<=**) for lexicographical ordering, and the bitwise arithmetic operators (**~**, **&** and **\|**) for NOT, AND and OR.
