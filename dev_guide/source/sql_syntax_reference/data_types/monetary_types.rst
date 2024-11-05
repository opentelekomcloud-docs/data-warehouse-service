:original_name: dws_06_0010.html

.. _dws_06_0010:

Monetary Types
==============

The **money** type stores a currency amount with fixed fractional precision. The range shown in :ref:`Table 1 <en-us_topic_0000001460880888__t46041f911048428d9ee80bf0b323b430>` assumes there are two fractional digits. Input is accepted in a variety of formats, including integer and floating-point literals, as well as typical currency formatting, such as $1,000.00. Output is generally in the latter form but depends on the locale.

.. _en-us_topic_0000001460880888__t46041f911048428d9ee80bf0b323b430:

.. table:: **Table 1** Monetary types

   +-------+--------------+-----------------+------------------------------------------------+
   | Name  | Storage Size | Description     | Range                                          |
   +=======+==============+=================+================================================+
   | money | 8 bytes      | Currency amount | -92233720368547758.08 to +92233720368547758.07 |
   +-------+--------------+-----------------+------------------------------------------------+

Values of the **numeric**, **int**, and **bigint** data types can be cast to **money**. Conversion from the **real** and **double precision** data types can be done by casting to **numeric** first, for example:

::

   SELECT '12.34'::float8::numeric::money;

However, this is not recommended. Floating point numbers should not be used to handle money due to the potential for rounding errors.

A **money** value can be cast to **numeric** without loss of precision. Conversion to other types could potentially lose precision, and must also be done in two stages:

::

   SELECT '52093.89'::money::numeric::float8;

When a **money** value is divided by another **money** value, the result is **double precision** (that is, a pure number, not money); the currency units cancel each other out in the division.
