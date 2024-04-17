Structure Analysis
===============================================================================

ProDy comes with many functions that can be used to calculate structural
properties and compare structures. We demonstrate only some of these functions.
For more detailed examples, see :ref:`structure-analysis` tutorial.


.. ipython:: python

   from prody import *
   from pylab import *
   ion()

Measure geometric properties
-------------------------------------------------------------------------------

Let's parse a structure:


.. ipython:: python

   p38 = parsePDB('5uoj')

Functions for analyzing structures can be found in :mod:`~prody.measure`
module. For example, you can calculate phi (φ) and psi (ψ) for the 10th
residue, or the radius of gyration of the protein as follows:

.. ipython:: python

   calcPhi(p38[10,])
   calcPsi(p38[10,])
   calcGyradius(p38)


Compare and align structures
-------------------------------------------------------------------------------

You can also compare different structures using some of the methods in
:mod:`.proteins` module.  Let's parse another p38 MAP kinase structure


.. ipython:: python

   bound = parsePDB('1zz2')


You can find similar chains in structure 5uoj and 1zz2 using
:func:`.matchChains` function:


.. ipython:: python

   apo_chA, bnd_chA, seqid, overlap = matchChains(p38, bound)[0]
   apo_chA
   bnd_chA
   seqid
   overlap

Matching Cα atoms are selected and returned as :class:`.AtomMap` instances.
We can use them to calculate RMSD and superpose structures.

.. ipython:: python

   calcRMSD(bnd_chA, apo_chA)
   bnd_chA, transformation = superpose(bnd_chA, apo_chA)
   calcRMSD(bnd_chA, apo_chA)


.. ipython:: python

   showProtein(p38);
   @savefig prody_tutorial_structure_compare.png width=4in
   showProtein(bound);


Writing PDB files
-------------------------------------------------------------------------------

PDB files can be written using the :func:`.writePDB` function.
The function accepts objects containing or referring to atomic data.

Output selected atoms:

.. ipython:: python

   writePDB('5uoj_calphas.pdb', p38.select('calpha'))


Output a chain:

.. ipython:: python

   chain_A = p38['A']
   writePDB('5uoj_chain_A.pdb', chain_A)


As you may have noticed, this function returns the file name after it is
successfully written.  This is a general behavior for ProDy output functions.
For more PDB writing examples see :ref:`writepdb`.