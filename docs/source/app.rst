
How to Use
==========

One of the significant characteristics of the DTF library is that it's easy to use.
There are only three steps required to adopt the DTF data transfer to a multi-component workflow.

Step One: Prepare a configuration file
--------------------------------------
User should prepare a configuration file in INI file format to describe the basic information about the workflow to the DTF library and switch on/off DTF functionalities by key-value pairs.
Figure :numref:`config_file` gives a simple example of a configuration file.


.. _config_file:

.. figure:: img/config-file.png
	:align: center
	
	An example of a DTF configuration file.

As shown in the figure above, there are two types of sections in the configuration file, the ``[INFO]`` section and the ``[FILE]`` section.

[INFO] Section
^^^^^^^^^^^^^^

Every configuration file should start with the ``[INFO]`` section to specify the coupled components and other global settings.
A ``[INFO]`` section should contain:
* ``ncomp``: the number of components
* ``comp_name``: the name of each component

For example in :numref:`config_file`, the ``[INFO]`` section of the configuration file consists of two components ``bin1`` and ``bin2``.

Other optional settings are also available:

* ``buffer_data``: all of the output data will be buffered inside DTF when its value is set to 1. This option is ignored when the file I/O mode is specified, which will be introduced in :ref:`file_section` below.

* ``iodb_build_mode``: this option is for specifying how the metadata of I/O requests will be distributed among the matcher processes. The avaiable settings are:

	* ``varid``: I/O requests will be distributed by variable ID. This setting is not recommanded when a file has variables than the number of matcher processes.
	* ``range``: This is the default setting. Each matcher process is responsible for a particular subblock of a variable's data. Unless the environment variable ``DTF_VAR_BLOCK_RANGE`` is set into a specific value, data array of each variable will be evenly divided into subblocks along the slowest changing dimension.

* ``do_checksum``: checksum will be computed for debugging purpose if its value is set to 1.
* ``log_ioreqs``: I/O requests will be logged for debugging purpose if its value is set to 1.

.. _file_section:

[FILE] Section
^^^^^^^^^^^^^^

A configuration file may contain multiple ``[FILE]`` sections, while each of them describes a different PnetCDF file used for transferring data between components.

The compulsory settings of each ``[FILE]`` section are:

* ``filename``: name of the file for transferring data between the components. "%" symbol can be used as a wildcard in the file name to define a name pattern matching a group of files which have the identical dimensions and variables (e.g. ``filename=000%/file.00%``. User should be responsible for providing the file name or file pattern that won't accidentally match against other irrelevant files.


Step Two: Insert three DTF function calls 
-----------------------------------------

Three intuitive DTF function calls should be inserted into the source code of each component of a mutli-component system.
These three functions are:


Step Three: Compile your code
-------------------------------

Code Sample 
-----------
