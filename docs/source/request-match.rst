
I/O Request Matching
--------------------
I/O request matching is one of the important features in DTF.
During execution, designated numbers of processes of the writer component will be assigned as matcher processes, which will collect all the I/O requests and match each read request against the corresponding write request(s) according to the metadata contained in each request.
The number of matcher processes can be designated by users.

For example, as shown in the figure :ref:`request-m`, there are two matcher processes in the writer component.
Firstly the matcher processes collect all the I/O requests from other writer processes and reader processes.
Matcher processes then perform request matching based on the ``start`` and ``count`` metadata contained in the I/O request and look for the write requests which write data to the array blocks that will be read by each reader request.
The requested data blocks of each read request may be matched with multiple write requests, each of which writes to a sub-block of the requested array block.
In this example, the read request of process 2 of the reader component is matched against process 1, 2 and 3 of the writer component.
Once a match is found, the matcher processes ask the corresponding writer process to start sending the requested data to the reader process.
After receiving the message from the matcher, the writer process copies the requested data to the send buffer along with the metadata and sends to the reader process.
Once the reader process receives the data, it unpacks the message and copies the data to its user buffer.

.. _request-m:

.. figure:: img/request-m.png
    :align: center

    A request matching example.

For optimal performance, I/O requests are divided and distributed among the matcher processes and each of them is assigned to handle the I/O requests for a sub-block of the multi-dimentional variable.
There is a trade-off in this approach: request matching in parallel improves performance; However, if there are too many matchers the request may end up being split too many times resulting in more communication between readers and writers.
Therefore, user should be responsible to choose the best number of matchers.
