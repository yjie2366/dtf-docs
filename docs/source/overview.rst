.. _overview:

Overview of DTF
===============

The current implementation of DTF works with multi-component systems whose inter-component data transfer is built upon Parallel NetCDF I/O (PnetCDF) library.
The original PnetCDF library uses MPI-IO under the hood, which allows multiple processes to write data to the same NetCDF file in parallel.
By compiling with the provided DTF-based PnetCDF library, original MPI-IO operations will be transparently redirected to message passing, which is implemented in the DTF library.
Metadate of every added PnetCDF variable will be stored in a DTF variable object, and each I/O operation will be registered as a read/write I/O request object in DTF.
Each I/O request object contains the following metadata:

* ``varid``: the PnetCDF variable ID;
* ``rw_flag``: for distinguishing between read and write request;
* ``datatype``: requested data type;
* ``start``: corner coordinate of the requested data block;
* ``count``: length of the requested data block in each dimension;
* ``buffer``: address of the user buffer.

There are two important features during data transfer of DTF library, which is I/O request matching and I/O replaying.

.. include:: request-match.rst
