.. _replay:

I/O Replay
----------
Some multi-component workflows contain multiple components executing iteratively with constant I/O patterns and data size of each iteration.
Only the contents of the data blocks are being updated during each iteration.
In the latest version of the DTF, an option to record and replay the I/O pattern is introduced as an optimization for the iterative frameworks, which can easily enabled and disabled by users.
How to enable I/O replay will be introduced in the section :ref:`usage`.

When the I/O replay is enabled, during the first iteration, writer processes will store the information about all the request matching results, e.g. source and destination process of each data block.
In the subsequent iterations, the I/O request matching phase will be skipped so that all the writer processes can immediately start sending the requested data to the reader processes by following the recorded pattern.
