Introduction
===============================================================================

This tutorial describes sampling alternate protein conformations along
:ref:`anm` modes, then optimizing them using a molecular dynamics program.
Conformations obtained in this way can be useful in, for example, docking
studies when the target binding site is flexible and can be affected by
motions of protein along collective modes.

We will use a structure of mitogen-activated protein kinase 14
(:wiki:`MAPK14`), which is also known as p38 MAPK.  The structure
identifier is :pdb:`5uoj`.  PDB and PSF files are provided in `documentation
files <conformational_sampling_files.zip>`_.


Required Programs
-------------------------------------------------------------------------------

Latest version of ProDy_, Matplotlib_, VMD_ and NAMD_ are required.

.. _NAMD: http://www.ks.uiuc.edu/Research/namd/
.. _VMD: http://www.ks.uiuc.edu/Research/vmd/

Recommended Programs
-------------------------------------------------------------------------------

IPython_ is highly recommended for interactive usage.


Getting Started
-------------------------------------------------------------------------------

To follow this tutorial, you will need the following files:

.. files.txt will be automatically generated

.. literalinclude:: files.txt

We recommend that you will follow this tutorial by typing commands in an
IPython session, e.g.::

  $ ipython

or with pylab environment::

  $ ipython --pylab


First, we will make necessary imports from ProDy and Matplotlib
packages.

.. ipython:: python

   from prody import *
   from pylab import *
   ion()

We have included these imports in every part of the tutorial, so that
code copied from the online pages is complete. You do not need to repeat
imports in the same Python session.
