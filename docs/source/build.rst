.. highlight:: bash

How to Build
============

Recent DTF library and DTF-based PnetCDF library can be downloaded from `DTF github repository`_. 

.. _DTF github repository: https://github.com/maneka07/DTF/

Build DTF Library
-----------------

::

	git clone https://github.com/maneka07/DTF.git	

Assume the path to the downloaded DTF library is ``DTF_SRC_DIR`` on your system.
User needs to build the DTF library first:
::

	cd ${DTF_SRC_DIR}/libdtf && make

The default MPI compiler is ``mpicc``. Other MPI compilers can be specified through the ``MPICC`` variable in the Makefile.
The shared object ``libdtf.so`` will be generated in the ``${DTF_SRC_DIR}/libdtf`` directory after successful build.

Build PnetCDF Library
---------------------

Next, user should build the DTF-based PnetCDF library by executing the commands below.
Assume that PnetCDF library will be installed at the path ``PNETCDF_INSTALL_DIR`` and paths to the MPI compilers are ``MPI_C_COMPILER``, ``MPI_CXX_COMPILER`` and ``MPI_FORTRAN_COMPILER``.
::

	cd ${DTF_SRC_DIR}/pnetcdf
	autoreconf
	./configure --prefix=${PNETCDF_INSTALL_DIR} \
		CFLAGS="-I${DTF_SRC_DIR}/libdtf" \
		LDFLAGS="-L${DTF_SRC_DIR}/libdtf -ldtf -Wl,-rpath=${DTF_SRC_DIR}/libdtf" \
		CC=${MPI_C_COMPILER} FC=${MPI_FORTRAN_COMPILER} MPICXX=${MPI_CXX_COMPILER}
	make -j && make install


Build Splitworld Wrapper Library (Optional)
-------------------------------------------

There are two launch modes for multi-component workflows to execute multiple programs simultaneously.
One of them is to execute programs individually in background by different ``mpiexec``.
The other mode is to use the MPMD launch mode of ``mpiexec``, which allows user to execute multiple programs at the same time by the same ``mpiexec``.

If user runs DTF-based multi-component programs in the MPMD launch mode, an additional library called Splitworld wrapper should be built as well.
Splitowrld wrapper library can be downloaded from `Splitwrapper github repository`_ by executing the command:

.. _Splitwrapper github repository: https://github.com/maneka07/split_world_wrap

::

	git clone https://github.com/maneka07/split_world_wrap.git

Build the library simply by executing the command:

::
	
	cd split_world_wrap && make

MPI Compiler can be specified through the ``CC`` variable in the Makefile.

The shared object ``libsplitworld.so`` will be generated after successful build.
