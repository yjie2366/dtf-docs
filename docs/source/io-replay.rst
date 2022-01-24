I/O Replay
----------
Some multi-component workflows contain multiple components executing iteratively with constant I/O patterns and data size of each iteration.
Only the contents of the data blocks are being updated during each iteration.
In the latest version of the DTF, an option to record and replay the I/O pattern is introduced as an optimization for the iterative frameworks, which can easily enabled and disabled by users.
How to enable I/O replay will be introduced in the section :ref:`usage`.

When the I/O replay is enabled, during the first iteration, writer processes will store the information about what variable blocks they have sent to which processes in the other component as a record of the request matching.
In all the subsequent iterations, the I/O request matching phase will be skipped during data transfer so that all the writer processes can immediately start sending the required data to the readers by following the recorded pattern.
