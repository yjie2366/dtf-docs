
I/O Request Matching
--------------------
I/O request matching is one of the important features in DTF.
During execution, designated numbers of processes of the writer component will be assigned as matcher processes, which will collect all the I/O requests and match each read request against the corresponding write request(s) according to the metadata contained in each request.
The number of matcher processes can be designated by users.

For example, as shown in figure :numref:`request-m`

.. _request-m:

.. figure:: img/request-m.png
    :align: center

    A request matching example.
