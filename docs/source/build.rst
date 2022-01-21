How to Build
============

Recent DTF library and DTF-based PnetCDF library can be downloaded from `DTF github repository`_. 
.. _DTF github repository: https://github.com/maneka07/DTF/

::

	git clone https://github.com/maneka07/DTF.git	

Assume the path to the downloaded DTF library is ``DTF_SRC_DIR``.

Firstly, build the DTF library by executing:

::

	cd ${DTF_SRC_DIR}/libdtf && make

The default MPI compiler is ``mpicc``. Other MPI compilers can be used by modifying the value of ``MPICC`` variable.
The shared object ``libdtf.so`` is generated in ``${DTF_SRC_DIR}/libdtf`` directory after successful build.

