
Step Two: Insert three DTF function calls 
-----------------------------------------

The design of DTF aims to minimize code modification to the workflow.
To enable DTF data transfer, users are required to include the header file ``dtf.h`` and put three intuitive DTF function calls into the source code of each component.

The required DTF functions in C language are:

.. code-block:: c
	
	/*
	DESCRIPTION:
		Initialize the DTF library. This function should be called after MPI_Init().
	
	PARAMETERS:
		* [IN] filename - path to the DTF configuration file
	   	* [IN] module_name - name of the component which is calling this function;
			The value of this parameter must be one of the component names listed
			in the [INFO] section in the configuration file specified by `filename`.
	
	RETURN VALUE:
		* 0 - Initialization succeeded
		* 1 - Initialization failed
	\*/

	int dtf_init(const char \*filename, char \*module_name);


	/*
	DESCRIPTION:
		Finalize the DTF library. This function should be called before MPI_Finalize().

	PARAMETERS:
		none
	
	RETURN VALUE:
		* 0 - Finalization succeeded
		* 1 - Finalization failed
	\*/

	int dtf_finalize();


	/*
	DESCRIPTION:
		Command DTF to start transferring data between components.

	PARAMETERS:
		* [IN] filename - name or name pattern of the PnetCDF file specified in the
				corresponding [FILE] section of the configuration file
		* [IN] ncid - PnetCDF file ID. The ID that is returned by the PnetCDF file
				create or open function

	RETURN VALUE:
		* 0 - the return value of this function is always zero. Applications will 
			abort if an error occurs during the function execution.
	\*/

	int dtf_transfer(const char \*filename, int ncid);

.. warning::
	The ``dtf_transfer()`` function should be called by both of the components which transfer data between each other through the file specified in each [FILE] section.
