.. Copyright (c) 2016, Wolf Vollprecht, Johan Mabille and Sylvain Corlay

   Distributed under the terms of the BSD 3-Clause License.

   The full license is in the file LICENSE, distributed with this software.


.. raw:: html

   <style>
   .rst-content .section>img {
       width: 30px;
       margin-bottom: 0;
       margin-top: 0;
       margin-right: 15px;
       margin-left: 15px;
       float: left;
   }
   </style>

Installation
============

xtensor-zarr is a header-only library but depends on some traditional libraries that need to be installed.
On Linux, installation of the dependencies can be done through the package manager, anaconda or manual compilation.

.. image:: conda.svg

Using the conda package
-----------------------

A package for xtensor-zarr is available on the conda package manager.
The package will also pull all the dependencies (xtensor, xtensor-io).

.. code::

    conda install xtensor-zarr -c conda-forge

The easiest way to make use of xtensor-zarr in your code is by using cmake for your project.
In order for cmake to pick up the xtensor-zarr dependency, just utilize the interface target and link the xtensor-zarr library to your target.

.. code:: cmake

  add_executable(my_exec my_exec.cpp)
  target_link_libraries(my_exec
    PUBLIC
      xtensor-zarr
  )

This should be enough to add the correct directories to the include_directories and link the required libraries.
However, depending on your system setup there might be problems upon executing, as the dynamic library is not picked
up correctly. So if you run into errors that read something like "Library could not be opened ... ", then set the
``RPATH``, the runtime library search path, to the ``conda`` library path of your environment. We utilize the
``CMAKE_INSTALL_PREFIX`` for this purpose, so if you call cmake like this ``cmake .. -DCMAKE_INSTALL_PREFIX=$CONDA_PREFIX``,
add the following to your ``CMakeLists.txt``.

.. code:: cmake

  set_target_properties(my_exec
    PROPERTIES
      INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib;${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}"
      BUILD_WITH_INSTALL_RPATH ON
  )



.. image:: cmake.svg

From source with cmake
----------------------

You can also install ``xtensor-zarr`` from source with cmake.
However, you need to make sure to have the required libraries available on your machine.

On Unix platforms, from the source directory:

.. code::

    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/path/to/prefix ..
    make install

On Windows platforms, from the source directory:

.. code::

    mkdir build
    cd build
    cmake -G "NMake Makefiles" -DCMAKE_INSTALL_PREFIX=/path/to/prefix ..
    nmake
    nmake install
