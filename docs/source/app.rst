
How to Use
==========

One of the significant characteristics of the DTF library is that it's easy to use.
There are only three steps required to adopt the DTF data transfer to a multi-component workflow.

Step One: Prepare a configuration file
--------------------------------------
User should prepare a configuration file in INI file format to describe the basic information about the workflow to the DTF library and switch on/off DTF functionalities by key-value pairs.
There are two types of sections should be included in the configuration file, the ``[INFO]`` section and the ``[FILE]`` section.

[INFO] Section
^^^^^^^^^^^^^^

Every configuration file should start with the ``[INFO]`` section to specify the coupled components and other global settings.
A ``[INFO]`` section should contain:

* ``ncomp``: the number of components

* ``comp_name``: the name of each component

Other optional settings include: 

* ``buffer_data``: all of the output data will be buffered inside DTF when its value is set to 1. This option is ignored when the file I/O mode is specified, which will be introduced in :ref:`file_section` below.

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

.. note::
	Users should be responsible for providing the correct file name or name pattern that won't accidentally match against other irrelevant files.
	The specified file path passed to the PnetCDF open, PnetCDF create and DTF data transfer functions should be the same for both the reader and writer components.

* ``comp1``: name of one of the coupled components

* ``comp2``: name of the other component

* ``mode``: data transfer mode. There are two modes supported in DTF:

	* ``transfer``: data will be transferred through DTF data transfer if this value is set.

	* ``file``: data will be transferred through PnetCDF file I/O if this value is set. In this case DTF will simply play a role as an arbitrator which postpones the reader component's processing until the file is ready to be read, i.e. the writer has finished its writing and closed the file.

The optional settings for this section are listed below:

* ``exclude_name``: name or name patterns of the files that will be excluded from name matching.

* ``replay_io``: as introduced in :ref:`replay`, the I/O request matching will be skipped from the second cycle when its value is set to 1. It's applicable when an iterative workflow has identical I/O pattern for each iteration.

* ``num_sessions``: the number of I/O sessions, i.e. open and close, that will be performed on the file by the coupled components. The correct setting of this option is related to garbage collection. (Default: 1)

* ``mirror_io_root``: if the rank of root process in coupled components is identical, e.g. both of the root processes are rank 0, setting the value of this option to 1 will improve performance. (Default: 0)

.. note::
	The rank mentioned above refers to the rank of processe in ``MPI_COMM_WORLD`` communicator of each component if launched by different ``mpiexec``.
	When MPMD launch mode is used, it refers to the rank of process in the sub-communicator splitted from ``MPI_COMM_WORLD`` by the Splitworld Wrapper, which has been introduced in :ref:`split_world`.

* ``write_only``: this setting should be set into 1 if the coupled components only perform write access to this file. This setting is for ``file`` mode only.

.. note::
	
Step Two: Insert three DTF function calls 
-----------------------------------------

Three intuitive DTF function calls should be inserted into the source code of each component of a mutli-component system.
These three functions are:


Step Three: Compile your code
-------------------------------

Code Sample 
-----------
