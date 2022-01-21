How to Build
============

Recent DTF library and DTF-based PnetCDF library can be downloaded from `DTF github repository`_. 

.. _DTF github repository: https://github.com/maneka07/DTF/

::

	git clone https://github.com/maneka07/DTF.git	

Assume the path to the downloaded DTF library is ``DTF_SRC_DIR`` on your system.
User needs to build the DTF library first:
::

	cd ${DTF_SRC_DIR}/libdtf && make

The default MPI compiler is ``mpicc``. Other MPI compilers can be specified through the ``MPICC`` variable in the Makefile.
The shared object ``libdtf.so`` will be generated in the ``${DTF_SRC_DIR}/libdtf`` directory after successful build.

Next, user should build the DTF-based PnetCDF library directory.
Assume that PnetCDF library will be installed at path ``PNETCDF_INSTALL_DIR``.
::

	cd ${DTF_SRC_DIR}/pnetcdf
	autoreconf
	./configure --prefix=${PNETCDF_INSTALL_DIR} CFLAGS="-I${DTF_SRC_DIR}/libdtf" LDFLAGS="-L${DTF_SRC_DIR}/libdtf -ldtf -Wl,-rpath=${DTF_SRC_DIR}/libdtf" CC=${MPI_C_COMPILER} FC=${MPI_FORTRAN_COMPILER} MPICXX=${MPI_CXX_COMPILER}
	make -j && make install


