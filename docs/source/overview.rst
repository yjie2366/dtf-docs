.. _overview:

Overview of DTF
===============

The current implementation of DTF works with multi-component systems whose inter-component data transfer is built upon Parallel NetCDF I/O (PnetCDF) library.
The original PnetCDF library uses MPI-IO under the hood, which allows multiple processes to write data to the same NetCDF file in parallel.
By compiling with the provided DTF-based PnetCDF library, original MPI-IO operations will be transparently redirected to message passing, which is implemented in the DTF library.
The design of DTF aims at minimizing code modification of the original PnetCDF-based I/O kernel.
Common PnetCDF function calls during each I/O phase, such as ``ncmpi_open()``, ``ncmpi_def_dim`` and ``ncmpi_def_var`` will be intercepted by the DTF library, and the metadataof this 'file' will be stored in DTF data structures.
Each PnetCDF I/O operation such as ``ncmpi_put_var()`` and ``ncmpi_get_var()`` will registered as a read or write I/O request object in the DTF library.
Each I/O request object contains the following metadata:

* ``varid``: the PnetCDF variable ID;
* ``rw_flag``: for distinguishing between read and write request;
* ``datatype``: requested data type;
* ``start``: corner coordinate of the requested data block;
* ``count``: length of the requested data block in each dimension;
* ``buffer``: address of the user buffer.

These I/O requests will be used during DTF data transfer function for I/O request matching and data sending.

There are two important features during DTF data transfer, I/O request matching and I/O replay, which will be introduced in the following subsections.

.. include:: request-match.rst

.. include:: io-replay.rst
