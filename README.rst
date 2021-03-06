|buildstatus|_
|coverage|_

About
=====

Mangling of various file formats that conveys binary information
(Motorola S-Record, Intel HEX, TI-TXT and binary files).

Project homepage: https://github.com/eerimoq/bincopy

Documentation: https://bincopy.readthedocs.io

Installation
============

.. code-block:: python

    pip install bincopy

Example usage
=============

Scripting
---------

A basic example converting from Intel HEX to Intel HEX, SREC, binary,
array and hexdump formats:

.. code-block:: python

    >>> import bincopy
    >>> f = bincopy.BinFile("tests/files/in.hex")
    >>> print(f.as_ihex())
    :20010000214601360121470136007EFE09D219012146017E17C20001FF5F16002148011979
    :20012000194E79234623965778239EDA3F01B2CA3F0156702B5E712B722B7321460134219F
    :00000001FF

    >>> print(f.as_srec())
    S32500000100214601360121470136007EFE09D219012146017E17C20001FF5F16002148011973
    S32500000120194E79234623965778239EDA3F01B2CA3F0156702B5E712B722B73214601342199
    S5030002FA
    
    >>> print(f.as_ti_txt())
    @0100
    21 46 01 36 01 21 47 01 36 00 7E FE 09 D2 19 01
    21 46 01 7E 17 C2 00 01 FF 5F 16 00 21 48 01 19
    19 4E 79 23 46 23 96 57 78 23 9E DA 3F 01 B2 CA
    3F 01 56 70 2B 5E 71 2B 72 2B 73 21 46 01 34 21
    q        

    >>> f.as_binary()
    bytearray(b'!F\x016\x01!G\x016\x00~\xfe\t\xd2\x19\x01!F\x01~\x17\xc2\x00\x01
    \xff_\x16\x00!H\x01\x19\x19Ny#F#\x96Wx#\x9e\xda?\x01\xb2\xca?\x01Vp+^q+r+s!
    F\x014!')
    >>> list(f.segments)
    [Segment(address=256, data=bytearray(b'!F\x016\x01!G\x016\x00~\xfe\t\xd2\x19\x01
    !F\x01~\x17\xc2\x00\x01\xff_\x16\x00!H\x01\x19\x19Ny#F#\x96Wx#\x9e\xda?\x01
    \xb2\xca?\x01Vp+^q+r+s!F\x014!'))]
    >>> f.minimum_address
    256
    >>> f.maximum_address
    320
    >>> len(f)
    64
    >>> f[f.minimum_address]
    33
    >>> f[f.minimum_address:f.minimum_address + 1]
    bytearray(b'!')

See the `test suite`_ for additional examples.

Command line tool
-----------------

Print general information about given binary format file(s).

.. code-block:: text

   $ bincopy info tests/files/in.hex
   Data ranges:

       0x00000100 - 0x00000140 (64 bytes)

Convert file(s) from one format to another.

.. code-block:: text

   $ bincopy convert -i ihex -o srec tests/files/in.hex -
   S32500000100214601360121470136007EFE09D219012146017E17C20001FF5F16002148011973
   S32500000120194E79234623965778239EDA3F01B2CA3F0156702B5E712B722B73214601342199
   S5030002FA
   $ bincopy convert -i binary -o hexdump tests/files/in.hex -
   00000000  3a 32 30 30 31 30 30 30  30 32 31 34 36 30 31 33  |:200100002146013|
   00000010  36 30 31 32 31 34 37 30  31 33 36 30 30 37 45 46  |60121470136007EF|
   00000020  45 30 39 44 32 31 39 30  31 32 31 34 36 30 31 37  |E09D219012146017|
   00000030  45 31 37 43 32 30 30 30  31 46 46 35 46 31 36 30  |E17C20001FF5F160|
   00000040  30 32 31 34 38 30 31 31  39 37 39 0a 3a 32 30 30  |02148011979.:200|
   00000050  31 32 30 30 30 31 39 34  45 37 39 32 33 34 36 32  |12000194E7923462|
   00000060  33 39 36 35 37 37 38 32  33 39 45 44 41 33 46 30  |3965778239EDA3F0|
   00000070  31 42 32 43 41 33 46 30  31 35 36 37 30 32 42 35  |1B2CA3F0156702B5|
   00000080  45 37 31 32 42 37 32 32  42 37 33 32 31 34 36 30  |E712B722B7321460|
   00000090  31 33 34 32 31 39 46 0a  3a 30 30 30 30 30 30 30  |134219F.:0000000|
   000000a0  31 46 46 0a                                       |1FF.            |

Contributing
============

#. Fork the repository.

#. Install prerequisites.

   .. code-block:: text

      pip install -r requirements.txt

#. Implement the new feature or bug fix.

#. Implement test case(s) to ensure that future changes do not break
   legacy.

#. Run the tests.

   .. code-block:: text

      make test

#. Create a pull request.

.. |buildstatus| image:: https://travis-ci.org/eerimoq/bincopy.svg
.. _buildstatus: https://travis-ci.org/eerimoq/bincopy

.. |coverage| image:: https://coveralls.io/repos/github/eerimoq/bincopy/badge.svg?branch=master
.. _coverage: https://coveralls.io/github/eerimoq/bincopy

.. _test suite: https://github.com/eerimoq/bincopy/blob/master/tests/test_bincopy.py
