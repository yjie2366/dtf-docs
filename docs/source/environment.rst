
Step Three: Compilation and Execution
------------------------------------

Compilation
^^^^^^^^^^^
After the above preparations are complete, all the components should be compiled with the DTF library and PnetCDF library using MPI compiler.
Assume that the path to the DTF library is ``DTF_SRC_DIR``, and PnetCDF library is installed in the directory ``PNETCDF_INSTALL_DIR``.
Users should append the following values to the original build options of each component. For example:

.. code-block:: Makefile

	CFLAGS += -I${DTF_SRC_DIR} -I${PNETCDF_INSTALL_DIR}/include
	LIBS += -L${DTF_SRC_DIR} -L${PNETCDF_INSTALL_DIR}/lib
	LDFLAGS += -ldtf -lpnetcdf -Wl,-rpath=${DTF_SRC_DIR}


Environment Variables
^^^^^^^^^^^^^^^^^^^^^

Before execution, there are some compulsory and optional environment variables provided by DTF to further adjust its runtime behavior.

The only **compulsory** environment variable is ``DTF_GLOBAL_PATH``, which should be set into the path toa directory that can be accessed by all the processes in the workflow.

The optional environment variables are listed below:

* ``MAX_WORKGROUP_SIZE``: The number of processes that will be handled by each matcher process. (Default: 64)

	* # of matchers per component = # of processes per component / ``${MAX_WORKGROUP_SIZE}``
	* The value of this variable affects the overall performance. It's recommended to execute the program with different values and find the best setting.

* ``DTF_VERBOSE_LEVEL``: logging levels. At each level, addtional information will be output to ``stdout`` besides the lower-level output. Available values are listed below. (Default: 0)
	* 0 - output only errors and warnings
	* 1 - output debug information additionally
	* 2 - output extended debug information additionally

* ``DTF_INI_FILE``: path to the configuration file. (No default value)
	* If this variable is set, the path passed to the ``dtf_init()`` parameter will be overriden.

* ``DTF_PRINT_STATS``: DTF will collect and output internal execution statistics if this variable is set. (Default: 0)

* ``DTF_USE_MSG_BUFFER``: DTF will use the preallocated buffer to transfer data if this variable is set. The size of this buffer is defined by the variable ``DTF_DATA_MSG_SIZE_LIMIT``. The writer process will transfer data to the reader process(es) in a sequential manner if this variable is set. Otherwise, the writer process will transfer data to all the matched reader process(es) in parallel, which may consume more memory. (Default: 0)

* ``DTF_DATA_MSG_SIZE_LIMIT``: as introduced above, this variable is used in conjunction with the variable ``DTF_USE_MSG_BUFFER`` to define the size of the preallocated buffer. (Default: 64 MB)

* ``DTF_VAR_BLOCK_RANGE``: the length of the divided data block in the slowest changing dimension for each matcher process if the value of this variable is bigger than 0. (Default: 0)
	* By default, the slowest changing dimension of a data array will be divided among the number of matcher processes. Each matcher process will be responsible for its assigned data block.

* ``DTF_TIMEOUT``: the timeout limit of DTF. The application will be terminated if DTF does not progress within the defined time limit (Default: 180s)
	* The time limit will be ignored if ``DTF_IGNORE_IDLE`` is set

* ``DTF_IGNORE_IDLE``: if this variable is set, the application will keep waiting even if DTF is not progressing. (Default: 0)

* ``MPMD_COMP``: the value of this variable is used for distinguishing between the components in MPMD execution mode. The value should be unique for each component.

Execution
^^^^^^^^^

There are two different execution modes can be used to run a DTF-based workflow, background execution and MPMD mode-based execution.


