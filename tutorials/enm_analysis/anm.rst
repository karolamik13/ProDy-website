.. _anm:

Anisotropic Network Model (ANM)
===============================================================================

This example shows how to perform ANM calculations, and retrieve
normal mode data.  An :class:`.ANM` instance that stores Hessian matrix
(and also Kirchhoff matrix) and normal mode data describing the intrinsic
dynamics of the protein structure will be obtained.  :class:`.ANM` instances
and individual normal modes (:class:`.Mode`) can be used as input to functions
in :mod:`.dynamics` module.

See [Doruker00]_ and [Atilgan01]_ for more information on the theory of ANM.

.. [Doruker00] Doruker P, Atilgan AR, Bahar I. Dynamics of proteins predicted
   by molecular dynamics simulations and analytical approaches: Application to
   a-amylase inhibitor. *Proteins* **2000** 40:512-524.

.. [Atilgan01] Atilgan AR, Durrell SR, Jernigan RL, Demirel MC, Keskin O,
   Bahar I. Anisotropy of fluctuation dynamics of proteins with an
   elastic network model. *Biophys. J.* **2001** 80:505-515.


Parse structure
-------------------------------------------------------------------------------

We start by importing everything from the ProDy package:

.. ipython:: python

   from prody import *
   from pylab import *
   ion()

We start with parsing a PDB file by passing an identifier.
Note that if a file is not found in the current working directory, it will be
downloaded.

.. ipython:: python

   p38 = parsePDB('5uoj')
   p38

We want to use only Cα atoms, so we select them:

.. ipython:: python

   calphas = p38.select('protein and name CA')
   calphas

We can also make the same selection like this:

.. ipython:: python

   calphas2 = p38.select('calpha')
   calphas2


To check whether the selections are the same, we can try:

.. ipython:: python

   calphas == calphas2


Note that, ProDy atom selector gives the flexibility to select any set of atoms
to be used in ANM  calculations.

Build Hessian
-------------------------------------------------------------------------------

We instantiate an :class:`.ANM` instance:

.. ipython:: python

   anm = ANM('p38 ANM analysis')

Then, build the Hessian matrix by passing selected atoms (351 Cα's)
to :meth:`.ANM.buildHessian` method:

.. ipython:: python

   anm.buildHessian(calphas)

We can get a copy of the Hessian matrix using :meth:`.ANM.getHessian` method:

.. ipython:: python

   anm.getHessian().round(3)


Parameters
-------------------------------------------------------------------------------

We didn't pass any parameters to :meth:`.ANM.buildHessian` method, but it
accepts *cutoff* and *gamma* parameters, for which  default values are
``cutoff=15.0`` and ``gamma=1.0``.

.. ipython:: python

   anm.getCutoff()
   anm.getGamma()


Note that it is also possible to use an externally calculated Hessian
matrix. Just pass it to the ANM instance using :meth:`.ANM.setHessian` method.

Calculate normal modes
-------------------------------------------------------------------------------

Calculate modes using :meth:`.ANM.calcModes` method:

.. ipython:: python

   anm.calcModes()

Note that by default 20 non-zero (or non-trivial) and 6 trivial modes are
calculated. Trivial modes are not retained. To calculate a different number
of non-zero modes or to keep zero modes, try ``anm.calcModes(50, zeros=True)``.

Normal modes data
-------------------------------------------------------------------------------

.. ipython:: python

   anm.getEigvals().round(3)
   anm.getEigvecs().round(3)


You can get the covariance matrix as follows:

.. ipython:: python

   anm.getCovariance().round(2)

Covariance matrices are calculated using the available modes (slowest 20 modes
in this case). If the user calculates M slowest modes, only they will be used
in the calculation of covariances.

Individual modes
-------------------------------------------------------------------------------

Normal mode indices in Python start from 0, so the slowest mode has index 0.
By default, modes with zero eigenvalues are excluded. If they were retained,
the slowest non-trivial mode would have index 6.

Get the slowest mode by indexing :class:`.ANM` instance as follows:

.. ipython:: python

   slowest_mode = anm[0]
   slowest_mode.getEigval().round(3)
   slowest_mode.getEigvec().round(3)


Write NMD file
-------------------------------------------------------------------------------

ANM results in NMD format can be visualized using :ref:`nmwiz` VMD_ plugin.
The following statement writes the slowest 3 ANM modes into an NMD file:

.. ipython:: python

   writeNMD('p38_anm_modes.nmd', anm[:3], calphas)


Note that slicing an :class:`.ANM` objects returns a list of modes.
In this case, slowest 3 ANM modes were written into NMD file.

View modes in VMD
-------------------------------------------------------------------------------

First make sure that the VMD path is correct

.. ipython:: python

   pathVMD()


.. ipython:: python
   :verbatim:

   # if this is incorrect use setVMDpath to correct it
   viewNMDinVMD('p38_anm_modes.nmd')

This will show the slowest 3 modes in VMD using NMWiz. This concludes the ANM
example. Many of the methods demonstrated here apply to other NMA models, such
as :class:`.GNM` and :class:`.EDA`.

Advanced visualization in jupyter notebooks
-------------------------------------------------------------------------------

You can visualize structures and modes determined from ANM or GNM calculations
in jupyter notebooks using another python module, py3Dmol. It is a java-script
library that can visualize structural elements with light weight customization. 

.. _notebook: http://nbviewer.jupyter.org/github/3dmol/3Dmol.js/blob/f827da26251817ee536838b4b7b5e9d55764606e/py3Dmol/prody.ipynb

You can find an example notebook_.
