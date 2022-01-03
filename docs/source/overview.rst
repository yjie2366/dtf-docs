.. _overview:

Overview of DTF
===============

The current implementation of DTF works with multi-component systems whose inter-component data transfer is built upon Parallel NetCDF I/O (PnetCDF) library.
The original PnetCDF library uses MPI-IO under the hood, which allows multiple processes to write data to the same NetCDF file in parallel.
By compiling with the provided DTF-based PnetCDF library, original MPI-IO operations will be transparently redirected to message passing, which is implemented in the DTF library.
Metadate of every added PnetCDF variable will be stored in a DTF variable object, and each I/O operation will be registered as a read/write request object in DTF.

.. _request-m:

.. figure:: img/request-m.png
    :align: center

    A request matching example.
