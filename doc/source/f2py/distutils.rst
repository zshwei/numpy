=============================
Using via `numpy.distutils`
=============================

.. currentmodule:: numpy.distutils.core

:mod:`numpy.distutils` is part of NumPy, and extends the standard Python
``distutils`` module to deal with Fortran sources and F2PY signature files, e.g.
compile Fortran sources, call F2PY to construct extension modules, etc.

.. topic:: Example

  Consider the following `setup file`__ for the ``fib`` examples in the previous
  section:

  .. literalinclude:: ./code/setup_example.py
    :language: python

  Running

  ::

    python setup_example.py build

  will build two extension modules ``scalar`` and ``fib2`` to the
  build directory.

  __ setup_example.py

Extensions to ``distutils``
===========================

:mod:`numpy.distutils` extends ``distutils`` with the following features:

* :class:`Extension` class argument ``sources`` may contain Fortran source
  files. In addition, the list ``sources`` may contain at most one
  F2PY signature file, and in this case, the name of an Extension module must
  match with the ``<modulename>`` used in signature file. It is
  assumed that an F2PY signature file contains exactly one ``python
  module`` block.

  If ``sources`` do not contain a signature file, then F2PY is used to scan
  Fortran source files to construct wrappers to the Fortran codes.

  Additional options to the F2PY executable can be given using the
  :class:`Extension` class argument ``f2py_options``.

* The following new ``distutils`` commands are defined:

  ``build_src``
    to construct Fortran wrapper extension modules, among many other things.
  ``config_fc``
    to change Fortran compiler options.

  Additionally, the ``build_ext`` and ``build_clib`` commands are also enhanced
  to support Fortran sources.

  Run

  ::

    python <setup.py file> config_fc build_src build_ext --help

  to see available options for these commands.

* When building Python packages containing Fortran sources, one
  can choose different Fortran compilers by using the ``build_ext``
  command option ``--fcompiler=<Vendor>``. Here ``<Vendor>`` can be one of the
  following names::

    absoft sun mips intel intelv intele intelev nag compaq compaqv gnu vast pg hpux

  See ``numpy_distutils/fcompiler.py`` for an up-to-date list of
  supported compilers or run

  ::

     f2py -c --help-fcompiler
