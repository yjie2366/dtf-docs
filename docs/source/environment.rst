
Step Three: Compilation and Execution
------------------------------------

Compilation
^^^^^^^^^^^
After the introduced preparations have done, all the components should be compiled with the DTF library and PnetCDF library using MPI compiler.
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

* ``MAX_WORKGROUP_SIZE``: 

* ``DTF_VERBOSE_LEVEL``:

* ``DTF_INI_FILE``:

* ``DTF_PRINT_STATS``:

* ``DTF_USE_MSG_BUFFER``:

* ``DTF_DATA_MSG_SIZE_LIMIT``:

* ``DTF_VAR_BLOCK_RANGE``:

* ``DTF_TIMEOUT``:

* ``DTF_IGNORE_IDLE``:

* ``MPMD_COMP``:

Execution
^^^^^^^^^


