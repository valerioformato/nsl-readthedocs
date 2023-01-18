Getting started
===============

Requirements
------------
Requirements for NSL are mainly driven by NAIA:

* A C++ compiler with full c++14 support (tested with gcc >= 9.3.0)
* CMake version >= 3.13
* A ROOT installation compiled with c++14 support (tested with ROOT >= 6.22/08, recommended 6.26/02)

If you are working in an environment with a functioning NAIA install you should be good to go.


Building and installing
-----------------------

Follow this simple procedure:

* Clone this repository

  * ``git clone https://:@gitlab.cern.ch:8443/ams-italy/nsl.git`` (Kerberos)
  * ``git clone ssh://git@gitlab.cern.ch:7999/ams-italy/nsl.git`` (SSH) 
  * ``git clone https://gitlab.cern.ch/ams-italy/nsl.git`` (HTTPS) 

* Create a build and install directory

  * e.g: ``mkdir nsl.build nsl.install``

* Build the project (assuming the ``NAIADIR`` environment variable is set)

  * ``cd nsl.build`` 
  * ``cmake ../nsl -DCMAKE_INSTALL_PREFIX=${your-install-path-here}``
  * ``make all install``

.. note:: 
  If you don't have the ``NAIADIR`` variable set but you have a NAIA installation you can point CMake to it:

  .. code-block:: bash

   cmake ../nsl -DNAIA_DIR=${your-naia-install-path-here}/cmake

  Alternatively you can have CMake download NAIA for you: 

  .. code-block:: bash
    
   cmake ../nsl -DNSL_DOWNLOAD_NAIA=ON -DNAIA_DOWNLOAD_VERSION=1.0.1

  In the last example you can omit the NAIA version, a default version (which should be fairly recent) will be downloaded


Using the project
-----------------

To use the NAIA ntuples your project needs:

* the headers in ``nsl.install/include``
* the ``nsl.install/lib/libNSLSelections.so`` library

Similarly to what NAIA does, NSL exports the ``NSL::Selections`` CMake target which is the main one to link against.

.. code-block:: cmake

  find_package(NSL REQUIRED)
  
  set(SOURCES MyProgram.cpp)

  add_executable(MyProgram ${SOURCES})
  target_link_libraries(MyProgram NAIA::NAIAChain NSL::Selections)

.. note::

   For any question or in case you need help write to valerio.formato@cern.ch 
