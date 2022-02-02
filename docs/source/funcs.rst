
Step Two: Insert three DTF function calls 
-----------------------------------------

The design of DTF aims to minimize code modification to the workflow.
To enable DTF data transfer, users are required to include the header file ``dtf.h`` and put three intuitive DTF function calls into the source code of each component.

The required DTF functions are:

.. code-block:: c
	
	/* 
	Initialize the DTF library. This function should be called after MPI_Init().
	
	Parameters:
		* [IN] filename - path to the DTF configuration file
	   	* [IN] module_name - name of the component which is calling this function;
			The value of this parameter must be one of the component names listed
			in the [INFO] section in the configuration file specified by `filename`.
	Return Value:
		* 0 - DTF library is successfully initialized
		* 1 - Initialization failed
	*/

	int dtf_init(const char *filename, char *module_name);


