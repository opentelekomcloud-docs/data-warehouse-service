:original_name: dws_06_0019.html

.. _dws_06_0019:

UUID Type
=========

Universally Unique Identifier (UUID) is a 128-bit identifier used in the computer system to identify information.

All elements in a distributed system can be uniquely identified by an UUID, without having to be identified in the central control end. In many application scenarios, an ID is required to identify only one object. A common example is the ID column of a database table. Another example is the front end UI libraries, because they usually need to dynamically create various UI elements that require unique IDs.

For details about UUID functions and UUID application examples supported by GaussDB(DWS), see :ref:`UUID Functions <dws_06_0040>`.

UUID Formats
------------

UUIDs are standardized as part of the Distributed Computing Environment (DCE) in RFC 4122 released by the Internet Engineering Task Force (IETF) of the Open Software Foundation (OSF). A standard UUID consists of 36 characters, including 32 hexadecimal digits and four hyphens (-). The format is 8-4-4-4-12. An example of a standard UUID is as follows:

::

   a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11

GaussDB(DWS) also accepts the following alternative forms for input: use of upper-case letters and digits, the standard format surrounded by braces, omitting some or all hyphens, and adding a hyphen after any group of four digits. Examples

::

   A0EEBC99-9C0B-4EF8-BB6D-6BB9BD380A11
   {a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11}
   a0eebc999c0b4ef8bb6d6bb9bd380a11
   a0ee-bc99-9c0b-4ef8-bb6d-6bb9-bd38-0a11

UUID Composition
----------------

A UUID uses time and space information to ensure global uniqueness. A UUID consists of a timestamp, a clock sequence, and a node ID (MAC address). However, multiple nodes in a distributed GaussDB(DWS) cluster may be deployed on the same machine and have the same MAC address. As a result, UUID conflicts may occur. Therefore, GaussDB(DWS) replaces the last 48 bits of the MAC address with the sequence number of the CN or DN that generates the UUID and the current thread ID to ensure that the UUID is globally unique in the distributed cluster.
