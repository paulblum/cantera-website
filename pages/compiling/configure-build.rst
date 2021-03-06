.. title: Configure and Build
.. description: Configure and Build Cantera

.. jumbotron::

   .. raw:: html

      <h1 class="display-3">Configure & Build Cantera</h1>

   .. class:: lead

      These directions are for the current release of Cantera, version 2.4. For
      the development version, see `these instructions <configure-build-dev.html>`_.

.. _sec-determine-config:

Determine configuration options
===============================

* Run ``scons help`` to see a list of all of the configuration options for Cantera, or
  see all of the options on the :ref:`Configuration Options <scons-config>` page.

* Configuration options are specified as additional arguments to the ``scons``
  command. For example:

  .. code:: bash

     scons command option_name=value

  where ``scons`` is the program that manages the build steps, and ``command``
  is most commonly one of

    * ``build``
    * ``test``
    * ``clean``

  Other commands are possible, and are explained in the :ref:`Build Commands <sec-build-commands>`
  section.

* SCons saves configuration options specified on the command line in the file
  ``cantera.conf`` in the root directory of the source tree, so generally it is
  not necessary to respecify configuration options when rebuilding Cantera. To
  unset a previously set configuration option, either remove the corresponding
  line from ``cantera.conf`` or use the syntax:

  .. code:: bash

     scons command option_name=

* Sometimes, changes in your environment can cause SCons's configuration tests
  (for example, checking for libraries or compiler capabilities) to unexpectedly fail.
  To force SCons to re-run these tests rather than trusting the cached results,
  run scons with the option ``--config=force``.

* The following lists of options are not complete, they show only some commonly
  used options. The entire list of options can be found on the
  :ref:`Configuration options <scons-config>` page.

Common Options
^^^^^^^^^^^^^^^

* :ref:`blas_lapack_libs <blas-lapack-libs>`

  * On OS X, the Accelerate framework is automatically used to provide
    optimized versions of BLAS and LAPACK, so the ``blas_lapack_libs``
    option should generally be left unspecified.

* :ref:`blas_lapack_dir <blas-lapack-dir>`
* :ref:`boost_inc_dir <boost-inc-dir>`
* :ref:`debug <debug>`
* :ref:`optimize <optimize>`
* :ref:`prefix <prefix>`
* :ref:`sundials_include <sundials-include>`
* :ref:`sundials_libdir <sundials-libdir>`

General Python Module Options
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, SCons will try to build the full Python interface for
whichever version of Python is running SCons. This requires that
NumPy is installed for that version of Python, and that Cython is
installed for whichever Python is running SCons. The following SCons
options control how the Python module is built:

* :ref:`python_cmd <python-cmd>`
* :ref:`python_package <python-package>`
* :ref:`python_prefix <python-prefix>`

Note that these general options should not be used at the same time
as the Python-version specific options discussed below. If SCons
detects that it is being run with Python 2, and the
:ref:`python2_package <python2-package>` option is set, the build will
raise an error and exit; or if SCons detects that it is being run with
Python 3, and the :ref:`python3_package <python3-package>` option is
set, the build will raise an error and exit.

If a user wishes to build multiple Python interfaces, or a Python
interface for the version of Python that is not running SCons, they
should use the version-specific options below, and set the
:ref:`python_package <python-package>` option to ``none``.

Python 2 Module Options
^^^^^^^^^^^^^^^^^^^^^^^

By default, if SCons detects a Python 2 interpreter installed in a
default location (that is, if ``python2`` is on the ``PATH`` environment
variable) or ``python2_package`` is ``full``, SCons will try to build
the Python module for Python 2. The following SCons options control how
the Python 2 module is built:

* :ref:`python2_cmd <python2-cmd>`
* :ref:`python2_package <python2-package>`
* :ref:`python2_prefix <python2-prefix>`

Python 3 Module Options
^^^^^^^^^^^^^^^^^^^^^^^

By default, if SCons detects a Python 3 interpreter installed in a
default location (that is, if ``python3`` is on the ``PATH`` environment
variable) or ``python3_package`` is ``full``, SCons will try to build
the Python module for Python 3. The following SCons options control how
the Python 3 module is built:

* :ref:`python3_cmd <python3-cmd>`
* :ref:`python3_package <python3-package>`
* :ref:`python3_prefix <python3-prefix>`

Windows Only Options
^^^^^^^^^^^^^^^^^^^^

.. note::

    The ``cantera.conf`` file uses the backslash character ``\`` as an escape
    character. When modifying this file, backslashes in paths need to be escaped
    like this: ``boost_inc_dir = 'C:\\Program Files (x86)\\boost\\include'``
    This does not apply to paths specified on the command line. Alternatively,
    you can use forward slashes (``/``) in paths.

