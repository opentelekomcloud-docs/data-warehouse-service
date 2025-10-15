:original_name: dws_06_0016.html

.. _dws_06_0016:

Network Address Types
=====================

GaussDB(DWS) offers data types to store IPv4, IPv6, and MAC addresses.

It is better to use network address types instead of plaintext types to store IPv4, IPv6, and MAC addresses, because these types offer input error checking and specialized operators and functions. For details, see :ref:`Network Address Functions and Operators <dws_06_0038>`.

.. table:: **Table 1** Network Address Types

   ======= ============= ===============================
   Name    Storage Space Description
   ======= ============= ===============================
   cidr    7 or 19 bytes IPv4 or IPv6 networks
   inet    7 or 19 bytes IPv4 or IPv6 hosts and networks
   macaddr 6 bytes       MAC addresses
   ======= ============= ===============================

When sorting **inet** or **cidr** data types, IPv4 addresses will always sort before IPv6 addresses, including IPv4 addresses encapsulated or mapped to IPv6 addresses, such as ::10.2.3.4 or ::ffff:10.4.3.2.

cidr
----

The **cidr** type (Classless Inter-Domain Routing) holds an IPv4 or IPv6 network specification. The format for specifying networks is **address/y** where **address** is the network represented as an IPv4 or IPv6 address, and y is the number of bits in the netmask. If **y** is omitted, it is calculated using assumptions from the older classful network numbering system, except it will be at least large enough to include all of the octets written in the input.

-  Example 1: Convert a value in the CIDR format to an IP address segment.

   For example, **10.0.0.0/8** is converted into a 32-bit binary address **00001010.00000000.00000000.00000000**. **/8** indicates an 8-bit network ID. The first eight bits of the 32-bit binary address are fixed. The corresponding network segment is 00001010.00000000.00000000.00000000-00001010.11111111.11111111.11111111. **10.0.0.0/8** indicates that the subnet mask is 255.0.0.0 and the corresponding network segment is 10.0.0.0-10.255.255.255.

-  Example 2: Convert an IP address segment to the CIDR format.

   For the IP address segment **192.168.0.0-192.168.31.255**, the last two segments can be converted into a binary address **00000000.00000000-00011111.11111111**. The first 19 bits (8 x 2 + 3) are fixed. Therefore, the binary address is 192.168.0.0/19.

.. table:: **Table 2** **cidr** type input examples

   +--------------------------------------+--------------------------------------+----------------------------------+
   | cidr Input                           | cidr Output                          | abbrev (cidr)                    |
   +======================================+======================================+==================================+
   | 192.168.100.128/25                   | 192.168.100.128/25                   | 192.168.100.128/25               |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 192.168/24                           | 192.168.0.0/24                       | 192.168.0/24                     |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 192.168/25                           | 192.168.0.0/25                       | 192.168.0.0/25                   |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 192.168.1                            | 192.168.1.0/24                       | 192.168.1/24                     |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 192.168                              | 192.168.0.0/24                       | 192.168.0/24                     |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 10.1.2                               | 10.1.2.0/24                          | 10.1.2/24                        |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 10.1                                 | 10.1.0.0/16                          | 10.1/16                          |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 10                                   | 10.0.0.0/8                           | 10/8                             |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 10.1.2.3/32                          | 10.1.2.3/32                          | 10.1.2.3/32                      |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 2001:4f8:3:ba::/64                   | 2001:4f8:3:ba::/64                   | 2001:4f8:3:ba::/64               |
   +--------------------------------------+--------------------------------------+----------------------------------+
   | 2001:4f8:3:ba:2e0:81ff:fe22:d1f1/128 | 2001:4f8:3:ba:2e0:81ff:fe22:d1f1/128 | 2001:4f8:3:ba:2e0:81ff:fe22:d1f1 |
   +--------------------------------------+--------------------------------------+----------------------------------+

inet
----

The **inet** type holds an IPv4 or IPv6 host address, and optionally its subnet, all in one field. The subnet is represented by the number of network address bits present in the host address (the "netmask"). If the netmask is 32 and the address is IPv4, then the value does not indicate a subnet, only a single host. In IPv6, the address length is 128 bits, so 128 bits specify a unique host address.

The input format for this type is **address/y** where **address** is an IPv4 or IPv6 address and **y** is the number of bits in the netmask. If the **/y** portion is missing, the netmask is 32 for IPv4 and 128 for IPv6, so the value represents just a single host. On display, the **/y** portion is suppressed if the netmask specifies a single host.

The essential difference between the **inet** and **cidr** data types is that **inet** accepts values with nonzero bits to the right of the netmask, whereas **cidr** does not.

macaddr
-------

The **macaddr** type stores MAC addresses, known for example from Ethernet card hardware addresses (although MAC addresses are used for other purposes as well). Input is accepted in the following formats:

.. code-block::

   '08:00:2b:01:02:03'
   '08-00-2b-01-02-03'
   '08002b:010203'
   '08002b-010203'
   '0800.2b01.0203'
   '08002b010203'

These examples would all specify the same address. Upper and lower cases are accepted for the digits a through f. Output is always in the first of the forms shown.
