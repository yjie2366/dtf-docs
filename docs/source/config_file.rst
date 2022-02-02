
Step One: Prepare a configuration file
--------------------------------------
User should prepare a configuration file in INI format to describe the basic information about the workflow to the DTF library and switch on/off DTF functionalities by key-value pairs.
There are two types of sections should be included in the configuration file, the ``[INFO]`` section and the ``[FILE]`` section.

[INFO] Section
^^^^^^^^^^^^^^

Every configuration file should start with the ``[INFO]`` section to specify the coupled components and other global settings.
A ``[INFO]`` section must contain the following settings:

* ``ncomp``: the number of components

* ``comp_name``: the name of each component. Each component name should be assigned to an individual option.

Available **optional** settings are listed below:

* ``buffer_data``: all of the output data will be buffered inside DTF when its value is set to 1. 

* ``iodb_build_mode``: this option is for specifying how the metadata of I/O requests will be distributed among the matcher processes. The avaiable settings are:

	* ``varid``: I/O requests will be distributed by variable ID. This setting is not recommanded when a file has variables than the number of matcher processes.

	* ``range``: This is the default setting. Each matcher process is responsible for a particular subblock of a variable's data. Unless the environment variable ``DTF_VAR_BLOCK_RANGE`` is set into a specific value, data array of each variable will be evenly divided into subblocks along the slowest changing dimension.

* ``do_checksum``: checksum will be computed for debugging purpose when its value is set to 1.

* ``log_ioreqs``: I/O requests will be logged for debugging purpose when its value is set to 1.

.. _file_section:

[FILE] Section
^^^^^^^^^^^^^^

A configuration file may contain multiple ``[FILE]`` sections, while each of them describes a different PnetCDF file used for transferring data between the coupled components.
The compulsory settings of each ``[FILE]`` section are:

* ``filename``: name of the file for transferring data between the components. "%" symbol can be used as a wildcard in the file name to define a name pattern matching a group of files which have the identical dimensions and variables (e.g. ``filename=000%/file.00%``. 

.. warning::
	Users should be responsible for providing the correct file name or name pattern that won't accidentally match against other irrelevant files.
	The specified file path passed to the PnetCDF open, PnetCDF create and DTF data transfer functions should be the same for both the reader and writer components.

* ``comp1``: name of one of the coupled components

* ``comp2``: name of the other component

* ``mode``: data transfer mode. There are two modes supported in DTF:

	* ``transfer``: data will be transferred through DTF data transfer if this value is set.

	* ``file``: data will be transferred through PnetCDF file I/O if this value is set. In this case DTF will simply play a role as an arbitrator which postpones the reader component's processing until the file is ready to be read, i.e. the writer has finished its writing and closed the file.

The **optional** settings for this section are listed below:

* ``exclude_name``: name or name patterns of the files that will be excluded from name matching.

* ``replay_io``: as introduced in :ref:`replay`, the I/O request matching will be skipped from the second cycle when its value is set to 1. It's applicable when an iterative workflow has identical I/O pattern for each iteration.

* ``num_sessions``: the number of I/O sessions, i.e. open and close, that will be performed on the file by the coupled components. The correct setting of this option is related to garbage collection. (Default: 1)

* ``mirror_io_root``: if the rank of root process in coupled components is identical, e.g. both of the root processes are rank 0, setting the value of this option to 1 will improve performance. (Default: 0)

.. note::
	The rank mentioned above refers to the rank of processe in ``MPI_COMM_WORLD`` communicator of each component if launched by different ``mpiexec``.
	When MPMD launch mode is used, it refers to the rank of process in the sub-communicator splitted from ``MPI_COMM_WORLD`` by the Splitworld Wrapper, which has been introduced in :ref:`split_world`.

* ``write_only``: this setting should be set into 1 if the coupled components only perform write access to this file. This setting is for ``file`` mode only.


This table summarizes which optional settings are available under each data transfer mode respectively:

+--------------------------+---------------+-----------+
|                          | Transfer Mode | File Mode |
+========+=================+===============+===========+
| [INFO] | buffer_data     |       ✓       |      x    |
|        +-----------------+---------------+-----------+
|        | iodb_build_mode |       ✓       |      x    |
|        +-----------------+---------------+-----------+
|        | do_checksum     |       ✓       |      x    |
|        +-----------------+---------------+-----------+
|        | log_ioreqs      |       ✓       |      x    |
+--------+-----------------+---------------+-----------+
| [FILE] | exclude_name    |       ✓       |      ✓    |
|        +-----------------+---------------+-----------+
|        | replay_io       |       ✓       |      x    |
|        +-----------------+---------------+-----------+
|        | num_sessions    |       ✓       |      x    |
|        +-----------------+---------------+-----------+
|        | mirror_io_root  |       ✓       |      x    |
|        +-----------------+---------------+-----------+
|        | write_only      |       x       |      ✓    |
+--------+-----------------+---------------+-----------+


A Configuration File Example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is a configuration file example for a workflow that combines two different components, which are named as ``foo`` and ``bar`` respectively.
We assume that all the PnetCDF data files are stored under the directory ``data/``.
During the workflow execution, two groups of different PnetCDF files will be used for transferring data between the coupled components, one group of them is the *analysis* file, and the other group is the *history* file.
Two I/O sessions will be performed on the *analysis* file, while the file will be ignored by DTF if its name or path contains "060000" substring.
Only one I/O session will be performed on all the *history* file without any exception.
Assuming that the workflow contains multiple iterations with identical I/O pattern on both types of files, ``replay_io`` option can be enabled to improve data transfer efficiency.

Besides, both of the components perform write operations on another group of files named *mean* to store average values.
Therefore, the I/O mode of this file is set into ``file`` and ``write_only`` option is set into 1.

According to the description above, the configuration file for this workflow should be:

::

	[INFO]
	ncomp=2
	comp_name="foo"
	comp_name="bar"
	buffer_data=1
	
	[FILE]
	filename="data/analysis.%"
	exclude_name="060000"
	comp1="foo"
	comp2="bar"
	mode="transfer"
	replay_io=1
	
	[FILE]
	filename="data/history.%"
	comp1="foo"
	comp2="bar"
	mode="transfer"
	replay_io=1
	
	[FILE]
	filename="data/mean"
	comp1="foo"
	comp2="bar"
	mode="file"
	write_only=1