* In Windows there aren't any proper default locations for many of the packages
  that Cantera depends on, so you will need to specify these paths explicitly.

* Remember to put double quotes around any paths with spaces in them, such as
  ``"C:\Program Files"``.

* By default, SCons attempts to use the same architecture as the copy of Python
  that is running SCons, and the most recent installed version of the Visual
  Studio compiler. If you aren't building the Python module, you can override
  this with the configuration options ``target_arch`` and ``msvc_version``.

* To compile with MinGW, specify the :ref:`toolchain <toolchain>` option::

    toolchain=mingw

* :ref:`msvc_version <msvc-version>`
* :ref:`target_arch <target-arch>`
* :ref:`toolchain <toolchain>`

MATLAB Toolbox Options
^^^^^^^^^^^^^^^^^^^^^^

Building the MATLAB toolbox requires an installed copy of MATLAB, and the path
to the directory where MATLAB is installed must be specified using the following
option:

* :ref:`matlab_path <matlab-path>`

Fortran Module Options
^^^^^^^^^^^^^^^^^^^^^^

Building the Fortran module requires a compatible Fortran comiler. SCons will
attempt to find a compatible compiler by default in the ``PATH`` environment
variable. The following options control how the Fortran module is built:

* :ref:`f90_interface <f90-interface>`
* :ref:`FORTRAN <fortran>`

Documentation Options
^^^^^^^^^^^^^^^^^^^^^

The following options control if the documentation is built:

* :ref:`doxygen_docs <doxygen-docs>`
* :ref:`sphinx_docs <sphinx-docs>`

Less Common Options
^^^^^^^^^^^^^^^^^^^

* :ref:`CC <cc>`
* :ref:`CXX <cxx>`
* :ref:`env_vars <env-vars>`
* :ref:`layout <layout>`
* :ref:`VERBOSE <verbose>`
* :ref:`gtest_flags <gtest-flags>`

.. _sec-build-commands:

Build Commands
==============

The following options are possible as commands to SCons (that is, the first
argument after ``scons``):

.. code:: bash

   scons command

* ``scons help``
    Print a description of user-specifiable options.

* ``scons build``
    Compile Cantera and the language interfaces using
    default options.

* ``scons clean``
    Delete files created while building Cantera.

* ``[sudo] scons install``
    Install Cantera.

* ``[sudo] scons uninstall``
    Uninstall Cantera.

* ``scons test``
    Run all tests which did not previously pass or for which the
    results may have changed.

* ``scons test-reset``
    Reset the passing status of all tests.

* ``scons test-clean``
    Delete files created while running the tests.

* ``scons test-help``
    List available tests.

* ``scons test-NAME``
    Run the test named ``NAME``.

* ``scons <command> dump``
    Dump the state of the SCons environment to the
    screen instead of doing ``<command>``, for example,
    ``scons build dump``. For debugging purposes.

* ``scons samples``
    Compile the C++ and Fortran samples.

* ``scons msi``
    Build a Windows installer (.msi) for Cantera.

* ``scons sphinx``
    Build the Sphinx documentation

* ``scons doxygen``
    Build the Doxygen documentation

Compile Cantera & Test
======================

* Run SCons with the list of desired configuration options:

  .. code:: bash

     scons build ...

.. caution::

   If you are compiling with a version of SCons installed by Homebrew on macOS, the appropriate
   way to perform any commands with SCons is

   .. code:: bash

      python3 /usr/local/bin/scons command ...

   This ensures that the dependencies are chosen from the correct version of Python.

* If Cantera compiles successfully, you should see a message that looks like::

    *******************************************************
    Compilation completed successfully.

    - To run the test suite, type 'scons test'.
    - To install, type '[sudo] scons install'.
    *******************************************************

* If you do not see this message, check the output for errors to see what went
  wrong.

* Cantera has a series of tests that can be run with the command:

.. code:: bash

   scons test

* When the tests finish, you should see a summary indicating the number of
  tests that passed and failed.

* If you have tests that fail, try looking at the following to determine the
  source of the error:

  * Messages printed to the console while running ``scons test``
  * Output files generated by the tests

Building Documentation
^^^^^^^^^^^^^^^^^^^^^^

To build the Cantera HTML documentation, run the commands:

.. code:: bash

   scons doxygen
   scons sphinx

or append the options ``sphinx_docs=y`` and ``doxygen_docs=y`` to the build
command:

.. code:: bash

   scons build doxygen_docs=y sphinx_docs=y

.. container:: container

   .. container:: row

      .. container:: col-6 text-left

         .. container:: btn btn-primary
            :tagname: a
            :attributes: href=source-code.html

            Previous: Download the Source Code


      .. container:: col-6 text-right

         .. container:: btn btn-primary
            :tagname: a
            :attributes: href=dependencies.html

            Next: Dependencies
